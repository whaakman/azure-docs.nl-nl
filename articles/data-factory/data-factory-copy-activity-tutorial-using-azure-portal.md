---
title: 'Zelfstudie: Een pijplijn maken met de kopieeractiviteit in Azure Portal | Microsoft Docs'
description: In deze zelfstudie maakt u een Azure Data Factory-pijplijn met een kopieeractiviteit. Hiervoor gebruikt u de Data Factory-editor in de Azure-portal.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 3205077236dd44253b3fa36d6eace36fb307871e
ms.openlocfilehash: 2fe52756ea5522e0d9d763afc1c89d45bf830877


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Zelfstudie: een pijplijn maken met de kopieeractiviteit in Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

In deze zelfstudie ziet u hoe u een Azure-gegevensfactory maakt en bewaakt met Azure Portal. In de pijplijn in de gegevensfactory wordt gebruikgemaakt van een kopieeractiviteit om gegevens van Azure-blobopslag te kopiëren naar Azure SQL Database.

Hier volgen de stappen die u uitvoert als onderdeel van deze zelfstudie:

| Stap | Beschrijving |
| --- | --- |
| [Een Azure Data Factory maken](#create-data-factory) |In deze stap maakt u een Azure-gegevensfactory met de naam **ADFTutorialDataFactory**. |
| [Gekoppelde services maken](#create-linked-services) |In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **AzureSqlLinkedService**. <br/><br/>Met de AzureStorageLinkedService wordt een Azure-opslag gekoppeld en met AzureSqlLinkedService wordt de Azure SQL Database gekoppeld aan ADFTutorialDataFactory. De invoergegevens van de pijplijn staan in een blobcontainer in Azure Blob Storage en de uitvoergegevens worden opgeslagen in een tabel in de Azure SQL Database. Daarom voegt u deze twee gegevensarchieven als gekoppelde services toe aan de gegevensfactory. |
| [Invoer- en uitvoergegevenssets maken](#create-datasets) |In de vorige stap hebt u gekoppelde services gemaakt die verwijzen naar gegevensarchieven die invoer- of uitvoergegevens bevatten. In deze stap definieert u twee gegevenssets, **InputDataset** en **OutputDataset**, die staan voor de invoer- en uitvoergegevens die zijn opgeslagen in de gegevensarchieven. <br/><br/>In InputDataset geeft u op welke blobcontainer een blob bevat met de brongegevens en voor OutputDataset geeft u op in welke SQL-tabel de uitvoergegevens worden opgeslagen. U geeft ook andere eigenschappen op, zoals de structuur, de beschikbaarheid en het beleid. |
| [Een pijplijn maken](#create-pipeline) |In deze stap maakt u een pijplijn met de naam **ADFTutorialPipeline** in ADFTutorialDataFactory. <br/><br/>U voegt een **kopieeractiviteit** toe aan de pijplijn waarmee invoergegevens van de Azure-blob naar de uitvoer-Azure SQL-tabel worden gekopieerd. Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit. |
| [De pijplijn bewaken](#monitor-pipeline) |In deze stap bewaakt u segmenten van de invoer- en uitvoertabellen middels de Azure-portal. |

## <a name="prerequisites"></a>Vereisten
U dient eerst te voldoen aan de vereisten in het artikel [Overzicht van de zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voordat u deze zelfstudie volgt.

## <a name="create-data-factory"></a>Een gegevensfactory maken
In deze stap gebruikt u Azure Portal om een Azure Data Factory met de naam **ADFTutorialDataFactory** te maken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en klik op **Nieuw**. Selecteer **Intelligence en analytische gegevens** en klik op **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. In de blade **Nieuwe gegevensfactory**:
   
   1. Voer **ADFTutorialDataFactory** in als **naam**. 
      
         ![Blade voor een nieuwe gegevensfactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Naam van gegevensfactory niet beschikbaar](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Selecteer uw Azure-**abonnement**.
   3. Voer een van de volgende stappen uit voor de resourcegroep:
      
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
          Voor sommige van de stappen in deze zelfstudie wordt ervan uitgegaan dat u voor de resourcegroep de naam **ADFTutorialResourceGroup** gebruikt. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
   4. Selecteer de **locatie** voor de gegevensfactory. Alleen regio's die worden ondersteund door de Data Factory-service worden weergegeven in de vervolgkeuzelijst.
   5. Selecteer **vastmaken aan Startboard**.     
   6. Klik op **Create**.
      
      > [!IMPORTANT]
      > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepsniveau.
      > 
      > De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.                
      > 
      > 
3. Klik op het belpictogram op de werkbalk om de statusmeldingen/berichten te zien. 
   
   ![Meldingen](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
4. Wanneer het maken is voltooid, ziet u de blade **Data Factory** zoals in de afbeelding is weergegeven.
   
   ![Startpagina van de gegevensfactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. Zie [Ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor alle bronnen en sinks die worden ondersteund door de kopieerbewerking. Zie [Gekoppelde services berekenen](data-factory-compute-linked-services.md) voor de lijst met compute-services die worden ondersteund door Data Factory. In deze zelfstudie gebruikt u geen compute-service. 

In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **AzureSqlLinkedService**. Met de gekoppelde AzureStorageLinkedService-service wordt een Azure-opslagaccount gekoppeld en met AzureSqlLinkedService wordt een Azure SQL Database gekoppeld aan **ADFTutorialDataFactory**. Later in deze zelfstudie maakt u een pijplijn waarmee gegevens worden gekopieerd van een blobcontainer in AzureStorageLinkedService naar een SQL-tabel in AzureSqlLinkedService.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Een gekoppelde service maken voor het Azure-opslagaccount
1. In de blade **Gegevensfactory** klikt u op de tegel **Maken en implementeren** om de **editor** van de gegevensfactory te openen.
   
   ![Tegel Ontwerpen en implementeren](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. In de **editor**, klikt u op **Nieuw gegevensarchief** op de werkbalk en selecteert u **Azure Storage** uit de vervolgkeuzelijst. U ziet het JSON-sjabloon voor het maken van een gekoppelde Azure-service in het rechterdeelvenster. 
   
    ![Knop Nieuw gegevensarchief in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Vervang `<accountname>` en `<accountkey>` door de naam van uw account en de accountsleutel van uw Azure-opslagaccount. 
   
    ![JSON voor Blob Storage in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Klik op **Implementeren** op de werkbalk. De geïmplementeerde **AzureStorageLinkedService** wordt nu in de structuurweergave weergeven. 
   
    ![Blob Storage implementeren in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure-blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over JSON-eigenschappen.
> 
> 

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Een gekoppelde service maken voor de Azure SQL Database
1. In de **Data Factory-editor** klikt u op **Nieuw gegevensarchief** op de werkbalk en selecteert u **Azure SQL Database** uit de vervolgkeuzelijst. U ziet het JSON-sjabloon voor het maken van een gekoppelde Azure SQL-service in het rechterdeelvenster.
2. Vervang `<servername>`, `<databasename>`, `<username>@<servername>` en `<password>` door de namen van uw Azure SQL-server, -database en -gebruikersaccount en voer uw wachtwoord in. 
3. Klik op **Implementeren** op de werkbalk om **AzureSqlLinkedService** te maken en te implementeren.
4. Bevestig dat **AzureSqlLinkedService** in de structuurweergave wordt weergegeven. 

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap hebt u de gekoppelde services **AzureStorageLinkedService** en **AzureSqlLinkedService** gemaakt om een Azure-opslagaccount en een Azure SQL Database te koppelen aan de gegevensfactory **ADFTutorialDataFactory**. In deze stap definieert u twee gegevenssets, **InputDataset** en **OutputDataset**, die staan voor de invoer- en uitvoergegevens die zijn opgeslagen in de gegevensarchieven waarnaar wordt verwezen door respectievelijk AzureStorageLinkedService en AzureSqlLinkedService. In InputDataset geeft u op welke blobcontainer een blob bevat met de brongegevens en voor OutputDataset geeft u op in welke SQL-tabel de uitvoergegevens worden opgeslagen. 

### <a name="create-input-dataset"></a>Invoergegevensset maken
In deze stap maakt u een gegevensset met de naam **InputDataset** die verwijst naar een blobcontainer in Azure Storage. Deze container wordt vertegenwoordigd door de gekoppelde **AzureStorageLinkedService**-service.

1. In de **Editor** voor de Data Factory, klikt u op **... Meer**, **Nieuwe gegevensset** en vervolgens op **Azure Blob-opslag** in de vervolgkeuzelijst. 
   
    ![Nieuw gegevenssetmenu](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment: 
   
    ```JSON
    {
      "name": "InputDataset",
      "properties": {
        "structure": [
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```   
    Houd rekening met de volgende punten: 
   
   * De gegevensset **type** wordt ingesteld op **AzureBlob**.
   * **linkedServiceName** wordt ingesteld op **AzureStorageLinkedService**. U hebt deze gekoppelde service gemaakt in stap 2.
   * **folderPath** wordt ingesteld op de container **adftutorial**. U kunt ook de naam van een blob in de map opgeven met behulp van de **fileName**-eigenschap. Omdat u de naam van de blob niet opgeeft, worden de gegevens uit alle blobs in de container gezien als invoergegevens.  
   * De indeling **type** wordt ingesteld op **TextFormat**
   * Er zijn twee velden in het tekstbestand: **FirstName** en **LastName**, gescheiden door een kommateken (**columnDelimiter**)    
   * De **beschikbaarheid** wordt ingesteld op **elk uur** (de **frequentie** wordt ingesteld op elk **uur** en het **interval** wordt ingesteld op **1**). Daarom zoekt Data Factory elk uur naar invoergegevens in de hoofdmap van de opgegeven blobcontainer (**adftutorial**). 
     
     Als u geen **fileName** opgeeft voor een **invoer**gegevensset, worden alle bestanden/blobs uit de invoermap (**folderPath**) beschouwd als invoer. Als u een fileName opgeeft in de JSON, wordt alleen het opgegeven bestand/de opgegeven blob gezien als invoer.
     
     Als u geen **fileName** opgeeft voor een **uitvoertabel**, krijgen de bestanden die worden gegenereerd in **folderPath** een naam op basis van de volgende indeling: Data.&lt;Guid\&gt;.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).
     
     Als u **folderPath** en **fileName** dynamisch wilt instellen op basis van de **SliceStart**-tijd, gebruikt u de eigenschap **partitionedBy**. In het volgende voorbeeld worden voor folderPath Year, Month en Day gebruikt van de SliceStart-waarde (tijd waarop is begonnen met het verwerken van het segment). Voor fileName wordt gebruikgemaakt van Hour van de SliceStart-waarde. Als er bijvoorbeeld een segment wordt geproduceerd voor 2016-09-20T08:00:00, wordt folderName ingesteld op wikidatagateway/wikisampledataout/2016/09/20 en wordt fileName ingesteld op 08.csv. 

    ```JSON     
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
       { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
       { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
       { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
       { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],
    ```
3. Klik op **Implementeren** op de werkbalk om de tabel **InputDataset** te implementeren. Bevestig dat **InputDataset** in de structuurweergave wordt weergegeven.

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure-blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

### <a name="create-output-dataset"></a>Uitvoergegevensset maken
In dit deel van de stap maakt u een uitvoergegevensset met de naam **OutputDataset**. Deze gegevensset wijst naar een SQL-tabel in de Azure SQL-database die wordt vertegenwoordigd door **AzureSqlLinkedService**. 

1. In de **Editor** voor de Data Factory, klikt u op **... Meer**, **Nieuwe gegevensset** en vervolgens op **Azure SQL** in de vervolgkeuzelijst. 
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment:

    ```JSON   
    {
      "name": "OutputDataset",
      "properties": {
        "structure": [
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "emp"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```     
    Houd rekening met de volgende punten: 
   
   * De gegevensset **type** wordt ingesteld op **AzureSQLTable**.
   * **linkedServiceName** wordt ingesteld op **AzureSqlLinkedService** (u hebt deze gekoppelde service gemaakt in stap 2).
   * **tablename** wordt ingesteld op **emp**.
   * De tabel emp in de database bevat drie kolommen: **ID**, **FirstName** en **LastName**. ID is een identiteitskolom, zodat u alleen **FirstName** en **LastName** hoeft op te geven.
   * De **beschikbaarheid** wordt ingesteld op **elk uur** (de **frequentie** wordt ingesteld op **elk uur** en het **interval** wordt ingesteld op **1**).  De Data Factory-service maakt elk uur een uitvoergegevenssegment in de tabel **emp** in de Azure SQL-database.
3. Klik op **Implementeren** op de werkbalk om de tabel **OutputDataset** te implementeren. Bevestig dat **OutputDataset** in de structuurweergave wordt weergegeven. 

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

## <a name="create-pipeline"></a>Pijplijn maken
In deze stap maakt u een pijplijn met een **kopieeractiviteit** die gebruikmaakt van **InputDataset** als invoer en **OutputDataset** als uitvoer.

1. In de **Editor** voor de Data Factory, klikt u op **... Meer** en vervolgens op **Nieuwe pijplijn**. U kunt ook met de rechtermuisknop op **Pijplijnen** klikken in de boomstructuur vervolgens klikken op **Nieuwe pijplijn**.
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment: 

    ```JSON   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 
    ```   
    
    Houd rekening met de volgende punten:
   
   * In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**.
   * De invoer voor de activiteit is ingesteld op **InputDataset** en de uitvoer voor de activiteit is ingesteld op **OutputDataset**.
   * In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type.
     
     Vervang de waarde van de eigenschap **start** door de huidige dag en de waarde **end** door de volgende dag. U hoeft alleen de datum in te vullen en kunt de tijd overslaan. Dit wordt dan bijvoorbeeld 2016-02-03, wat gelijk staat aan 2016-02-03T00:00:00Z
     
     Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2016-10-14T16:32:41Z. De **eindtijd** is optioneel, maar we gebruiken hem in deze zelfstudie. 
     
     Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**.
     
     In het voorgaande voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.
3. Klik op **Implementeren** op de werkbalk om de tabel **ADFTutorialPipeline** te implementeren. Controleer of de pijplijn in de structuurweergave wordt weergegeven. 
4. Sluit nu de blade **Editor** door op **X** te klikken. Klik opnieuw op **X** om de **Data Factory**-startpagina te zien voor de **ADFTutorialDataFactory**.

**Gefeliciteerd!** U hebt een Azure-gegevensfactory, gekoppelde services, tabellen en een pijplijn gemaakt en u hebt een planning ingesteld voor de pijplijn.   

### <a name="view-the-data-factory-in-a-diagram-view"></a>De gegevensfactory weergeven in een diagramweergave
1. In de blade **Gegevensfactory** klikt u op **Diagram**.
   
    ![Blade Gegevensfactory - tegel Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. U ziet een diagram dat lijkt op de volgende afbeelding: 
   
    ![Diagramweergave](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)
   
    U kunt inzoomen, uitzoomen, zoomen naar 100%, passend maken, pijplijnen en tabellen automatisch positioneren en de afkomst weergeven (upstream- en downstreamitems van geselecteerde items worden gemarkeerd).  U kunt dubbelklikken op een object (invoer-/uitvoertabel of pijplijn) om de eigenschappen ervan te bekijken. 
3. Klik met de rechtermuisknop op **ADFTutorialPipeline** in de diagramweergave en klik op **Pijplijn openen**. 
   
    ![Pijplijn openen](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. U ziet hier de activiteiten in de pijplijn in combinatie met de invoer- en uitvoergegevenssets van de activiteiten. In deze zelfstudie hebt u slechts één activiteit in de pijplijn (kopieeractiviteit), met InputDataset als invoergegevensset en OutputDataset als uitvoergegevensset.   
   
    ![Geopende pijplijnweergave](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Klik op **Gegevensfactory** in het koppelingenmenu linksboven om terug te keren naar de diagramweergave. In de diagramweergave worden alle pijplijnen weergegeven. In dit voorbeeld hebt u slechts één pijplijn gemaakt.   

## <a name="monitor-pipeline"></a>De pijplijn bewaken
In deze stap gebruikt u de Azure-portal om te controleren wat er gebeurt in een Azure data factory. 

### <a name="monitor-pipeline-using-diagram-view"></a>De pijplijn bewaken met Diagramweergave
1. Klik op **X** om de **Diagram**-weergave te sluiten en de Data Factory-startpagina voor de gegevensfactory weer te geven . Als u de webbrowser hebt gesloten, voert u de volgende stappen uit: 
   1. Ga naar de [Azure-portal](https://portal.azure.com/). 
   2. Dubbelklik op **ADFTutorialDataFactory** op de **Startboard** (of) klik op **Gegevensfactory's** in het menu aan de linkerkant en zoek naar ADFTutorialDataFactory. 
2. U ziet het aantal tabellen en de namen van de tabellen en pijplijn die u op deze blade hebt gemaakt.
   
    ![startpagina met namen](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
3. Klik nu op de tegel **Gegevenssets**.
4. In de blade **Gegevenssets** klikt u op **InputDataset**. Deze gegevensset is de invoergegevensset voor **ADFTutorialPipeline**.
   
    ![Gegevenssets waarvoor InputDataset is geselecteerd](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Klik op **... (ellips)** om alle gegevenssegmenten te bekijken.
   
    ![Alle invoergegevenssegmenten](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    De gegevenssegmenten tot aan de huidige tijd hebben de status **Gereed** omdat het bestand **emp.txt** aanwezig is in de blobcontainer **adftutorial\input**. Controleer of er geen segmenten worden weergegeven in het gedeelte **Recent mislukte segmenten** onderaan.
   
    Zowel **Onlangs bijgewerkt segmenten** als **Onlangs mislukte segmenten** worden gesorteerd op basis van de **TIJD VAN DE LAATSTE UPDATE**. 
   
    Klik op **Filter** op de werkbalk om de segmenten te filteren.  
   
    ![Invoersegmenten filteren](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Sluit de blades totdat u de blade **Gegevenssets** ziet. Klik op de **OutputDataset**. Deze gegevensset is de uitvoergegevensset voor **ADFTutorialPipeline**.
   
    ![blade gegevenssets](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
7. De blade **OutputDataset** wordt weergegeven zoals in de volgende afbeelding:
   
    ![blade tabel](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
8. De gegevenssegmenten tot nu zijn al gemaakt en hebben de status **Gereed**. Er worden geen segmenten weergegeven in het gedeelte **Probleemsegmenten** onderaan.
9. Klik op **... (Ellips)** om alle segmenten te bekijken.
   
    ![blade gegevenssegmenten](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
10. Als u op een gegevenssegment uit de lijst klikt, ziet u de blade **Gegevenssegment**.
    
     ![blade Gegevenssegment](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Als het segment niet de status **Gereed** heeft, kunt u de upstreamsegmenten bekijken die niet de status Gereed hebben en die voorkomen dat de huidige status wordt uitgevoerd. U ziet deze segmenten in de lijst **Upstreamsegmenten die niet gereed zijn**.
11. In de blade **GEGEVENSSEGMENT** ziet u in de lijst onderaan alle activiteiten die worden uitgevoerd. Klik op een **activiteit die wordt uitgevoerd** om de blade **Details uitvoering van activiteit** weer te geven. 
    
    ![Details uitvoering van activiteit](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Klik op **X** om alle blades te sluiten tot u weer op de startblade bent voor **ADFTutorialDataFactory**.
13. (optioneel) Klik op **Pijplijnen** op de startpagina van **ADFTutorialDataFactory**, klik op **ADFTutorialPipeline** in de blade **Pijplijnen** en open een detailanalyse van invoertabellen (**Verbruikt**) of uitvoertabellen (**Geproduceerd**).
14. Open **SQL Server Management Studio**, maak verbinding maken met de Azure SQL Database en controleer of de rijen zijn ingevoegd in de tabel **emp** in de database.
    
    ![SQL-queryresultaten](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor-manage-app"></a>De pijplijn bewaken met de app Bewaking en beheer
U kunt de toepassing Bewaking en beheer ook gebruiken om uw pijplijnen te bewaken. Zie [Azure Data Factory-pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van deze toepassing.

1. Klik op de tegel **Bewaking en beheer** op de startpagina van uw gegevensfactory.
   
    ![De tegel Bewaking en beheer](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. De toepassing **Bewaking en beheer** wordt weergegeven. Wijzig de **Begintijd** en de **Eindtijd** om de starttijd (2016-07-12) en de eindtijd (2016-07-13) van uw pijplijn op te geven. Klik vervolgens op **Toepassen**. 
   
    ![De app Bewaking en beheer](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Selecteer een activiteitsvenster in de lijst **Activiteitsvensters** voor details van dit venster. 
    ![Details van activiteitsvenster](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u een Azure-gegevensfactory gemaakt om gegevens te kopiëren van een Azure-blob naar een Azure SQL-database. U gebruikt de Azure-portal om de data factory, gekoppelde services, datasets en een pijplijn te maken. Hier volgen de hoofdstappen die u in deze zelfstudie hebt uitgevoerd:  

1. U hebt een Azure-**gegevensfactory** gemaakt.
2. U hebt **gekoppelde services** gemaakt:
   1. Een gekoppelde **Azure Storage**-service om uw Azure-opslagaccount te koppelen dat invoergegevens bevat.     
   2. Een gekoppelde **Azure SQL**-service om uw Azure SQL Database te koppelen die uitvoergegevens bevat. 
3. U hebt **gegevenssets** gemaakt waarin de invoer- en uitvoergegevens van pijplijnen worden beschreven.
4. U hebt een **pijplijn** gemaakt met een **kopieeractiviteit** met **BlobSource** als bron en **SqlSink** als sink.  

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
|:--- |:--- |
| [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) |Dit artikel biedt gedetailleerde informatie over de kopieeractiviteit die u tijdens deze zelfstudie hebt gemaakt. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) |In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |
| [Pijplijnen](data-factory-create-pipelines.md) |In dit artikel worden pijplijnen en activiteiten in Azure Data Factory nader uitgelegd. |
| [Gegevenssets](data-factory-create-datasets.md) |Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory. |
| [Pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) |In dit artikel wordt beschreven hoe u pijplijnen bewaakt en beheert en hoe u fouten hierin oplost met de app voor bewaking en beheer. |




<!--HONumber=Dec16_HO1-->


