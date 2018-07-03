---
title: 'Snelstart: Azure IoT Edge + Linux | Microsoft Docs'
description: In deze snelstart leert u hoe u vooraf geschreven code op afstand implementeert op een IoT Edge-apparaat.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 86bf28249321a705e8855de35121611b05009854
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063490"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Snelstart: uw eerste IoT Edge-module implementeren op een Linux x64-apparaat

Met Azure IoT Edge kunt u profiteren van de kracht van de cloud op uw Internet of Things-apparaten. In deze snelstart gebruikt u de cloudinterface om vooraf geschreven code op afstand te implementeren op een IoT Edge-apparaat.

In deze snelstart leert u de volgende zaken:

1. Een IoT Hub maken.
2. Een IoT Edge-apparaat registreren in uw IoT-hub.
3. De IoT Edge-runtime op uw apparaat installeren en starten.
4. Op afstand een module implementeren op een IoT Edge-apparaat.

![Snelstartarchitectuur][2]

In deze snelstart verandert u uw Linux-computer of virtuele machine in een IoT Edge-apparaat. Vervolgens implementeert u een module vanuit Azure Portal op uw apparaat. De module die u in deze snelstart implementeert, is een gesimuleerde sensor waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. De andere Azure IoT Edge-zelfstudies zijn een uitbreiding op het werk dat u hier doet. Hierin worden modules geïmplementeerd waarmee de gesimuleerde gegevens worden geanalyseerd voor zakelijke inzichten. 

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account][lnk-account] aan voordat u begint.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U gebruikt de Azure CLI om veel van de stappen in deze snelstart uit te voeren, en Azure IoT heeft een extensie om extra functionaliteit in te schakelen. 

Voeg de Azure IoT-extensie toe aan het exemplaar van Cloud Shell.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin met de snelstart door uw IoT Hub in Azure Portal te maken.
![IoT Hub maken][3]

Het gratis niveau van IoT Hub werkt voor deze snelstart. Als u in het verleden IoT Hub hebt gebruikt en al een gratis hub hebt gemaakt, kunt u die IoT-hub gebruiken. Elk abonnement biedt toegang tot slechts één gratis IoT-hub. 

1. Maak een resourcegroep in Azure Cloud Shell. Met de volgende code wordt een resourcegroep met de naam **TestResources** gemaakt in de regio **West US - west**. Door alle resources voor de snelstarts en zelfstudies in een groep te plaatsen, kunt u ze samen beheren. 

   ```azurecli-interactive
   az group create --name TestResources --location westus
   ```

1. Maak een IoT-hub in uw nieuwe resourcegroep. Met de volgende code wordt een gratis **F1**-hub gemaakt in de resourcegroep **TestResources**. Vervang *{hub_name}* door een unieke naam voor uw IoT-hub.

   ```azurecli-interactive
   az iot hub create --resource-group TestResources --name {hub_name} --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Een apparaat registreren][4]

Maak een apparaat-id voor uw gesimuleerde apparaat, zodat het met uw IoT-hub kan communiceren. Omdat IoT Edge-apparaten zich anders gedragen en anders kunnen worden beheerd dan typische IoT-apparaten, geeft u vanaf het begin aan dat dit een IoT Edge-apparaat is. 

1. Voer in Azure Cloud Shell de volgende opdracht in om een ​​apparaat met de naam **myEdgeDevice** in uw hub te maken.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

1. Haal de verbindingsreeks voor uw apparaat op, zodat uw fysieke apparaat aan de bijbehorende identiteit in IoT Hub wordt gekoppeld. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Kopieer de verbindingsreeks en sla deze op. U gebruikt deze waarde voor het configureren van de IoT Edge-runtime in de volgende sectie. 


## <a name="install-and-start-the-iot-edge-runtime"></a>De IoT Edge-runtime installeren en starten

Installeer en start de Azure IoT Edge-runtime op uw apparaat. 
![Een apparaat registreren][5]

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze bevat drie onderdelen. De **IoT Edge-beveiligingsdaemon** wordt telkens gestart wanneer een Edge-apparaat wordt opgestart en start het apparaat op door de IoT Edge-agent te starten. De **IoT Edge-agent** faciliteert de implementatie en bewaking van modules op het IoT Edge-apparaat, inclusief de IoT Edge-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd. 

### <a name="register-your-device-to-use-the-software-repository"></a>Uw apparaat registreren voor gebruik van de softwareopslagplaats

De pakketten die u nodig hebt voor het uitvoeren van de IoT Edge-runtime worden beheerd in een softwareopslagplaats. Configureer uw IoT Edge-apparaat voor toegang tot deze opslagplaats. 

De stappen in deze sectie zijn bestemd voor apparaten met **Ubuntu 16.04**. Voor toegang tot de softwareopslagplaats op andere versies van Linux, raadpleegt u [Azure IoT Edge-runtime installeren in Linux (x64)](how-to-install-iot-edge-linux.md) of [Azure IoT Edge-runtime installeren in Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

1. Installeer de opslagplaatsconfiguratie op de computer die u als een IoT Edge-apparaat gebruikt.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Installeer een openbare sleutel voor toegang tot de opslagplaats.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Een container-runtime installeren

De IoT Edge-runtime is een set van containers, en de logica die u op uw IoT Edge-apparaat implementeert, wordt geleverd als containers. Bereid uw apparaat voor op deze onderdelen door een container-runtime te installeren.

Werk **apt get-** bij.

   ```bash
   sudo apt-get update
   ```

Installeer Moby, een container-runtime, en de bijbehorende CLI-opdrachten. 

   ```bash
   sudo apt-get install moby-engine
   sudo apt-get install moby-cli   
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>De IoT Edge-beveiligingsdeamon installeren en configureren

