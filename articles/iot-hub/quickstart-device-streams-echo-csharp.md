---
title: Communiceren met een apparaat-app in C# via Azure IoT Hub apparaat-streams (preview) | Microsoft Docs
description: In deze quickstart voert u twee voorbeeldtoepassingen in C# uit die communiceren via een apparaatstream die tot stand is gebracht via IoT Hub.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 8df57d3d36dcae851c9c0e23ea609e200a429605
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65832922"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Quickstart: Communiceren met de apparaattoepassing van een in C# via IoT Hub apparaat-streams (preview)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. In deze quickstart worden twee C#-programma's gebruikt die apparaatstreams benutten om gegevens heen en weer (echo) te verzenden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

*  De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-Hubs die zijn gemaakt in de volgende regio's:

   *  **US - centraal**

   *  **VS-midden EUAP**

De twee voorbeeldtoepassingen die u uitvoert in deze snelstartgids zijn geschreven in C#. .NET Core SDK 2.1.0 of hoger moet zijn geïnstalleerd op uw ontwikkelcomputer.

*  Download de [.NET Core-SDK voor meerdere platformen van .NET](https://www.microsoft.com/net/download/all).

Gebruik de volgende opdracht om de huidige versie van C# op uw ontwikkelcomputer te controleren:

```
dotnet --version
```

*  Voer de volgende opdracht om toe te voegen van de Microsoft Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie worden IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS) specifieke opdrachten toegevoegd aan Azure CLI.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* Download het C#-voorbeeldproject van https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip en pak het ZIP-archief uit. U hebt het zowel aan de apparaat- als servicezijde nodig.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Azure Cloud Shell te maken van de apparaat-id.

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyDevice**: dit is de naam van het geregistreerde apparaat. Gebruik MyDevice, zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet eruit zoals in het volgende voorbeeld:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

3. U hebt ook de *serviceverbindingsreeks* van uw IoT Hub nodig, zodat de toepassing aan de servicezijde verbinding kan maken met uw IoT Hub en een apparaatstream kan opzetten. Met de volgende opdracht haalt u deze waarde voor uw IoT-hub op:

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Noteer de geretourneerde waarde, die er als volgt uitziet:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Communicatie tussen apparaat en service via apparaatstreams

In deze sectie maakt u de apparaat-side '-toepassing en de toepassing aan de serverkant uitvoeren en communicatie tussen de twee.

### <a name="run-the-service-side-application"></a>De toepassing aan de servicezijde uitvoeren

Navigeer naar `iot-hub/Quickstarts/device-streams-echo/service` in uw uitgepakte projectmap. Houd de volgende informatie bij de hand:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `ServiceConnectionString` | Geef de serviceverbindingsreeks van uw IoT Hub op. |
| `DeviceId` | Geef de id op van het apparaat dat u eerder hebt gemaakt, bijvoorbeeld MyDevice. |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Er treedt een time-out op als de toepassing aan de apparaatzijde niet op tijd reageert.

### <a name="run-the-device-side-application"></a>De toepassing aan de apparaatzijde uitvoeren

Navigeer naar de map `iot-hub/Quickstarts/device-streams-echo/device` in uw uitgepakte projectmap. Houd de volgende informatie bij de hand:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `DeviceConnectionString` | Geef de apparaatverbindingsreeks van uw IoT Hub op. |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Aan het einde van de laatste stap initieert de toepassing aan de servicezijde een stream naar uw apparaat die vervolgens via de stroom een tekenreeksbuffer naar de service verzendt. In dit voorbeeld wordt het programma aan de serverkant gewoon een echo terug dezelfde gegevens aan het apparaat aan te tonen geslaagde bidirectionele communicatie tussen de twee toepassingen. Zie afbeelding hieronder.

Console-uitvoer op het apparaat aan:

![Apparaat-side-console-uitvoer](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Console-uitvoer aan de servicezijde: ![Console-uitvoer aan de servicezijde](./media/quickstart-device-streams-echo-csharp/service-console-output.png )

Het verkeer dat via de stream wordt verstuurd, wordt via IoT Hub gesluisd en niet rechtstreeks verzonden. De voordelen die worden beschreven in [apparaat streamt voordelen](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT-hub ingesteld, een apparaat geregistreerd, een apparaatstroom tussen C#-toepassingen aan de apparaat- en servicezijde tot stand gebracht en de stroom gebruikt gegevens heen en weer tussen de toepassingen te verzenden.

Gebruik de onderstaande koppelingen voor meer informatie over apparaatstreams:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
