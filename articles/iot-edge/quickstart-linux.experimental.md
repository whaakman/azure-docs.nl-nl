---
title: Azure IoT Edge simuleren op Linux | Microsoft Docs
description: In deze snelstart leert u hoe u vooraf geschreven code op afstand implementeert op een IoT Edge-apparaat.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/02/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: dfbe931bbe5887e9c0545558c4d2b2565718dd0a
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578487"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Snelstart: uw eerste IoT Edge-module implementeren op een Linux x64-apparaat

Met Azure IoT Edge kunt u analyses en gegevensverwerking uitvoeren op uw apparaten, in plaats van dat u alle gegevens moet doorsturen naar de cloud. In de IoT Edge-zelfstudies kunt u zien hoe u verschillende typen modules implementeert, maar eerst hebt u een apparaat nodig dat u kunt testen. 

In deze snelstart leert u de volgende zaken:

1. Een IoT Hub maken.
2. Een IoT Edge-apparaat registreren in uw IoT-hub.
3. De IoT Edge-runtime starten.
4. Op afstand een module implementeren op een IoT Edge-apparaat.

![Zelfstudiearchitectuur][2]

In deze snelstart verandert u uw Linux-computer of virtuele machine in een IoT Edge-apparaat. Vervolgens implementeert u een module vanuit Azure Portal op uw apparaat. De module die u in deze snelstart implementeert, is een gesimuleerde sensor waarmee temperatuur-, luchtvochtigheids- en drukgegevens worden gegenereerd. De andere Azure IoT Edge-zelfstudies bouwen voort op het werk dat u hier doet door modules te implementeren waarmee de gesimuleerde gegevens worden geanalyseerd voor zakelijke inzichten. 

Als u nog geen actief abonnement op Azure hebt, maakt u een [gratis Azure-account][lnk-account] aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

In deze snelstart wordt een Linux-machine als een IoT Edge-apparaat gebruikt. Als die niet beschikbaar is om te testen, volg dan de instructies in [Een virtuele Linux-machine maken in Azure Portal](../virtual-machines/linux/quick-create-portal.md). 
* U hoeft de stappen voor het installeren en uitvoeren van de webserver niet te volgen. Wanneer u verbinding hebt met uw virtuele machine, kunt u stoppen.  
* Maak uw virtuele machine in een nieuwe resourcegroep en gebruik deze wanneer u de overige Azure-resources voor deze snelstart maakt. Geef deze een herkenbare naam, bijvoorbeeld *IoTEdgeResources*. 
* U hoeft geen heel grote virtuele machine te maken om IoT Edge te testen. **B1ms** is voldoende. 

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

