---
title: Azure Stream Analytics met Azure IoT-rand implementeren | Microsoft Docs
description: Azure Stream Analytics implementeren als een module die u wilt een edge-apparaat
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0d19d1142cf15221f84692f7e613edd6b46b4083
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Implementeren van Azure Stream Analytics als een module van de rand van de IoT - voorbeeld

IoT-apparaten kunnen grote hoeveelheden gegevens produceren. Soms heeft deze gegevens worden geanalyseerd of verwerkt alvorens de cloud te verminderen, de grootte van verzonden gegevens of voor het verwijderen van de round trip latentie van inzicht actie worden uitgevoerd.

[Azure Stream Analytics] [ azure-stream] (ASA) biedt een rijke structured querysyntaxis voor analyse van gegevens in de cloud en IoT rand apparaten. Zie voor meer informatie over ASA op IoT rand [ASA documentatie](../stream-analytics/stream-analytics-edge.md).

Deze zelfstudie helpt bij het maken van een Azure Stream Analytics-taak en de implementatie ervan op een Edge van de IoT-apparaat voor het verwerken van een lokale telemetriestroom rechtstreeks op het apparaat en het genereren van waarschuwingen naar station directe actie op het apparaat.  Er zijn twee modules die zijn betrokken bij deze zelfstudie. Een gesimuleerde temperatuursensor module (tempSensor) die gegevens van de temperatuur van 20 genereert en 120 graden verhoogd met 1 om de vijf seconden en een ASA-module die groter zijn dan 100 graden temperaturen gefilterd. De ASA-module ook hersteld de tempSensor wanneer de gemiddelde 30 seconden 100 bereikt.

Procedures voor:

> [!div class="checklist"]
> * Een taak ASA om gegevens te verwerken op de rand maken
> * Verbinding maken met de nieuwe ASA-taak met de andere zijde van de IoT-modules
> * De taak ASA implementeren voor een IoT-randapparaat

## <a name="prerequisites"></a>Vereisten

* Een IoT-Hub 
* Het apparaat dat u hebt gemaakt en geconfigureerd in de Quick Start of in Azure IoT-rand implementeren om een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] en [Linux] [ lnk-tutorial1-lin].
* Docker op uw IoT-randapparaat
    * [Docker installeren op Windows] [ lnk-docker-windows] en zorg ervoor dat deze wordt uitgevoerd.
    * [Docker installeren op Linux] [ lnk-docker-linux] en zorg ervoor dat deze wordt uitgevoerd.
* Python 2.7.x op uw IoT-randapparaat
    * [Python 2.7 installeren op Windows][lnk-python].
    * De meeste Linux-distributies, Ubuntu, waaronder nog Python 2.7 geïnstalleerd.  Gebruik de volgende opdracht om ervoor te zorgen dat pip is geïnstalleerd: `sudo apt-get install python-pip`.

> [!NOTE]
> Merk op uw apparaat de verbindingstekenreeks en de rand van de IoT-apparaat-ID zijn nodig voor deze zelfstudie.

IoT-rand wordt gebruikgemaakt van vooraf samengestelde Azure Service IoT Edge-modules voor snelle implementatie en Azure Stream Analytics (ASA) is één van deze module. U kunt een ASA-taak maken vanuit de portal en vervolgens keert naar IoT Hub-portal aan als een Module van de rand IoT implementeren.  

Zie voor meer informatie over Azure Stream Analytics de **overzicht** sectie van de [Stream Analytics-documentatie][azure-stream].

## <a name="create-an-asa-job"></a>Een ASA-taak maken

In deze sectie maakt u een Azure Stream Analytics-taak voor het nemen van gegevens uit uw IoT-hub, telemetriegegevens verzonden van uw apparaat opvragen en doorsturen van de resultaten naar een Azure Storage-Container (BLOB). Zie voor meer informatie de **overzicht** sectie van de [Stream Analytics-documentatie][azure-stream]. 

> [!NOTE]
> Een Azure Storage-account is vereist voor het bieden van een eindpunt moet worden gebruikt als uitvoer in uw ASA-taak. Het volgende voorbeeld maakt gebruik van het type BLOB-opslag.  Zie voor meer informatie de **Blobs** sectie van de [documentatie bij Azure Storage][azure-storage].

1. Navigeer in de Azure-portal naar **maken een resource -> opslag**, klikt u op **alle**, en klik op **opslagaccount - blob, bestand, tabel, wachtrij**.

2. Voer een naam voor uw opslagaccount en selecteert u dezelfde locatie waar uw IoT-Hub is opgeslagen. Klik op **Create**. Noteer de naam voor later.

    ![Nieuw opslagaccount][1]

