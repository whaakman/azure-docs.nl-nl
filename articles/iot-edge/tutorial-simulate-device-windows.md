---
title: Azure IoT-Edge van Windows simuleren | Microsoft Docs
description: De runtime Azure IoT Edge op een gesimuleerd apparaat in Windows installeren en implementeren van uw eerste module
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 08c501b9132bb21f47f099725d1fad5556befb4c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Azure IoT rand implementeren op een gesimuleerd apparaat in de Windows - voorbeeld

Azure IoT-rand wordt verplaatst van de kracht van de cloud op uw Internet der dingen (IoT)-apparaten. Deze zelfstudie wordt u stapsgewijs hoe u een gesimuleerde IoT randapparaat waarmee sensorgegevens wordt gegenereerd. Procedures voor:

![Zelfstudie-architectuur][2]

Het gesimuleerde apparaat die u in deze zelfstudie maakt is een monitor op een o turbine die temperatuur en vochtigheid druk gegevens genereert. U bent geïnteresseerd in deze gegevens omdat uw turbines worden uitgevoerd op verschillende niveaus van efficiëntie, afhankelijk van de voorwaarden weer. De andere zijde van Azure IoT-zelfstudies bouwen voort op het werk dat u hier doen door het implementeren van modules die de gegevens voor zakelijke inzichten analyseren. 

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt ervan uitgegaan dat u een computer of virtuele machine met Windows om te simuleren dat een Internet of Things-apparaat. 

>[!TIP]
>Als u Windows in een virtuele machine uitvoert, schakelt u [geneste virtualisatie] [ lnk-nested] en ten minste 2 GB geheugen toewijzen. 

1. Zorg ervoor dat u een ondersteunde Windowsversie:
   * Windows 10 
   * Windows Server
2. Installeer [Docker voor Windows] [ lnk-docker] en zorg ervoor dat deze wordt uitgevoerd.
3. Installeer [Python 2.7 op Windows] [ lnk-python] en zorg ervoor dat u de opdracht pip kunt gebruiken.
4. Voer de volgende opdracht voor het downloaden van het script IoT Edge-besturingselement.

   ```
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT-rand kunt uitvoeren containers Windows of Linux-containers. U moet uitvoeren voor het gebruik van Windows-containers:
>    * Windows 10 vallen auteurs bijwerken, of
>    * WindowsServer 1709 (Build 16299), of
>    * Windows IoT Core (16299 bouwen) op een apparaat x64 64
>
> Volg de instructies in voor Windows IoT Core [IoT rand runtime installeren op Windows IoT Core][lnk-install-iotcore]. Anders gewoon [Docker voor het gebruik van Windows-containers configureren][lnk-docker-containers], en desgewenst valideren uw vereisten met de volgende powershell-opdracht:
>    ```
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>Een IoT Hub maken

De zelfstudie begint met het maken van uw IoT-Hub.
![IoT Hub maken][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Een IoT-Edge-apparaat registreren

Een IoT-Edge-apparaat registreren bij uw nieuwe IoT-Hub.
![Een apparaat registreren][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Configureren van de rand van de IoT-runtime

Installeren en starten van de rand van Azure IoT-runtime op uw apparaat. 
![Een apparaat registreren][5]

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. Eerst vergemakkelijkt de agent IoT rand implementatie en controle van modules die zich in de IoT-randapparaat. Ten tweede beheert de rand van de IoT-hub communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. 


Gebruik de volgende stappen uit om te installeren en starten van de rand van de IoT-runtime:

1. Configureer de runtime door uw verbindingsreeks rand van de IoT-apparaat uit de vorige sectie.

   ```
   iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
   ```

1. Start de runtime.

   ```
   iotedgectl start
   ```

1. Controleer de Docker om te zien of de rand van de IoT-agent wordt uitgevoerd als een module.

   ```
   docker ps
   ```

## <a name="deploy-a-module"></a>Een module implementeren

Uw Azure-IoT-randapparaat beheren vanuit de cloud voor het implementeren van een module van telemetriegegevens naar IoT Hub verzendt.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze snelstartgids gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

De berichten worden verzonden vanaf de module tempSensor weergeven:

```cmd/sh
sudo docker logs -f tempSensor
```

U kunt ook weergeven met de telemetrie verzenden van het apparaat met behulp van de [IoT Hub explorer hulpprogramma][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een nieuwe IoT Edge-apparaat en de Azure IoT rand cloud-interface gebruikt voor het implementeren van code op het apparaat. U hebt nu een gesimuleerd apparaat genereren van onbewerkte gegevens over de omgeving. 

Deze zelfstudie is de vereiste voor alle van de andere zijde van de IoT-zelfstudies. U kunt door te gaan naar een van de andere zelfstudies voor meer informatie over hoe Azure IoT rand kunt u deze gegevens tot zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Ontwikkelen en implementeren van C#-code als een module](tutorial-csharp-module.md)

<!-- Images -->
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-python]: https://www.python.org/downloads/
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-install-iotcore]: how-to-install-iot-core.md