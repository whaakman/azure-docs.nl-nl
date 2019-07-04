---
title: Azure IoT Hub apparaat-streams C# Quick Start voor SSH en RDP (preview) | Microsoft Docs
description: In deze snelstartgids hebt u twee voorbeelden uitvoeren C# toepassingen die de mogelijkheid tot SSH en RDP-scenario's van een IoT Hub apparaat-stream.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 914568ee4b669605807c8a0e386cd540145c9522
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446104"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Quickstart: SSH en RDP via een IoT Hub apparaat-stroom inschakelen met behulp van een C# -proxytoepassing (preview)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Deze Quick Start-handleiding bestaat uit twee C# toepassingen waarmee verkeer van de client / server-toepassing (zoals Secure Shell [SSH] en Remote Desktop Protocol [RDP] om te worden verzonden via een apparaat-stream die via een IoT-hub tot stand gebracht. Zie voor een overzicht van de installatie, [lokale proxy-voorbeeldtoepassing voor SSH of RDP](iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Dit artikel worden eerst de instellingen voor SSH (met behulp van poort 22) en vervolgens wordt beschreven hoe u de poort van de instellingen wijzigen voor RDP. Omdat apparaat streams en protocol-ongeacht de gebruikte toepassing, kan hetzelfde voorbeeld om te voldoen aan andere typen toepassingsverkeer worden gewijzigd. Deze wijziging omvat meestal alleen de communicatiepoort wijzigen in de naam die wordt gebruikt door de gewenste toepassing.

## <a name="how-it-works"></a>Hoe werkt het?

De volgende afbeelding ziet u hoe de end-to-end-connectiviteit tussen de SSH-client en de SSH-daemon-processen voor het inschakelen van de apparaat-local en service-local-proxy-toepassingen in dit voorbeeld. Hier gaan we ervan uit dat de daemon op hetzelfde apparaat als de apparaat-local proxytoepassing wordt uitgevoerd.