3. Ga naar het opslagaccount dat u zojuist hebt gemaakt in de Azure portal. Klik op **bladeren blobs** onder **Blob-Service**. 
4. Maak een nieuwe container voor de ASA-module voor het opslaan van gegevens. Stel de toegang op _Container_. Klik op **OK**.

    ![instellingen voor de opslag][10]

5. Navigeer in de Azure-portal naar **maken van een resource** > **Internet der dingen** en selecteer **Stream Analytics-taak**.

2. Geef een naam, kiest u **rand** als de Hosting-omgeving, en de overige standaardwaarden gebruiken.  Klik op **Create**.

    >[!NOTE]
    >Op dit moment worden niet ASA-jobs op IoT rand ondersteund in de regio VS West 2. Selecteer een andere locatie.

    ![ASA maken][5]

2. Ga naar de gemaakte taak onder **taak topologie**, selecteer **invoer**, klikt u op **toevoegen**.

3. Voer naam `temperature`, kies **gegevensstroom** als het brontype en gebruik van de standaardwaarden voor de andere parameters. Klik op **Create**.

    ![Invoer ASA][2]

    > [!NOTE]
    > Extra ingangen kunnen specifieke eindpunten van IoT rand bevatten.

4. Onder **taak topologie**, selecteer **uitvoer**, klikt u op **toevoegen**.

5. Voer naam `alert` en gebruik de standaardinstellingen. Klik op **Create**.

    ![ASA-uitvoer][3]

6. Onder **taak topologie**, selecteer **Query**, en voer de volgende:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 100
    ```

## <a name="deploy-the-job"></a>De taak implementeren

U bent nu klaar om te implementeren, de ASA-taak op het apparaat aan uw IoT-rand.

1. Navigeer in de Azure-portal in uw IoT-Hub naar **IoT rand (preview)** en open uw *{deviceId}*van blade.

1. Selecteer **modules ingesteld**, selecteer daarna **importeren Azure IoT rand servicemodule**.

1. Selecteer het abonnement en de rand van de ASA-taak die u hebt gemaakt. Selecteer vervolgens uw storage-account. Klik op **Opslaan**.

    ![set-module][6]

1. Klik op **IoT rand Module toevoegen** de temperatuursensor module wilt toevoegen. Voer _tempSensor_ naam `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` voor afbeeldings-URL. De overige instellingen ongewijzigd laat, en klik op **opslaan**.

    ![temperatuur-module][11]

1. De naam van de module ASA kopiëren. Klik op **volgende** routes configureren.

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

1. Ga terug naar de detailpagina voor het apparaat en klikt u op **vernieuwen**.  U ziet nu de nieuwe _{moduleName}_ module uitgevoerd samen met de **IoT rand agent** module en de **rand van de IoT hub**.

    ![module-uitvoer][7]

## <a name="view-data"></a>Gegevens weergeven

U kunt nu gaat u naar uw IoT-randapparaat de interactie tussen de ASA-module en de module tempSensor uitchecken.

1. Configureer de runtime door uw verbindingsreeks van de rand van de IoT-apparaat bij een opdrachtprompt:

    ```cmd/sh
    iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords  
    ```

1. Voer de opdracht de runtime starten:

    ```cmd/sh
    iotedgectl start  
    ```

1. Voer de opdracht om te zien van de modules dat wordt uitgevoerd:

    ```cmd/sh
    docker ps  
    ```

    ![docker-uitvoer][8]

1. Voer de opdracht om alle systeemlogboeken en metrische gegevens te zien. Gebruik de naam van de module van boven:

    ```cmd/sh
    docker logs -f {moduleName}  
    ```

    ![docker-logboek][9]

1. In de Azure-portal in uw opslagaccount onder **Blob-Service**, klikt u op **bladeren blobs**, selecteer uw container en selecteer de zojuist gemaakte JSON-bestand.

1. Klik op **downloaden** en bekijk de resultaten.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Azure Storage-container en geconfigureerd een Streaming Analytics-taak voor het analyseren van gegevens van uw IoT-Edge-apparaat.  U vervolgens een aangepaste module ASA om gegevens te verplaatsen van uw apparaat, via de stream naar een BLOB gedownload geladen.  U kunt door te gaan naar andere zelfstudies verdere zien hoe de rand van Azure IoT-oplossingen voor uw bedrijf kunt maken.

> [!div class="nextstepaction"] 
> [Een Azure Machine Learning-model implementeren als een module][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[2]: ./media/tutorial-deploy-stream-analytics/asa_input.png
[3]: ./media/tutorial-deploy-stream-analytics/asa_output.png
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