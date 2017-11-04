---
title: Integratie van Azure Stream Analytics en Machine Learning | Microsoft Docs
description: Het gebruik van een gebruiker gedefinieerde functie en Machine Learning in een Stream Analytics-taak
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: samacha
ms.openlocfilehash: 243ee799d2cddb1baf5b8046eee6eaf182463d2e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Gevoel analyse uitvoeren met behulp van Azure Stream Analytics en Azure Machine Learning
In dit artikel wordt beschreven hoe snel een eenvoudige Azure Stream Analytics-taak die Azure Machine Learning integreert instellen. U kunt een Machine Learning gevoel analytics-model van de Cortana Intelligence Gallery streaming tekstgegevens analyseren en te bepalen van de score gevoel in realtime. Met de Cortana Intelligence Suite kunt u deze taak uitvoeren zonder dat u de beschrijving van het bouwen van een gevoel analytics-model.

U kunt toepassen wat u leert van dit artikel voor dergelijke scenario's:

* Analyse van realtime gevoel op Twitter gegevensstromen.
* Records van de klant analyseren chats met het ondersteunend personeel.
* Evalueren van opmerkingen over forums, blogs en video's. 
* Veel andere realtime, voorspellende scoreprofiel scenario's.

In een Praktijkscenario krijgt u de gegevens rechtstreeks vanuit een gegevensstroom Twitter. Voor het vereenvoudigen van de zelfstudie we hebt geschreven zodat de Streaming Analytics-taak tweets opgehaald uit een CSV-bestand in Azure Blob-opslag. U kunt uw eigen CSV-bestand maken of u kunt een CSV-voorbeeldbestand, zoals wordt weergegeven in de volgende afbeelding:

