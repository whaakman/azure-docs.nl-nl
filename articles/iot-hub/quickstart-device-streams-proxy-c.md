---
title: Azure IoT Hub-apparaatstreams met C#, quickstart voor SSH/RDP (preview) | Microsoft Docs
description: In deze quickstart voert u een voorbeeld van een C#-toepassing uit die als een proxy fungeert om scenario's met SSH/RDP via IoT Hub-apparaatstreams mogelijk te maken.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 59a84190386b554716472b4cb46c94030a66a4cb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58077102"
---
# <a name="quickstart-sshrdp-over-iot-hub-device-streams-using-c-proxy-application-preview"></a>Quickstart: SSH/RDP via IoT Hub-apparaatstreams met behulp van C#-proxytoepassingen (preview)

[!INCLUDE [iot-hub-quickstarts-4-selector](../../includes/iot-hub-quickstarts-4-selector.md)]

Microsoft Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](./iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Bekijk [deze pagina](./iot-hub-device-streams-overview.md#local-proxy-sample-for-ssh-or-rdp) voor een overzicht van de instellingen.

In dit document wordt beschreven hoe u tunneling van SSH-verkeer instelt (met behulp van poort 22) via apparaatstreams. De configuratie voor RDP-verkeer is vergelijkbaar en vereist een eenvoudige wijziging in de configuratie. Omdat apparaatstreams toepassings- en protocolneutraal zijn, kan de bestaande quickstart zo worden bewerkt (door de communicatiepoorten te wijzigen) dat er andere soorten toepassingsverkeer mogelijk zijn.

## <a name="how-it-works"></a>Hoe werkt het?
De onderstaande afbeelding ziet u de installatie van hoe de apparaat - en service-lokale proxy's end-to-end-connectiviteit tussen de SSH-client en de SSH-daemon-processen kunnen. Gedurende de openbare preview biedt de C SDK alleen ondersteuning voor apparaatstreams aan de apparaatzijde. Daarom bevat deze quickstart alleen instructies voor het uitvoeren van de proxytoepassing in het apparaat. Voer een bijbehorende proxytoepassing in de service uit. Deze is beschikbaar in de handleidingen [Quickstart voor C#](./quickstart-device-streams-proxy-csharp.md) of [Quickstart voor Node.js](./quickstart-device-streams-proxy-nodejs.md).

![Alternatieve tekst](./media/quickstart-device-streams-proxy-csharp/device-stream-proxy-diagram.svg "Lokale proxy-instellingen")

1. De proxy in de service maakt verbinding met IoT Hub en begint een apparaatstream naar het doelapparaat.

2. De proxy in het apparaat voltooit de stream-handshake en zet een end-to-end-streaming-tunnel op van het streaming-eindpunt van IoT Hub naar de servicezijde.

3. De proxy in het apparaat maakt verbinding met de SSH-daemon (SSHD) die naar poort 22 van het apparaat luistert (dit kan worden geconfigureerd, zoals [hieronder](#run-the device-local-proxy-application) is beschreven).

4. De proxy in de service wacht op nieuwe SSH-verbindingen van de gebruiker door te luisteren naar een opgegeven poort, in dit geval poort 2222 (dit kan eveneens worden geconfigureerd, zoals [hieronder](#run-the-device-local-proxy-application) is beschreven). Wanneer een gebruiker verbinding maakt via een SSH-client, zorgt de tunnel ervoor dat SSH-toepassingsverkeer kan worden overgebracht tussen de SSH-client en serverprogramma's.

> [!NOTE]
> SSH-verkeer dat via een apparaatstream wordt verstuurd, gaat via een tunnel van het streaming-eindpunt van IoT Hub in plaats van dat het rechtstreeks tussen de service en het apparaat wordt verzonden. Dit heeft [deze voordelen](./iot-hub-device-streams-overview.md#benefits). In de afbeelding ziet u bovendien hoe de SSH-daemon wordt uitgevoerd op hetzelfde apparaat (of machine) als de proxy in het apparaat. In deze quickstart zorgt de verstrekking van het IP-adres van de SSH-daemon ervoor dat de proxy in het apparaat en daemon ook op verschillende computers worden uitgevoerd.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-Hubs die zijn gemaakt in de volgende regio's:

  * **US - centraal**
  * **VS-midden EUAP**

* [Visual Studio 2017](https://www.visualstudio.com/vs/) installeren met de workload ['Desktopontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) ingeschakeld.
* Installeer de meest recente versie van [Git](https://git-scm.com/download/).

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Voor deze snelstart gebruikt u de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). U bereidt een ontwikkelomgeving voor die wordt gebruikt om de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) van GitHub te klonen en te bouwen. De SDK op GitHub bevat de voorbeeldcode gebruikt in deze snelstartgids. 

1. Download versie 3.13.4 van de [CMake-bouwsysteem](https://cmake.org/download/). Controleer het gedownloade binaire bestand met behulp van de bijbehorende cryptografische hash-waarde. Het volgende voorbeeld Windows PowerShell gebruikt om te controleren of de cryptografische hash voor 3.13.4-versie van de x64 MSI-distributiepunt:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.13.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "64AC7DD5411B48C2717E15738B83EA0D4347CD51B940487DFF7F99A870656C09"
    True
    ```

    De volgende hash-waarden voor versie 3.13.4 werden vermeld op de CMake-site ten tijde van dit schrijven:

    ```
    563a39e0a7c7368f81bfa1c3aff8b590a0617cdfe51177ddc808f66cc0866c76  cmake-3.13.4-Linux-x86_64.tar.gz
    7c37235ece6ce85aab2ce169106e0e729504ad64707d56e4dbfc982cb4263847  cmake-3.13.4-win32-x86.msi
    64ac7dd5411b48c2717e15738b83ea0d4347cd51b940487dff7f99a870656c09  cmake-3.13.4-win64-x64.msi
    ```

    Het is belangrijk dat de Visual Studio-vereisten (Visual Studio en de workload 'Desktop development with C++') zijn geïnstalleerd op uw computer **voordat** vanaf de `CMake` installatie. Zodra de vereisten voldaan is en de download is geverifieerd, installeert u de CMake-build-systeem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:
    
    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```
    De grootte van deze opslagplaats is momenteel ongeveer 220 MB. Deze bewerking kan enkele minuten in beslag nemen.

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

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdrachten uit in Azure Cloud Shell om de IoT Hub CLI-extensie toe te voegen en de apparaat-id te maken. 

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyDevice**: dit is de naam van het geregistreerde apparaat. Gebruik MyDevice, zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u ze uitvoert.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
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

## <a name="ssh-to-a-device-via-device-streams"></a>SSH naar een apparaat via apparaatstreams

### <a name="run-the-device-local-proxy-application"></a>Voer de proxytoepassing in het apparaat uit

1. Bewerken van het bronbestand `iothub_client/samples/iothub_client_c2d_streaming_proxy_sample/iothub_client_c2d_streaming_proxy_sample.c` en geef de apparaatverbindingsreeks, doelapparaat IP-/ hostnaam en de SSH-poort 22:

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

Zoals besproken [eerder](#how-it-works), tot stand brengen van een end-to-end-stream zo tunnel SSH-verkeer vereist een lokale proxyserver aan elk einde (zowel op de service en het apparaat). Tijdens de openbare preview ondersteunt C-SDK voor IoT Hub apparaat stromen alleen op het apparaat plaats. Als u wilt bouwen en uitvoeren van de service-local-proxy, volgt u de stappen die beschikbaar zijn in de [ C# snelstartgids](./quickstart-device-streams-proxy-csharp.md) of de [Node.js Quick Start](./quickstart-device-streams-proxy-nodejs.md).

### <a name="establish-an-ssh-session"></a>Een SSH-sessie tot stand brengen

Wanneer zowel de apparaat - en service-local-proxy's worden uitgevoerd, gebruikt u uw SSH-client-programma en verbinding maken met de service-local-proxy op poort 2222 (in plaats van de SSH-daemon rechtstreeks).

```cmd/sh
ssh <username>@localhost -p 2222
```

U wordt nu gevraagd om uw referenties in te voeren in het SSH-aanmeldingsprompt.

Console-uitvoer in de proxy in het apparaat die verbinding maakt met de SSH-daemon via `IP_address:22`: ![Alternatieve tekst](./media/quickstart-device-streams-proxy-c/device-console-output.PNG "Proxy-uitvoer in het apparaat")

Console-uitvoer van het SSH-client-programma (SSH-client communiceert met SSH-daemon door verbinding te maken naar poort 22, waarmee de proxy-service-lokale luistert): ![Alternatieve tekst](./media/quickstart-device-streams-proxy-csharp/ssh-console-output.png "uitvoer van SSH-client")

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT Hub ingesteld, een apparaat geregistreerd, een apparaat en een proxyprogramma in een service geïmplementeerd om een apparaatstream via IoT Hub vast te stellen. U hebt eveneens de proxy's gebruikt om SSH-verkeer via een tunnel te versturen.

Gebruik de onderstaande koppelingen voor meer informatie over apparaatstreams:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
