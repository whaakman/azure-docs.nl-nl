---
title: Azure IoT Edge simuleren op Windows | Microsoft Docs
description: De Azure IoT Edge-runtime installeren op een gesimuleerd apparaat in Windows en de eerste module implementeren
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 11/16/2017
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 7ad99a49a578de4997a2d76d48da33aba6847f3c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631187"
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-windows----preview"></a>Azure IoT Edge implementeren op een gesimuleerd apparaat in Windows - preview

Met Azure IoT Edge kunt u analyses en gegevensverwerking uitvoeren op uw apparaten, in plaats van dat u alle gegevens moet doorsturen naar de cloud. In de IoT Edge-zelfstudies kunt u zien hoe u verschillende typen modules implementeert die zijn gemaakt op basis van Azure-services of aangepaste code, maar eerst hebt u een apparaat nodig dat u kunt testen. 

In deze zelfstudie leert u het volgende:

1. Een IoT Hub maken
2. Een IoT Edge-apparaat registreren
3. De IoT Edge-runtime starten
4. Een module implementeren

![Zelfstudiearchitectuur][2]

Het gesimuleerde apparaat dat u in deze zelfstudie maakt, is een monitor op een windturbine waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. U bent geïnteresseerd in deze gegevens omdat uw turbines verschillende efficiëntieniveaus hebben, afhankelijk van de weersomstandigheden. De andere Azure IoT Edge-zelfstudies zijn een uitbreiding op het werk dat u hier doet. Hierin worden modules geïmplementeerd waarmee de gegevens worden geanalyseerd voor zakelijke inzichten. 

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie wordt ervan uitgegaan dat u een computer of virtuele machine met Windows gebruikt om een Internet of Things-apparaat te simuleren. 

>[!TIP]
>Als u Windows uitvoert op een virtuele machine, schakelt u [geneste virtualisatie][lnk-nested] in en moet u ten minste 2 GB geheugen toewijzen. 

1. Zorg ervoor dat u een ondersteunde versie van Windows gebruikt:
   * Windows 10 
   * Windows Server
2. Installeer [Docker voor Windows][lnk-docker] en contoleer of deze wordt uitgevoerd.
3. Installeer [Python op Windows][lnk-python] en zorg ervoor dat u de opdracht pip kunt gebruiken. Deze zelfstudie is getest met Python-versies >=2.7.9 en >=3.5.4.  
4. Voer de volgende opdracht uit om script voor de besturing van IoT Edge te downloaden.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Met Azure IoT Edge kunnen zowel Windows- als Linux-containers worden uitgevoerd. Als u werkt met een van de volgende Windows-versies, kunt u Windows-containers gebruiken:
>    * Windows 10 Fall Creators Update
>    * Windows Server 1709 (Build 16299)
>    * Windows IoT Core (Build 16299) op een x64-apparaat
>
> Voor Windows IoT Core volgt u de instructies in [IoT Edge-runtime installeren op Windows IoT Core][lnk-install-iotcore]. Anders [configureert u Docker voor het gebruik van Windows-containers][lnk-docker-containers]. Gebruik de volgende opdracht om de vereisten te valideren:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```


## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin aan de zelfstudie door uw IoT Hub te maken.
![IoT Hub maken][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Een apparaat registreren][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>De IoT Edge-runtime configureren

Installeer en start de Azure IoT Edge-runtime op uw apparaat. 
![Een apparaat registreren][5]

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze bevat twee modules. Met de **IoT Edge-agent** worden de implementatie en bewaking van modules op het IoT Edge-apparaat mogelijk gemaakt. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd. Wanneer u de runtime op het nieuwe apparaat configureert, wordt in het begin alleen de IoT Edge-agent gestart. De IoT Edge-hub komt later, wanneer u een module implementeert. 


Configureer de runtime met behulp van de verbindingsreeks van het IoT Edge-apparaat uit de vorige sectie.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Start de runtime.

```cmd
iotedgectl start
```

Controleer Docker om te zien of de IoT Edge-agent wordt uitgevoerd als een module.

```cmd
docker ps
```

![Zie edgeAgent in Docker](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Een module implementeren

Beheer uw Azure IoT Edge-apparaat vanuit de cloud om een module te implementeren waarmee telemetriegegevens worden verzonden naar IoT Hub.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

In deze zelfstudie hebt u een nieuw IoT Edge-apparaat gemaakt en de IoT Edge-runtime erop geïnstalleerd. Vervolgens hebt u Azure Portal gebruikt om een IoT Edge-module te pushen om deze uit te voeren op het apparaat zonder dat er wijzigingen in het apparaat zelf hoeven te worden aangebracht. In dit geval worden met de module die u hebt gepusht, omgevingsgegevens gemaakt die u voor de zelfstudies kunt gebruiken. 

Open nogmaals de opdrachtprompt op de computer waarop het gesimuleerde apparaat wordt uitgevoerd. Bevestig dat de module die vanuit de cloud is geïmplementeerd, op uw IoT Edge -apparaat wordt uitgevoerd. 

```cmd
docker ps
```

![Drie modules op uw apparaat bekijken](./media/tutorial-simulate-device-windows/docker-ps2.png)

Bekijk de berichten die vanaf de module tempSensor naar de cloud worden verzonden. 

```cmd
docker logs -f tempSensor
```

![De gegevens van uw module bekijken](./media/tutorial-simulate-device-windows/docker-logs.png)

U kunt ook de [IoT Hub-verkenner][lnk-iothub-explorer] gebruiken om de telemetrie te bekijken die het apparaat verzendt. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een nieuw IoT Edge-apparaat gemaakt en de Azure IoT Edge-cloudinterface gebruikt om code te implementeren op het apparaat. U hebt nu een gesimuleerd apparaat waarmee onbewerkte gegevens over de omgeving worden gegenereerd. 

Deze zelfstudie is vereist voor alle andere IoT Edge-zelfstudies. U kunt doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [C#-code ontwikkelen en implementeren als een module](tutorial-csharp-module.md)

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