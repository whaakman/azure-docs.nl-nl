---
title: Quick Start Azure IoT rand + Windows | Microsoft Docs
description: Azure IoT rand uitproberen door het uitvoeren van analyses op een gesimuleerde edge-apparaat
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 17675f870a015e86f98bf286a9b1c2bbc05c16cd
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Snelstartgids: Uw eerste rand van de IoT-module van de Azure-portal naar een Windows-apparaat implementeren - voorbeeld

Gebruik de Azure IoT rand cloud-interface vooraf gedefinieerde code om extern te implementeren naar een IoT-randapparaat in deze snelstartgids. Aan deze taak, gebruikt u eerst uw Windows-apparaat om te simuleren een Edge van de IoT-apparaat, kunt u vervolgens een module te implementeren.

Als u geen een actief Azure-abonnement hebt, maakt u een [gratis account] [ lnk-account] voordat u begint.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt ervan uitgegaan dat u een computer of virtuele machine met Windows om te simuleren dat een Internet of Things-apparaat. Als u Windows in een virtuele machine uitvoert, schakelt u [geneste virtualisatie] [ lnk-nested] en ten minste 2 GB geheugen toewijzen. 

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

## <a name="create-an-iot-hub-with-azure-cli"></a>Een iothub maken met Azure CLI

Maak een IoT-hub in uw Azure-abonnement. Het gratis niveau van IoT Hub werkt voor deze snelstartgids. Als u IoT-Hub in het verleden hebt gebruikt en u al een gratis-hub gemaakt hebt, kunt u deze sectie overslaan en doorgaan met het [registreren van een IoT-randapparaat][anchor-register]. Elk abonnement kan slechts één gratis IoT-hub hebben. 

1. Meld u aan bij [Azure Portal][lnk-portal]. 
1. Selecteer de **Cloud Shell** knop. 

   ![Knop voor cloud-Shell][1]

1. Maak een resourcegroep. De volgende code maakt een resourcegroep aangeroepen **IoTEdge** in de **VS-West** regio:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Een iothub in uw nieuwe resourcegroep maken. De volgende code maakt een gratis **F1** hub aangeroepen **MyIotHub** in de resourcegroep **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Een IoT-Edge-apparaat registreren

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Configureren van de rand van de IoT-runtime

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. Eerst vergemakkelijkt de agent IoT rand implementatie en controle van modules die zich in de IoT-randapparaat. Ten tweede beheert de rand van de IoT-hub communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. 

Configureer de runtime door uw verbindingsreeks rand van de IoT-apparaat uit de vorige sectie.

```
iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords
```

Start de runtime.

```
iotedgectl start
```

Controleer de Docker om te zien of de rand van de IoT-agent wordt uitgevoerd als een module.

```
docker ps
```

![Zie edgeAgent in Docker](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Een module implementeren

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze snelstartgids gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

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
## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de IoT-Hub die u hebt gemaakt, kunt u de [az iot hub verwijderen] [ lnk-delete] opdracht om de resource en alle apparaten die zijn gekoppeld aan deze te verwijderen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe een module IoT rand implementeert op een rand van de IoT-apparaat. Nu gaat implementeren van verschillende soorten Azure-services als modules, zodat u gegevens aan de rand kunt analyseren. 

* [Azure-functie implementeren als een module](tutorial-deploy-function.md)
* [Azure Stream Analytics implementeren als module](tutorial-deploy-stream-analytics.md)
* [Uw eigen code implementeren als een module](tutorial-csharp-module.md)


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
