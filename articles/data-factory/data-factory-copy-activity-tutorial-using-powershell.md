---
title: 'Zelfstudie: Een pijplijn maken om gegevens te verplaatsen met Azure PowerShell | Microsoft Docs'
description: In deze zelfstudie maakt u een Azure Data Factory-pijplijn met een kopieeractiviteit. Hiervoor gebruikt u Azure PowerShell.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 09d8634d8d1b16edb058d0bb259b089a54748279
ms.lasthandoff: 04/12/2017


---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a>Zelfstudie: Een Data Factory-pijplijn maken die gegevens verplaatst met Azure PowerShell
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

In deze zelfstudie maakt en bewaakt u een exemplaar van Azure Data Factory met Azure PowerShell-cmdlets. In de pijplijn in de data factory die u in deze zelfstudie maakt, wordt gebruikgemaakt van een kopieeractiviteit om gegevens van een Azure-blob te kopiëren naar een Azure SQL Database.

Met de functie Kopieeractiviteit wordt de gegevensverplaatsing in Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over Kopieeractiviteit.   

> [!NOTE]
> Dit artikel omvat niet alle Data Factory-cmdlets. Zie [Data Factory Cmdlet Reference](/powershell/resourcemanager/azurerm.datafactories/v2.5.0/azurerm.datafactories) (Naslaginformatie voor Data Factory-cmdlets) voor uitgebreide documentatie over deze cmdlets.
>
> In de gegevenspijplijn in deze zelfstudie worden gegevens van een brongegevensarchief gekopieerd naar een doelgegevensarchief. Er worden geen invoergegevens mee getransformeerd in uitvoergegevens. Zie [Zelfstudie: een pijplijn maken om gegevens te transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) voor meer informatie over het transformeren van gegevens met Azure Data Factory.

## <a name="prerequisites"></a>Vereisten
- Neem [Overzicht van de zelfstudie en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) door voor een overzicht van de zelfstudie en voer de **vereiste** stappen uit.
- Installeer Azure PowerShell. Volg de instructies in [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Azure PowerShell installeren en configureren).

## <a name="in-this-tutorial"></a>In deze zelfstudie
De volgende tabel bevat de stappen die u moet uitvoeren als onderdeel van de zelfstudie.

