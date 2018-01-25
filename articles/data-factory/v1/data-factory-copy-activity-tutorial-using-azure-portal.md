---
title: "Zelfstudie: een Azure Data Factory-pijplijn maken om gegevens te kopiëren (Azure Portal) | Microsoft Docs"
description: "In deze zelfstudie gebruikt u Azure Portal om een Azure Data Factory-pijplijn te maken met een kopieeractiviteit om gegevens uit een Azure-blobopslag naar een Azure SQL-database te kopiëren."
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
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 6b22e06cc569ff58f4e988a014d39c90245bf260
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-use-azure-portal-to-create-a-data-factory-pipeline-to-copy-data"></a>Zelfstudie: Azure Portal gebruiken voor het maken van een Data Factory-pijplijn om gegevens te kopiëren 
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

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, welke nog een preview is, raadpleegt u de [zelfstudie kopieeractiviteit in de documentatie voor versie 2](../quickstart-create-data-factory-dot-net.md). 

In dit artikel leert u hoe u [Azure Portal](https://portal.azure.com) kunt gebruiken om een gegevensfactory te maken met een pijplijn waarmee gegevens worden gekopieerd van een Azure blobopslag naar een Azure SQL-database. Als u niet bekend bent met Azure Data Factory, lees dan het artikel [Inleiding tot Azure Data Factory](data-factory-introduction.md) voordat u deze zelfstudie volgt.   

In deze zelfstudie maakt u een pijplijn met één activiteit erin: kopieeractiviteit. De kopieeractiviteit in Data Factory kopieert gegevens uit een ondersteund gegevensarchief naar een ondersteund sinkgegevensarchief. Zie [Ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor een lijst met gegevensarchieven die worden ondersteund als bron en als sink. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over kopieeractiviteiten.

Een pijplijn kan meer dan één activiteit hebben. Ook kunt u twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Meerdere activiteiten in een pijplijn](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) voor meer informatie. 

> [!NOTE] 
> In de gegevenspijplijn in deze zelfstudie worden gegevens van een brongegevensarchief gekopieerd naar een doelgegevensarchief. Zie [Zelfstudie: een pijplijn maken om gegevens te transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) voor meer informatie over het transformeren van gegevens met Azure Data Factory.

## <a name="prerequisites"></a>Vereisten
U dient eerst te voldoen aan de vereisten in het artikel [Vereisten voor de zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voordat u deze zelfstudie volgt.

## <a name="steps"></a>Stappen
Hier volgen de stappen die u uitvoert als onderdeel van deze zelfstudie:

1. Een Azure-**gegevensfactory** maken. In deze stap maakt u een gegevensfactory met de naam ADFTutorialDataFactory. 
2. **Gekoppelde services** maken in de gegevensfactory. In deze stap maakt u twee gekoppelde services van het type: Azure Storage en Azure SQL Database. 
    
    De AzureStorageLinkedService koppelt uw Azure-opslagaccount aan de gegevensfactory. U hebt een container gemaakt en gegevens naar dit opslagaccount geüpload als onderdeel van de [vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    De AzureSqlLinkedService koppelt uw Azure SQL-database aan de gegevensfactory. De gegevens die worden gekopieerd uit de blobopslag worden opgeslagen in deze database. Als onderdeel van de [vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hebt u een SQL-tabel in deze database gemaakt.   
3. Maak **invoer- en uitvoergegevenssets** in de gegevensfactory.  
    
    De gekoppelde Azure Storage-service geeft de verbindingsreeks op die de Data Factory-service tijdens runtime gebruikt om verbinding te maken met uw Azure-opslagaccount. En de blobgegevensset voor invoer geeft de container en de map met de invoergegevens op.  

    Op dezelfde manier geeft de gekoppelde Azure SQL Database-service de verbindingsreeks op die de Data Factory-service in runtime gebruikt om verbinding te maken met uw Azure SQL-database. En de uitvoergegevensset van de SQL-tabel geeft de tabel in de database op waarnaar de gegevens uit de blobopslag worden gekopieerd.
4. Maak een **pijplijn** in de gegevensfactory. In deze stap maakt u een pijplijn met een kopieeractiviteit.   
    
    Met de kopieeractiviteit worden gegevens uit een blob in de Azure-blobopslag naar een tabel in de Azure SQL-database gekopieerd. U kunt een kopieeractiviteit gebruiken in een pijplijn om gegevens uit ondersteunde bronnen te kopiëren naar een ondersteunde bestemming. Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor een lijst met ondersteunde gegevensarchieven. 
5. Bewaak de pijplijn. In deze stap **bewaakt** u segmenten van de invoer- en uitvoergegevenssets met behulp van Azure Portal. 

## <a name="create-data-factory"></a>Een gegevensfactory maken
> [!IMPORTANT]
> Voldoe aan de [vereisten voor de zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) als u dat nog niet hebt gedaan.   

Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn kan één of meer activiteiten bevatten. Bijvoorbeeld een kopieeractiviteit om gegevens van een bron- naar een doelgegevensopslagplaats te kopiëren en een HDInsight Hive-activiteit om een Hive-script uit te voeren voor het transformeren van invoergegevens naar productuitvoergegevens. U begint in deze stap met het maken van de gegevensfactory.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/) en klik op **Nieuw** in het menu links. Selecteer **Gegevensanalyse** en klik op **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. In de blade **Nieuwe gegevensfactory**:
   
   1. Voer **ADFTutorialDataFactory** in als **naam**. 
      
         ![Blade voor een nieuwe gegevensfactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialDataFactory) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Naam van gegevensfactory niet beschikbaar](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
   3. Voer een van de volgende stappen uit voor de **Resourcegroep**:
      
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
          Voor sommige van de stappen in deze zelfstudie wordt ervan uitgegaan dat u voor de resourcegroep de naam **ADFTutorialResourceGroup** gebruikt. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
   4. Selecteer de **locatie** voor de gegevensfactory. Alleen regio's die worden ondersteund door de Data Factory-service worden weergegeven in de vervolgkeuzelijst.
   5. Selecteer **Vastmaken aan dashboard**.     
   6. Klik op **Create**.
      
      > [!IMPORTANT]
      > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepsniveau.
      > 
      > De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.                
      > 
      > 
3. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. Wanneer het maken is voltooid, ziet u de blade **Data Factory** zoals in de afbeelding is weergegeven.
   
   ![Startpagina van de gegevensfactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
U maakt gekoppelde services in een gegevensfactory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze zelfstudie gebruikt u niet een willekeurige compute-service, zoals Azure HDInsight of Azure Data Lake Analytics. U gebruikt twee gegevensarchieven van het type Azure Storage (bron) en Azure SQL Database (doel). 

Daarom maakt u twee gekoppelde services met de naam AzureStorageLinkedService en AzureSqlLinkedService van het type: AzureStorage en AzureSqlDatabase.  

De AzureStorageLinkedService koppelt uw Azure-opslagaccount aan de gegevensfactory. Dit opslagaccount is het account waarin u een container hebt gemaakt en gegevens hebt geüpload als onderdeel van de [vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

De AzureSqlLinkedService koppelt uw Azure SQL-database aan de gegevensfactory. De gegevens die worden gekopieerd uit de blobopslag worden opgeslagen in deze database. Als onderdeel van de [vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hebt u de emp-tabel in deze database gemaakt.  

### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
In deze stap koppelt u uw Azure Storage-account aan uw gegevensfactory. In deze sectie geeft u de naam en sleutel van uw Azure Storage-account op.  

1. Klik op de blade **Data Factory** op **Maken en implementeren**.
   
   ![Tegel Ontwerpen en implementeren](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. U ziet de **Data Factory-Editor** zoals weergegeven op de volgende afbeelding: 

    ![Data Factory Editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. Klik in de editor op de knop **Nieuw gegevensarchief** op de werkbalk en selecteer **Azure Storage** in de vervolgkeuzelijst. U ziet het JSON-sjabloon voor het maken van een gekoppelde Azure-service in het rechterdeelvenster. 
   
    ![Knop Nieuw gegevensarchief in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Vervang `<accountname>` en `<accountkey>` door de naam van uw account en de accountsleutel van uw Azure-opslagaccount. 
   
    ![JSON voor Blob Storage in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Klik op **Implementeren** op de werkbalk. De geïmplementeerde **AzureStorageLinkedService** wordt nu in de structuurweergave weergeven. 
   
    ![Blob Storage implementeren in de editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    Zie het artikel [Azure Blob Storage-connector](data-factory-azure-blob-connector.md#linked-service-properties) voor meer informatie over de JSON-eigenschappen in de definitie van de gekoppelde service.

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Een gekoppelde service maken voor de Azure SQL Database
In deze stap koppelt u uw Azure SQL Database aan uw gegevensfactory. U geeft in deze sectie de Azure SQL-servernaam, -databasenaam, -gebruikersnaam en -wachtwoord op. 

1. In de **Data Factory-editor** klikt u op **Nieuw gegevensarchief** op de werkbalk en selecteert u **Azure SQL Database** uit de vervolgkeuzelijst. U ziet het JSON-sjabloon voor het maken van een gekoppelde Azure SQL-service in het rechterdeelvenster.
2. Vervang `<servername>`, `<databasename>`, `<username>@<servername>` en `<password>` door de namen van uw Azure SQL-server, -database en -gebruikersaccount en voer uw wachtwoord in. 
3. Klik op **Implementeren** op de werkbalk om **AzureSqlLinkedService** te maken en te implementeren.
4. Bevestig dat **AzureSqlLinkedService** in de structuurweergave onder **Gekoppelde services** wordt weergegeven.  

    Zie [Azure SQL Database-connector](data-factory-azure-sql-connector.md#linked-service-properties) voor meer informatie over deze JSON-eigenschappen.

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap hebt u gekoppelde services gemaakt om uw Azure-opslagaccount en Azure SQL-database aan de gegevensfactory te koppelen. In deze stap definieert u twee gegevenssets, InputDataset en OutputDataset genaamd, die staan voor de invoer- en uitvoergegevens die zijn opgeslagen in de gegevensarchieven waarnaar wordt verwezen door respectievelijk de AzureStorageLinkedService en de AzureSqlLinkedService.

De gekoppelde Azure Storage-service geeft de verbindingsreeks op die de Data Factory-service tijdens runtime gebruikt om verbinding te maken met uw Azure-opslagaccount. En de blobgegevensset voor invoer (InputDataset) geeft de container en de map met de invoergegevens op.  

Op dezelfde manier geeft de gekoppelde Azure SQL Database-service de verbindingsreeks op die de Data Factory-service in runtime gebruikt om verbinding te maken met uw Azure SQL-database. En de uitvoergegevensset van de SQL-tabel (OututDataset) geeft de tabel in de database op waarnaar de gegevens uit de blobopslag worden gekopieerd. 

### <a name="create-input-dataset"></a>Invoergegevensset maken
In deze stap maakt u een gegevensset met de naam InputDataset die verwijst naar een blobbestand (emp.txt) in de hoofdmap van een blobcontainer (adftutorial) in Azure Storage. Deze container wordt vertegenwoordigd door de gekoppelde AzureStorageLinkedService-service. Als u geen waarde voor de fileName hebt opgeven (of hebt overgeslagen), worden gegevens uit alle blobs in de invoermap naar het doel gekopieerd. In deze zelfstudie geeft u een waarde op voor de fileName. 

1. In de **Editor** voor de Data Factory, klikt u op **... Meer**, **Nieuwe gegevensset** en vervolgens op **Azure Blob-opslag** in de vervolgkeuzelijst. 
   
    ![Nieuw gegevenssetmenu](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment: 
   
    ```json
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

    De volgende tabel bevat beschrijvingen van de JSON-eigenschappen die in het codefragment worden gebruikt:

    | Eigenschap | Beschrijving |
    |:--- |:--- |
    | type | De eigenschap type wordt ingesteld op **AzureBlob**, omdat de gegevens zich in een Azure-blobopslag bevinden. |
    | linkedServiceName | Deze eigenschap verwijst naar de **AzureStorageLinkedService** die u eerder hebt gemaakt. |
    | folderPath | Deze eigenschap verwijst naar de blob**container** en de **map** die de blobs voor invoer bevat. In deze zelfstudie is adftutorial de blobcontainer en is folder de hoofdmap. | 
    | fileName | Deze eigenschap is optioneel. Als u deze eigenschap niet opgeeft, worden alle bestanden uit folderPath gekozen. In deze zelfstudie wordt **emp.txt** opgegeven voor de fileName, zodat alleen dat bestand wordt opgehaald voor de verwerking. |
    | format -> type |Het invoerbestand is in de tekstindeling, zodat we **TextFormat** gebruiken. |
    | columnDelimiter | De kolommen in het invoerbestand worden gescheiden door een **komma (`,`)**. |
    | frequency/interval | Als frequency wordt ingesteld op **Hour** en het interval wordt ingesteld op **1**, betekent dit dat de invoersegmenten één keer per **uur** beschikbaar worden gemaakt. Met andere woorden, de Data Factory-service zoekt elk uur naar invoergegevens in de hoofdmap van de opgegeven blobcontainer (**adftutorial**). Er wordt gezocht naar gegevens binnen de begin- en eindtijd van de pijplijn, niet voor of na deze tijden.  |
    | external | Deze eigenschap wordt ingesteld op **true** als de gegevens niet worden gegenereerd door deze pijplijn. De invoergegevens in deze zelfstudie bevinden zich in het bestand emp.txt, dat niet wordt gegenereerd door deze pijplijn. Daarom stellen we deze eigenschap in op true. |

    Zie het [artikel over Azure Blob-connectoren](data-factory-azure-blob-connector.md#dataset-properties) voor meer informatie over deze JSON-eigenschappen.      
3. Klik op **Implementeren** op de werkbalk om de tabel **InputDataset** te implementeren. Bevestig dat **InputDataset** in de structuurweergave wordt weergegeven.

### <a name="create-output-dataset"></a>Uitvoergegevensset maken
De gekoppelde Azure SQL Database-service geeft de verbindingsreeks op die de Data Factory-service in runtime gebruikt om verbinding te maken met uw Azure SQL-database. De uitvoergegevensset van de SQL-tabel (OututDataset) die u in deze stap hebt gemaakt, geeft de tabel in de database op waarnaar de gegevens uit de blobopslag worden gekopieerd.

1. In de **Editor** voor de Data Factory, klikt u op **... Meer**, **Nieuwe gegevensset** en vervolgens op **Azure SQL** in de vervolgkeuzelijst. 
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment:

    ```json   
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

    De volgende tabel bevat beschrijvingen van de JSON-eigenschappen die in het codefragment worden gebruikt:

    | Eigenschap | Beschrijving |
    |:--- |:--- |
    | type | De eigenschap type wordt ingesteld op **AzureSqlTable** omdat gegevens naar een tabel in een Azure SQL-database worden gekopieerd. |
    | linkedServiceName | Deze eigenschap verwijst naar de **AzureSqlLinkedService** die u eerder hebt gemaakt. |
    | tableName | Geeft de **tabel** aan waarnaar de gegevens worden gekopieerd. | 
    | frequency/interval | De frequentie is ingesteld op **Hour** en het interval is **1**, wat betekent dat de uitvoersegmenten worden geproduceerd **per uur** tussen de begin- en eindtijd van de pijplijn, niet voor of na deze tijden.  |

    De tabel emp in de database bevat drie kolommen: **ID**, **FirstName** en **LastName**. ID is een identiteitskolom, zodat u alleen **FirstName** en **LastName** hoeft op te geven.

    Zie het [artikel over Azure SQL-connectoren](data-factory-azure-sql-connector.md#dataset-properties) voor meer informatie over deze JSON-eigenschappen.
3. Klik op **Implementeren** op de werkbalk om de tabel **OutputDataset** te implementeren. Bevestig dat **OutputDataset** in de structuurweergave onder **Gegevenssets** wordt weergegeven. 

## <a name="create-pipeline"></a>Pijplijn maken
In deze stap maakt u een pijplijn met een **kopieeractiviteit** die gebruikmaakt van **InputDataset** als invoer en **OutputDataset** als uitvoer.

Momenteel is de uitvoergegevensset dat wat de planning aanstuurt. In deze zelfstudie is de uitvoergegevensset geconfigureerd voor het produceren van een segment eenmaal per uur. De pijplijn heeft een begintijd en eindtijd die één dag uit elkaar liggen, ofwel 24 uur. Daarom worden 24 segmenten van de uitvoergegevensset door de pijplijn geproduceerd. 

1. In de **Editor** voor de Data Factory, klikt u op **... Meer** en vervolgens op **Nieuwe pijplijn**. U kunt ook met de rechtermuisknop op **Pijplijnen** klikken in de boomstructuur vervolgens klikken op **Nieuwe pijplijn**.
2. Vervang JSON in het rechterdeelvenster met het volgende JSON-fragment: 

    ```json   
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
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```   
    
    Houd rekening met de volgende punten:
   
    - In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**. Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over kopieeractiviteiten. In Data Factory-oplossingen kunt u ook [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) gebruiken.
    - De invoer voor de activiteit is ingesteld op **InputDataset** en de uitvoer voor de activiteit is ingesteld op **OutputDataset**. 
    - In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type. Zie [Ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor een volledige lijst van gegevensarchieven die worden ondersteund door kopieeractiviteiten als bronnen en sinks. Klik op de koppeling in de tabel voor informatie over het gebruik van een specifiek ondersteund gegevensarchief als een bron/sink.
    - Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2016-10-14T16:32:41Z. De **eindtijd** is optioneel, maar we gebruiken hem in deze zelfstudie. Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**.
     
    In het voorgaande voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.

    Zie het artikel [Pijplijnen maken](data-factory-create-pipelines.md) voor beschrijvingen van JSON-eigenschappen in de definitie van een pijplijn. Zie [Gegevensverplaatsingsactiviteiten](data-factory-data-movement-activities.md) voor beschrijvingen van JSON-eigenschappen in de definitie van een kopieeractiviteit. Zie het [artikel over Azure Blob-connectoren](data-factory-azure-blob-connector.md) voor beschrijvingen van JSON-eigenschappen die worden ondersteund door BlobSource. Zie het [artikel over Azure SQL Database-connectoren](data-factory-azure-sql-connector.md) voor beschrijvingen van JSON-eigenschappen die worden ondersteund door SqlSink.
3. Klik op **Implementeren** op de werkbalk om de tabel **ADFTutorialPipeline** te implementeren. Controleer of de pijplijn in de structuurweergave wordt weergegeven. 
4. Sluit nu de blade **Editor** door op **X** te klikken. Klik opnieuw op **X** om de **Data Factory**-startpagina te zien voor de **ADFTutorialDataFactory**.

**Gefeliciteerd!** U hebt een Azure-gegevensfactory gemaakt met een pijplijn die gegevens van een Azure-blobopslag kopieert naar een Azure SQL-database. 


## <a name="monitor-pipeline"></a>De pijplijn bewaken
In deze stap gebruikt u Azure Portal om te controleren wat er gebeurt in een Azure data factory.    

### <a name="monitor-pipeline-using-monitor--manage-app"></a>De pijplijn bewaken met de app Bewaking en beheer
In de volgende stappen ziet u hoe u pijplijnen in uw gegevensfactory kunt bewaken met behulp van de toepassing Bewaking en beheer: 

1. Klik op de tegel **Bewaking en beheer** op de startpagina van uw gegevensfactory.
   
    ![De tegel Bewaking en beheer](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. De toepassing **Bewaking en beheer** wordt op een afzonderlijk tabblad weergegeven. 

    > [!NOTE]
    > Als u ziet dat de webbrowser is vastgelopen bij "Autoriseren..." gaat u op een van de volgende manieren te werk: schakel het selectievakje **Cookies van derden en sitegegevens blokkeren** uit (of) maak een uitzondering voor **login.microsoftonline.com** en probeer de toepassing opnieuw te openen.

    ![De app Bewaking en beheer](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. Wijzig de **Begintijd** en de **Eindtijd** om de starttijd (2017-05-11) en de eindtijd (2017-05-12) van uw pijplijn op te geven. Klik vervolgens op **Toepassen**.       
3. U ziet de **activiteitsvensters** die zijn gekoppeld aan elk uur tussen de begin- en eindtijd van de pijplijn in de lijst in het middelste deelvenster. 
4. Selecteer een activiteitsvenster in de lijst **Activiteitsvensters** om de details van een activiteitsvenster te bekijken. 
    ![Details van activiteitsvenster](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

    In de activiteitsvensterverkenner aan de rechterkant ziet u dat de segmenten tot aan de huidige UTC-tijd (8:12 PM) alle zijn verwerkt (groen). De segmenten 8-9 PM, 9 - 10 PM, 10 - 11 PM, 11 PM - 12 AM zijn nog niet verwerkt.

    De sectie **Pogingen** in het rechterdeelvenster bevat informatie over de activiteit die wordt uitgevoerd voor het gegevenssegment. Als er een fout is, staat er meer informatie over de fout. Als bijvoorbeeld de invoermap of de container niet bestaat en de segmentverwerking mislukt, ziet u een foutbericht dat aangeeft dat de container of de map niet bestaat.

    ![Pogingen tot uitvoering van activiteit](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. Open **SQL Server Management Studio**, maak verbinding maken met de Azure SQL Database en controleer of de rijen zijn ingevoegd in de tabel **emp** in de database.
    
    ![SQL-queryresultaten](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

Zie [Azure Data Factory-pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van deze toepassing.

### <a name="monitor-pipeline-using-diagram-view"></a>De pijplijn bewaken met Diagramweergave
U kunt ook gegevenspijplijnen bewaken met behulp van de diagramweergave.  

1. In de blade **Gegevensfactory** klikt u op **Diagram**.
   
    ![Blade Gegevensfactory - tegel Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. U ziet een diagram dat lijkt op de volgende afbeelding: 
   
    ![Diagramweergave](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. Dubbelklik in de diagramweergave op **InputDataset** om segmenten voor de gegevensset weer te geven.  
   
    ![Gegevenssets waarvoor InputDataset is geselecteerd](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Klik op **Alles bekijken** om alle gegevenssegmenten te bekijken. U ziet 24 uurlijkse segmenten tussen de begin- en eindtijd van de pijplijn. 
   
    ![Alle invoergegevenssegmenten](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    De gegevenssegmenten tot aan de huidige UTC-tijd hebben de status **Gereed** omdat het bestand **emp.txt** aanwezig is in de blobcontainer **adftutorial\input**. De segmenten voor de toekomstige tijden hebben nog niet de status Gereed. Controleer of er geen segmenten worden weergegeven in het gedeelte **Recent mislukte segmenten** onderaan.
6. Sluit de blades totdat u het diagram ziet (of) scroll naar links om de diagramweergave weer te geven. Dubbelklik vervolgens op **OutputDataset**. 
8. Klik op de koppeling **Alles bekijken** op de blade **Tabel** voor **OutputDataset** om alle segmenten te bekijken.

    ![blade gegevenssegmenten](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. U ziet dat alle segmenten tot aan de huidige UTC-tijd van de status **Uitvoering in behandeling** naar = > **Bezig** ==> **Gereed** gaan. De segmenten in het verleden (voor de huidige tijd) worden standaard van laatste naar oudste verwerkt. Als de huidige tijd bijvoorbeeld 8:12 PM UTC is, wordt het segment voor 7 PM - 8 PM eerder verwerkt dan het segment 6 PM - 7 PM. Het segment 8 PM - 9 PM wordt standaard verwerkt aan het einde van het tijdsinterval, ofwel na 9 PM.  
10. Als u op een gegevenssegment uit de lijst klikt, ziet u de blade **Gegevenssegment**. Een hoeveelheid gegevens die is gekoppeld aan een activiteitsvenster wordt een segment genoemd. Een segment kan één bestand of meerdere bestanden zijn.  
    
     ![blade Gegevenssegment](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Als het segment niet de status **Gereed** heeft, kunt u de upstreamsegmenten bekijken die niet de status Gereed hebben en die voorkomen dat de huidige status wordt uitgevoerd. U ziet deze segmenten in de lijst **Upstreamsegmenten die niet gereed zijn**.
11. In de blade **GEGEVENSSEGMENT** ziet u in de lijst onderaan alle activiteiten die worden uitgevoerd. Klik op een **activiteit die wordt uitgevoerd** om de blade **Details uitvoering van activiteit** weer te geven. 
    
    ![Details uitvoering van activiteit](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    Op deze blade ziet u hoe lang de kopieerbewerking duurde, wat de doorvoer is, hoeveel bytes aan gegevens zijn gelezen en geschreven, de uitvoeringstijd, de begintijd, eindtijd, enzovoort.  
12. Klik op **X** om alle blades te sluiten tot u weer op de startblade bent voor **ADFTutorialDataFactory**.
13. Klik op de tegel **Gegevenssets** of op de tegel **Pijplijnen** om de blades weer te geven die u hebt gezien in de voorgaande stappen (optioneel). 
14. Open **SQL Server Management Studio**, maak verbinding maken met de Azure SQL Database en controleer of de rijen zijn ingevoegd in de tabel **emp** in de database.
    
    ![SQL-queryresultaten](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u een Azure-gegevensfactory gemaakt om gegevens te kopiëren van een Azure-blob naar een Azure SQL-database. U gebruikt Azure Portal om de data factory, gekoppelde services, datasets en een pijplijn te maken. Hier volgen de hoofdstappen die u in deze zelfstudie hebt uitgevoerd:  

1. U hebt een Azure-**gegevensfactory** gemaakt.
2. U hebt **gekoppelde services** gemaakt:
   1. Een gekoppelde **Azure Storage**-service om uw Azure-opslagaccount te koppelen dat invoergegevens bevat.     
   2. Een gekoppelde **Azure SQL**-service om uw Azure SQL Database te koppelen die uitvoergegevens bevat. 
3. U hebt **gegevenssets** gemaakt waarin de invoer- en uitvoergegevens van pijplijnen worden beschreven.
4. U hebt een **pijplijn** gemaakt met een **kopieeractiviteit** met **BlobSource** als bron en **SqlSink** als sink.  

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u voor een kopieerbewerking een Azure Blob-opslag gebruikt als brongegevensarchief en een Azure SQL-database als doelgegevensarchief. De volgende tabel bevat een lijst met gegevensarchieven die worden ondersteund als bron en doel voor de kopieeractiviteit: 

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

Klik op de koppeling voor de gegevensopslag in de tabel voor meer informatie over het kopiëren van gegevens naar/uit een gegevensarchief.