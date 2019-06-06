---
title: Communiceren met een apparaat-app in C# via Azure IoT Hub apparaat-streams (preview) | Microsoft Docs
description: In deze snelstartgids hebt u twee voorbeelden uitvoeren C# toepassingen die via de stroom van een apparaat tot stand gebracht via IoT Hub communiceren.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 74a8fc40cff12070f7cea99981eb4e8321d7c1ef
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735140"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Quickstart: Communiceren met de apparaattoepassing van een in C# via IoT Hub apparaat-streams (preview)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. In deze snelstartgids bestaat uit twee C# toepassingen die profiteren van apparaat-streams gegevens heen en weer (echo) te verzenden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-hubs die zijn gemaakt in de volgende regio's:
  * US - centraal
  * VS-midden EUAP

* De twee voorbeeldtoepassingen die u in deze Quick Start uitvoert zijn geschreven met behulp van C#. U moet de .NET Core SDK 2.1.0 of hoger op uw ontwikkelcomputer.
  * Download de [.NET Core-SDK voor meerdere platformen van .NET](https://www.microsoft.com/net/download/all).
  * Controleer of de huidige versie van C# op een ontwikkelcomputer met behulp van de volgende opdracht uit:

   ```
   dotnet --version
   ```

* De Azure IoT-extensie voor Azure CLI toevoegen aan uw exemplaar van Cloud Shell met de volgende opdracht. De IOT-extensie wordt toegevoegd voor IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS)-specifieke opdrachten naar de Azure CLI.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* [Het voorbeeld downloaden C# project](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) en pak het ZIP-archief. U hebt deze nodig op zowel de zijde van het apparaat en de servicezijde.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Cloud Shell voor het maken van de apparaat-id:

   > [!NOTE]
   > * Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.
   > * Gebruik *Mijnapparaat*, zoals wordt weergegeven. Het is de naam van het geregistreerde apparaat. Als u een andere naam voor uw apparaat, gebruikt u die naam in dit artikel en de naam van het apparaat in de voorbeeldtoepassingen bijwerken voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Aan de *apparaatverbindingsreeks* voor het apparaat dat u zojuist hebt geregistreerd, kunt u de volgende opdracht uitvoeren in Cloud Shell:

   > [!NOTE]
   > Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Houd er rekening mee de apparaatverbindingsreeks voor later gebruik in deze Quick Start. Het lijkt op het volgende voorbeeld:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

3. U hebt ook de *serviceverbindingsreeks* van uw IoT Hub nodig, zodat de toepassing aan de servicezijde verbinding kan maken met uw IoT Hub en een apparaatstream kan opzetten. Met de volgende opdracht haalt u deze waarde voor uw IoT-hub op:

   > [!NOTE]
   > Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Houd er rekening mee de geretourneerde waarde voor later gebruik in deze Quick Start. Het lijkt op het volgende voorbeeld:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Communiceren tussen het apparaat en de service via apparaat stromen

In deze sectie maakt u de apparaat-side '-toepassing en de toepassing aan de serverkant uitvoeren en communicatie tussen de twee.

### <a name="run-the-service-side-application"></a>De toepassing aan de servicezijde uitvoeren

Ga naar de *iot-hub/snelstartgidsen/apparaat-streams-echo/service* Active directory in de map van de uitgepakte project. De volgende informatie bij de hand houden:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `ServiceConnectionString` | De service-verbindingsreeks van uw IoT-hub opgeven. |
| `DeviceId` | Geef de ID van het apparaat dat u eerder hebt gemaakt (bijvoorbeeld *Mijnapparaat*). |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Er treedt een time-out op als de toepassing aan de apparaatzijde niet op tijd reageert.

### <a name="run-the-device-side-application"></a>De toepassing aan de apparaatzijde uitvoeren

Ga naar de *iot-hub/snelstartgidsen/apparaat-streams-echo/apparaat* Active directory in de map van de uitgepakte project. De volgende informatie bij de hand houden:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `DeviceConnectionString` | Geef de apparaatverbindingsreeks van uw IoT Hub op. |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Aan het einde van de laatste stap initieert de service-side '-toepassing een stroom op uw apparaat. Nadat de stroom tot stand is gebracht, wordt in de toepassing een buffer voor tekenreeks naar de service via de stroom verzendt. In dit voorbeeld de toepassing aan de serverkant gewoon een echo weer dezelfde gegevens op het apparaat, die laat zien van een geslaagde bidirectionele communicatie tussen de twee toepassingen.

Console-uitvoer op het apparaat plaats:

![Console-uitvoer op het apparaat plaats](./media/quickstart-device-streams-echo-csharp/device-console-output.png)

Console-uitvoer aan de servicezijde:

![Console-uitvoer aan de servicezijde](./media/quickstart-device-streams-echo-csharp/service-console-output.png)

Het verkeer wordt verzonden via de stroom is tunnel via de IoT hub plaats van rechtstreeks verzonden. De voordelen die worden beschreven in [apparaat streamt voordelen](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u hebt instellen van een IoT-hub, een apparaat hebt geregistreerd, een apparaat-stroom tussen tot stand gebracht C# toepassingen aan de zijkant apparaat- en service en de stroom gebruikt voor het verzenden van gegevens heen en weer tussen de toepassingen.

Zie voor meer informatie over het apparaat stromen:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
