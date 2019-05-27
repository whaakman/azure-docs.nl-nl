---
title: Azure IoT Hub-apparaatstreams met C#, quickstart voor SSH/RDP (preview) | Microsoft Docs
description: In deze Quick Start, moet u C met een voorbeeldtoepassing die als een proxy fungeert om SSH/RDP-scenario's tot IoT Hub apparaat stromen uitvoeren.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: ae5db52d7ac00080c2a740820debe6384cfa8dff
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872650"
---
# <a name="quickstart-sshrdp-over-an-iot-hub-device-stream-using-a-c-proxy-application-preview"></a>Quickstart: SSH/RDP via een IoT Hub apparaat-stream met behulp van een toepassing in C-proxy (preview)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Zie [de voorbeeldpagina van lokale Proxy](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) voor een overzicht van de installatie.

In dit document wordt beschreven hoe u tunneling van SSH-verkeer instelt (met behulp van poort 22) via apparaatstreams. De configuratie voor RDP-verkeer is vergelijkbaar en vereist een eenvoudige wijziging in de configuratie. Omdat apparaatstreams toepassings- en protocolneutraal zijn, kan de bestaande quickstart zo worden bewerkt (door de communicatiepoorten te wijzigen) dat er andere soorten toepassingsverkeer mogelijk zijn.

## <a name="how-it-works"></a>Hoe het werkt

De onderstaande afbeelding ziet u de installatie van hoe de apparaat - en service-lokale proxy's end-to-end-connectiviteit tussen de SSH-client en de SSH-daemon-processen kunnen. Gedurende de openbare preview biedt de C SDK alleen ondersteuning voor apparaatstreams aan de apparaatzijde. Daarom bevat deze quickstart alleen instructies voor het uitvoeren van de proxytoepassing in het apparaat. U moet een van de volgende servicezijde Quick starts worden uitgevoerd:

