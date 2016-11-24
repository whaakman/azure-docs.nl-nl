---
title: 'Zelfstudie: een pijplijn maken met de kopieeractiviteit in Visual Studio | Microsoft Docs'
description: In deze zelfstudie maakt u een Azure Data Factory-pijplijn met een kopieeractiviteit. Hiervoor gebruikt u Visual Studio.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/17/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 2a5905c5dcb36777e7b88d6ac44877fdec72e7a5
ms.openlocfilehash: 3f078e5b5a76e1a6bc840890d2cd90f60a163384


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Zelfstudie: een pijplijn maken met de kopieeractiviteit in Visual Studio
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

In deze zelfstudie ziet u hoe u een Azure-gegevensfactory maakt en bewaakt met Visual Studio. In de pijplijn in de gegevensfactory wordt gebruikgemaakt van een kopieeractiviteit om gegevens van Azure-blobopslag te kopiëren naar Azure SQL Database.

Hier volgen de stappen die u uitvoert als onderdeel van deze zelfstudie:

1. Twee gekoppelde services maken: **AzureStorageLinkedService1** en **AzureSqlinkedService1**. 
   
    Met de AzureStorageLinkedService1 wordt een Azure-opslag gekoppeld en met AzureSqlLinkedService1 wordt een Azure SQL Database gekoppeld aan de gegevensfactory **ADFTutorialDataFactoryVS**. De invoergegevens van de pijplijn staan in een blobcontainer in Azure Blob Storage en de uitvoergegevens worden opgeslagen in een tabel in de Azure SQL Database. Daarom voegt u deze twee gegevensarchieven als gekoppelde services toe aan de gegevensfactory.
2. Maak twee gegevenssets, **InputDataset** en **OutputDataset**, die staan voor de invoer- en uitvoergegevens die zijn opgeslagen in de gegevensarchieven. 
   
    Voor de InputDataset geeft u de blobcontainer op die een blob bevat met de brongegevens. Voor OutputDataset geeft u de SQL-tabel op waarin de uitvoergegevens zijn opgeslagen. U geeft ook andere eigenschappen op, zoals de structuur, de beschikbaarheid en het beleid.
3. Maak een pijplijn met de naam **ADFTutorialPipeline** in ADFTutorialDataFactoryVS. 
   
    De pijplijn heeft een **kopieeractiviteit** waarmee invoergegevens van de Azure-blob naar de uitvoer-Azure SQL-tabel worden gekopieerd. Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit. 
4. Maak een gegevensfactory met de naam **VSTutorialFactory**. Implementeer de gegevensfactory en alle gegevensfactory-entiteiten (gekoppelde services, tabellen en de pijplijn).    

## <a name="prerequisites"></a>Vereisten
1. Lees het artikel [Overzicht van de zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) en voer de **vereiste** stappen uit. 
2. U moet een **beheerder van het Azure-abonnement** zijn om Data Factory-entiteiten te kunnen publiceren naar Azure Data Factory.  
3. De volgende zaken moeten op uw computer zijn geïnstalleerd: 
   * Visual Studio 2013 of Visual Studio 2015
   * Download de Azure SDK voor Visual Studio 2013 of Visual Studio 2015. Ga naar de [Azure-downloadpagina](https://azure.microsoft.com/downloads/) en klik in het gedeelte **.NET** op **VS 2013** of **VS 2015**.
   * Download de nieuwste Azure Data Factory-invoegtoepassing voor Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) of [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). U kunt de invoegtoepassing ook als volgt bijwerken: klik in het menu op **Extra** -> **Extensies en updates** -> **Online** -> **Visual Studio-galerie** -> **Microsoft Azure Data Factory-hulpprogramma's voor Visual Studio** -> **Bijwerken**.

