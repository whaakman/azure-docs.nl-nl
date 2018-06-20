---
title: 'Snelstart: Azure IoT Edge + Windows | Microsoft Docs'
description: Azure IoT Edge uitproberen door analyses uit te voeren op een gesimuleerd Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/03/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 2fd16ab4ade61b1a08f93294051f4246e47839b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631731"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Snelstart: Uw eerste IoT Edge-module van Azure Portal naar een Windows-apparaat implementeren - preview

In deze snelstart gebruikt u de cloudinterface van Azure IoT Edge om vooraf geschreven code op afstand naar een IoT Edge-apparaat te implementeren. Om dit te bereiken, moet u eerst uw Windows-apparaat gebruiken om een IoT Edge-apparaat te simuleren, waarna u er een module naar kunt implementeren.

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account][lnk-account] aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie wordt ervan uitgegaan dat u een computer of virtuele machine met Windows gebruikt om een Internet of Things-apparaat te simuleren. Als u Windows in een virtuele machine uitvoert, schakelt u [geneste virtualisatie][lnk-nested] in en moet u ten minste 2 GB geheugen toewijzen. 

1. Zorg ervoor dat u een ondersteunde versie van Windows gebruikt:
   * Windows 10 
   * Windows Server
2. Installeer [Docker voor Windows] [ lnk-docker] en contoleer of deze wordt uitgevoerd.
3. Installeer [Python op Windows] [ lnk-python] en zorg ervoor dat u de opdracht pip kunt gebruiken. Deze snelstart is getest met Python-versies >=2.7.9 en >=3.5.4.  
4. Voer de volgende opdracht uit om script voor besturing van IoT Edge te downloaden.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> Azure IoT Edge kan zowel Windows-containers als Linux-containers uitvoeren. Voor Windows-containers is het volgende vereist:
>    * Windows 10 Fall Creators Update of
>    * Windows Server 1709 (Build 16299) of
>    * Windows IoT Core (Build 16299) op een x64-apparaat
>
> Voor Windows IoT Core volgt u de instructies in [IoT Edge-runtime installeren op Windows IoT Core][lnk-install-iotcore]. Anders kunt u gewoon [Docker voor het gebruik van Windows-containers configureren][lnk-docker-containers] en desgewenst uw vereisten valideren met de volgende Powershell-opdracht:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```

## <a name="create-an-iot-hub-with-azure-cli"></a>Een IoT Hub maken met de Azure-CLI

Maak een IoT-hub in uw Azure-abonnement. Het gratis niveau van IoT Hub werkt voor deze snelstart. Als u IoT-Hub in het verleden al hebt gebruikt en u al een gratis hub hebt gemaakt, kunt u deze sectie overslaan en doorgaan met [Een IoT Edge-apparaat registreren][anchor-register]. Elk abonnement biedt toegang tot slechts één gratis IoT-hub. 

1. Meld u aan bij [Azure Portal][lnk-portal]. 
1. Selecteer de knop **Cloud Shell**. 

   ![Knop Cloud Shell][1]

1. Maak een resourcegroep. In het volgende voorbeeld wordt een resourcegroep met de naam **IoTEdge** gemaakt in de **regio VS - west**:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Maak een IoT-hub in uw nieuwe resourcegroep. Met de volgende code wordt een gratis **F1** hub met de naam **MyIotHub** gemaakt in de resourcegroep **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren.

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>De IoT Edge-runtime configureren.

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze omvat twee modules. Op de eerste plaats faciliteert de IoT Edge-agent de implementatie en bewaking van modules op het IoT Edge-apparaat. Ten tweede beheert de IoT Edge-hub de communicatie tussen modules op het IoT Edge-apparaat en die tussen het apparaat en IoT Hub. 

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

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de IoT Edge-runtime erop geïnstalleerd. Vervolgens hebt u Azure Portal gebruikt om een IoT Edge-module te pushen om te worden uitgevoerd op het apparaat zonder dat er wijzigingen in het apparaat zelf aangebracht hoefden te worden. In dit geval worden met de module die u hebt gepusht, omgevingsgegevens gemaakt die u voor de zelfstudies kunt gebruiken. 

Open nogmaals de opdrachtprompt op de computer waarop het gesimuleerde apparaat wordt uitgevoerd. Bevestig dat de module die vanuit de cloud is geïmplementeerd op uw IoT Edge -apparaat wordt uitgevoerd. 

```cmd
docker ps
```

![Drie modules op uw apparaat bekijken](./media/tutorial-simulate-device-windows/docker-ps2.png)

Bekijk de berichten die vanaf de module tempSensor naar de cloud worden verzonden. 

```cmd
docker logs -f tempSensor
```

![De gegevens van uw module bekijken](./media/tutorial-simulate-device-windows/docker-logs.png)

Met behulp van de [IoT Hub-verkenner][lnk-iothub-explorer] kunt u ook de telemetrie bekijken die het apparaat verzendt. 
## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de volgende opdracht als u het gesimuleerde apparaat dat u hebt gemaakt, evenals de Docker-containers die voor elke module zijn gestart, wilt verwijderen: 

```cmd
iotedgectl uninstall
```

Als u de IoT-hub die u hebt gemaakt niet meer nodig hebt, kunt u met de opdracht [az iot hub delete] [ lnk-delete] de resource en alle apparaten die eraan zijn gekoppeld verwijderen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een IoT Edge-module implementeert naar een IoT Edge-apparaat. Probeer nu om verschillende typen Azure-services als modules te implementeren, zodat u de gegevens aan de rand kunt analyseren. 

* [Azure Function implementeren als module](tutorial-deploy-function.md)
* [Azure Stream Analytics implementeren als module](tutorial-deploy-stream-analytics.md)
* [Uw eigen code implementeren als module](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-python]: https://www.python.org/downloads/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete
[lnk-install-iotcore]: how-to-install-iot-core.md

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