* [SSH/RDP via IoT Hub device-streams met behulp van C# proxy](./quickstart-device-streams-proxy-csharp.md)

* [SSH/RDP via IoT Hub apparaat-stromen met behulp van NodeJS proxy](./quickstart-device-streams-proxy-nodejs.md).

![Installatie van lokale proxy](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg)

1. Service-lokale proxy verbinding maakt met IoT hub en een apparaat stream naar het doelapparaat wordt gestart.

2. De proxy in het apparaat voltooit de stream-handshake en zet een end-to-end-streaming-tunnel op van het streaming-eindpunt van IoT Hub naar de servicezijde.

3. Apparaat-local-proxy maakt verbinding met de SSH-daemon (SSHD) luistert op poort 22 op het apparaat (dit kan worden geconfigureerd, zoals beschreven in de [ *voert u de toepassing van de lokale proxy apparaat* sectie](#run-the device-local-proxy-application)).

4. Service-lokale proxy wacht op voor nieuwe SSH-verbindingen van de gebruiker door te luisteren op een specifieke poort die in dit geval poort 2222 is (dit kan ook worden geconfigureerd, zoals beschreven in de [uitvoeren het toepassingsgedeelte van apparaat-lokale proxy](#run-the-device-local-proxy-application). Wanneer een gebruiker verbinding maakt via een SSH-client, zorgt de tunnel ervoor dat SSH-toepassingsverkeer kan worden overgebracht tussen de SSH-client en serverprogramma's.

> [!NOTE]
> SSH-verkeer dat via een apparaatstream wordt verstuurd, gaat via een tunnel van het streaming-eindpunt van IoT Hub in plaats van dat het rechtstreeks tussen de service en het apparaat wordt verzonden. Lees voor meer informatie over de [voordelen van het gebruik van Iot Hub apparaat-streams](iot-hub-device-streams-overview.md#benefits). In de afbeelding ziet u bovendien hoe de SSH-daemon wordt uitgevoerd op hetzelfde apparaat (of machine) als de proxy in het apparaat. In deze quickstart zorgt de verstrekking van het IP-adres van de SSH-daemon ervoor dat de proxy in het apparaat en daemon ook op verschillende computers worden uitgevoerd.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-Hubs die zijn gemaakt in de volgende regio's:

   * **US - centraal**

   * **VS-midden EUAP**

* Installeer [Visual Studio 2019](https://www.visualstudio.com/vs/) met de [' bureaubladontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) werkbelasting ingeschakeld.
* Installeer de meest recente versie van [Git](https://git-scm.com/download/).

* Voer de volgende opdracht om toe te voegen van de Microsoft Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie worden IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS) specifieke opdrachten toegevoegd aan Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

In deze Quick Start hebt u de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). U moet een ontwikkelomgeving die is gebruikt om te klonen en bouw voorbereiden de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) vanuit GitHub. De SDK op GitHub bevat de voorbeeldcode gebruikt in deze snelstartgids. 


1. Download de [CMake-bouwsysteem](https://cmake.org/download/).

    Het is belangrijk dat de vereisten voor Visual Studio met (Visual Studio en de workload Desktopontwikkeling met C++) op uw computer zijn geïnstalleerd **voordat** de `CMake`-installatie wordt gestart. Zodra aan de vereisten is voldaan en de download is geverifieerd, installeert u het CMake-bouwsysteem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:

    
   ```
   git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
   ```

   Deze bewerking kan enkele minuten in beslag nemen.

3. Maak de submap `cmake` in de hoofdmap van de Git-opslagplaats en navigeer naar die map. 

   ```
   cd azure-iot-sdk-c
   mkdir cmake
   cd cmake
   ```

4. Voer de volgende opdrachten uit de `cmake` directory voor het bouwen van een versie van de SDK die specifiek zijn voor uw clientplatform voor ontwikkeling.

   * In Linux:

      ```bash
      cmake ..
      make -j
      ```

   * Voer in Windows, de volgende opdrachten in de opdrachtprompt voor ontwikkelaars voor Visual Studio 2015 of 2017. Er wordt een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd in de map `cmake`.

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

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u de Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Azure Cloud Shell te maken van de apparaat-id.

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyDevice**: dit is de naam van het geregistreerde apparaat. Gebruik MyDevice, zoals wordt weergegeven. Als u een andere naam voor uw apparaat, moet u ook deze naam in dit artikel gebruiken en bijwerken van de naam van het apparaat in de voorbeeldtoepassingen voordat u ze uit te voeren.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om op te halen de _apparaatverbindingsreeks_ voor het apparaat dat u hebt geregistreerd:

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet eruit zoals in het volgende voorbeeld:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de snelstartgids.

## <a name="ssh-to-a-device-via-device-streams"></a>SSH naar een apparaat via apparaatstreams

In deze sectie maakt maken u een end-to-end-stream zo tunnel SSH-verkeer.

### <a name="run-the-device-local-proxy-application"></a>Voer de proxytoepassing in het apparaat uit

1. Bewerken van het bronbestand `iothub_client_c2d_streaming_proxy_sample.c` in de map `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/` en geef de apparaatverbindingsreeks, doelapparaat IP-/ hostnaam en de SSH-poort 22:

   ```C
   /* Paste in the your iothub connection string  */
   static const char* connectionString = "[Connection string of IoT Hub]";
   static const char* localHost = "[IP/Host of your target machine]"; // Address of the local server to connect to.
   static const size_t localPort = 22; // Port of the local server to connect to.
   ```

2. Het voorbeeld worden gecompileerd:

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

3. Voer het gecompileerde programma uit op het apparaat:

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

Zoals beschreven in de [hoe het werkt sectie](#how-it-works), tot stand brengen van een end-to-end-stream zo tunnel SSH-verkeer vereist een lokale proxyserver aan elk einde (zowel op de service en het apparaat). Tijdens de openbare preview ondersteunt C-SDK voor IoT Hub apparaat stromen alleen op het apparaat plaats. Als u wilt bouwen en uitvoeren van de service-local-proxy, volg de stappen die beschikbaar zijn voor het uitvoeren van de service-local-proxy in een van de volgende Quick starts:

   * [SSH/RDP via IoT Hub device-streams met behulp van C# toepassingsproxy-apps](./quickstart-device-streams-proxy-csharp.md)

   * [SSH/RDP via IoT Hub apparaat-stromen met behulp van Node.js toepassingsproxy-apps](./quickstart-device-streams-proxy-nodejs.md).

### <a name="establish-an-ssh-session"></a>Een SSH-sessie tot stand brengen

Wanneer zowel de apparaat - en service-local-proxy's worden uitgevoerd, gebruikt u uw SSH-client-programma en verbinding maken met de service-local-proxy op poort 2222 (in plaats van de SSH-daemon rechtstreeks).

```cmd/sh
ssh <username>@localhost -p 2222
```

Op dit moment worden weergegeven met de SSH-aanmeldingsprompt uw referenties in te voeren.

Console-uitvoer in de proxy in het apparaat die verbinding maakt met de SSH-daemon via `IP_address:22`: ![Apparaat-local-proxy-uitvoer](./media/quickstart-device-streams-proxy-c/device-console-output.png)

Console-uitvoer van het SSH-client-programma (SSH-client communiceert met SSH-daemon door verbinding te maken naar poort 22, waarmee de proxy-service-lokale luistert): ![SSH-client-uitvoer](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT Hub ingesteld, een apparaat geregistreerd, een apparaat en een proxyprogramma in een service geïmplementeerd om een apparaatstream via IoT Hub vast te stellen. U hebt eveneens de proxy's gebruikt om SSH-verkeer via een tunnel te versturen.

Gebruik de onderstaande koppelingen voor meer informatie over apparaatstreams:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
