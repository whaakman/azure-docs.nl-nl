---
title: Azure IoT-Edge van Windows simuleren | Microsoft Docs
description: De runtime Azure IoT Edge op een gesimuleerd apparaat in Windows installeren en implementeren van uw eerste module
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/16/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0207418cf71902ce9bc9d2911124d1d46889d893
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2017
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Azure IoT rand implementeren op een gesimuleerd apparaat in de Windows - voorbeeld

Azure IoT-rand kunt u voor het uitvoeren van analyses en het verwerken van gegevens op uw apparaten, in plaats van alle gegevens push naar de cloud. De rand van de IoT-zelfstudies laten zien hoe u verschillende soorten modules die zijn gebouwd op basis van de Azure-services of aangepaste code implementeren, maar u moet eerst een apparaat om te testen. 

In deze zelfstudie leert u het volgende:

1. Een IoT Hub maken
2. Een IoT-Edge-apparaat registreren
3. Starten van de rand van de IoT-runtime
4. Een module implementeren

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

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT-rand kunt uitvoeren containers Windows of Linux-containers. Als u werkt met een van de volgende versies van Windows, kunt u Windows-containers:
>    * Windows 10 vallen auteurs bijwerken
>    * WindowsServer 1709 (Build 16299)
>    * Windows IoT Core (16299 bouwen) op een apparaat x64 64
>
> Volg de instructies in voor Windows IoT Core [IoT rand runtime installeren op Windows IoT Core][lnk-install-iotcore]. Anders gewoon [Docker voor het gebruik van Windows-containers configureren][lnk-docker-containers]. Gebruik de volgende opdracht voor het valideren van uw vereisten:
>    ```powershell
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

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. De **IoT rand agent** vereenvoudigt de implementatie en controle van modules die zich in de rand van de IoT-apparaat. De **rand van de IoT hub** beheert de communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. Wanneer u de runtime op het nieuwe apparaat configureert, worden alleen de rand van de IoT-agent wordt gestart op het eerste. Wanneer u een module implementeert, wordt de rand van de IoT-hub later geleverd. 


Configureer de runtime door uw verbindingsreeks rand van de IoT-apparaat uit de vorige sectie.

```cmd
iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Start de runtime.

```cmd
iotedgectl start
```

Controleer de Docker om te zien of de rand van de IoT-agent wordt uitgevoerd als een module.

```cmd
docker ps
```

![Zie edgeAgent in Docker](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Een module implementeren

Uw Azure-IoT-randapparaat beheren vanuit de cloud voor het implementeren van een module van telemetriegegevens naar IoT Hub verzendt.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze zelfstudie hebt gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

Open de opdrachtprompt op de computer die uw gesimuleerde apparaat nogmaals uit te voeren. Bevestig dat de module op basis van de cloud geïmplementeerd op uw IoT-randapparaat wordt uitgevoerd. 

```cmd
docker ps
```

![Drie modules op uw apparaat weergeven](./media/tutorial-simulate-device-windows/docker-ps2.png)

Bekijk de berichten worden verzonden vanaf de module tempSensor naar de cloud. 

```cmd
docker logs -f tempSensor
```

![De gegevens van uw module weergeven](./media/tutorial-simulate-device-windows/docker-logs.png)

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