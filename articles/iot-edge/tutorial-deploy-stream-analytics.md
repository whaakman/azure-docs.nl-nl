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
ms.openlocfilehash: f40fb81fc03e796b906db12bf3bf6904b27b46eb
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Implementeren van Azure Stream Analytics als een module van de rand van de IoT - voorbeeld

IoT-apparaten kunnen grote hoeveelheden gegevens produceren. Verklein de hoeveelheid geüploade gegevens of elimineren de round trip latentie van inzicht actie worden uitgevoerd, moet de gegevens soms worden geanalyseerd of verwerkt voordat het de cloud bereikt.

Azure IoT-rand wordt gebruikgemaakt van vooraf samengestelde Azure service IoT rand modules voor snelle implementatie. [Azure Stream Analytics] [ azure-stream] is één van deze module. U kunt een Azure Stream Analytics-taak maken vanuit de portal en gaat u naar de portal Azure IoT Hub voor als een module IoT rand implementeren. 

Azure Stream Analytics biedt in de syntaxis van een rijke structured query voor gegevensanalyse in de cloud en IoT rand. Zie voor meer informatie over Azure Stream Analytics op IoT rand [Azure Stream Analytics-documentatie](../stream-analytics/stream-analytics-edge.md).

Deze zelfstudie leidt u door een Azure Stream Analytics-taak maken en deze op een Edge van de IoT-apparaat is geïmplementeerd. In dat geval kunt u een lokale telemetriestroom rechtstreeks op het apparaat verwerken en genereren van waarschuwingen die onmiddellijke actie station op het apparaat. 

De zelfstudie toont twee modules: 
* Een gesimuleerde temperatuursensor module (tempSensor) die gegevens van de temperatuur van 20 en 120 graden genereert, verhoogd met 1 om de vijf seconden. 
* Een Stream Analytics-module die de tempSensor wordt opnieuw ingesteld wanneer de gemiddelde 30 seconden 70 bereikt. In een productieomgeving kunt u deze functionaliteit te een machine afgesloten of voorzorgsmaatregelen te nemen wanneer de temperatuur gevaarlijke niveaus bereikt. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Azure Stream Analytics-taak om gegevens te verwerken op de rand.
> * Verbinding maken met de nieuwe Azure Stream Analytics-taak met andere modules IoT rand.
> * De Azure Stream Analytics-taak op een Edge van de IoT-apparaat implementeren.

## <a name="prerequisites"></a>Vereisten

* Een IoT-hub. 
* Het apparaat dat u hebt gemaakt en geconfigureerd in de Quick Start of in de artikelen over het implementeren van Azure IoT Edge op een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] of in [Linux] [ lnk-tutorial1-lin]. U moet weten van de sleutel voor het apparaat verbinding en de apparaat-ID. 
* Docker uitgevoerd op het apparaat aan uw IoT-rand.
    * [Docker installeren op Windows][lnk-docker-windows].
    * [Docker installeren op Linux][lnk-docker-linux].
* Python 2.7.x op uw apparaat IoT rand.
    * [Python 2.7 installeren op Windows][lnk-python].
    * De meeste Linux-distributies, Ubuntu, waaronder nog Python 2.7 geïnstalleerd. Om ervoor te zorgen dat pip is geïnstalleerd, gebruikt u de volgende opdracht: `sudo apt-get install python-pip`.

## <a name="create-an-azure-stream-analytics-job"></a>Een Azure Stream Analytics-taak maken

In deze sectie maakt u een Azure Stream Analytics-taak voor het nemen van gegevens uit uw IoT-hub, telemetriegegevens verzonden van uw apparaat opvragen en doorsturen naar de resultaten uitgevoerd naar een Azure Blob storage-container. Zie voor meer informatie de sectie 'Overzicht' van de [Stream Analytics-documentatie][azure-stream]. 

### <a name="create-a-storage-account"></a>Een opslagaccount maken

Een Azure Storage-account is vereist voor het bieden van een eindpunt moet worden gebruikt als uitvoer in uw Azure Stream Analytics-taak. Het voorbeeld in deze sectie wordt het type Blob-opslag. Zie voor meer informatie de sectie 'Blobs' van de [documentatie bij Azure Storage][azure-storage].

1. In de Azure portal, gaat u naar **maken van een resource**, voer **opslagaccount** in het zoekvak en selecteer vervolgens **opslagaccount - blob, bestand, tabel, wachtrij**.

2. In de **storage-account maken** deelvenster Voer een naam voor uw storage-account, selecteert u dezelfde locatie waar uw IoT-hub is opgeslagen en selecteer vervolgens **maken**. Noteer de naam voor later gebruik.

    ![Een opslagaccount maken][1]

3. Ga naar het opslagaccount dat u zojuist hebt gemaakt en selecteer vervolgens **bladeren blobs**. 

4. Maak een nieuwe container voor de Azure Stream Analytics-module voor het opslaan van gegevens, stelt u de toegang op **Container**, en selecteer vervolgens **OK**.

    ![instellingen voor de opslag][10]

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. In de Azure portal, gaat u naar **maken van een resource** > **Internet der dingen**, en selecteer vervolgens **Stream Analytics-taak**.

