---
title: Azure IoT Hub device streamt Node.js Quick Start voor SSH en RDP (preview) | Microsoft Docs
description: In deze snelstartgids hebt uitvoeren u een voorbeeld van Node.js-toepassing die als een proxy fungeert voor het inschakelen van scenario's voor SSH en RDP via IoT Hub apparaat-streams.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 83339273d9161c3947df191d10e788980db39b28
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445970"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-nodejs-proxy-application-preview"></a>Quickstart: SSH en RDP via een IoT Hub apparaat-stroom inschakelen met behulp van een Node.js-proxytoepassing (preview)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. 

Deze snelstartgids wordt beschreven hoe de uitvoering van een Node.js-proxy-toepassing die wordt uitgevoerd aan de servicezijde Secure Shell (SSH) en Remote Desktop Protocol (RDP)-verkeer moet worden verzonden naar het apparaat via de stroom van een apparaat inschakelen. Zie voor een overzicht van de installatie, [lokale Proxy voorbeeld](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp). 

Tijdens de openbare preview ondersteunt de Node.js-SDK apparaat stromen aan de servicezijde alleen. Als gevolg hiervan, deze quickstart bevat informatie over instructies voor het uitvoeren van alleen de service-lokale proxytoepassing. Als u wilt de apparaat-local proxytoepassing uitvoert, Zie:  

   * [SSH en RDP via IoT Hub apparaat stromen met behulp van een toepassing C-proxy inschakelen](./quickstart-device-streams-proxy-c.md)
   * [SSH en RDP via IoT Hub apparaat-streams inschakelen met behulp van een C# voor een toepassingsproxy](./quickstart-device-streams-proxy-csharp.md)

Dit artikel worden de instellingen voor SSH (via poort 22) en vervolgens wordt beschreven hoe u de instellingen wijzigen voor RDP (die gebruikmaakt van poort 3389). Omdat apparaat streams en protocol-ongeacht de gebruikte toepassing, kunt u hetzelfde voorbeeld om te voldoen aan de andere typen verkeer van client / server-toepassing, meestal door het wijzigen van de communicatiepoort wijzigen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-hubs die zijn gemaakt in de volgende regio's:

  * US - centraal
  * VS-midden EUAP

* Als u wilt de service-local-toepassing uitvoert in deze Quick Start, moet u Node.js v10.x.x of hoger op uw ontwikkelcomputer.
  * Download [Node.js](https://nodejs.org) voor meerdere platforms.
  * Controleer of de huidige versie van Node.js op uw computer ontwikkeling met behulp van de volgende opdracht uit:

   ```
   node --version
   ```

* De Azure IoT-extensie voor Azure CLI toevoegen aan uw exemplaar van Cloud Shell met de volgende opdracht. De IOT-extensie wordt toegevoegd voor IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS)-specifieke opdrachten naar de Azure CLI.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

* Als u dit nog niet hebt gedaan, [download het Node.js-voorbeeldproject](https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip) en pak het ZIP-archief.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Als u [Snelstart: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-node.md), kunt u deze stap overslaan.

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Als u voltooid [Quick Start: Als u telemetrie vanaf een apparaat wilt verzenden naar een IoT-hub](quickstart-send-telemetry-node.md), kunt u deze stap overslaan.

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Cloud Shell voor het maken van de apparaat-id:

   > [!NOTE]
   > * Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.
   > * Gebruik *Mijnapparaat*, zoals wordt weergegeven. Het is de naam van het geregistreerde apparaat. Als u een andere naam voor uw apparaat, gebruikt u die naam in dit artikel en de naam van het apparaat in de voorbeeldtoepassingen bijwerken voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

1. Als u wilt zorgen dat de back-end-toepassing verbinding maken met uw IoT-hub en de berichten ophalen, moet u ook een *serviceverbindingsreeks*. De volgende opdracht haalt de tekenreeks voor uw IoT-hub:

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

Zoals eerder vermeld, de IoT Hub Node.js-SDK biedt ondersteuning voor apparaat stromen aan de servicezijde alleen. Voor de apparaat-local-toepassing, gebruikt u een apparaat proxy-toepassing die beschikbaar is in een van de volgende Quick starts:

   * [SSH en RDP via IoT Hub apparaat stromen met behulp van een toepassing C-proxy inschakelen](./quickstart-device-streams-proxy-c.md)
   * [SSH en RDP via IoT Hub apparaat-streams inschakelen met behulp van een C# voor een toepassingsproxy](./quickstart-device-streams-proxy-csharp.md) 

Voordat u met de volgende stap doorgaat, moet u ervoor dat de apparaat-local proxytoepassing wordt uitgevoerd.

### <a name="run-the-service-local-proxy-application"></a>Voer de proxytoepassing in de service uit

Met de apparaat-local proxytoepassing die wordt uitgevoerd, moet u de van service-local-proxytoepassing die geschreven in Node.js door het volgende te doen uitvoeren:

1. Voor de omgevingsvariabelen, geeft u de Servicereferenties van uw, de apparaat-ID van het doel waarop de SSH-daemon wordt uitgevoerd en het poortnummer voor de proxy die wordt uitgevoerd op het apparaat.

   ```
   # In Linux
   export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
   export STREAMING_TARGET_DEVICE="MyDevice"
   export PROXY_PORT=2222

   # In Windows
   SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
   SET STREAMING_TARGET_DEVICE=MyDevice
   SET PROXY_PORT=2222
   ```

   De bovenstaande waarden zodat deze overeenkomt met uw apparaat-ID en de verbindingsreeks te wijzigen.

1. Ga naar de *snelstartgidsen/apparaat-streams-service* map in uw map uitgepakte project, en voer de service-local-proxytoepassing.

   ```
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service

   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   # Run the service-local proxy application
   node proxy.js
   ```

### <a name="ssh-to-your-device-via-device-streams"></a>SSH naar uw apparaat via apparaatstreams

Voer in Linux, SSH met behulp van `ssh $USER@localhost -p 2222` op een terminal. In Windows, gebruikt u uw favoriete SSH-client (bijvoorbeeld PuTTY).

Console-uitvoer op de service-lokale nadat SSH-sessie tot stand is gebracht (de proxy van service-lokale toepassing luistert op poort 2222):

![SSH-terminal uitvoer](./media/quickstart-device-streams-proxy-nodejs/service-console-output.png)

Console-uitvoer van de toepassing van de SSH-client (SSH-client communiceert met SSH-daemon door verbinding te maken naar poort 22, waarmee de service-local-proxytoepassing luistert):

![SSH-client-uitvoer](./media/quickstart-device-streams-proxy-nodejs/ssh-console-output.png)

### <a name="rdp-to-your-device-via-device-streams"></a>RDP naar uw apparaat via apparaatstreams

Nu uw RDP-clienttoepassing gebruiken en maak verbinding met de proxy voor de service op poort 2222, een willekeurige poort die u eerder hebt gekozen.

> [!NOTE]
> Zorg ervoor dat uw apparaat-proxy correct is geconfigureerd voor RDP met poort 3389.

![De RDP-client verbinding maakt met de service-local proxytoepassing](./media/quickstart-device-streams-proxy-nodejs/rdp-screen-capture.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u instellen van een IoT-hub, een apparaat hebt geregistreerd en geïmplementeerd een proxy voor servicetoepassing RDP en SSH inschakelen voor een IoT-apparaat. Het RDP en SSH-verkeer wordt via een apparaat-stroom via de IoT hub worden tunnel. Dit proces elimineert de noodzaak voor directe verbinding met het apparaat.

Zie voor meer informatie over het apparaat stromen:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
