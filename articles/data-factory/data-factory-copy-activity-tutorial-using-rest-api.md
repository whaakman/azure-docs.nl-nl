<properties 
    pageTitle="Zelfstudie: een pijplijn maken met de kopieeractiviteit in REST API | Microsoft Azure" 
    description="In deze zelfstudie maakt u een Azure Data Factory-pijplijn met een kopieeractiviteit. Hiervoor gebruikt u REST API." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>


# Zelfstudie: een pijplijn maken met de kopieeractiviteit in REST API
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)

In deze zelfstudie ziet u hoe u een Azure-gegevensfactory maakt en bewaakt met een REST API. In de pijplijn in de gegevensfactory wordt gebruikgemaakt van een kopieeractiviteit om gegevens van Azure-blobopslag te kopiëren naar Azure SQL Database.

> [AZURE.NOTE] 
> Dit artikel omvat niet alle Data Factory-REST API. Zie [Naslaginformatie voor Data Factory-REST API](https://msdn.microsoft.com/library/azure/dn906738.aspx) voor uitgebreide documentatie over Data Factory-cmdlets.
  

## Vereisten

- Neem het artikel [Overzicht van de zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) door en voer de **vereiste** stappen uit.
- Installeer [Curl](https://curl.haxx.se/dlwiz/) op uw computer. U kunt in het hulpprogramma Curl REST-opdrachten gebruiken om een gegevensfactory te maken. 
- Volg de instructies in [dit artikel](../resource-group-create-service-principal-portal.md) voor het volgende: 
    1. Maak een webtoepassing met de naam **ADFCopyTutorialApp** in Azure Active Directory.
    2. Haal de **client-id** en **geheime sleutel** op. 
    3. Haal de **tenant-id** op. 
    4. Wijs de toepassing **ADFCopyTutorialApp** toe aan de rol **Inzender Data Factory**.  
- Installeer [Azure PowerShell](../powershell-install-configure.md).  
- Start **PowerShell** en voer de volgende opdracht uit. Houd Azure PowerShell open tot het einde van deze zelfstudie. Als u het programma sluit en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.
    1. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij de Azure Portal.
    
            Login-AzureRmAccount   
    2. Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven.

            Get-AzureRmSubscription 
    3. Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **&lt;NameOfAzureSubscription**&gt; door de naam van uw Azure-abonnement. 

            Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    1. Maak een Azure-resourcegroep met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren in PowerShell.  

            New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

        Als de resourcegroep al bestaat, geeft u aan of u deze wilt bijwerken (Y) of ongewijzigd wilt laten (N). 

        Voor sommige van de stappen in deze zelfstudie wordt ervan uitgegaan dat u de resourcegroep met de naam ADFTutorialResourceGroup gebruikt. Als u een andere resourcegroep gebruikt, moet u voor deze zelfstudie de naam van uw resourcegroep gebruiken in plaats van ADFTutorialResourceGroup.
  
## JSON-definities maken
Maak de volgende JSON-bestanden in de map waar curl.exe staat. 

### datafactory.json 
> [AZURE.IMPORTANT] Naam moet uniek zijn, dus het is raadzaam om voor- en/of achtervoegsels te gebruiken bij ADFCopyTutorialDF, zodat het een unieke naam wordt. 

    {  
        "name": "ADFCopyTutorialDF",  
        "location": "WestUS"
    }  

### azurestoragelinkedservice.json
> [AZURE.IMPORTANT] Vervang **accountname** en **accountkey** door de naam en sleutel van uw Azure Storage-account. Zie [Toegangssleutels voor opslag weergeven, kopiëren en opnieuw genereren](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys) voor meer informatie over het verkrijgen van uw toegangssleutel voor opslag.

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

### azuersqllinkedservice.json
> [AZURE.IMPORTANT] Vervang **servername**, **databasename**, **username** en **password** door de naam van uw Azure SQL-server, de naam van de SQL-database, het gebruikersaccount en het wachtwoord van het account.  

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }


### inputdataset.json

    {
      "name": "AzureBlobInput",
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

Met de JSON-definitie wordt een gegevensset gedefinieerd met de naam **AzureBlobInput**. Deze bevat de invoergegevens voor een activiteit in de pijplijn. Bovendien wordt hiermee aangegeven dat de invoergegevens zich in het bestand **emp.txt** bevinden dat is opgeslagen in de blobcontainer **adftutorial**. 

 Houd rekening met de volgende punten: 

- De gegevensset **type** wordt ingesteld op **AzureBlob**.
- **linkedServiceName** wordt ingesteld op **AzureStorageLinkedService**. 
- **folderPath** is ingesteld op de **adftutorial**-container en **fileName** is ingesteld op **emp.txt**.  
- De indeling **type** wordt ingesteld op **TextFormat**
- Er zijn twee velden in het tekstbestand: **FirstName** en **LastName**, gescheiden door een kommateken (**columnDelimiter**) 
- De **beschikbaarheid** wordt ingesteld op **elk uur** (de frequentie wordt ingesteld op elk uur en het interval wordt ingesteld op 1). Daarom zoekt Data Factory elk uur naar invoergegevens in de hoofdmap van de opgegeven blobcontainer (**adftutorial**). 

Als u geen **fileName** opgeeft voor een invoergegevensset, worden alle bestanden/blobs uit de invoermap (**folderPath**) beschouwd als invoer. Als u een fileName opgeeft in de JSON, wordt alleen het opgegeven bestand/de opgegeven blob gezien als invoer.

Als u geen **fileName** opgeeft voor een **uitvoertabel**, krijgen de bestanden die worden gegenereerd in **folderPath** een naam op basis van de volgende indeling: Data.&lt;Guid&gt;.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

Als u **folderPath** en **fileName** dynamisch wilt instellen op basis van de **SliceStart**-tijd, gebruikt u de eigenschap **partitionedBy**. In het volgende voorbeeld worden voor folderPath Year, Month en Day gebruikt van de SliceStart-waarde (tijd waarop is begonnen met het verwerken van het segment). Voor fileName wordt gebruikgemaakt van Hour van de SliceStart-waarde. Als er bijvoorbeeld een segment wordt geproduceerd voor 2014-10-20T08:00:00, wordt folderName ingesteld op wikidatagateway/wikisampledataout/2014/10/20 en wordt fileName ingesteld op 08.csv. 

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],


### outputdataset.json
    
    {
      "name": "AzureSqlOutput",
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


Met de JSON-definitie wordt een gegevensset gedefinieerd met de naam **AzureSqlOutput**. Deze bevat de uitvoergegevens voor een activiteit in de pijplijn. Bovendien geeft deze aan dat de resultaten zijn opgeslagen in de tabel: **emp** in de database die staat voor de AzureSqlLinkedService. In het gedeelte **availability** wordt opgegeven dat de uitvoergegevensset elk uur (frequentie: uur en interval: 1) wordt geproduceerd.

Houd rekening met de volgende punten: 

- De gegevensset **type** wordt ingesteld op **AzureSQLTable**.
- **linkedServiceName** wordt ingesteld op **AzureSqlLinkedService**.
- **tablename** wordt ingesteld op **emp**.
- De tabel emp in de database bevat drie kolommen: **ID**, **FirstName** en **LastName**. ID is een identiteitskolom, zodat u alleen **FirstName** en **LastName** hoeft op te geven.
- De **beschikbaarheid** wordt ingesteld op **elk uur** (de **frequentie** wordt ingesteld op **elk uur** en het **interval** wordt ingesteld op **1**).  De Data Factory-service maakt elk uur een uitvoergegevenssegment in de tabel **emp** in de Azure SQL-database.

### pipeline.json

    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
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
        "start": "2016-08-12T00:00:00Z",
        "end": "2016-08-13T00:00:00Z"
      }
    }


