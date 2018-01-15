---
title: Quick Start Azure IoT rand + Linux | Microsoft Docs
description: Azure IoT rand uitproberen door het uitvoeren van analyses op een gesimuleerde edge-apparaat
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 440b70f4d04728973d77e54e7f6303e1ad7fcd89
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-or-mac-device---preview"></a>Snelstartgids: Uw eerste rand van de IoT-module naar een Linux- of Mac-apparaat implementeren - voorbeeld

Azure IoT-rand wordt verplaatst van de kracht van de cloud op uw Internet of Things-apparaten. Informatie over het gebruik van de cloud-interface vooraf gedefinieerde code om extern te implementeren naar een IoT-randapparaat in dit onderwerp.

Als u geen een actief Azure-abonnement hebt, maakt u een [gratis account] [ lnk-account] voordat u begint.

## <a name="prerequisites"></a>Vereisten

Deze snelstartgids maakt gebruik van uw computer of virtuele machine, zoals een Internet of Things-apparaat. Als u wilt uw machine in een Edge van de IoT-apparaat, zijn de volgende services zijn vereist:

* Python pip voor het installeren van de rand van de IoT-runtime.
   * Linux: `sudo apt-get install python-pip`.
   * Mac OS: `sudo easy_install pip`.
* Docker om uit te voeren van de rand IoT modules
   * [Docker voor Linux installeren] [ lnk-docker-ubuntu] en zorg ervoor dat deze wordt uitgevoerd. 
   * [Docker voor Mac installeert] [ lnk-docker-mac] en zorg ervoor dat deze wordt uitgevoerd. 

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

Maak een apparaat-id waarmee uw gesimuleerde apparaat kan communiceren met uw IoT-hub. Aangezien IoT Edge-apparaten zich gedragen en anders dan typische IoT-apparaten kunnen worden beheerd, kunt declareren u deze optie om te worden van een IoT-randapparaat vanaf het begin. 

1. Navigeer naar uw IoT-hub in de Azure portal.
1. Selecteer **IoT rand (preview)**.
1. Selecteer **toevoegen IoT randapparaat**.
1. Geef uw gesimuleerde apparaat een unieke apparaat-ID.
1. Selecteer **opslaan** apparaat toevoegen.
1. Selecteer het nieuwe apparaat in de lijst met apparaten. 
1. Kopieer de waarde voor **verbindingsreeks--primaire sleutel** en op te slaan. U gebruikt deze waarde voor het configureren van de rand van de IoT-runtime in de volgende sectie. 

## <a name="install-and-start-the-iot-edge-runtime"></a>Installeren en starten van de rand van de IoT-runtime

De runtime IoT-rand wordt geïmplementeerd op alle rand van de IoT-apparaten. Het omvat twee modules. Eerst vergemakkelijkt de agent IoT rand implementatie en controle van modules die zich in de IoT-randapparaat. Ten tweede beheert de rand van de IoT-hub communicatie tussen het apparaat aan de rand van de IoT-modules en tussen het apparaat en IoT Hub. 

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

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

In deze snelstartgids gemaakt van een nieuwe IoT Edge-apparaat en de rand van de IoT-runtime is geïnstalleerd. Vervolgens gebruikt u de Azure-portal voor de push-een IoT-Edge-module worden uitgevoerd op het apparaat zonder dat wijzigingen aanbrengen in het apparaat zelf. In dit geval wordt maakt de module die u gepusht milieu gegevens die u voor de zelfstudies gebruiken kunt. 

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

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet langer de IoT-Hub die u hebt gemaakt, kunt u de [az iot hub verwijderen] [ lnk-delete] opdracht om de resource en alle apparaten die zijn gekoppeld aan deze te verwijderen:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe een module IoT rand implementeert op een rand van de IoT-apparaat. Nu gaat implementeren van verschillende soorten Azure-services als modules, zodat u gegevens aan de rand kunt analyseren. 

* [Uw eigen code implementeren als een module](tutorial-csharp-module.md)
* [Azure-functie implementeren als een module](tutorial-deploy-function.md)
* [Azure Stream Analytics implementeren als module](tutorial-deploy-stream-analytics.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
