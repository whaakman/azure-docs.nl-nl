---
title: Azure Stream Analytics met Azure IoT-rand implementeren | Microsoft Docs
description: Azure Stream Analytics implementeren als een module die u wilt een edge-apparaat
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5a143bbf7abb5304ac51782d517c02ec184a05a2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Implementeren van Azure Stream Analytics als een module van de rand van de IoT - voorbeeld

IoT-apparaten kunnen grote hoeveelheden gegevens produceren. Soms heeft deze gegevens worden geanalyseerd of verwerkt alvorens de cloud te verminderen, de grootte van verzonden gegevens of voor het verwijderen van de round trip latentie van inzicht actie worden uitgevoerd.

IoT-rand wordt gebruikgemaakt van vooraf samengestelde Azure Service IoT Edge-modules voor snelle implementatie en [Azure Stream Analytics] [ azure-stream] (ASA) is één van deze module. U kunt een ASA-taak maken vanuit de portal en vervolgens keert naar IoT Hub-portal aan als een Module van de rand IoT implementeren.  

Azure Stream Analytics biedt in de syntaxis van een rijke structured query voor gegevensanalyse in de cloud en IoT rand. Zie voor meer informatie over ASA op IoT rand [ASA documentatie](../stream-analytics/stream-analytics-edge.md).

Deze zelfstudie helpt bij het maken van een Azure Stream Analytics-taak en de implementatie ervan op een Edge van de IoT-apparaat voor het verwerken van een lokale telemetriestroom rechtstreeks op het apparaat en het genereren van waarschuwingen naar station directe actie op het apparaat.  Er zijn twee modules die zijn betrokken bij deze zelfstudie. Een gesimuleerde temperatuursensor module (tempSensor) genereert gegevens van de temperatuur van 20 tot 120 graden, om de vijf seconden met 1 verhoogd. Een Stream Analytics-module Hiermee stelt u de tempSensor wanneer de gemiddelde 30 seconden 70 bereikt. In een productieomgeving, kan deze functionaliteit worden gebruikt voor een machine afgesloten of voorzorgsmaatregelen treffen wanneer de temperatuur gevaarlijke niveaus bereikt. 

Procedures voor:

> [!div class="checklist"]
> * Een taak ASA om gegevens te verwerken op de rand maken
> * Verbinding maken met de nieuwe ASA-taak met de andere zijde van de IoT-modules
> * De taak ASA implementeren voor een IoT-randapparaat

## <a name="prerequisites"></a>Vereisten

* Een IoT-Hub 
* Het apparaat dat u hebt gemaakt en geconfigureerd in de Quick Start of in Azure IoT-rand implementeren om een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] en [Linux] [ lnk-tutorial1-lin]. U moet weten van de sleutel voor het apparaat verbinding en de apparaat-ID. 
* Docker uitgevoerd op uw IoT-randapparaat
    * [Docker op Windows installeren][lnk-docker-windows]
    * [Docker installeren op Linux][lnk-docker-linux]
* Python 2.7.x op uw IoT-randapparaat
    * [Python 2.7 installeren op Windows][lnk-python].
    * De meeste Linux-distributies, Ubuntu, waaronder nog Python 2.7 geïnstalleerd.  Gebruik de volgende opdracht om ervoor te zorgen dat pip is geïnstalleerd: `sudo apt-get install python-pip`.


## <a name="create-an-asa-job"></a>Een ASA-taak maken

In deze sectie maakt u een Azure Stream Analytics-taak voor het nemen van gegevens uit uw IoT-hub, telemetriegegevens verzonden van uw apparaat opvragen en doorsturen van de resultaten naar een Azure Storage-Container (BLOB). Zie voor meer informatie de **overzicht** sectie van de [Stream Analytics-documentatie][azure-stream]. 

### <a name="create-a-storage-account"></a>Een opslagaccount maken

Een Azure Storage-account is vereist voor het bieden van een eindpunt moet worden gebruikt als uitvoer in uw ASA-taak. Het volgende voorbeeld maakt gebruik van het type BLOB-opslag.  Zie voor meer informatie de **Blobs** sectie van de [documentatie bij Azure Storage][azure-storage].

1. Navigeer in de Azure-portal naar **maken van een resource** en voer `Storage account` in de zoekbalk. Selecteer **opslagaccount - blob, bestand, tabel, wachtrij**.

2. Voer een naam voor uw opslagaccount en selecteert u dezelfde locatie waar uw IoT-hub zich bevindt. Klik op **Create**. Vergeet niet de naam op voor later.

    ![Nieuw opslagaccount][1]