| Stap | Beschrijving |
| --- | --- |
| [Een Azure Data Factory maken](#create-data-factory) |In deze stap maakt u een Azure-gegevensfactory met de naam **ADFTutorialDataFactoryPSH**. |
| [Gekoppelde services maken](#create-linked-services) |In deze stap maakt u twee gekoppelde services: **StorageLinkedService** en **AzureSqlLinkedService**. Met de StorageLinkedService wordt een Azure Storage-service gekoppeld en met AzureSqlLinkedService wordt een Azure SQL-database gekoppeld aan ADFTutorialDataFactoryPSH. |
| [Invoer- en uitvoergegevenssets maken](#create-datasets) |In deze stap definieert u twee gegevenssets (EmpTableFromBlob en EmpSQLTable). Deze gegevenssets worden gebruikt als invoer- en uitvoertabellen voor **Kopieeractiviteit** in de ADFTutorialPipeline die u in de volgende stap maakt. |
| [Een pijplijn maken en uitvoeren](#create-pipeline) |In deze stap maakt u een pijplijn met de naam **ADFTutorialPipeline** in de data factory ADFTutorialDataFactoryPSH. De pijplijn gebruikt Kopieeractiviteit om gegevens van een Azure-blob naar een uitvoer-Azure-databasetabel te kopiëren. |
| [Gegevenssets en pijplijn bewaken](#monitor-pipeline) |In deze stap bewaakt u de gegevenssets en de pijplijn met Azure PowerShell. |

## <a name="create-a-data-factory"></a>Een data factory maken
In deze stap gebruikt u Azure PowerShell om een Azure data factory met de naam **ADFTutorialDataFactoryPSH** te maken.

1. Start **PowerShell**. Houd Azure PowerShell open tot het einde van deze zelfstudie. Als u het programma sluit en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.

    Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```PowerShell
    Get-AzureRmSubscription
    ```

    Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **&lt;NameOfAzureSubscription**&gt; door de naam van uw Azure-abonnement:

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. Maak een Azure-resourcegroep met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    Voor sommige van de stappen in deze zelfstudie wordt ervan uitgegaan dat u de resourcegroep met de naam **ADFTutorialResourceGroup** gebruikt. Als u een andere resourcegroep gebruikt, moet u voor deze zelfstudie die groep gebruiken in plaats van ADFTutorialResourceGroup.
3. Voer de cmdlet **New-AzureRmDataFactory** uit om een data factory met de naam **ADFTutorialDataFactoryPSH** te maken:  

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u het volgende foutbericht krijgt, moet u de naam (bijvoorbeeld uwnaamADFTutorialDataFactoryPSH) wijzigen. Gebruik deze naam in plaats van ADFTutorialFactoryPSH tijdens het uitvoeren van de stappen in de zelfstudie. Raadpleeg [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory - Naamgevingsregels) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* Als u Data Factory-exemplaren wilt maken, moet u bijdrager of beheerder zijn van het Azure-abonnement.
* De naam van de data factory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.
* Mogelijk wordt de volgende fout weergegeven: **Dit abonnement is niet geregistreerd voor gebruik van de naamruimte Microsoft.DataFactory**. Voer een van de volgende handelingen uit en probeer opnieuw te publiceren:

  * Voer in Azure PowerShell de volgende opdracht uit om de Data Factory-provider te registreren:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    Voer de volgende opdracht uit om te bevestigen dat de Data Factory-provider is geregistreerd:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Meld u met het Azure-abonnement aan bij de [Azure-portal](https://portal.azure.com). Ga naar een Data Factory-blade of maak een data factory in de Azure-portal. Door deze actie wordt de provider automatisch voor u geregistreerd.

## <a name="create-linked-services"></a>Gekoppelde services maken
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. Een gegevensarchief kan Azure Storage-service, een Azure SQL Database of een on-premises SQL Server-database zijn die invoergegevens bevat of de uitvoergegevens opslaat van een Data Factory-pijplijn. Een compute-service is een service die invoergegevens verwerkt en die uitvoergegevens produceert.

In deze stap maakt u twee gekoppelde services: **StorageLinkedService** en **AzureSqlLinkedService**. Met StorageLinkedService wordt een Azure-opslagaccount gekoppeld en met AzureSqlLinkedService wordt een Azure SQL Database gekoppeld aan de data factory **ADFTutorialDataFactoryPSH**. Later in deze zelfstudie maakt u een pijplijn waarmee gegevens worden gekopieerd van een blobcontainer in StorageLinkedService naar een SQL-tabel in AzureSqlLinkedService.

### <a name="create-a-linked-service-for-an-azure-storage-account"></a>Een gekoppelde service maken voor een Azure-opslagaccount
1. Maak een JSON-bestand met de naam **StorageLinkedService.json** in de map **C:\ADFGetStartedPSH**. Geef dit bestand de volgende inhoud. (Maak de map ADFGetStartedPSH als deze nog niet bestaat.)

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
     }
    ```
   Vervang **accountname** en **accountkey** door de naam en sleutel van uw Azure Storage-account.
2. Schakel in **Azure PowerShell** over naar de map **ADFGetStartedPSH**.
3. U kunt de cmdlet **New-AzureRmDataFactoryLinkedService** gebruiken om een gekoppelde service te maken. Voor deze cmdlet en andere Data Factory-cmdlets die u in deze zelfstudie gebruikt, moet u waarden doorgeven voor de parameters **ResourceGroupName** en **DataFactoryName**. U kunt ook **Get-AzureRmDataFactory** gebruiken om een DataFactory-object te verkrijgen. U geeft daarmee het object door zonder ResourceGroupName en DataFactoryName te hoeven typen telkens wanneer u een cmdlet uitvoert. Voer de volgende opdracht uit om de uitvoer van de cmdlet **Get-AzureRmDataFactory** toe te wijzen aan de variabele **$df**:

    ```PowerShell   
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
    ```

4. Voer nu de cmdlet **New-AzureRmDataFactoryLinkedService** uit om de gekoppelde **StorageLinkedService**-service te maken.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```

    Als u de cmdlet **Get-AzureRmDataFactory** niet had uitgevoerd en u de uitvoer niet had toegewezen aan de **$df**-variabele, zou u als volgt waarden moeten opgeven voor de parameters ResourceGroupName en DataFactoryName.   

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json
    ```

Als u Azure PowerShell gedurende deze zelfstudie sluit, moet u de volgende keer dat u Azure PowerShell opent de cmdlet Get-AzureRmDataFactory uitvoeren om de zelfstudie te voltooien.

### <a name="create-a-linked-service-for-an-azure-sql-database"></a>Een gekoppelde service maken voor een Azure SQL-database
1. Maak een JSON-bestand met de naam AzureSqlLinkedService.json en de volgende inhoud:

    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
   Vervang **servername**, **databasename**, **username@servername** en **password** door de namen van uw Azure SQL-server, database, gebruiker en wachtwoord.
2. Voer de volgende opdracht uit om een gekoppelde service te maken:

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```

   Controleer of de instelling **Toegang tot Azure-services toestaan** is ingeschakeld voor uw SQL-databaseserver. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

   1. Klik op de hub **BLADEREN** aan de linkerkant en klik op **SQL-servers**.
   2. Selecteer de server en klik op **INSTELLINGEN** op de blade **SQL-SERVER**.
   3. Klik in de blade **INSTELLINGEN** op **Firewall**.
   4. In de blade **Firewallinstellingen**schakelt u **Toegang tot Azure-services toestaan** **in**.
   5. Klik op de hub **ACTIEF** aan de linkerkant om over te schakelen naar de blade **Data Factory** die u al had geopend.

## <a name="create-datasets"></a>Gegevenssets maken
In de vorige stap hebt u services gemaakt om een Azure-opslagaccount en Azure SQL-database aan de data factory te koppelen. In deze stap maakt u gegevenssets die staan voor de invoer- en uitvoergegevens voor Kopieeractiviteit in de pijplijn die u tijdens de volgende stap maakt.

Een tabel is een rechthoekige gegevensset. Op dit moment wordt alleen dit type gegevensset ondersteund. De invoertabel in deze zelfstudie verwijst naar een blobcontainer in de Azure Storage. De uitvoertabel verwijst naar een SQL-tabel in de Azure SQL-database.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Azure Blob-opslag en Azure SQL Database voorbereiden voor de zelfstudie
Sla deze stap over als u de zelfstudie uit het artikel [Copy data from Blob storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Gegevens uit Blob-opslag kopiëren naar SQL Database) hebt doorgenomen.

Voer de volgende stappen uit om de Blob-opslag en Azure SQL Database voor te bereiden voor deze zelfstudie.

1. Maak een blobcontainer met de naam **adftutorial** in Blob-opslag waar **StorageLinkedService** naar wijst.
2. Maak een tekstbestand met de naam **emp.txt** en upload het als blob naar de container **adftutorial**.
3. Maak een tabel met de naam **emp** in de SQL-database waarnaar **AzureSqlLinkedService** wijst.

4. Start Kladblok. Kopieer de volgende tekst en sla deze als **emp.txt** op in de map **C:\ADFGetStartedPSH** op de vaste schijf.

    ```
    John, Doe
    Jane, Doe
    ```
5. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/) om de container **adftutorial** te maken en om het bestand **emp.txt** te uploaden naar de container.

    ![Azure Opslagverkenner](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
6. Gebruik het volgende SQL-script om de tabel **emp** te maken in uw SQL-database.  

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    Als u SQL Server 2014 op uw computer hebt geïnstalleerd, volgt u de instructies in [Stap 2: Verbinding maken met de SQL Database in het artikel Azure SQL Database beheren met SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) om verbinding te maken met uw SQL-databaseserver en het SQL-script uit te voeren.

    Als de client geen toegang heeft tot de SQL-databaseserver, moet u de firewall voor uw SQL-databaseserver zo instellen dat toegang vanaf uw apparaat (IP-adres) is toegestaan. Raadpleeg [dit artikel](../sql-database/sql-database-configure-firewall-settings.md) voor de stappen.

### <a name="create-an-input-dataset"></a>Een invoergegevensset maken
Een tabel is een rechthoekige gegevensset en bevat een schema. In deze stap maakt u een tabel met de naam **EmpBlobTable**. Deze tabel wijst naar een blobcontainer in Azure Storage, voorgesteld door de gekoppelde **StorageLinkedService**-service. Deze blobcontainer (adftutorial) bevat de invoergegevens van het bestand **emp.txt**.

1. Maak een JSON-bestand met de naam **EmpBlobTable.json** in de map **C:\ADFGetStartedPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "EmpTableFromBlob",
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
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "emp.txt",
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
    ```

   Houd rekening met de volgende punten:

   * De gegevensset **type** wordt ingesteld op **AzureBlob**.
   * **linkedServiceName** wordt ingesteld op **StorageLinkedService**.
   * **folderPath** wordt ingesteld op de container **adftutorial**.
   * **fileName** wordt ingesteld op **emp.txt**. Als u de naam van de blob niet opgeeft, worden de gegevens uit alle blobs in de container gezien als invoergegevens.  
   * De indeling **type** wordt ingesteld op **TextFormat**.
   * Er zijn twee velden in het tekstbestand: **FirstName** en **LastName**, gescheiden door een komma (columnDelimiter).    
   * **Beschikbaarheid** is ingesteld op **Elk uur** (frequentie is ingesteld op elk uur en interval ingesteld op 1). Daarom zoekt Data Factory elk uur naar invoergegevens in de hoofdmap van de blobcontainer (adftutorial).

   Als u geen **fileName** opgeeft voor een **invoertabel**, worden alle bestanden en blobs uit de invoermap (folderPath) beschouwd als invoer. Als u een fileName opgeeft in de JSON, wordt alleen het opgegeven bestand of de opgegeven blob gezien als invoer.

   Als u geen **fileName** opgeeft voor een **uitvoertabel**, krijgen de bestanden die worden gegenereerd in **folderPath** een naam op basis van de volgende indeling: Data.<Guid\>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   Als u **folderPath** en **fileName** dynamisch wilt instellen op basis van de **SliceStart**-tijd, gebruikt u de eigenschap **partitionedBy**. In het volgende voorbeeld worden voor folderPath Year, Month en Day gebruikt van de SliceStart-waarde (tijd waarop is begonnen met het verwerken van het segment). Voor fileName wordt gebruikgemaakt van Hour van de SliceStart-waarde. Als er bijvoorbeeld een segment wordt geproduceerd voor 2016-10-20T08:00:00, wordt folderName ingesteld op wikidatagateway/wikisampledataout/2016/10/20 en wordt fileName ingesteld op 08.csv.

    ```json
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

   Zie de [naslaginformatie voor JSON-scriptverwerking](data-factory-data-movement-activities.md) voor meer informatie over JSON-eigenschappen.
2. Voer de volgende opdracht uit om de Data Factory-gegevensset te maken.

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json
    ```

### <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
In deze stap maakt u een uitvoergegevensset met de naam **EmpSQLTable**. Deze gegevensset wijst naar een SQL-tabel (emp) in de Azure SQL-database die wordt vertegenwoordigd door **AzureSqlLinkedService**. De pijplijn kopieert invoergegevens uit de invoerblob naar de tabel **emp**.

1. Maak een JSON-bestand met de naam **EmpSQLTable.json** in de map **C:\ADFGetStartedPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "EmpSQLTable",
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

   * Gegevensset **type** wordt ingesteld op **AzureSqlTable**.
   * **linkedServiceName** wordt ingesteld op **AzureSqlLinkedService**.
   * **tablename** wordt ingesteld op **emp**.
   * De tabel emp in de database bevat drie kolommen: **ID**, **FirstName** en **LastName**. ID is een identiteitskolom, zodat u alleen **FirstName** en **LastName** hoeft op te geven.
   * **Beschikbaarheid** is ingesteld op **Elk uur** (frequentie is ingesteld op elk uur en interval is ingesteld op 1). De Data Factory-service maakt elk uur een uitvoergegevenssegment in de tabel **emp** in de Azure SQL-database.
2. Voer de volgende opdracht uit om de data factory-gegevensset te maken.

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json
    ```

## <a name="create-a-pipeline"></a>Een pijplijn maken
In deze stap maakt u een pijplijn met **Kopieeractiviteit**. De pijplijn gebruikt **EmpTableFromBlob** als invoer en **EmpSQLTable** als uitvoer.

1. Maak een JSON-bestand met de naam **ADFTutorialPipeline.json** in de map **C:\ADFGetStartedPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "ADFTutorialPipeline",
        "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
                {
                    "name": "CopyFromBlobToSQL",
                    "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                    "type": "Copy",
                    "inputs": [{ "name": "EmpTableFromBlob" }],
                    "outputs": [{ "name": "EmpSQLTable" }],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "SqlSink"
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
            "start": "2016-08-09T00:00:00Z",
            "end": "2016-08-10T00:00:00Z",
            "isPaused": false
        }
     }
    ```
   Houd rekening met de volgende punten:

   * In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**.
   * De invoer voor de activiteit is ingesteld op **EmpTableFromBlob** en de uitvoer voor de activiteit is ingesteld op **EmpSQLTable**.
   * In het gedeelte **transformation** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type.

   Vervang de waarde van de eigenschap **start** door de huidige dag en die van de eigenschap **end** door de volgende dag. Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2016-10-14T16:32:41Z. In deze zelfstudie wordt de **eindtijd** gebruikt, maar dit is optioneel.

   Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9/9/9999** op als waarde voor de eigenschap **end**.

   In het voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.

   Zie de [naslaginformatie voor JSON-scriptverwerking](data-factory-data-movement-activities.md) voor meer informatie over JSON-eigenschappen.
2. Voer de volgende opdracht uit om de data factory-tabel te maken.

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

Gefeliciteerd. U hebt een Azure-data factory, gekoppelde services, tabellen en een pijplijn gemaakt. U hebt de pijplijn ook gepland.

## <a name="monitor-the-pipeline"></a>De pijplijn bewaken
In deze stap gebruikt u Azure PowerShell om te bewaken wat er gebeurt in een Azure-gegevensfactory.

1. Voer **Get-AzureRmDataFactory** uit en wijs de uitvoer toe aan een $df-variabele.

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
    ```

2. Voer **Get-AzureRmDataFactorySlice** uit voor meer informatie over alle segmenten van **EmpSQLTable**, de uitvoertabel van de pijplijn.  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00
    ```

   Vervang het jaar, de maand en de datum van de parameter **StartDateTime** door het huidige jaar, de huidige maand en de huidige datum. Deze instelling moet overeenkomen met de waarde **Start** in de JSON-pijplijn.

   U ziet 24 segmenten; één voor elk uur vanaf 12:00 uur ‘s nachts vandaag tot 12 uur 's nachts de volgende dag.

   **Voorbeelduitvoer:**

    ``` 
     ResourceGroupName : ADFTutorialResourceGroup
     DataFactoryName   : ADFTutorialDataFactoryPSH
     TableName         : EmpSQLTable
     Start             : 8/9/2016 12:00:00 AM
     End               : 8/9/2016 1:00:00 AM
     RetryCount        : 0
     Status            : Waiting
     LatencyStatus     :
     LongRetryCount    : 0
    ```
3. Voer **Get-AzureRmDataFactoryRun** uit om gegevens over het uitvoeren van de activiteit op te halen voor een **bepaald** segment. Wijzig de waarde van de parameter **StartDateTime** zodat deze overeenkomt met de **Start**-tijd van het segment uit de uitvoer. De waarde van **StartDateTime** moet de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben.

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2016-08-09T00:00:00
    ```

   De uitvoer ziet er uit als in het volgende uittreksel:

    ```  
    Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH
    TableName           : EmpSQLTable
    ProcessingStartTime : 8/9/2016 11:03:28 PM
    ProcessingEndTime   : 8/9/2016 11:04:36 PM
    PercentComplete     : 100
    DataSliceStart      : 8/9/2016 10:00:00 PM
    DataSliceEnd        : 8/9/2016 11:00:00 PM
    Status              : Succeeded
    Timestamp           : 8/9/2016 11:03:28 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy
    ```

Zie [Data Factory Cmdlet Reference][cmdlet-reference] (Naslaginformatie voor Data Factory-cmdlets) voor uitgebreide documentatie over Data Factory-cmdlets.

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u een Azure-gegevensfactory gemaakt om gegevens te kopiëren van een Azure-blob naar een Azure SQL-database. U hebt PowerShell gebruikt om de gegevensfactory, gekoppelde services, gegevenssets en pijplijn te maken. In deze zelfstudie hebt u de volgende hoofdstappen uitgevoerd:  

1. U hebt een Azure-**gegevensfactory** gemaakt.
2. U hebt **gekoppelde services** gemaakt:

   a. Een gekoppelde **Azure Storage**-service om uw Azure-opslagaccount te koppelen dat invoergegevens bevat.     
   b. Een gekoppelde **Azure SQL**-service om uw SQL-database te koppelen die uitvoergegevens bevat.
3. U hebt **gegevenssets** gemaakt waarin de invoer- en uitvoergegevens van pijplijnen worden beschreven.
4. U hebt een **pijplijn** gemaakt met **Kopieeractiviteit**, met **BlobSource** als de bron en **SqlSink** als de sink.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
|:--- |:--- |
| [Data Factory-cmdlet-verwijzing](/powershell/resourcemanager/azurerm.datafactories/v2.5.0/azurerm.datafactories) | Dit gedeelte bevat informatie over alle Data Factory-cmdlets |
| [Pijplijnen](data-factory-create-pipelines.md) |In dit artikel worden pijplijnen en activiteiten in Azure Data Factory nader uitgelegd. |
| [gegevenssets](data-factory-create-datasets.md) |Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) |In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |

[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md

