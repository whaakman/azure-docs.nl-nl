---
title: Een Azure-gegevensfactory maken met de Azure Data Factory-UI | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een gegevensfactory kunt maken met een pijplijn waarmee gegevens vanuit de ene map naar een andere map in Azure Blob Storage worden gekopieerd.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: ebce4e0d137d2e56cc914efad357593af7abb255
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-data-factory-using-the-azure-data-factory-ui"></a>Een gegevensfactory maken met de Azure Data Factory-UI
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-portal.md)

In deze quickstart wordt beschreven hoe u de Azure Data Factory-UI kunt gebruiken om een Azure-gegevensfactory te maken en te controleren. Met de pijplijn die u in deze data factory maakt, worden gegevens **gekopieerd** van één map naar een andere map in een Azure Blob Storage. Zie [Zelfstudie: Gegevens transformeren met Spark](tutorial-transform-data-spark-portal.md) voor meer informatie over het **transformeren** van gegevens met Azure Data Factory. 


> [!NOTE]
> Als u niet bekend bent met Azure Data Factory, raadpleegt u eerst de [Inleiding voor Azure Data Factory](data-factory-introduction.md) voordat u deze quickstart uitvoert. 
>
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van Data Factory gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Data Factory versie 1 - zelfstudie](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Bekijk deze video voor een beter begrip van de Data Factory-UI: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Navigeer naar [Azure Portal](https://portal.azure.com). 
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe gegevensfactory** **ADFTutorialDataFactory** in als de **naam**. 
      
     ![Pagina Nieuwe gegevensfactory](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![Naam niet beschikbaar - fout](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst ziet u alleen locaties die worden ondersteund in Data Factory. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich op andere locaties bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. Wanneer de gegevensfactory is gemaakt, ziet u de pagina **Gegevensfactory** zoals wordt weergegeven in de afbeelding.
   
    ![Startpagina van de gegevensfactory](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. Klik op de tegel **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 
11. Ga op de pagina Aan de slag naar het tabblad **Bewerken** in het linkerpaneel, zoals wordt weergegeven in de volgende afbeelding: 

    ![Pagina Aan de slag](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
In deze stap maakt u een gekoppelde service om uw Azure Storage-account te koppelen aan de gegevensfactory. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service tijdens runtime gebruikt om er een verbinding mee tot stand te brengen.

2. Klik op **Verbindingen** en klik vervolgens in de werkbalk op de knop **Nieuw**. 

    ![Nieuwe verbinding](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
3. Selecteer op de pagina **Nieuwe gekoppelde service** de optie **Azure Blob-opslag** en klik op **Doorgaan**. 

    ![Azure Storage selecteren](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
4. Voer op de pagina **Nieuwe gekoppelde service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** als de **Naam**.
    2. Selecteer uw Azure-opslagaccount als **Naam van opslagaccount**.
    3. Klik op **Verbinding testen** om te bevestigen dat de Data Factory-service verbinding mag maken met het opslagaccount. 
    4. Klik op **Opslaan** om de gekoppelde service op te slaan. 

        ![Instellingen voor de gekoppelde Azure Storage-service](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
5. Controleer of **AzureStorageLinkedService** wordt weergegeven in de lijst met gekoppelde services. 

    ![Gekoppelde Azure Storage-service in de lijst](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u twee gegevenssets: **InputDataset** en **OutputDataset**. Deze gegevenssets zijn van het type **AzureBlob**. Ze verwijzen naar de **gekoppelde Azure Storage-service** die u in de vorige stap hebt gemaakt. 

De invoergegevensset vertegenwoordigt de brongegevens in de invoermap. In de definitie van de invoergegevensset geeft u de blob-container (**adftutorial**), en de map (**invoer**) op, en het bestand (**emp.txt**) dat de brongegevens bevat. 

De uitvoergegevensset vertegenwoordigt de gegevens die worden gekopieerd naar de bestemming. In de definitie van de uitvoergegevensset geeft u de blob-container (**adftutorial**), en de map (**uitvoer**) op, en het bestand waarin de brongegevens zijn gekopieerd. Aan elke uitvoering van een pijplijn is een unieke id gekoppeld, die kan worden verkregen met behulp van de systeemvariabele **RunId**. De naam van het uitvoerbestand wordt dynamisch geëvalueerd op basis van de run-id van de pijplijn.   

In de instellingen voor de gekoppelde service hebt u het Azure Storage-account opgegeven dat de brongegevens bevat. In de instellingen voor de brongegevensset geeft u de precieze locatie van de brongegevens op (blob-container, map en bestand). In de instellingen voor de sink-gegevensset geeft u de locatie op waarnaar de gegevens worden gekopieerd (blob-container, map en bestand). 
 
1. Klik op de **plusknop (+)** en selecteer **Gegevensset**.

    ![Nieuw gegevenssetmenu](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. Selecteer op de pagina **Nieuwe gegevensset** de optie **Azure Blob-opslag** en klik op **Voltooien**. 

    ![Azure Blob-opslag selecteren](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. Voer in het venster **Eigenschappen** voor de gegevensset **InputDataset** in als **Naam**. 

    ![Algemene instellingen voor gegevenssets](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. Ga naar het tabblad **Verbinding** en voer de volgende stappen uit: 

    1. Selecteer **AzureStorageLinkedService** voor de gekoppelde service. 
    2. Klik op de knop **Bladeren** voor het **Bestandspad**. 
        ![Naar het invoerbestand bladeren](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
    3. Ga in het venster **Een bestand of map kiezen** naar de map **invoer** in de container **adftutorial**, selecteer het bestand **emp.txt** en klik op **Voltooien**.

        ![Naar het invoerbestand bladeren](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    4. (optioneel) Klik op **Gegevens vooraf bekijken** om een voorbeeld van de gegevens in het bestand emp.txt te bekijken.     
5. Herhaal de stappen om de uitvoergegevensset te maken.  

    1. Klik in het linkerdeelvenster op de **plusknop (+)** en selecteer **Gegevensset**.
    2. Selecteer op de pagina **Nieuwe gegevensset** de optie **Azure Blob-opslag** en klik op **Voltooien**.
    3. Geef **OutputDataset** op als de naam.
    4. Voer **adftutorial/output** in als de map. Met de kopieeractiviteit maakt u de uitvoermap als deze nog niet bestaat. 
    5. Voer `@CONCAT(pipeline().RunId, '.txt')` in als bestandsnaam. Telkens wanneer u een pijplijn uitvoert, heeft de pijplijnuitvoering een unieke gekoppelde id. Via de expressie wordt de run-id van de pijplijn samengevoegd met **.txt** om de naam van het uitvoerbestand te evalueren. Zie [Systeemvariabelen](control-flow-system-variables.md) en [Expressietaal](control-flow-expression-language-functions.md) voor de lijst met ondersteunde systeemvariabelen en expressies.

        ![Instellingen voor de uitvoergegevensset](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## <a name="create-a-pipeline"></a>Een pijplijn maken 
In deze stap maakt en valideert u een pijplijn via een **kopieeractiviteit** die gebruikmaakt van de invoer- en uitvoergegevenssets. Met de kopieeractiviteit kopieert u gegevens uit het bestand dat is opgegeven bij de instellingen voor de invoergegevensset, naar het bestand dat is opgegeven in de instellingen voor de uitvoergegevensset. Als in de invoergegevensset alleen een map is opgegeven (en geen bestandsnaam), worden met de kopieeractiviteit alle bestanden in de bronmap gekopieerd naar de bestemming. 

1. Klik op de **plusknop (+)** en selecteer **Pijplijn**. 

    ![Menu Nieuwe pijplijn](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. Geef **CopyPipeline** op als **Naam** in het venster **Eigenschappen**. 

    ![Algemene instellingen voor pijplijnen](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. Vouw in de **Activiteiten**-werkset de optie **Gegevensstroom** uit. Gebruik vervolgens slepen-en-neerzetten om de **kopieeractiviteit** uit de werkset **Activiteiten** te verplaatsen naar het ontwerpoppervlak voor pijplijnen. U kunt ook zoeken naar activiteiten in de werkset **Activiteiten**. Geef **CopyFromBlobToBlob** op als de **naam**.

    ![Algemene instellingen voor de kopieeractiviteit](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. Ga naar het tabblad **Bron** in de instellingen voor de kopieeractiviteit en selecteer **InputDataset** als de **brongegevensset**.

    ![Broninstellingen voor de kopieeractiviteit](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. Ga naar het tabblad **Sink** in de instellingen voor de kopieeractiviteit en selecteer **OutputDataset** als de **sink-gegevensset**.

    ![Sink-instellingen voor de kopieeractiviteit](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. Klik op **Valideren** om de instellingen voor de pijplijn te valideren. Controleer of de pijplijn is gevalideerd. Klik op de **pijl-rechts** (>>) om de uitvoergegevens van de validatie te sluiten. 

    ![De pijplijn valideren](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## <a name="test-run-the-pipeline"></a>De uitvoering van de pijplijn testen
In deze stap test u de uitvoering van de pijplijn voordat u deze implementeert in Data Factory. 

1. Klik in de werkbalk voor de pijplijn op **Testuitvoering**. 
    
    ![Testuitvoeringen voor pijplijnen](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. Controleer of de status van de pijplijnuitvoering wordt weergegeven op het tabblad **Uitvoer** van de pijplijninstellingen. 

    ![Uitvoer testen](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. Controleer of een uitvoerbestand wordt weergegeven in de **uitvoermap** van de container **adftutorial**. Als de uitvoermap niet bestaat, wordt deze automatisch gemaakt in de Data Factory-service. 
    
    ![Uitvoer controleren](./media/quickstart-create-data-factory-portal/verify-output.png)


## <a name="trigger-the-pipeline-manually"></a>De pijplijn handmatig activeren
In deze stap implementeert u entiteiten (gekoppelde services, gegevenssets, pijplijnen) in Azure Data Factory. Vervolgens activeert u handmatig een pijplijnuitvoering. U kunt entiteiten ook publiceren in uw eigen VSTS GIT-opslagplaats. Dit wordt behandeld in [een andere zelfstudie](tutorial-copy-data-portal.md?#configure-code-repository).

1. Voordat u een pijplijn activeert, moet u eerst entiteiten publiceren in Data Factory. Klik hiervoor in het linkerdeelvenster op **Publiceren**. 

    ![De knop Publiceren](./media/quickstart-create-data-factory-portal/publish-button.png)
2. Als u de pijplijn handmatig wilt activeren, klikt u in de werkbalk op **Activeren** en selecteert u **Nu activeren**. 
    
    ![Nu activeren](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## <a name="monitor-the-pipeline"></a>De pijplijn controleren

1. Ga naar het tabblad **Controleren** aan de linkerkant. Gebruik de knop **Vernieuwen** om de lijst te vernieuwen.

    ![De pijplijn controleren](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. Klik op de koppeling **Uitvoeringen van activiteit weergeven** onder **Acties**. Op deze pagina ziet u de status van de uitvoering van de kopieeractiviteit. 

    ![Uitvoeringen van de pijplijnactiviteit](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. Klik op de koppeling **Details** (afbeelding van een bril) in de kolom **Acties** om details over de kopieerbewerking weer te geven. Zie [Overzicht van kopieeractiviteit](copy-activity-overview.md) voor meer informatie over de eigenschappen. 

    ![Details van de kopieerbewerking](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. Controleer of een nieuw bestand wordt weergegeven in de **uitvoermap**. 
5. Als u vanuit de weergave **Uitvoeringen van activiteit** wilt terugkeren naar de weergave **Pijplijnuitvoeringen**, klikt u op de koppeling **Pijplijnen**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>De pijplijn activeren volgens een schema
Deze stap is optioneel in deze zelfstudie. U kunt een **Scheduler-trigger** maken om in te plannen dat de pijplijn periodiek wordt uitgevoerd (elk uur, dagelijks, enzovoort). In deze stap maakt u een trigger die elke minuut wordt uitgevoerd tot de datum/tijd die u opgeeft als de einddatum. 

1. Ga naar het tabblad **Bewerken**. 

    ![Naar het tabblad Bewerken gaan](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. Klik in het menu op **Activeren** en klik op **Nieuw/bewerken**. 

    ![Menu Nieuwe trigger](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. Klik op de pagina **Triggers toevoegen** op **Trigger kiezen...** , en klik op **Nieuw**. 

    ![Triggers toevoegen - nieuwe trigger](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. Selecteer op de pagina **Nieuwe trigger**, in het veld **Beëindigen**, de optie **Op datum**. Geef een eindtijd op die enkele minuten later is dan de huidige tijd, en klik op **Toepassen**. Aan elke pijplijnuitvoering zijn kosten verbonden. Geef daarom een eindtijd op die slechts enkele minuten later is dan de begintijd. Zorg ervoor dat de eindtijd op dezelfde dag is. Zorg er echter wel voor dat er voldoende tijd is om de pijplijn uit te voeren tussen de publicatietijd en de eindtijd. De trigger gaat pas van kracht nadat u de oplossing hebt gepubliceerd in Data Factory, niet wanneer u de trigger opslaat in de UI. 

    ![Triggerinstellingen](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. Controleer de optie **Geactiveerd** op de pagina **Nieuwe trigger** en klik op **Volgende** 

    ![Triggerinstellingen - knop Volgende](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. Lees op de pagina **Nieuwe trigger** de waarschuwing en klik op **Voltooien**.

    ![Triggerinstellingen - knop Voltooien](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Klik op **Publiceren** om wijzigingen te publiceren in Data Factory. 

    ![De knop Publiceren](./media/quickstart-create-data-factory-portal/publish-2.png)
8. Ga naar het tabblad **Controleren** aan de linkerkant. Klik op **Vernieuwen** om de lijst te vernieuwen. U ziet dat de pijplijn één keer per minuut wordt uitgevoerd vanaf het moment van publiceren tot de eindtijd. Bekijk de waarden in de kolom **Geactiveerd door**. De handmatige triggeruitvoering is afkomstig uit de stap (**Nu activeren**) die u eerder hebt uitgevoerd. 

    ![Geactiveerde uitvoeringen controleren](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. Klik op de pijl-omlaag naast **Pijplijnuitvoeringen** om naar de weergave **Triggeruitvoeringen** te gaan. 

    ![Triggeruitvoeringen controleren](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. Controleer of er tot de opgegeven datum en tijd voor elke pijplijnuitvoering een **uitvoerbestand** is gemaakt in de **uitvoermap**. 

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-portal.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 