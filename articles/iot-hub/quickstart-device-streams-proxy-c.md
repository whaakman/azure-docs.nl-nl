---
title: Azure IoT Hub apparaat-streams C-quickstart voor SSH en RDP (preview) | Microsoft Docs
description: In deze snelstartgids hebt uitvoert u een voorbeeld van C toepassing die als een proxy fungeert voor het inschakelen van scenario's voor SSH en RDP via IoT Hub apparaat stromen.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: e21d7381831553f8d82b00d9ed3be0b03f13bca9
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735126"
---
# <a name="quickstart-enable-ssh-and-rdp-over-an-iot-hub-device-stream-by-using-a-c-proxy-application-preview"></a>Quickstart: SSH en RDP via een IoT Hub apparaat-stroom inschakelen met behulp van een toepassing in C-proxy (preview)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Zie voor een overzicht van de installatie, [de voorbeeldpagina van lokale Proxy](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp).

Deze snelstartgids wordt beschreven hoe de instellingen voor Secure Shell (SSH)-verkeer (met behulp van poort 22) via apparaat streams tunneling. Setup van Remote Desktop Protocol (RDP)-verkeer is vergelijkbaar en vereist een wijziging van een eenvoudige configuratie. Omdat apparaat streams en protocol-ongeacht de gebruikte toepassing, kunt u in deze Quick Start om te voldoen aan andere typen toepassingsverkeer kunt wijzigen.

## <a name="how-it-works"></a>Hoe werkt het?

De volgende afbeelding ziet u hoe de end-to-end-connectiviteit tussen de SSH-client en de SSH-daemon-processen voor het inschakelen van de apparaat - en service-lokale proxy's. Tijdens de openbare preview ondersteunt de C-SDK apparaat stromen op het apparaat plaats. Als gevolg hiervan, deze quickstart bevat informatie over instructies voor het uitvoeren van alleen de apparaat-lokale proxytoepassing. U moet een van de volgende servicezijde Quick starts worden uitgevoerd:

* [SSH/RDP via IoT Hub device-streams met behulp van C# proxy](./quickstart-device-streams-proxy-csharp.md)
* [SSH/RDP via IoT Hub apparaat-stromen met behulp van NodeJS proxy](./quickstart-device-streams-proxy-nodejs.md).

![Installatie van lokale proxy](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. De proxy-service-lokale maakt verbinding met de IoT-hub en start een stroom apparaat naar het doelapparaat.

2. De apparaat-local-proxy is voltooid de handshake van de initialisatie van stream en maakt een end-to-end-streaming-tunnel via de IoT-hub streaming-eindpunt aan de servicezijde.

3. De apparaat-local-proxy verbinding met de SSH-daemon op poort 22 op het apparaat luistert. Deze instelling kan worden geconfigureerd, zoals beschreven in de sectie 'De apparaat-local-proxytoepassing uitvoeren'.

4. De proxy-service-lokale wacht op nieuwe SSH-verbindingen van een gebruiker door te luisteren op een specifieke poort, die in dit geval poort 2222 is. Deze instelling kan worden geconfigureerd, zoals beschreven in de sectie 'De apparaat-local-proxytoepassing uitvoeren'. Als de gebruiker verbinding via SSH-client maakt, kan de tunnel SSH toepassingsverkeer om te worden overgedragen tussen de SSH-client en server programma's.

> [!NOTE]
> SSH-verkeer dat wordt verzonden via de stroom van een apparaat is tunnel via de IoT-hub streaming-eindpunt plaats van rechtstreeks tussen service- en apparaat verzonden. Zie voor meer informatie de [voordelen van het gebruik van Iot Hub apparaat-streams](iot-hub-device-streams-overview.md#benefits). Daarnaast wordt ziet de afbeelding u de SSH-daemon wordt uitgevoerd op hetzelfde apparaat (of de machine) als de apparaat-local-proxy. In deze snelstartgids kan biedt het IP-adres van de SSH-daemon de apparaat-local-proxy en de daemon om uit te voeren op verschillende computers ook.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-hubs die zijn gemaakt in de volgende regio's:

  * US - centraal
  * VS-midden EUAP

* Installeer [Visual Studio 2019](https://www.visualstudio.com/vs/) met de [bureaubladontwikkeling met C++ ](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) werkbelasting ingeschakeld.
* Installeer de meest recente versie van [Git](https://git-scm.com/download/).

* Voer de volgende opdracht om toe te voegen van de Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie wordt toegevoegd voor IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS)-specifieke opdrachten naar de Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Voor deze snelstart gebruikt u de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). Voorbereiden van een ontwikkelomgeving die is gebruikt om te klonen en bouwen de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) vanuit GitHub. De SDK op GitHub bevat de voorbeeldcode die wordt gebruikt in deze Quick Start.

1. Download de [CMake-bouwsysteem](https://cmake.org/download/).

    Is het belangrijk dat de Visual Studio-vereisten (Visual Studio en de *bureaubladontwikkeling met C++*  werkbelasting) zijn geïnstalleerd op uw computer *voordat* u begint met de CMake-installatie. Nadat de vereisten voldaan is en de download is geverifieerd, kunt u de CMake-build-systeem kunt installeren.

1. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Houd rekening met deze bewerking is een paar minuten duren.

1. Maak een *cmake* submap in de hoofdmap van de Git-opslagplaats, zoals wordt weergegeven in de volgende opdracht, en navigeer naar die map.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Voer de volgende opdrachten uit de *cmake* directory voor het bouwen van een versie van de SDK die specifiek is voor uw clientplatform voor ontwikkeling.

   * In Linux:

      ```bash
      cmake ..
      make -j
      ```

   * Voer in Windows, de volgende opdrachten in de opdrachtprompt voor ontwikkelaars voor Visual Studio 2015 of 2017. Een Visual Studio-oplossing voor het gesimuleerde apparaat wordt gegenereerd de *cmake* directory.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) om een gesimuleerd apparaat te registreren.

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

## <a name="ssh-to-a-device-via-device-streams"></a>SSH naar een apparaat via apparaatstreams

In deze sectie maakt maken u een end-to-end-stream zo tunnel SSH-verkeer.

### <a name="run-the-device-local-proxy-application"></a>Voer de proxytoepassing in het apparaat uit

1. Bewerken van het bronbestand *iothub_client_c2d_streaming_sample.c* in de map *iothub_client/samples/iothub_client_c2d_streaming_sample*, en de verbindingsreeks van uw apparaat opgeven, doel apparaat IP/host-naam en de SSH-poort 22:

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[Connection string of IoT Hub]";
   static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

1. Het voorbeeld worden gecompileerd:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to cmake at root of repository
   cmake --build . -- /m /p:Configuration=Release
   ```

1. Voer het gecompileerde programma uit op het apparaat:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_proxy_sample
   ./iothub_client_c2d_streaming_proxy_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_proxy_sample\Release
   iothub_client_c2d_streaming_proxy_sample.exe
   ```

### <a name="run-the-service-local-proxy-application"></a>Voer de proxytoepassing in de service uit

Zoals beschreven in de sectie 'Hoe het werkt', vereist tot stand brengen van een end-to-end-stream zo tunnel SSH-verkeer een lokale proxyserver aan elk einde (op de service en de zijkanten apparaat). Tijdens de openbare preview ondersteunt de C-SDK voor IoT Hub apparaat stromen op het apparaat plaats. Als u wilt bouwen en uitvoeren van de service-local-proxy, volg de instructies in een van de volgende Quick starts:

   * [SSH/RDP via IoT Hub device-streams met behulp van C# toepassingsproxy-apps](./quickstart-device-streams-proxy-csharp.md)
   * [SSH/RDP via IoT Hub apparaat-stromen met behulp van Node.js toepassingsproxy-apps](./quickstart-device-streams-proxy-nodejs.md)

### <a name="establish-an-ssh-session"></a>Een SSH-sessie tot stand brengen

Wanneer zowel de apparaat - en service-local-proxy's worden uitgevoerd, gebruikt u uw SSH-client-programma en verbinding maken met de service-local-proxy op poort 2222 (in plaats van de SSH-daemon rechtstreeks).

```cmd/sh
ssh <username>@localhost -p 2222
```

Op dit moment het SSH-aanmelden-venster wordt u gevraagd uw referenties in te voeren.

De volgende afbeelding ziet u de console-uitvoer van de apparaat-local-proxy, die verbinding maakt met de SSH-daemon op `IP_address:22`:

![Apparaat-local-proxy-uitvoer](./media/quickstart-device-streams-proxy-c/device-console-output.png)

De volgende afbeelding ziet de uitvoer van de console van het SSH-client-programma. De SSH-client communiceert met de SSH-daemon door verbinding te maken naar poort 22, waarmee de proxy-service-lokale luistert op:

![SSH-client-uitvoer](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u instellen van een IoT-hub, een apparaat hebt geregistreerd, een apparaat- en een service-local-proxy wordt tot stand brengen van een apparaat-stream via IoT Hub geïmplementeerd en gebruikt de proxy voor de tunneling van SSH-verkeer.

Zie voor meer informatie over het apparaat stromen:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