De beveiligingsdeamon wordt geïnstalleerd als een systeemservice, zodat de IoT Edge-runtime elke keer wordt gestart wanneer uw apparaat wordt opgestart. De installatie bevat ook een versie van **hsmlib** waarmee de beveiligingsdeamon kan communiceren met de hardwarebeveiliging van het apparaat. 

1. Download en installeer de IoT Edge-beveiligingsdaemon. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Open het IoT Edge-configuratiebestand. Het is een beveiligd bestand, dus mogelijk moet u verhoogde bevoegdheden gebruiken om het te openen.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Voeg de verbindingsreeks van het IoT Edge-apparaat toe die u bij de registratie van uw apparaat hebt gekopieerd. Vervang de waarde van de variabele **device_connection_string** die u eerder in deze snelstart hebt gekopieerd.

4. Start de Edge-beveiligingsdeamon opnieuw op:

   ```bash
   sudo systemctl restart iotedge
   ```

5. Controleer of de Edge-beveiligingsdeamon wordt uitgevoerd als een systeemservice:

   ```bash
   sudo systemctl status iotedge
   ```

   ![Zie hoe de Edge-beveiligingsdeamon wordt uitgevoerd als een systeemservice](./media/quickstart-linux/iotedged-running.png)

   U kunt ook logboeken van de Edge-beveiligingsdeamon bekijken door de volgende opdracht uit te voeren:

   ```bash
   journalctl -u iotedge
   ```

6. De modules bekijken die op uw apparaat worden uitgevoerd: 

   ```bash
   iotedge list
   ```

   ![Eén module op uw apparaat bekijken](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Een module implementeren

Beheer uw Azure IoT Edge-apparaat vanuit de cloud om een module te implementeren waarmee telemetriegegevens worden verzonden naar IoT Hub.
![Een apparaat registreren][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de IoT Edge-runtime erop geïnstalleerd. Vervolgens hebt u Azure Portal gebruikt om een IoT Edge-module te pushen om te worden uitgevoerd op het apparaat zonder dat er wijzigingen in het apparaat zelf aangebracht hoefden te worden. In dit geval worden met de module die u hebt gepusht, omgevingsgegevens gemaakt die u voor de zelfstudies kunt gebruiken. 

Open nogmaals de opdrachtprompt op de computer waarop het gesimuleerde apparaat wordt uitgevoerd. Bevestig dat de module die vanuit de cloud is geïmplementeerd, op uw IoT Edge -apparaat wordt uitgevoerd:

   ```bash
   iotedge list
   ```

   ![Drie modules op uw apparaat bekijken](./media/quickstart-linux/iotedge-list-2.png)

De berichten bekijken die vanuit de module tempSensor worden verzonden:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
Na een af- en aanmelding is *sudo* niet vereist voor de bovenstaande opdracht.

![De gegevens van uw module bekijken](./media/quickstart-linux/iotedge-logs.png)

De temperatuursensormodule wacht mogelijk op verbinding met Edge Hub als de laatste regel die u in het logboek ziet `Using transport Mqtt_Tcp_Only` is. Probeer de module te beëindigen en opnieuw te laten starten door de Edge-agent. U kunt de module beëindigen met de opdracht `sudo docker stop tempSensor`.

U kunt ook de telemetriegegevens bekijken die het apparaat verzendt door de [IoT Hub-verkenner][lnk-iothub-explorer] of de [Azure IoT Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) te gebruiken. 


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met de IoT Edge-zelfstudies, kunt u het apparaat gebruiken dat u hebt geregistreerd en ingesteld in deze snelstart. Als u de installaties van uw apparaat wilt verwijderen, gebruikt u de volgende opdrachten.  

De IoT Edge-runtime verwijderen.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

De containers die zijn gemaakt op uw apparaat verwijderen. 

   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```

De container-runtime verwijderen.

   ```bash
   sudo apt-get remove --purge moby
   ```

Wanneer u de gemaakte Azure-resources niet meer nodig hebt, kunt u de gemaakte resourcegroep en alle bijbehorende gekoppelde resources verwijderen met de volgende opdracht:

   ```azurecli-interactive
   az group delete --name TestResources
   ```

## <a name="next-steps"></a>Volgende stappen

Deze snelstart is vereist voor alle IoT Edge-zelfstudies. U kunt doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van een Azure-functie](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png


<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