![voorbeeld tweets in een CSV-bestand](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

De Streaming Analytics-taak die u maakt geldt het gevoel analytics-model als een gebruiker gedefinieerde functie (UDF) op de voorbeeldgegevens van de tekst uit het bloblarchief. De uitvoer (het resultaat van de analyse gevoel) wordt aan dezelfde opslag-blob in een ander CSV-bestand geschreven. 

De volgende afbeelding ziet deze configuratie. Zoals aangegeven voor een meer realistische scenario kunt u de blob-opslag met Twitter-gegevens uit Azure Event Hubs invoer streaming vervangen. Bovendien kan u samenstellen een [Microsoft Power BI](https://powerbi.microsoft.com/) realtime visualisatie van de cumulatieve gevoel.    

![Overzicht van stream Analytics Machine Learning-integratie](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Vereisten
Voordat u begint, zorg er dan voor dat u hebt het volgende:

* Een actief Azure-abonnement.
* Een CSV-bestand met sommige gegevens in het. U kunt de eerder vermelde uit bestand downloaden [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), of u kunt uw eigen bestand maken. Voor dit artikel nemen we aan dat u het bestand vanuit GitHub.

Op een hoog niveau voor het voltooien van de taken in dit artikel wordt uitgelegd doen u het volgende:

1. Maak een Azure storage-account en een blob storage-container en een CSV-indeling invoerbestand uploaden naar de container.
3. Een gevoel analytics-model van de Cortana Intelligence Gallery aan uw Azure Machine Learning-werkruimte toevoegen en dit model implementeren als een webservice in de Machine Learning-werkruimte.
5. Maken van een Stream Analytics-taak die deze webservice als een functie aanroept om te bepalen gevoel voor de tekst invoeren.
6. De Stream Analytics-taak starten en controleer de uitvoer.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Maken van een storage-container en upload het CSV-bestand voor invoer
Voor deze stap kunt u een CSV-bestand, zoals beschikbaar is via GitHub.

1. Klik in de Azure-portal op **nieuw** &gt; **opslag** &gt; **opslagaccount**.

   ![Nieuw opslagaccount maken](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. Geef een naam (`samldemo` in het voorbeeld). De naam kan alleen kleine letters en cijfers gebruiken en deze uniek zijn binnen Azure. 

3. Geef een bestaande resourcegroep en locatie opgeven. Locatie, wordt u aangeraden de resources die zijn gemaakt in deze zelfstudie gebruik van dezelfde locatie.

    ![Geef details op storage-account](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. Selecteer het opslagaccount in de Azure-portal. Klik in de blade opslagaccount op **Containers** en klik vervolgens op  **+ &nbsp;Container** blob-opslag maken.

    ![blob-container maken](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. Geef een naam voor de container (`azuresamldemoblob` in het voorbeeld) en controleer of **toegangstype** is ingesteld op **Blob**. Wanneer u klaar bent, klikt u op **OK**.

    ![Geef details van blob-container](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. In de **Containers** blade, selecteert u de nieuwe container, waarna de blade voor die container.

7. Klik op **Uploaden**.

    ![Knop voor een container uploaden](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. In de **blob uploaden** blade de CSV-bestand opgeeft dat u wilt gebruiken voor deze zelfstudie. Voor **type Blob**, selecteer **blok-blob** en grootte van het blok ingesteld op 4 MB, hetgeen voldoende is voor deze zelfstudie.

    ![blob-bestand uploaden](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. Klik op de **uploaden** knop aan de onderkant van de blade.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Het gevoel analytics-model van de Cortana Intelligence Gallery toevoegen

Nu dat de voorbeeldgegevens in een blob, kunt u het model van de analyse gevoel in Cortana Intelligence Gallery inschakelen.

1. Ga naar de [gevoel predictive analytics-model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) pagina in de Cortana Intelligence Gallery.  

2. Klik op **openen in Studio**.  
   
   ![Stream Analytics Machine Learning, open Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Meld u aan om te gaan naar de werkruimte. Selecteer een locatie.

4. Klik op **uitvoeren** aan de onderkant van de pagina. Het proces wordt uitgevoerd, wat duurt ongeveer een minuut.

   ![Voer experiment in Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. Nadat het proces met succes is uitgevoerd, selecteert u **webservice implementeren** aan de onderkant van de pagina.

   ![experiment in Machine Learning Studio implementeren als een webservice](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Om te valideren dat de gevoel analytics-model is gereed om te gebruiken, klikt u op de **Test** knop. Tekst invoeren, zoals 'Ik hou Microsoft' opgeven. 

   ![Test-experiment in Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Als de test werkt, ziet u een resultaat vergelijkbaar met het volgende voorbeeld:

   ![testresultaten in Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. In de **Apps** kolom, klikt u op de **Excel 2010 of ouder werkmap** koppelen aan een Excel-werkmap downloaden. De werkmap bevat de API-sleutel en de URL die u later nodig hebt voor het instellen van de Stream Analytics-taak.

    ![Stream Analytics, Machine Learning, snelle weergave](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Maken van een Stream Analytics-taak die gebruikmaakt van de Machine Learning-model

Nu kunt u een Stream Analytics-taak die de tweets voorbeeld van het CSV-bestand in blob-opslag wordt gelezen. 

### <a name="create-the-job"></a>De taak maken

1. Ga naar de [Azure Portal](https://portal.azure.com).  

2. Klik op **nieuw** > **Internet der dingen** > **Stream Analytics-taak**. 

   ![Azure portal pad voor het ophalen van een nieuwe Stream Analytics-taak](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. Naam van de taak `azure-sa-ml-demo`, Geef een abonnement op, Geef een bestaande resourcegroep of maak een nieuwe en selecteer de locatie voor de taak.

   ![instellingen opgeven voor nieuwe Stream Analytics-taak](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-the-job-input"></a>De invoer van de taak configureren
De taak wordt de invoer in het CSV-bestand dat u eerder hebt geüpload naar blob storage.

1. Nadat de taak is gemaakt, klikt u onder **taak topologie** in de taakblade klikt u op de **invoer** vak.  
   
   ![Het selectievakje 'Invoer' in de blade voor Stream Analytics-taak](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. In de **invoer** blade, klikt u op **+ toevoegen**.

   ![Knop voor het toevoegen van invoer voor de Stream Analytics-taak toevoegen](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. Vul de **nieuwe invoer** blade met deze waarden:

    * **Invoeralias**: Gebruik de naam `datainput`.
    * **Gegevensbrontype**: Selecteer **gegevensstroom**.
    * **Bron**: Selecteer **Blob storage**.
    * **Optie importeren**: Selecteer **gebruik blob storage uit het huidige abonnement**. 
    * **Storage-account**. Selecteer het opslagaccount dat u eerder hebt gemaakt.
    * **Container**. Selecteer de container die u eerder hebt gemaakt (`azuresamldemoblob`).
    * **Gebeurtenis serialisatie-indeling**. Selecteer **CSV**.

    ![Instellingen voor nieuwe taak invoer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. Klik op **Create**.

### <a name="configure-the-job-output"></a>De taakuitvoer van de configureren
De taak verzendt resultaten naar dezelfde blobopslag-waar deze invoer opgehaald. 

1. Onder **taak topologie** in de taakblade klikt u op de **uitvoer** vak.  
  
   ![Nieuwe uitvoer voor Streaming Analytics-taak maken](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. In de **uitvoer** blade, klikt u op **+ toevoegen**, en voeg vervolgens een uitvoer met de alias `datamloutput`. 

3. Voor **Sink**, selecteer **Blob storage**. Vul vervolgens de rest van de uitvoerinstellingen met de dezelfde waarden die u hebt gebruikt voor de blobopslag voor invoer:

    * **Storage-account**. Selecteer het opslagaccount dat u eerder hebt gemaakt.
    * **Container**. Selecteer de container die u eerder hebt gemaakt (`azuresamldemoblob`).
    * **Gebeurtenis serialisatie-indeling**. Selecteer **CSV**.

   ![Instellingen voor nieuwe taakuitvoer](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. Klik op **Create**.   


### <a name="add-the-machine-learning-function"></a>De Machine Learning-functie toevoegen 
U kunt eerder een Machine Learning-model gepubliceerd naar een webservice. In ons scenario wordt elke tweet voorbeeld verzonden wanneer de stroom Analysis-taak wordt uitgevoerd, uit de invoer met de webservice voor gevoel analyse. De Machine Learning-webservice retourneert een gevoel (`positive`, `neutral`, of `negative`) en een kans van de tweet wordt positief. 

In deze sectie van de zelfstudie definieert u een functie in de stroom Analysis-taak. De functie kan worden aangeroepen om een tweet wordt verzonden naar de webservice en krijgt u het antwoord terug. 

1. Zorg ervoor dat u hebt de web service-URL en API-sleutel die u eerder in de Excel-werkmap hebt gedownload.

2. Ga terug naar de blade taak overzicht.

3. Onder **instellingen**, selecteer **functies** en klik vervolgens op **+ toevoegen**.

   ![Een functie toevoegen aan Stream Analytics-taak](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. Voer `sentiment` als de functie-alias en vul de rest van de blade met deze waarden:

    * **Type functie**: Selecteer **Azure ML**.
    * **Optie importeren**: Selecteer **importeren uit een ander abonnement**. Dit kunt u de URL en de sleutel op te geven.
    * **URL**: plakken in de URL van de webservice.
    * **Sleutel**: plakken in de API-sleutel.
  
    ![Instellingen voor een Machine Learning-functie toe te voegen aan de Stream Analytics-taak](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. Klik op **Create**.

### <a name="create-a-query-to-transform-the-data"></a>Een query maken om de gegevens transformeren

Stream Analytics maakt gebruik van een declaratieve, op basis van SQL-query te onderzoeken van de invoer en te verwerken. In deze sectie kunt u een query waarmee elke tweet uit invoer gelezen en roept vervolgens de Machine Learning-functie voor analyse van de gevoel maken. De query verzendt vervolgens het resultaat aan de uitvoer die u hebt gedefinieerd (blobopslag).

1. Ga terug naar de blade taak overzicht.

2.  Onder **taak topologie**, klikt u op de **Query** vak.

    ![Een query voor Streaming Analytics-taak maken](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. Voer de volgende query:

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    De query roept de functie die u eerder hebt gemaakt (`sentiment`) om te kunnen gevoel analyses uitvoeren op elke tweet in de invoer. 

4. Klik op **opslaan** de query op te slaan.


## <a name="start-the-stream-analytics-job-and-check-the-output"></a>De Stream Analytics-taak starten en de uitvoer

U kunt nu de Stream Analytics-taak starten.

### <a name="start-the-job"></a>Start de taak
1. Ga terug naar de blade taak overzicht.

2. Klik op **Start** boven aan de blade.

    ![Een query voor Streaming Analytics-taak maken](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. In de **starttaak**, selecteer **aangepaste**, en selecteer vervolgens één dag voor wanneer u het CSV-bestand naar blob storage uploaden. Wanneer u bent klaar, klikt u op **Start**.  


### <a name="check-the-output"></a>Controleer de uitvoer
1. De taak uitvoeren voor een paar minuten totdat u de activiteit in ziet, kunnen de **bewaking** vak. 

2. Als u een hulpprogramma dat u gebruikt hebt om de inhoud van de blob-opslag bekijken, die hulpprogramma gebruiken om te onderzoeken de `azuresamldemoblob` container. U kunt ook de volgende stappen uit in de Azure portal doen:

    1. Zoek in de portal de `samldemo` storage-account en vinden in het account de `azuresamldemoblob` container. Ziet u twee bestanden in de container: het bestand met de voorbeeld-tweets en een CSV-bestand dat is gegenereerd door de Stream Analytics-taak.
    2. Met de rechtermuisknop op het gegenereerde bestand en selecteer vervolgens **downloaden**. 

   ![CSV-taakuitvoer downloaden uit Blob-opslag](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Open het gegenereerde CSV-bestand. U ziet ongeveer het volgende voorbeeld:  
   
   ![Stream Analytics, Machine Learning, CSV weergeven](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Metrische gegevens weergeven
U kunt ook Azure Machine Learning functie-gerelateerde metrische gegevens weergeven. De volgende functie-gerelateerde metrische gegevens worden weergegeven in de **bewaking** vak op de taakblade:

* **Functie aanvragen** geeft het aantal aanvragen dat is verzonden naar een Machine Learning-webservice.  
* **Gebeurtenissen werken** geeft het aantal gebeurtenissen in de aanvraag. Standaard bevat elke aanvraag met een Machine Learning-webservice maximaal 1000 gebeurtenissen.  


## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST-API integreren en Machine Learning](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)



