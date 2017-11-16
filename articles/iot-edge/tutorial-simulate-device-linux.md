---
title: Azure IoT-Edge van Linux simuleren | Microsoft Docs
description: De runtime Azure IoT Edge op een gesimuleerd apparaat in Linux installeren en implementeren van uw eerste module
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 041919fd729880d429e08d8942f8d1ee087ccf61
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux---preview"></a>Azure IoT rand implementeren op een gesimuleerd apparaat in Linux - voorbeeld

Azure IoT-rand kunt u voor het uitvoeren van analyses en het verwerken van gegevens op uw apparaten, in plaats van alle gegevens push naar de cloud. De rand van de IoT-zelfstudies laten zien hoe u verschillende soorten modules die zijn gebouwd op basis van de Azure-services of aangepaste code implementeren, maar u moet eerst een apparaat om te testen. 

Deze zelfstudie wordt u stapsgewijs hoe u een gesimuleerde IoT randapparaat, zal het implementeren van een module die sensorgegevens genereert. Procedures voor:

![Zelfstudie-architectuur][2]

Het gesimuleerde apparaat die u in deze zelfstudie maakt is een monitor die temperatuur en vochtigheid druk gegevens genereert. De andere zijde van Azure IoT-zelfstudies bouwen voort op het werk dat u hier doen door het implementeren van modules die de gegevens voor zakelijke inzichten analyseren. 

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt ervan uitgegaan dat u een computer of virtuele machine waarop Linux wordt uitgevoerd om te simuleren dat een Internet of Things-apparaat. De volgende services zijn vereist voor een IoT-randapparaat met succes implementeren:

- [Docker voor Linux installeren] [ lnk-docker-ubuntu] en zorg ervoor dat deze wordt uitgevoerd. 
- De meeste Linux-distributies, Ubuntu, waaronder nog Python 2.7 geïnstalleerd. Gebruik de volgende opdracht om ervoor te zorgen dat pip is geïnstalleerd: `sudo apt-get install python-pip`.

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

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. Eerst vergemakkelijkt de agent IoT rand implementatie en controle van modules die zich in de IoT-randapparaat. Ten tweede beheert de rand van de IoT-hub communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. 

Gebruik de volgende stappen uit om te installeren en starten van de rand van de IoT-runtime:

1. Download het script IoT Edge-besturingselement op de machine waarop u het apparaat aan de rand van de IoT voert.

   ```
   sudo pip install -U azure-iot-edge-runtime-ctl
   ```

1. Configureer de runtime door uw verbindingsreeks rand van de IoT-apparaat uit de vorige sectie.

   ```
   sudo iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Start de runtime.

   ```
   sudo iotedgectl start
   ```

1. Controleer de Docker om te zien of de rand van de IoT-agent wordt uitgevoerd als een module.

   ```
   sudo docker ps
   ```

## <a name="deploy-a-module"></a>Een module implementeren

Uw Azure-IoT-randapparaat beheren vanuit de cloud voor het implementeren van een module van telemetriegegevens naar IoT Hub verzendt.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze snelstartgids gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

De berichten worden verzonden vanaf de module tempSensor weergeven:

```cmd/sh
docker logs -f tempSensor
```

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
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
