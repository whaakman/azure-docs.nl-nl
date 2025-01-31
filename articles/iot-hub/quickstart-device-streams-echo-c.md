---
title: Communiceren met een apparaat-app in C via Azure IoT Hub apparaat-streams (preview) | Microsoft Docs
description: In deze snelstartgids hebt uitvoeren u een C apparaat aan clientzijde-toepassing die communiceert met een IoT-apparaat via een apparaat-stream.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 4b6f987c68f9fe3ef95c82017b7d8be1d83083ea
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446136"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Quickstart: communiceren met een apparaattoepassing in C via IoT Hub-apparaatstreams (preview)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub apparaat-streams als op dit moment ondersteunt een [preview-functie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[IoT Hub-apparaatstreams](iot-hub-device-streams-overview.md) zorgen ervoor dat service- en apparaattoepassingen kunnen communiceren op een beveiligde manier die de firewall toestaat. Tijdens de openbare preview ondersteunt de C-SDK apparaat stromen op het apparaat plaats. Als gevolg hiervan, deze quickstart bevat informatie over instructies voor het uitvoeren van alleen de apparaat-side '-toepassing. Als u wilt een bijbehorende servicezijde-toepassing uitvoert, Zie:
 
   * [Communiceren met apps op apparaten in C# via IoT Hub apparaat-streams](./quickstart-device-streams-echo-csharp.md)
   * [Communiceren met apps op apparaten in Node.js via IoT Hub apparaat-streams](./quickstart-device-streams-echo-nodejs.md)

De C-toepassing aan de apparaatzijde in deze quickstart heeft de volgende functionaliteit:

* Een apparaatstream naar een IoT-apparaat tot stand brengen.
* Ontvangt gegevens die worden verzonden vanuit de toepassing aan de clientzijde van de service en de echo weer.

De code ziet u het proces voor het initiëren van een apparaat-stream, evenals hoe u kunt gebruiken om te verzenden en ontvangen van gegevens.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* De Preview-versie van apparaat stromen is momenteel alleen ondersteund voor IoT-hubs die zijn gemaakt in de volgende regio's:

  * US - centraal
  * VS-midden EUAP

* Installeer [Visual Studio 2017](https://www.visualstudio.com/vs/) met de [bureaubladontwikkeling met C++ ](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) werkbelasting ingeschakeld.

* Installeer de meest recente versie van [Git](https://git-scm.com/download/).

* Voer de volgende opdracht om toe te voegen van de Azure IoT-extensie voor Azure CLI met de Cloud Shell-sessie. De IOT-extensie wordt toegevoegd voor IoT Hub, IoT Edge en IoT Device Provisioning Service (DPS)-specifieke opdrachten naar de Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Voor deze snelstart gebruikt u de [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). Voorbereiden van een ontwikkelomgeving die is gebruikt om te klonen en bouwen de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) vanuit GitHub. De SDK op GitHub bevat de voorbeeldcode die wordt gebruikt in deze Quick Start.

1. Download de [CMake-bouwsysteem](https://cmake.org/download/).

    Voordat u begint met de CMake-installatie, is het belangrijk dat de Visual Studio-vereisten (Visual Studio en de *bureaubladontwikkeling met C++*  werkbelasting) zijn geïnstalleerd op uw computer. Nadat de vereisten voldaan is en u hebt geverifieerd dat het downloaden, kunt u de CMake-build-systeem kunt installeren.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Deze bewerking duurt een paar minuten.

3. Maak een *cmake* submap in de hoofdmap van de Git-opslagplaats, zoals wordt weergegeven in de volgende opdracht uit en ga vervolgens naar die map.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Voer de volgende opdrachten uit de *cmake* directory voor het bouwen van een versie van de SDK die specifiek is voor uw clientplatform voor ontwikkeling.

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

Voordat u deze verbinding kan maken, moet u een apparaat registreren bij uw IoT-hub. In deze sectie gebruikt u Azure Cloud Shell met de [IoT-extensie](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdracht in Cloud Shell voor het maken van de apparaat-id:

   > [!NOTE]
   > * Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.
   > * Gebruik *Mijnapparaat*, zoals wordt weergegeven. Het is de naam van het geregistreerde apparaat. Als u een andere naam voor uw apparaat, gebruikt u die naam in dit artikel en de naam van het apparaat in de voorbeeldtoepassingen bijwerken voordat u ze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Aan de *apparaatverbindingsreeks* voor het apparaat dat u zojuist hebt geregistreerd, kunt u de volgende opdrachten uitvoeren in Cloud Shell:

   > [!NOTE]
   > Vervang de *YourIoTHubName* tijdelijke aanduiding door de naam die u voor uw IoT-hub kiest.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Houd er rekening mee de apparaatverbindingsreeks voor later gebruik in deze Quick Start. Het lijkt op het volgende voorbeeld:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Communiceren tussen het apparaat en de service via apparaat stromen

In deze sectie maakt u de apparaat-side '-toepassing en de toepassing aan de serverkant uitvoeren en communicatie tussen de twee.

### <a name="run-the-device-side-application"></a>De toepassing aan de apparaatzijde uitvoeren

Als u wilt de apparaat-side '-toepassing uitvoert, het volgende doen:

1. Geef de referenties van uw apparaat door het bewerken van de *iothub_client_c2d_streaming_sample.c* bronbestand in de *iothub_client/samples/iothub_client_c2d_streaming_sample* map en klikt u vervolgens bieden de verbindingsreeks van uw apparaat.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Compileer de code als volgt:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Voer het gecompileerde programma uit:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>De toepassing aan de servicezijde uitvoeren

Zoals eerder vermeld, ondersteunt de C-SDK voor IoT Hub apparaat stromen op het apparaat plaats. Als u wilt bouwen en uitvoeren van de toepassing aan de serverkant, volg de instructies in een van de volgende Quick starts:

* [Communiceren met een apparaat-app in C# via IoT Hub apparaat-streams](./quickstart-device-streams-echo-csharp.md)
* [Communiceren met een apparaat-app in Node.js via IoT Hub apparaat-streams](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u instellen van een IoT-hub, een apparaat hebt geregistreerd, een apparaat-stroom tussen een toepassing C op het apparaat en een andere toepassing aan de service tot stand gebracht en de stroom gebruikt voor het verzenden van gegevens heen en weer tussen de toepassingen.

Zie voor meer informatie over het apparaat stromen:

> [!div class="nextstepaction"]
> [Overzicht van apparaatstreams](./iot-hub-device-streams-overview.md)