3. Ga naar het opslagaccount dat u zojuist hebt gemaakt. Klik op **bladeren blobs**. 
4. Maak een nieuwe container voor de ASA-module voor het opslaan van gegevens. Stel de toegang op **Container**. Klik op **OK**.

    ![instellingen voor de opslag][10]

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. Navigeer in de Azure-portal naar **maken van een resource** > **Internet der dingen** en selecteer **Stream Analytics-taak**.

2. Geef een naam, kiest u **rand** als de Hosting-omgeving, en de overige standaardwaarden gebruiken.  Klik op **Create**.

    >[!NOTE]
    >Op dit moment worden niet ASA-jobs op IoT rand ondersteund in de regio VS-West 2. 

3. Ga in de taak gemaakt. Selecteer **invoer** klikt u vervolgens op **toevoegen**.

4. Voer voor de invoeralias `temperature`, het brontype ingesteld op **gegevensstroom**, en gebruik de standaardinstellingen voor de andere parameters. Klik op **Create**.

   ![Invoer ASA](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Selecteer **uitvoer** klikt u vervolgens op **toevoegen**.

6. Voer voor de uitvoeralias `alert`, en gebruik de standaardinstellingen voor de andere parameters. Klik op **Create**.

   ![ASA-uitvoer](./media/tutorial-deploy-stream-analytics/asa_output.png)


7. Selecteer **Query**.
8. De standaardtekst vervangen door de volgende query:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```
9. Klik op **Opslaan**.

## <a name="deploy-the-job"></a>De taak implementeren

U bent nu klaar om te implementeren, de ASA-taak op het apparaat aan uw IoT-rand.

1. Navigeer in de Azure-portal in uw IoT-hub naar **IoT rand (preview)** en open de detailpagina voor uw IoT-Edge-apparaat.
1. Selecteer **modules ingesteld**.
1. Als u de module tempSensor op dit apparaat hebt geïmplementeerd, kunnen er automatisch invullen. Als dat niet het geval is, gebruik de volgende stappen om toe te voegen die module:
   1. Klik op **IoT rand Module toevoegen**
   1. Voer `tempSensor` als de naam en `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` voor de URI van de installatiekopie. 
   1. De overige instellingen ongewijzigd laat, en klik op **opslaan**.
1. Selecteer om uw taak ASA rand toe **Import Azure Stream Analytics IoT rand Module**.
1. Selecteer uw abonnement en de rand van de ASA-taak die u hebt gemaakt. 
1. Selecteer uw abonnement en het opslagaccount dat u hebt gemaakt. Klik op **Opslaan**.

    ![set-module][6]

1. De naam die automatisch is gegenereerd voor de module ASA kopiëren. 

    ![temperatuur-module][11]

1. Klik op **volgende** routes configureren.
1. Kopieer de volgende **Routes**.  Vervang _{moduleName}_ met de naam van de module die u hebt gekopieerd:

    ```json
    {
        "routes": {                                                               
          "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream", 
          "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream", 
          "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")", 
          "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")" 
        }
    }
    ```

1. Klik op **Volgende**.

1. In de **Template bekijken** stap, klikt u op **indienen**.

1. Ga terug naar de detailpagina voor het apparaat en klikt u op **vernieuwen**.  U ziet nu de nieuwe Stream Analytics-module met samen met de **IoT rand agent** module en de **rand van de IoT hub**.

    ![module-uitvoer][7]

## <a name="view-data"></a>Gegevens weergeven

U kunt nu gaat u naar uw IoT-randapparaat de interactie tussen de ASA-module en de module tempSensor uitchecken.

Controleer of alle modules worden uitgevoerd in Docker:

   ```cmd/sh
   docker ps  
   ```

   ![docker-uitvoer][8]

Zie alle logboeken en metrische gegevens systeemgegevens. Gebruik de naam van de Stream Analytics-module:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

U moet mogelijk moeten worden gecontroleerd van de machine temperatuur geleidelijk toename totdat het 70 graden gedurende 30 seconden bereikt. Vervolgens wordt de Stream Analytics-module een reset geactiveerd en de temperatuur machine vallen terug tot en met 21. 

   ![docker-logboek][9]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure Storage-container en geconfigureerd een Streaming Analytics-taak voor het analyseren van gegevens van uw IoT-Edge-apparaat.  U vervolgens een aangepaste module ASA om gegevens te verplaatsen van uw apparaat, via de stream naar een BLOB gedownload geladen.  U kunt door te gaan naar andere zelfstudies verdere zien hoe de rand van Azure IoT-oplossingen voor uw bedrijf kunt maken.

> [!div class="nextstepaction"] 
> [Een Azure Machine Learning-model implementeren als een module][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/en-us/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/en-us/azure/storage/
[azure-stream]: https://docs.microsoft.com/en-us/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/