Houd rekening met de volgende punten:

- In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **CopyActivity**.
- De invoer voor de activiteit is ingesteld op **AzureBlobInput** en de uitvoer voor de activiteit is ingesteld op **AzureSqlOutput**.
- In het gedeelte **transformation** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type.

Vervang de waarde van de eigenschap **start** door de huidige dag en de waarde **end** door de volgende dag. U hoeft alleen de datum in te vullen en kunt de tijd overslaan. Dit wordt dan bijvoorbeeld 2015-02-03, wat gelijk staat aan 2015-02-03T00:00:00Z

Zowel de begin- als einddatum en -tijd moeten de [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601) hebben. Bijvoorbeeld: 2014-10-14T16:32:41Z. De **eindtijd** is optioneel, maar we gebruiken hem in deze zelfstudie. 

Als u geen waarde opgeeft voor de eigenschap **end**, wordt automatisch **start + 48 uur** gebruikt. Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**.

In het bovenstaande voorbeeld zijn er 24 gegevenssegmenten omdat er elk uur één gegevenssegment wordt gemaakt.
    
> [AZURE.NOTE] Zie [Anatomie van een pijplijn](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor meer informatie over de JSON-eigenschappen die in het voorgaande voorbeeld worden gebruikt.

## Globale variabelen instellen

Voer in Azure PowerShell de volgende opdrachten uit nadat u de waarden hebt vervangen door uw eigen waarden:

> [AZURE.IMPORTANT] Zie de sectie [Vereisten](#prerequisites) voor instructies over het verkrijgen van de client-id, het clientgeheim, de tenant-id en de abonnements-id.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "ADFCopyTutorialDF"

## Verifiëren met AAD 
Voer de volgende opdracht uit om te verifiëren met Azure Active Directory (AAD). 

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 

## Een gegevensfactory maken

In deze stap maakt u een Azure-gegevensfactory met de naam **ADFCopyTutorialDF**. Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn kan één of meer activiteiten bevatten. Bijvoorbeeld een kopieeractiviteit om gegevens te kopiëren van een brongegevensarchief naar een doelgegevensarchief. Een HDInsight Hive-activiteit om een Hive-script uit te voeren dat invoergegevens omzet in productuitvoergegevens. Voer de volgende opdracht uit om de gegevensfactory te maken: 

1. Wijs de opdracht toe aan de variabele met de naam **cmd**. 

    Bevestig dat de naam van de gegevensfactory die u hier opgeeft (ADFCopyTutorialDF) overeenkomt met de naam die is opgegeven in de **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gegevensfactory is gemaakt, ziet u de JSON voor de gegevensfactory in de **resultaten**. Anders wordt er een foutbericht weergegeven.  

        Write-Host $results

Houd rekening met de volgende punten:
 
- De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u deze fout ziet in de resultaten: **Naam gegevensfactory 'ADFCopyTutorialDF' is niet beschikbaar**, voert u de volgende stappen uit:  
    1. Wijzig de naam (bijvoorbeeld uwnaamADFCopyTutorialDF) in het **datafactory.json**-bestand.
    2. In de eerste opdracht waar de **$cmd**-variabele een waarde is toegewezen, vervangt u ADFCopyTutorialDF door de nieuwe naam en voert u de opdracht uit. 
    3. Voer de volgende twee opdrachten uit voor het aanroepen van de REST API om de gegevensfactory te maken en de resultaten van de bewerking af te drukken. 
    
    Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
- Als u Data Factory-exemplaren wilt maken, moet u bijdrager/beheerder zijn van het Azure-abonnement
- De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als DNS-naam en wordt daarmee ook voor iedereen zichtbaar.
- Als u de foutmelding **This subscription is not registered to use namespace Microsoft.DataFactory** ontvangt, voert u een van de volgende stappen uit en probeert u opnieuw te publiceren: 

    - Voer in Azure PowerShell de volgende opdracht uit om de Data Factory-provider te registreren. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        U kunt de volgende opdracht uitvoeren om te bevestigen dat de Data Factory-provider is geregistreerd. 
    
            Get-AzureRmResourceProvider
    - Meld u bij de [Azure Portal](https://portal.azure.com) aan met behulp van het Azure-abonnement en navigeer naar een Data Factory-blade of maak een gegevensfactory in de Azure Portal. Door deze actie wordt de provider automatisch voor u geregistreerd.

Voordat u een pijplijn maakt, moet u eerst enkele Data Factory-entiteiten maken. U maakt eerst gekoppelde services om de bron- en doelgegevensarchieven te koppelen aan uw gegevensarchief. Vervolgens definieert u in- en uitvoergegevenssets die de gegevens in de gekoppelde gegevensarchieven vertegenwoordigen. Ten slotte maakt u de pijplijn met een activiteit die deze gegevenssets gebruikt.

## Gekoppelde services maken
Met gekoppelde services worden gegevensarchieven of compute-services gekoppeld aan een Azure Data Factory. Een gegevensarchief kan Azure Storage, een Azure SQL Database of een on-premises SQL Server-database zijn die invoergegevens bevat of de uitvoergegevens opslaat van een Data Factory-pijplijn. Een compute-service is een service die invoergegevens verwerkt en die uitvoergegevens produceert. 

In deze stap maakt u twee gekoppelde services: **AzureStorageLinkedService** en **AzureSqlLinkedService**. Met de gekoppelde AzureStorageLinkedService-service wordt een Azure-opslagaccount gekoppeld en met AzureSqlLinkedService wordt een Azure SQL Database gekoppeld aan de gegevensfactory **ADFCopyTutorialDF**. Later in deze zelfstudie maakt u een pijplijn waarmee gegevens worden gekopieerd van een blobcontainer in AzureStorageLinkedService naar een SQL-tabel in AzureSqlLinkedService.

### Een gekoppelde Azure Storage-service maken
In deze stap koppelt u uw Azure Storage-account aan uw gegevensfactory. Bij deze zelfstudie gebruikt u het Azure Storage-account om invoergegevens op te slaan. 

1. Wijs de opdracht toe aan de variabele met de naam **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gekoppelde service is gemaakt, ziet u de JSON voor de gekoppelde service in de **resultaten**. Anders wordt er een foutbericht weergegeven.
  
        Write-Host $results

### Gekoppelde Azure SQL-service maken
In deze stap koppelt u uw Azure SQL Database aan uw gegevensfactory. In deze zelfstudie gebruikt u dezelfde Azure SQL Database voor het opslaan van de uitvoergegevens.

1. Wijs de opdracht toe aan de variabele met de naam **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gekoppelde service is gemaakt, ziet u de JSON voor de gekoppelde service in de **resultaten**. Anders wordt er een foutbericht weergegeven.
  
        Write-Host $results

## Gegevenssets maken

In de vorige stap hebt u de gekoppelde services **AzureStorageLinkedService** en **AzureSqlLinkedService** gemaakt om een Azure Storage-account en een Azure SQL Database te koppelen aan de gegevensfactory **ADFCopyTutorialDF**. In deze stap maakt u gegevenssets die staan voor de invoer- en uitvoergegevens voor de kopieeractiviteit in de pijplijn die u tijdens de volgende stap maakt. 

De invoergegevensset in deze zelfstudie verwijst naar een blobcontainer in Azure Storage waarnaar AzureStorageLinkedService wijst. De uitvoergegevensset verwijst naar een SQL-tabel in de Azure SQL-database waarnaar AzureSqlLinkedService wijst.  

### Azure Blob Storage en Azure SQL Database voorbereiden voor de zelfstudie
Voer de volgende stappen uit om Azure Blob Storage en de Azure SQL Database voor te bereiden voor gebruik in deze zelfstudie. 
 
* Maak een blobcontainer met de naam **adftutorial** in de Azure Blob Storage waar **AzureStorageLinkedService** naar verwijst. 
* Maak het tekstbestand **emp.txt** en upload het als blob naar de container **adftutorial**. 
* Maak een tabel met de naam **emp** in de Azure SQL Database waarnaar **AzureSqlLinkedService** verwijst.


1. Open Kladblok, plak de volgende tekst en sla het bestand als **emp.txt** op in de map **C:\ADFGetStartedPSH** op de vaste schijf. 

        John, Doe
        Jane, Doe
                
2. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/) om de container **adftutorial** te maken en om het bestand **emp.txt** te uploaden naar de container.

    ![Azure Opslagverkenner](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Gebruik het volgende SQL-script om de tabel **emp** te maken in uw Azure SQL Database.  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    Als u SQL Server 2014 op uw computer hebt geïnstalleerd: volg de instructies uit [Stap 2: verbinding maken met de SQL Database van de beherende Azure SQL Database met SQL Server Management Studio][sql-management-studio] om verbinding te maken met uw Azure SQL-server en om het SQL-script uit te voeren.

    Als de client geen toegang heeft tot de Azure SQL-server, moet u de firewall configureren voor uw Azure SQL-server zodat toegang vanaf uw apparaat (IP-adres) wordt toegestaan. Raadpleeg [dit artikel](../sql-database/sql-database-configure-firewall-settings.md) voor stappen waarmee u uw firewall kunt configureren voor uw Azure SQL-server.
        
### Invoergegevensset maken 
In deze stap maakt u een gegevensset met de naam **AzureBlobInput** die verwijst naar een blobcontainer in Azure Storage. Deze container wordt vertegenwoordigd door de gekoppelde **AzureStorageLinkedService**-service. Deze blobcontainer (**adftutorial**) plaatst de invoergegevens in het bestand **emp.txt**. 

1. Wijs de opdracht toe aan de variabele met de naam **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**. Anders wordt er een foutbericht weergegeven.
  
        Write-Host $results

### Uitvoergegevensset maken
In deze stap maakt u een uitvoertabel met de naam **AzureSqlOutput**. Deze gegevensset wijst naar een SQL-tabel (**emp**) in de Azure SQL-database die wordt vertegenwoordigd door **AzureSqlLinkedService**. De pijplijn kopieert invoergegevens uit de invoerblob naar de tabel **emp**. 

1. Wijs de opdracht toe aan de variabele met de naam **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**. Anders wordt er een foutbericht weergegeven.
  
        Write-Host $results 

## Pijplijn maken
In deze stap maakt u een pijplijn met een **kopieeractiviteit** die gebruikmaakt van **AzureBlobInput** als invoer en **AzureSqlOutput** als uitvoer.

1. Wijs de opdracht toe aan de variabele met de naam **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Voer de opdracht uit via **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Bekijk de resultaten. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**. Anders wordt er een foutbericht weergegeven.  

        Write-Host $results

**Gefeliciteerd.** U hebt een Azure-gegevensfactory gemaakt met een pijplijn die gegevens van Azure-blobopslag kopieert naar de Azure SQL Database.

## De pijplijn bewaken
In deze stap gebruikt u Data Factory-REST API voor het bewaken van segmenten die worden geproduceerd door de pijplijn.

    $ds ="AzureSqlOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;


    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }

Voer deze opdrachten uit totdat een segment de status **Gereed** of **Mislukt** heeft. Als het segment de status Gereed heeft, controleert u de uitvoergegevens in de **emp**-tabel in uw Azure SQL Database. 

Voor elk segment worden twee rijen gegevens uit het bronbestand gekopieerd naar de emp-tabel in de Azure SQL Database. U ziet daarom 24 nieuwe records in de emp-tabel wanneer alle segmenten zijn verwerkt (in de status Gereed). 


## Samenvatting
In deze zelfstudie hebt u REST API gebruikt om een Azure-gegevensfactory te maken voor het kopiëren van gegevens van een Azure-blob naar een Azure SQL Database. In deze zelfstudie hebt u de volgende hoofdstappen uitgevoerd:  

1.  U hebt een Azure-**gegevensfactory** gemaakt.
2.  U hebt **gekoppelde services** gemaakt:
    1. Een gekoppelde Azure Storage-service om uw Azure Storage-account te koppelen dat invoergegevens bevat.    
    2. Een gekoppelde Azure SQL-service om uw Azure SQL Database te koppelen die uitvoergegevens bevat. 
3.  U hebt **gegevenssets** gemaakt waarin de invoer- en uitvoergegevens van pijplijnen worden beschreven.
4.  U hebt een **pijplijn** gemaakt met een kopieeractiviteit met BlobSource als bron en SqlSink als sink. 

## Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) | Dit artikel biedt gedetailleerde informatie over de kopieeractiviteit die u tijdens deze zelfstudie hebt gemaakt. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |
| [Pijplijnen](data-factory-create-pipelines.md) | Met behulp van dit artikel krijgt u inzicht in de pijplijnen en activiteiten in Azure Data Factory en in de wijze waarop u deze kunt gebruiken om end-to-end gegevensgestuurde werkstromen te maken voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) | Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory.
| [Pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe u pijplijnen bewaakt en beheert en hoe u fouten hierin oplost met de app voor bewaking en beheer. 



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
 


<!--HONumber=Sep16_HO3-->