![Installatie van de toepassing lokaal proxy](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. De van service-local-proxytoepassing verbinding maakt met de IoT-hub en initieert de stroom van een apparaat naar het doelapparaat.

1. De apparaat-local-proxytoepassing de stream inleiding handshake is voltooid en maakt een end-to-end-streaming-tunnel via de IoT-hub streaming-eindpunt aan de servicezijde.

1. De apparaat-local-proxytoepassing verbindt met de SSH-daemon op poort 22 op het apparaat luistert. Deze instelling kan worden geconfigureerd, zoals beschreven in de sectie 'De apparaat-local-proxytoepassing uitvoeren'.

1. De service-local-proxytoepassing wacht voor nieuwe SSH-verbindingen van een gebruiker door te luisteren op een specifieke poort, die in dit geval poort 2222 is. Deze instelling kan worden geconfigureerd, zoals beschreven in de sectie 'De service-local-proxytoepassing uitvoeren'. Als de gebruiker verbinding via de SSH-client maakt, kan de tunnel SSH toepassingsverkeer om te worden overgedragen tussen de SSH-client en server-toepassing.

> [!NOTE]
> SSH-verkeer dat wordt verzonden via de stroom van een apparaat is tunnel via de IoT-hub streaming-eindpunt plaats van rechtstreeks tussen service- en apparaat verzonden. Zie voor meer informatie de [voordelen van het gebruik van Iot Hub apparaat-streams](iot-hub-device-streams-overview.md#benefits).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-hubs die zijn gemaakt in de volgende regio's:

  * US - centraal
  * VS-midden EUAP

* De twee voorbeeldtoepassingen die u in deze Quick Start uitvoert zijn geschreven met behulp van C#. U moet de .NET Core SDK 2.1.0 of hoger op uw ontwikkelcomputer.

  U kunt downloaden de [.NET Core-SDK voor meerdere platformen van .NET](https://www.microsoft.com/net/download/all).

* Controleer of de huidige versie van C# op een ontwikkelcomputer met behulp van de volgende opdracht uit:

    ```
    dotnet --version
    ```

* Voer de volgende opdracht om toe te voegen van de Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie wordt toegevoegd voor IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS)-specifieke opdrachten naar de Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

* [Het voorbeeld downloaden C# project](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip), en pak het ZIP-archief.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstartgids gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Cloud Shell voor het maken van de apparaat-id:

   > [!NOTE]
   > * Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.
   > * Gebruik *Mijnapparaat*, zoals wordt weergegeven. Het is de naam van het geregistreerde apparaat. Als u een andere naam voor uw apparaat, gebruikt u die naam in dit artikel en de naam van het apparaat in de voorbeeldtoepassingen bijwerken voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Aan de *apparaatverbindingsreeks* voor het apparaat dat u zojuist hebt geregistreerd, kunt u de volgende opdrachten uitvoeren in Cloud Shell:

   > [!NOTE]
   > Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Houd er rekening mee de apparaatverbindingsreeks voor later gebruik in deze Quick Start. Het lijkt op het volgende voorbeeld:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

1. Als u wilt verbinding maken met uw IoT-hub en de stream van een apparaat tot stand brengen, moet u ook de *serviceverbindingsreeks* van uw IoT-hub kunt u de toepassing aan de serverkant inschakelen. Met de volgende opdracht haalt u deze waarde voor uw IoT-hub op:

   > [!NOTE]
   > Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Houd er rekening mee de geretourneerde waarde voor later gebruik in deze Quick Start. Het lijkt op het volgende voorbeeld:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="ssh-to-a-device-via-device-streams"></a>SSH naar een apparaat via apparaatstreams

In deze sectie maakt maken u een end-to-end-stream zo tunnel SSH-verkeer.

### <a name="run-the-device-local-proxy-application"></a>Voer de proxytoepassing in het apparaat uit

Ga naar de *apparaat-streams-proxy/apparaat* Active directory in de map van de uitgepakte project. De volgende informatie bij de hand houden:

| Argumentnaam | Argumentwaarde |
|----------------|-----------------|
| `deviceConnectionString` | De verbindingsreeks van het apparaat dat u eerder hebt gemaakt. |
| `targetServiceHostName` | Het IP-adres waar de SSH-server luistert. Het adres zou worden `localhost` alsof het hetzelfde IP-adres waar de proxytoepassing van de apparaat-local-wordt uitgevoerd. |
| `targetServicePort` | De poort die wordt gebruikt door uw toepassingsprotocol (voor SSH, standaard, zou dit poort 22).  |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $deviceConnectionString localhost 22

# In Windows
dotnet run %deviceConnectionString% localhost 22
```

### <a name="run-the-service-local-proxy-application"></a>Voer de proxytoepassing in de service uit

Navigeer naar `device-streams-proxy/service` in uw uitgepakte projectmap. Houd de volgende informatie bij de hand:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `iotHubConnectionString` | De serviceverbindingsreeks van uw IoT Hub. |
| `deviceId` | De id van het apparaat dat u eerder hebt gemaakt. |
| `localPortNumber` | Een andere lokale poort waarmee de SSH-client verbinding te maken. We gebruiken poort 2222 in dit voorbeeld, maar u kunt andere willekeurige getallen. |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-the-ssh-client"></a>De SSH-client wordt uitgevoerd

Nu uw SSH-clienttoepassing gebruiken en maak verbinding met de service-local-proxytoepassing op poort 2222 (in plaats van de SSH-daemon rechtstreeks).

```
ssh <username>@localhost -p 2222
```

Op dit moment het SSH-aanmelden-venster wordt u gevraagd uw referenties in te voeren.

Console-uitvoer aan de servicezijde (de service-local-proxytoepassing luistert op poort 2222):

![Uitvoer van de toepassing service-local-proxy](./media/quickstart-device-streams-proxy-csharp/service-console-output.png)

Console-uitvoer van de apparaat-local proxytoepassing, die verbinding maakt met de SSH-daemon op *IP_address:22*:

![Uitvoer van de toepassing apparaat-local-proxy](./media/quickstart-device-streams-proxy-csharp/device-console-output.png)

Console-uitvoer van de SSH-clienttoepassing. De SSH-client communiceert met de SSH-daemon door verbinding te maken naar poort 22, waarmee de service-local-proxytoepassing luistert op:

![Uitvoer van SSH-client-toepassing](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="rdp-to-a-device-via-device-streams"></a>RDP naar een apparaat via apparaatstreams

De instellingen voor RDP is vergelijkbaar met de instellingen voor SSH (hierboven beschreven). U gebruikt de RDP-doel-IP en poort 3389 in plaats daarvan en gebruikt u de RDP-client (in plaats van de SSH-client).

### <a name="run-the-device-local-proxy-application-rdp"></a>De apparaat-local-proxytoepassing (RDP) worden uitgevoerd

Ga naar de *apparaat-streams-proxy/apparaat* Active directory in de map van de uitgepakte project. De volgende informatie bij de hand houden:

| Argumentnaam | Argumentwaarde |
|----------------|-----------------|
| `DeviceConnectionString` | De verbindingsreeks van het apparaat dat u eerder hebt gemaakt. |
| `targetServiceHostName` | De hostnaam of IP-adres waarop RDP-server wordt uitgevoerd. Het adres zou worden `localhost` alsof het hetzelfde IP-adres waar de proxytoepassing van de apparaat-local-wordt uitgevoerd. |
| `targetServicePort` | De poort die wordt gebruikt door uw toepassingsprotocol (voor RDP, standaard, zou dit poort 3389).  |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/device

# Run the application
# In Linux or macOS
dotnet run $DeviceConnectionString localhost 3389

# In Windows
dotnet run %DeviceConnectionString% localhost 3389
```

### <a name="run-the-service-local-proxy-application-rdp"></a>Uitvoeren van de service-local-proxytoepassing (RDP)

Navigeer naar `device-streams-proxy/service` in uw uitgepakte projectmap. Houd de volgende informatie bij de hand:

| Parameternaam | Parameterwaarde |
|----------------|-----------------|
| `iotHubConnectionString` | De serviceverbindingsreeks van uw IoT Hub. |
| `deviceId` | De id van het apparaat dat u eerder hebt gemaakt. |
| `localPortNumber` | Een andere lokale poort waarmee de SSH-client verbinding te maken. In dit voorbeeld gebruiken we poort 2222, maar u kunt dat wijzigen in willekeurige andere getallen. |

Compileer de code en voer deze als volgt uit:

```
cd ./iot-hub/Quickstarts/device-streams-proxy/service/

# Build the application
dotnet build

# Run the application
# In Linux or macOS
dotnet run $serviceConnectionString MyDevice 2222

# In Windows
dotnet run %serviceConnectionString% MyDevice 2222
```

### <a name="run-rdp-client"></a>RDP-client uitvoeren

Nu uw RDP-clienttoepassing gebruiken en maak verbinding met de service-local-proxytoepassing op poort 2222 (dit was een willekeurige poort die u eerder hebt gekozen).

![RDP verbinding maakt met de service-local proxytoepassing](./media/quickstart-device-streams-proxy-csharp/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u instellen van een IoT-hub, een apparaat hebt geregistreerd, geïmplementeerde toepassingen tot stand brengen van een apparaat-stroom via de IoT hub apparaat-local en service-local-proxy en de proxy-toepassingen gebruikt voor tunneling van SSH of RDP-verkeer. De dezelfde paradigma aankan andere client / server-protocollen, waarop de server wordt uitgevoerd op het apparaat (bijvoorbeeld de SSH-daemon).

Zie voor meer informatie over het apparaat stromen:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
