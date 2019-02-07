---
title: Gegevens verplaatsen van en naar SQL Server | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens naar/van SQL Server-database die zich on-premises of in een Azure-VM met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 38070c3073febbdbea896c177ae68d4b9519314d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813378"
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Verplaatsen van gegevens naar en vanuit on-premises SQL Server of op IaaS (Azure VM) met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-sqlserver-connector.md)
> * [Versie 2 (huidige versie)](../connector-sql-server.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [SQL Server-connector in V2](../connector-sql-server.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens naar/van een on-premises SQL Server-database te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van een SQL Server-database** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met een SQL Server-database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Ondersteunde versies van SQL Server
De ondersteuning van dit SQL Server-connector is kopiëren van gegevens van/naar de volgende versies van exemplaar dat wordt gehost on-premises of in Azure IaaS met behulp van zowel de SQL-verificatie en de Windows-verificatie: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005

## <a name="enabling-connectivity"></a>Verbindingen inschakelen
De concepten en stappen die nodig zijn om verbinding te maken met on-premises SQL Server die wordt gehost of in virtuele machines van Azure IaaS (Infrastructure-as-a-Service) zijn hetzelfde. In beide gevallen moet u het gebruik van Data Management Gateway voor connectiviteit.

Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway. Instellen van een gatewayexemplaar is een vereiste is om verbinding te maken met SQL Server.

Terwijl u gateway op de dezelfde on-premises computer of cloud VM-exemplaar als de SQL Server voor betere prestaties installeren kunt, wordt u aangeraden dat u ze op afzonderlijke computers installeren. Met de gateway en de SQL Server op afzonderlijke computers vermindert conflicten tussen resources.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een on-premises SQL Server-database verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten.
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens van een SQL Server-database naar een Azure blob-opslag kopieert, u twee gekoppelde services om uw SQL Server-database en Azure storage-account koppelen aan uw data factory. Zie voor de gekoppelde service-eigenschappen die specifiek voor SQL Server-database zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de SQL-tabel in SQL Server-database die de invoergegevens bevat. En u een andere gegevensset om op te geven van de blob-container en de map waarin de gegevens die zijn gekopieerd van SQL Server-database maken. Zie voor de gegevensseteigenschappen die specifiek voor SQL Server-database zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u SqlSource als een bron- en BlobSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure Blob Storage naar SQL Server-Database kopiëren wilt, gebruikt u BlobSource en SqlSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor SQL Server-Database zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een on-premises SQL Server-database, [JSON voorbeelden](#json-examples-for-copying-data-from-and-to-sql-server) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke SQL Server:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
U maakt een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server-database koppelen aan een data factory. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor on-premises gekoppelde SQL Server-service.

De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de gekoppelde SQL Server-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |Geef connectionString informatie die nodig zijn voor het verbinding maken met de on-premises SQL Server-database met behulp van SQL-verificatie of Windows-verificatie. |Ja |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u van Windows-verificatie gebruikmaakt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **New-AzureRmDataFactoryEncryptValue** cmdlet te gebruiken in de verbindingsreeks, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Voorbeelden
**JSON voor het gebruik van SQL-verificatie**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**JSON voor het gebruik van Windows-verificatie**

Data Management Gateway zal zich voordoen als het opgegeven gebruikersaccount verbinding maken met de on-premises SQL Server-database.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
In de voorbeelden hebt u een gegevensset van het type gebruikt **SqlServerTable** om weer te geven van een tabel in een SQL Server-database.

Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (SQL Server, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **SqlServerTable** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de SQL Server-Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Als u gegevens uit een SQL Server-database verplaatst, u het brontype instellen in de kopieeractiviteit naar **SqlSource**. Als u gegevens naar een SQL Server-database verplaatst, u stelt het sink-type in de kopieeractiviteit naar **SqlSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SqlSource en SqlSink.

Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

### <a name="sqlsource"></a>SqlSource
Wanneer u de gegevensbron in een kopieeractiviteit is van het type **SqlSource**, de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. Kan verwijzen naar meerdere tabellen uit de database waarnaar wordt verwezen door de invoergegevensset. Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: Selecteer een MyTable. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource de Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron SQL Server-Database de gegevens op te halen.

U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de structuur gebruikt voor het bouwen van een select-query op de SQL Server-Database uit te voeren. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Bij het gebruik **sqlReaderStoredProcedureName**, u moet nog steeds een waarde opgeven voor de **tableName** eigenschap in de gegevensset JSON. Er zijn geen verdere controles uitgevoerd op deze tabel al.

### <a name="sqlsink"></a>SqlSink
**SqlSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |timespan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond opgeven. Zie voor meer informatie, [herhaalbare kopie](#repeatable-copy) sectie. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie, [herhaalbare kopie](#repeatable-copy) sectie. |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |
| sqlWriterStoredProcedureName |De naam van de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in doeltabel, bijvoorbeeld toepast op upsert-bewerking of transformeren met behulp van uw eigen bedrijfslogica. <br/><br/>Houd er rekening mee worden deze opgeslagen procedure **per batch aangeroepen**. Als u uitvoeren die alleen wordt eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `sqlWriterCleanupScript` eigenschap. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef de typenaam tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Een typenaam van de tabel. |Nee |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a>JSON-voorbeelden voor het kopiëren van gegevens van en naar SQL Server
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De volgende voorbeelden laten zien hoe het kopiëren van gegevens naar en van SQL Server en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van SQL Server naar Azure Blob
Het volgende voorbeeld laat zien:

1. Een gekoppelde service van het type [OnPremisesSqlServer](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [SqlSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld tijdreeksen worden gegevens gekopieerd van een SQL Server-tabel naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**Gekoppelde SQL Server-service**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob storage-gekoppelde service**

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
**SQL Server-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in SQL Server en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens. U kunt een query over meerdere tabellen binnen dezelfde database met behulp van een enkele gegevensset, maar één tabel moet worden gebruikt voor van de gegevensset tableName typeProperty.

Instellen van "extern": "true" wordt geïnformeerd Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Pijplijn met kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van deze gegevenssets voor invoer en uitvoer en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```
In dit voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron SQL Server-Database de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist). De sqlReaderQuery kunt verwijzen naar meerdere tabellen in de database waarnaar wordt verwezen door de invoergegevensset. Het is niet beperkt tot alleen de tabel als van de gegevensset tableName typeProperty instellen.

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de structuur worden gebruikt voor het bouwen van een select-query op de SQL Server-Database uit te voeren. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

Zie de [Sql-bron](#sqlsource) sectie en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door SqlSource en BlobSink.

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar SQL Server
Het volgende voorbeeld laat zien:

1. De gekoppelde service van het type [OnPremisesSqlServer](#linked-service-properties).
2. De gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en SqlSink.

De voorbeeld-kopieën time series-gegevens van een Azure blob naar een SQL-Server tabel per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde SQL Server-service**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob storage-gekoppelde service**

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
**Azure Blob-invoergegevensset**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**SQL Server-uitvoergegevensset**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam 'MyTable' in SQL Server. Maken van de tabel in SQL Server met hetzelfde aantal kolommen als u verwacht dat de Blob CSV-bestand bevatten. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Pijplijn met kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van deze gegevenssets voor invoer en uitvoer en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

```json
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ]
  }
}
```

## <a name="troubleshooting-connection-issues"></a>Verbindingsproblemen oplossen
1. Uw SQL Server configureren voor externe verbindingen kan accepteren. Start **SQL Server Management Studio**, met de rechtermuisknop op **server**, en klikt u op **eigenschappen**. Selecteer **verbindingen** in de lijst en Controleer **toestaan van externe verbindingen met de server**.

    ![Externe verbindingen inschakelen](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Zie [configureren van de RAS-Server-configuratieoptie](https://msdn.microsoft.com/library/ms191464.aspx) voor gedetailleerde stappen.
2. Start **SQL Server Configuration Manager**. Vouw **SQL Server-netwerkconfiguratie** voor het exemplaar dat u wilt gebruiken en selecteer **protocollen voor MSSQLSERVER**. Hier ziet u protocollen in het rechterdeelvenster. Schakel TCP/IP in met de rechtermuisknop op **TCP/IP** en te klikken op **inschakelen**.

    ![Schakel TCP/IP in](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    Zie [in- of uitschakelen van een Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) voor meer informatie en alternatieve manieren van het inschakelen van TCP/IP-protocol.
3. Dubbelklik in het venster dezelfde **TCP/IP** starten **TCP/IP-eigenschappen** venster.
4. Schakel over naar de **IP-adressen** tabblad. Schuif omlaag naar Zie **IPAll** sectie. Noteer de ** TCP-poort ** (de standaardwaarde is **1433**).
5. Maak een **regel voor de Windows-Firewall** op de computer waarmee inkomend verkeer via deze poort.
6. **Verbinding controleren**: Voor verbinding met de volledig gekwalificeerde naam SQL-Server, SQL Server Management Studio uit een andere computer te gebruiken. Bijvoorbeeld: "\<machine\>.\< domein\>. corp.\<bedrijf\>.com, 1433. "

   > [!IMPORTANT]

   > Zie [gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) voor gedetailleerde informatie.
   >
   > Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.
   >
   >


## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase
Deze sectie bevat een voorbeeld waarin gegevens uit een tabel met geen identiteitskolom gekopieerd naar een doeltabel met een id-kolom.

**Brontabel:**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```
**Doeltabel:**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

U ziet dat de doeltabel een identiteitskolom.

**Source dataset-JSON-definitie**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Bestemming gegevensset JSON-definitie**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

U ziet dat als de bron en doel-tabel hebben verschillende schema (doel heeft een extra kolom met de identiteit). In dit scenario, moet u opgeven **structuur** eigenschap in de definitie van doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van de opgeslagen procedure van SQL-sink
Zie [opgeslagen procedure voor SQL-sink aanroepen in de kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel voor een voorbeeld van een opgeslagen procedure van SQL-sink in een kopieeractiviteit van een pijplijn aanroepen.

## <a name="type-mapping-for-sql-server"></a>Toewijzing van het type voor SQL server
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, de Copy-activiteit voert een automatische conversie van de typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar en van SQL server, worden de volgende toewijzingen gebruikt van SQL-type naar .NET-type en vice versa.

De toewijzing is hetzelfde als de SQL Server gegevenstypetoewijzing voor ADO.NET.

| SQL Server Database Engine-type | .NET framework-type |
| --- | --- |
| bigint |Int64 |
| binaire bestanden |Byte[] |
| bits |Booleaans |
| CHAR |Tekenreeks, Char] |
| date |DateTime |
| Datum en tijd |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| decimaal |decimaal |
| FILESTREAM-kenmerk (varbinary(max)) |Byte[] |
| Float |Double-waarde |
| image |Byte[] |
| int |Int32 |
| money |decimaal |
| nchar |Tekenreeks, Char] |
| ntext |Tekenreeks, Char] |
| numerieke |decimaal |
| nvarchar |Tekenreeks, Char] |
| echte |Enkelvoudig |
| ROWVERSION |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |decimaal |
| sql_variant |Object * |
| tekst |Tekenreeks, Char] |
| time |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="mapping-source-to-sink-columns"></a>Toewijzing van bron naar het sink-kolommen
Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Herhaalbare kopiëren
Het kopiëren van gegevens naar SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Om uit te voeren in plaats daarvan een UPSERT, Zie [Repeatable schrijven naar SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel.

Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