2. In de **nieuwe Stream Analytics-taak** deelvenster de volgende handelingen uit:

    a. In de **taaknaam** typt u de taaknaam van een.
    
    b. Onder **hostomgeving**, selecteer **rand**.
    
    c. Gebruik de standaardwaarden in de resterende velden.

    > [!NOTE]
    > Op dit moment worden Azure Stream Analytics-taken op de rand van de IoT worden niet ondersteund in de regio VS-West 2. 

3. Selecteer **Maken**.

4. In de gemaakte taak onder **taak topologie**, selecteer **invoer**, en selecteer vervolgens **toevoegen**.

5. In de **nieuwe invoer** deelvenster de volgende handelingen uit:

    a. In de **invoer alias** Voer **temperatuur**.
    
    b. In de **brontype** de optie **gegevensstroom**.
    
    c. Gebruik de standaardwaarden in de resterende velden.

   ![Azure Stream Analytics-invoer](./media/tutorial-deploy-stream-analytics/asa_input.png)

6. Selecteer **Maken**.

7. Onder **taak topologie**, selecteer **uitvoer**, en selecteer vervolgens **toevoegen**.

8. In de **nieuwe uitvoer** deelvenster de volgende handelingen uit:

    a. In de **uitvoeraliassen** in het vak **waarschuwing**.
    
    b. Gebruik de standaardwaarden in de resterende velden. 
    
    c. Selecteer **Maken**.

   ![Azure Stream Analytics-uitvoer](./media/tutorial-deploy-stream-analytics/asa_output.png)


9. Onder **taak topologie**, selecteer **Query**, en vervang de standaardtekst door de volgende query:

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

10. Selecteer **Opslaan**.

## <a name="deploy-the-job"></a>De taak implementeren

U bent nu klaar om te implementeren, de Azure Stream Analytics-taak op het apparaat aan uw IoT-rand.

1. Ga in de Azure-portal in uw IoT-hub naar **IoT rand (preview)**, en open vervolgens de detailpagina voor uw IoT-randapparaat.

2. Selecteer **modules ingesteld**.  
    Als u de module tempSensor op dit apparaat hebt geïmplementeerd, kan deze automatisch invullen. Als dit niet het geval is, voegt u de module als volgt:

   a. Selecteer **IoT rand Module toevoegen**.

   b. Typ de naam **tempSensor**.
    
   c. Voer voor de installatiekopie URI **microsoft/azureiotedge-simulated-temperatuur-temperatuursensor: 1.0-preview**. 

   d. De andere instellingen ongewijzigd laat.
   
   e. Selecteer **Opslaan**.

3. Selecteer om de taak Azure Stream Analytics rand toe **Import Azure Stream Analytics IoT rand Module**.

4. Selecteer uw abonnement en de rand van Azure Stream Analytics-taak die u hebt gemaakt. 

5. Selecteer uw abonnement en het opslagaccount dat u maakte en selecteer vervolgens **opslaan**.

    ![set-module][6]

6. Kopieer de naam van uw Azure Stream Analytics-module. 

    ![temperatuur-module][11]

7. Selecteer voor het configureren van routes **volgende**.

8. Kopieer de volgende code naar **Routes**. Vervang _{moduleName}_ met de naam van de module die u hebt gekopieerd:

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

9. Selecteer **volgende**.

10. In de **Template bekijken** stap, selecteer **indienen**.

11. Terug naar de pagina met apparaten en selecteer vervolgens **vernieuwen**.  
    Hier ziet u de nieuwe Stream Analytics-module wordt uitgevoerd, samen met de module IoT Edge-agent en de rand van de IoT-hub.

    ![module-uitvoer][7]

## <a name="view-data"></a>Gegevens weergeven

U kunt nu gaat u naar uw IoT-randapparaat de interactie tussen de Azure Stream Analytics-module en de module tempSensor uitchecken.

1. Controleer of alle modules worden uitgevoerd in Docker:

   ```cmd/sh
   docker ps  
   ```

   ![docker-uitvoer][8]

2. Alle system-logboeken en metrische gegevens weergeven. Gebruik de naam van de Stream Analytics-module:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

U moet mogelijk moeten worden gecontroleerd van de machine temperatuur geleidelijk toename totdat het 70 graden gedurende 30 seconden bereikt. Vervolgens wordt de Stream Analytics-module een reset geactiveerd en de temperatuur machine zakt terug tot en met 21. 

   ![docker-logboek][9]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure storage-container en geconfigureerd een Streaming Analytics-taak voor het analyseren van gegevens van uw IoT-Edge-apparaat. U vervolgens een aangepaste module voor Azure Stream Analytics om gegevens te verplaatsen van uw apparaat, via de stream naar een blob gedownload geladen. Als u wilt zien hoe Azure IoT rand meer oplossingen voor uw bedrijf kunt maken, door te gaan naar de andere zelfstudies.

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
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/
