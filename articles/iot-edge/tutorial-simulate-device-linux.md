---
title: Azure IoT-Edge van Linux simuleren | Microsoft Docs
description: De runtime Azure IoT Edge op een gesimuleerd apparaat in Linux installeren en implementeren van uw eerste module
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 55770c92f5d5959e83066b425bc6ccf2b9dcc62e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux-or-macos---preview"></a>Azure IoT rand implementeren op een gesimuleerd apparaat in Linux- of Mac OS - voorbeeld

Azure IoT-rand kunt u voor het uitvoeren van analyses en het verwerken van gegevens op uw apparaten, in plaats van alle gegevens push naar de cloud. De rand van de IoT-zelfstudies laten zien hoe u verschillende soorten modules die zijn gebouwd op basis van de Azure-services of aangepaste code implementeren, maar u moet eerst een apparaat om te testen. 

In deze zelfstudie leert u het volgende:

1. Een IoT Hub maken
2. Een IoT-Edge-apparaat registreren
3. Starten van de rand van de IoT-runtime
4. Een module implementeren

![Zelfstudie-architectuur][2]

Het gesimuleerde apparaat die u in deze zelfstudie maakt is een monitor die temperatuur en vochtigheid druk gegevens genereert. De andere zijde van Azure IoT-zelfstudies bouwen voort op het werk dat u hier doen door het implementeren van modules die de gegevens voor zakelijke inzichten analyseren. 

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie maakt gebruik van uw computer of virtuele machine, zoals een Internet of Things-apparaat. Als u wilt uw machine in een Edge van de IoT-apparaat, zijn de volgende services zijn vereist:

* Python pip voor het installeren van de rand van de IoT-runtime.
   * Linux: `sudo apt-get install python-pip`.
   * Mac OS: `sudo easy_install pip`.
* Docker om uit te voeren van de rand IoT modules
   * [Docker voor Linux installeren] [ lnk-docker-ubuntu] en zorg ervoor dat deze wordt uitgevoerd. 
   * [Docker voor Mac installeert] [ lnk-docker-mac] en zorg ervoor dat deze wordt uitgevoerd. 

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

De zelfstudie begint met het maken van uw IoT-Hub.
![IoT Hub maken][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Een IoT-Edge-apparaat registreren

Een IoT-Edge-apparaat registreren bij uw nieuwe IoT-Hub.
![Een apparaat registreren][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Installeren en starten van de rand van de IoT-runtime

Installeren en starten van de rand van Azure IoT-runtime op uw apparaat. 
![Een apparaat registreren][5]

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. De **IoT rand agent** vereenvoudigt de implementatie en controle van modules die zich in de rand van de IoT-apparaat. De **rand van de IoT hub** beheert de communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. Wanneer u de runtime op het nieuwe apparaat configureert, worden alleen de rand van de IoT-agent wordt gestart op het eerste. Wanneer u een module implementeert, wordt de rand van de IoT-hub later geleverd. 

Download het script IoT Edge-besturingselement op de machine waarop u het apparaat aan de rand van de IoT voert:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Configureer de runtime door uw verbindingsreeks rand van de IoT-apparaat uit het vorige gedeelte:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Start de runtime:
```cmd
sudo iotedgectl start
```

Controleer de Docker om te zien of de rand van de IoT-agent wordt uitgevoerd als een module:
```cmd
sudo docker ps
```

![Zie edgeAgent in Docker](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Een module implementeren

Uw Azure-IoT-randapparaat beheren vanuit de cloud voor het implementeren van een module van telemetriegegevens naar IoT Hub verzendt.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze zelfstudie hebt gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

Open de opdrachtprompt op de computer die uw gesimuleerde apparaat nogmaals uit te voeren. Bevestig dat de module op basis van de cloud geïmplementeerd op uw IoT-randapparaat wordt uitgevoerd:

```cmd
sudo docker ps
```

![Drie modules op uw apparaat weergeven](./media/tutorial-simulate-device-linux/docker-ps2.png)

De berichten die naar de cloud worden verzonden vanaf de module tempSensor weergeven:

```cmd
sudo docker logs -f tempSensor
```

![De gegevens van uw module weergeven](./media/tutorial-simulate-device-linux/docker-logs.png)

U kunt ook weergeven met de telemetrie verzenden van het apparaat met behulp van de [IoT Hub explorer hulpprogramma][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een nieuwe IoT Edge-apparaat en de Azure IoT rand cloud-interface gebruikt voor het implementeren van code op het apparaat. U hebt nu een gesimuleerd apparaat genereren van onbewerkte gegevens over de omgeving. 

Deze zelfstudie is de vereiste voor alle van de andere zijde van de IoT-zelfstudies. U kunt door te gaan naar een van de andere zelfstudies voor meer informatie over hoe Azure IoT rand kunt u deze gegevens tot zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Ontwikkelen en implementeren van C#-code als een module](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