## <a name="create-visual-studio-project"></a>Een Visual Studio-project maken
1. Open **Visual Studio 2013**. Klik op **File**, houd de muisaanwijzer op **New** en klik op **Project**. Het dialoogvenster **New Project** wordt weergegeven.  
2. Selecteer in het dialoogvenster **New Project** de sjabloon **DataFactory** en klik op **Empty Data Factory Project**. Als u de sjabloon DataFactory niet ziet, sluit u Visual Studio, installeert u de Azure SDK voor Visual Studio 2013 en opent u Visual Studio opnieuw.  
   
    ![Het dialoogvenster New Project](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Geef een **naam** op voor het project, geef een **locatie** op en geef een naam op voor de **oplossing**. Klik vervolgens op **OK**.
   
    ![Solution Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Gekoppelde services maken
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. Zie [Ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor alle bronnen en sinks die worden ondersteund door de kopieerbewerking. Zie [Gekoppelde services berekenen](data-factory-compute-linked-services.md) voor de lijst met compute-services die worden ondersteund door Data Factory. In deze zelfstudie gebruikt u geen compute-service. 

In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService1** en **AzureSqlLinkedService1**. Met de gekoppelde AzureStorageLinkedService1-service wordt een Azure-opslagaccount gekoppeld en met AzureSqlLinkedService wordt een Azure SQL Database gekoppeld aan de gegevensfactory **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>De gekoppelde Azure Storage-service maken
1. Klik in Solution Explorer met de rechtermuisknop op **Linked Services**. Houd de muisaanwijzer op **Add** en klik op **New Item**.      
2. Selecteer in het dialoogvenster **Add New Item** de optie **Azure Storage Linked Service** in de lijst en klik op **Add**. 
   
    ![Nieuwe gekoppelde service](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Vervang `<accountname>` en `<accountkey>`* door de naam van uw Azure-opslagaccount en de bijbehorende sleutel. 
   
    ![Een gekoppelde Azure Storage-service](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Sla het bestand **AzureStorageLinkedService1.json** op.

> Zie [Gegevens verplaatsen van/naar Azure-blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over JSON-eigenschappen.
> 
> 

### <a name="create-the-azure-sql-linked-service"></a>De gekoppelde Azure SQL-service maken
1. Klik met de rechtermuisknop opnieuw op het knooppunt **Linked Services** in **Solution Explorer**. Houd de muisaanwijzer op **Add** en klik op **New Item**. 
2. Selecteer deze keer **Azure SQL Linked Service** en klik op **Add**. 
3. In het bestand **AzureSqlLinkedService1.json** vervangt u `<servername>`, `<databasename>`, `<username@servername>` en `<password>` door de namen van uw Azure SQL-server, -database en -gebruikersaccount en voert u uw wachtwoord in.    
4. Sla het bestand **AzureSqlLinkedService1.json** op. 

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap hebt u de gekoppelde services **AzureStorageLinkedService1** en **AzureSqlLinkedService1** gemaakt om een Azure-opslagaccount en een Azure SQL Database te koppelen aan de gegevensfactory **ADFTutorialDataFactory**. In deze stap definieert u twee gegevenssets, **InputDataset** en **OutputDataSet**, die staan voor de invoer- en uitvoergegevens die zijn opgeslagen in de gegevensarchieven waarnaar wordt verwezen door respectievelijk AzureStorageLinkedService1 en AzureSqlLinkedService1. Voor InputDataset geeft u de blobcontainer op die een blob bevat met de brongegevens. Voor OutputDataset geeft u de SQL-tabel op waarin de uitvoergegevens zijn opgeslagen.

### <a name="create-input-dataset"></a>Invoergegevensset maken
In deze stap maakt u een gegevensset met de naam **InputDataset** die verwijst naar een blobcontainer in Azure Storage. Deze container wordt vertegenwoordigd door de gekoppelde **AzureStorageLinkedService1**-service. Een tabel is een rechthoekige gegevensset en is het enige type gegevensset dat nu wordt ondersteund. 

1. Klik in **Solution Explorer** met de rechtermuisknop op **Tables**. Houd de muisaanwijzer op **Add** en klik op **New Item**.
2. In het dialoogvenster **Add New Item** selecteert u **Azure Blob** en klikt u op **Add**.   
3. Vervang de JSON-tekst door de volgende tekst en sla het bestand **AzureBlobLocation1.json** op. 
   
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
           "linkedServiceName": "AzureStorageLinkedService1",
           "typeProperties": {
             "folderPath": "adftutorial/",
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
   
           "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
           "fileName": "{Hour}.csv",
           "partitionedBy": 
           [
               { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
               { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
               { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
               { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure-blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

### <a name="create-output-dataset"></a>Uitvoergegevensset maken
In deze stap maakt u een uitvoergegevensset met de naam **OutputDataset**. Deze gegevensset wijst naar een SQL-tabel in de Azure SQL-database die wordt vertegenwoordigd door **AzureSqlLinkedService1**. 

1. Klik in **Solution Explorer** opnieuw met de rechtermuisknop op **Tables**. Houd de muisaanwijzer op **Add** en klik op **New Item**.
2. In het dialoogvenster **Add New Item** selecteert u **Azure SQL** en klikt u op **Add**. 
3. Vervang de JSON-tekst door de volgende JSON en sla het bestand **AzureSqlTableLocation1.json** op.
   
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
           "linkedServiceName": "AzureSqlLinkedService1",
           "typeProperties": {
             "tableName": "emp"
           },
           "availability": {
             "frequency": "Hour",
             "interval": 1
           }
         }
       }
   
    Houd rekening met de volgende punten: 
   
   * De gegevensset **type** wordt ingesteld op **AzureSQLTable**.
   * **linkedServiceName** wordt ingesteld op **AzureSqlLinkedService** (u hebt deze gekoppelde service gemaakt in stap 2).
   * **tablename** wordt ingesteld op **emp**.
   * De tabel emp in de database bevat drie kolommen: **ID**, **FirstName** en **LastName**. ID is een identiteitskolom, zodat u alleen **FirstName** en **LastName** hoeft op te geven.
   * De **beschikbaarheid** wordt ingesteld op **elk uur** (de **frequentie** wordt ingesteld op **elk uur** en het **interval** wordt ingesteld op **1**).  De Data Factory-service maakt elk uur een uitvoergegevenssegment in de tabel **emp** in de Azure SQL-database.

> [!NOTE]
> Zie [Gegevens verplaatsen van/naar Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over JSON-eigenschappen.
> 
> 

## <a name="create-pipeline"></a>Pijplijn maken
U hebt tot nu toe gekoppelde invoer- en uitvoerservices gemaakt. U maakt nu met een **kopieeractiviteit** een pijplijn om gegevens van de Azure-blob te kopiëren naar de Azure SQL Database. 

1. Klik in **Solution Explorer** met de rechtermuisknop op **Pipelines**. Houd de muisaanwijzer op **Add** en klik op **New Item**.  
2. Selecteer **Copy Data Pipeline** in het dialoogvenster **Add New Item** en klik op **Add**. 
3. Vervang de JSON door de volgende JSON en sla het bestand **CopyActivity1.json** op.
   
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
                 "style": "StartOfInterval",
                 "retry": 0,
                 "timeout": "01:00:00"
               }
             }
           ],
           "start": "2015-07-12T00:00:00Z",
           "end": "2015-07-13T00:00:00Z",
           "isPaused": false
         }
       }
   
   Houd rekening met de volgende punten:
   
   * In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**.
   * De invoer voor de activiteit is ingesteld op **InputDataset** en de uitvoer voor de activiteit is ingesteld op **OutputDataset**.
   * In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type.
   
   Vervang de waarde van de eigenschap **start** door de huidige dag en de waarde **end** door de volgende dag. U hoeft alleen de datum in te vullen en kunt de tijd overslaan. Dit wordt dan bijvoorbeeld 2016-02-03, wat gelijk staat aan 2016-02-03T00:00:00Z
   
   Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2016-10-14T16:32:41Z. De **eindtijd** is optioneel, maar we gebruiken hem in deze zelfstudie. 
   
   Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**.
   
   In het voorgaande voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.

## <a name="publishdeploy-data-factory-entities"></a>Data Factory-entiteiten publiceren/implementeren
In deze stap publiceert u Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) die u eerder hebt gemaakt. U specificeert ook de naam van de nieuwe gegevensfactory die moet worden gemaakt voor deze entiteiten.  

1. Klik met de rechtermuisknop op het project in Solution Explorer. Klik vervolgens op **Publish**. 
2. Als u het dialoogvenster **Sign in to your Microsoft account** ziet, voert u uw referenties in voor het account met het Azure-abonnement en klikt u op **Sign in**.
3. Het volgende dialoogvenster wordt weergegeven:
   
   ![Het dialoogvenster Publish](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Op de pagina Configure data factory voert u de volgende stappen uit: 
   
   1. Selecteer **Create New Data Factory**.
   2. Voer **VSTutorialFactory** in als **naam**.  
      
      > [!IMPORTANT]
      > De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u tijdens het publiceren een foutmelding ontvangt over de naam van de gegevensfactory, wijzigt u de naam ervan (naar bijvoorbeeld uwnaamVSTutorialFactory) en publiceert u opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.        
      > 
      > 
   3. Selecteer uw Azure-abonnement voor het veld **Abonnement**.
      
      > [!IMPORTANT]
      > Als u geen abonnement niet ziet, controleert u of u bent aangemeld met een account dat een beheerder of co-beheerder is van het abonnement.  
      > 
      > 
   4. Selecteer de **resourcegroep** voor de gegevensfactory die u wilt maken. 
   5. Selecteer de **regio** voor de gegevensfactory. Alleen regio's die worden ondersteund door de Data Factory-service worden weergegeven in de vervolgkeuzelijst.
   6. Klik op **Next** om over te schakelen naar de pagina **Publish Items**.
      
       ![Pagina Data Factory configureren](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. Op de pagina **Publish Items** controleert u of alle Data Factory-entiteiten zijn geselecteerd en klikt u op **Next** om over te schakelen naar de pagina **Summary**.
   
   ![Pagina Items publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Controleer de samenvatting en klik op **Next** om te beginnen met het implementatieproces en om de **implementatiestatus** te bekijken.
   
   ![Pagina Samenvatting publiceren](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. Op de pagina **Deployment Status** ziet u de status van het implementatieproces. Klik op Finish wanneer de implementatie is uitgevoerd. 
   ![Pagina implementatiestatus](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Houd rekening met de volgende punten: 

* Als u de foutmelding **This subscription is not registered to use namespace Microsoft.DataFactory** ontvangt, voert u een van de volgende stappen uit en probeert u opnieuw te publiceren: 
  
  * Voer in Azure PowerShell de volgende opdracht uit om de Data Factory-provider te registreren. 
    
          Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
      U kunt de volgende opdracht uitvoeren om te bevestigen dat de Data Factory-provider is geregistreerd. 
    
          Get-AzureRmResourceProvider
  * Meld u bij de [Azure Portal](https://portal.azure.com) aan met behulp van het Azure-abonnement en navigeer naar een Data Factory-blade of maak een gegevensfactory in de Azure Portal. Door deze actie wordt de provider automatisch voor u geregistreerd.
* De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.

> [!IMPORTANT]
> Als u Data Factory-exemplaren wilt maken, moet u beheerder/co-beheerder van het Azure-abonnement zijn
> 
> 

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u een Azure-gegevensfactory gemaakt om gegevens te kopiëren van een Azure-blob naar een Azure SQL-database. U hebt Visual Studio gebruikt om de gegevensfactory, gekoppelde services, gegevenssets en pijplijn te maken. In deze zelfstudie hebt u de volgende hoofdstappen uitgevoerd:  

1. U hebt een Azure-**gegevensfactory** gemaakt.
2. U hebt **gekoppelde services** gemaakt:
   1. Een gekoppelde **Azure Storage**-service om uw Azure-opslagaccount te koppelen dat invoergegevens bevat.     
   2. Een gekoppelde **Azure SQL**-service om uw Azure SQL Database te koppelen die uitvoergegevens bevat. 
3. U hebt **gegevenssets** gemaakt waarin de invoer- en uitvoergegevens van pijplijnen worden beschreven.
4. U hebt een **pijplijn** gemaakt met een **kopieeractiviteit** met **BlobSource** als bron en **SqlSink** als sink. 

## <a name="use-server-explorer-to-view-data-factories"></a>Server Explorer gebruiken om gegevensfactory’s weer te geven
1. Klik in het menu van **Visual Studio** op **View** en vervolgens op **Server Explorer**.
2. Vouw in het Server Explorer-venster **Azure** en **Data Factory** uit. Wanneer u **Sign in to Visual Studio** ziet, voert u het **account** in dat aan uw Azure-abonnement is gekoppeld, en klikt u op **Continue**. Voer het **wachtwoord** in en klik op **Sign in**. Visual Studio haalt informatie op uit alle Azure Data Factory’s in uw abonnement. U ziet de status van deze bewerking in het venster **Data Factory Task List**.
    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Klik met de rechtermuisknop op een gegevensfactory en selecteer Export Data Factory to New Project om een Visual Studio-project te maken op basis van een bestaande gegevensfactory.
    ![Een gegevensfactory exporteren naar een VS-project](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Data Factory-hulpprogramma's voor Visual Studio bijwerken
Voer de volgende stappen uit om Azure Data Factory-hulpprogramma's voor Visual Studio bij te werken:

1. Klik in het menu op **Extra** en selecteer **Extensies en updates**. 
2. Selecteer **Updates** in het linkerdeelvenster en selecteer vervolgens **Visual Studio-galerie**.
3. Selecteer **Azure Data Factory-hulpprogramma's voor Visual Studio** en klik op **Bijwerken**. Als u deze vermelding niet ziet, beschikt u al over de nieuwste versie van de hulpprogramma's. 

Zie [Gegevenssets en pijplijn bewaken](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) voor instructies over het gebruik van de Azure Portal om de pijplijn en gegevenssets te bewaken die u tijdens deze zelfstudie hebt gemaakt.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
|:--- |:--- |
| [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) |Dit artikel biedt gedetailleerde informatie over de kopieeractiviteit die u tijdens deze zelfstudie hebt gemaakt. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) |In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |
| [Pijplijnen](data-factory-create-pipelines.md) |Op basis van dit artikel krijgt u inzicht in pijplijnen en activiteiten in Azure Data Factory |
| [Gegevenssets](data-factory-create-datasets.md) |Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory. |
| [Pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) |In dit artikel wordt beschreven hoe u pijplijnen bewaakt en beheert en hoe u fouten hierin oplost met de app voor bewaking en beheer. |




<!--HONumber=Nov16_HO2-->