Begin met de snelstart door uw IoT Hub in Azure Portal te maken.
![IoT Hub maken][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Een IoT Edge-apparaat registreren

Registreer een IoT Edge-apparaat bij uw net gemaakte IoT Hub.
![Een apparaat registreren][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>De IoT Edge-runtime installeren en starten

Installeer en start de Azure IoT Edge-runtime op uw apparaat. 
![Een apparaat registreren][5]

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. Deze bevat drie onderdelen. De **IoT Edge-beveiligingsdaemon** wordt telkens gestart wanneer een Edge-apparaat wordt opgestart en start het apparaat op door de IoT Edge-agent te starten. De **IoT Edge-agent** faciliteert de implementatie en bewaking van modules op het IoT Edge-apparaat, inclusief de IoT Edge-hub. Met de **IoT Edge-hub** wordt de communicatie tussen modules op het IoT Edge-apparaat en tussen het apparaat en IoT Hub beheerd. 

Voer de volgende stappen uit op de Linux-machine of virtuele machine die u voor deze snelstart hebt voorbereid. 

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

Installeer **Moby**, een container-runtime.

   ```bash
   sudo apt-get install moby-engine
   ```

Installeer de CLI-opdrachten voor Moby. 

   ```bash
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

3. Voeg de verbindingsreeks van het IoT Edge-apparaat toe. Zoek de variabele **device_connection_string** en werk de waarde bij met de tekenreeks die u hebt gekopieerd na de registratie van uw apparaat.

4. Sla het bestand op en sluit het. 

   `CTRL + X`, `Y`, `Enter`

4. Start de IoT Edge-beveiligingsdaemon opnieuw op.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Controleer of de Edge-beveiligingsdaemon wordt uitgevoerd als een systeemservice.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Zie hoe de Edge-beveiligingsdeamon wordt uitgevoerd als een systeemservice](./media/quickstart-linux/iotedged-running.png)

   U kunt ook logboeken van de Edge-beveiligingsdeamon bekijken door de volgende opdracht uit te voeren:

   ```bash
   journalctl -u iotedge
   ```

6. Bekijk de modules die op uw apparaat worden uitgevoerd. 

   >[!TIP]
   >U moet eerst *sudo* gebruiken om `iotedge`-opdrachten uit te voeren. Meld u af bij de computer en meld u weer aan om machtigingen bij te werken. Daarna kunt u `iotedge`-opdrachten zonder verhoogde bevoegdheden uitvoeren. 

   ```bash
   sudo iotedge list
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
   sudo iotedge list
   ```

   ![Drie modules op uw apparaat bekijken](./media/quickstart-linux/iotedge-list-2.png)

De berichten bekijken die vanuit de module tempSensor worden verzonden:

  ```bash
   sudo iotedge logs tempSensor -f 
   ```

Na een af- en aanmelding is *sudo* niet vereist voor de bovenstaande opdracht.

![De gegevens van uw module bekijken](./media/quickstart-linux/iotedge-logs.png)

De temperatuursensormodule wacht mogelijk op verbinding met Edge Hub als de laatste regel die u in het logboek ziet `Using transport Mqtt_Tcp_Only` is. Probeer de module te beëindigen en opnieuw te laten starten door de Edge-agent. U kunt de module beëindigen met de opdracht `sudo docker stop tempSensor`.

U kunt ook de telemetriegegevens bekijken die het apparaat verzendt door de [Azure IoT Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) te gebruiken. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met de IoT Edge-zelfstudies, kunt u het apparaat gebruiken dat u hebt geregistreerd en ingesteld in deze snelstart. Anders kunt u de Azure-resources die u hebt gemaakt en de IoT Edge-runtime van uw apparaat verwijderen. 

### <a name="delete-azure-resources"></a>Azure-resources verwijderen

Als u uw virtuele machine en IoT-hub in een nieuwe resourcegroep hebt gemaakt, kunt u die groep en alle bijbehorende resources verwijderen. Als er iets in die resourcegroep staat dat u wilt behouden, verwijder dan alleen de afzonderlijke resources die u wilt opschonen. 

Voer de volgende stappen uit als u een resourcegroep wilt verwijderen: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Resourcegroepen**.
2. Typ in het tekstvak **Filteren op naam...** de naam van de resourcegroep die uw IoT Hub bevat. 
3. Klik rechts van de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.
4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep nogmaals om te bevestigen en klik op **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.

### <a name="remove-the-iot-edge-runtime"></a>De IoT Edge-runtime verwijderen

Als u de installaties van uw apparaat wilt verwijderen, gebruikt u de volgende opdrachten.  

Verwijder de IoT Edge-runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Wanneer de IoT Edge-runtime is verwijderd, worden de containers die deze heeft gemaakt gestopt. Ze worden echter niet van uw apparaat verwijderd. Geef alle containers weer.

   ```bash
   sudo docker ps -a
   ```

Verwijder de containers die door de IoT Edge-runtime op uw apparaat zijn gemaakt. Verander de naam van de container tempSensor als u die anders had genoemd. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Verwijder de container-runtime.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een nieuw IoT Edge-apparaat gemaakt en de Azure IoT Edge-cloudinterface gebruikt om code te implementeren op het apparaat. U hebt nu een gesimuleerd apparaat waarmee onbewerkte gegevens over de omgeving worden gegenereerd. 

Deze snelstart is vereist voor alle IoT Edge-zelfstudies. U kunt doorgaan met elke andere zelfstudie om te leren hoe Azure IoT Edge u verder kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Sensorgegevens filteren met behulp van een Azure-functie](tutorial-deploy-function.md)


<!-- Images -->
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
[lnk-account]: https://azure.microsoft.com/free
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
