---
title: "Gegevens kopiëren naar/van Azure SQL Database | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Azure SQL Database met Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e5718cfdca4e12edcb98e79807ffe86d7be16b07
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Gegevens kopiëren van en naar Azure SQL Database met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-sql-connector.md)
> * [Versie 2 - Preview](../connector-azure-sql-database.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure SQL Database-connector in V2](../connector-azure-sql-database.md).

In dit artikel wordt uitgelegd hoe het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen naar en van Azure SQL Database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.  

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van Azure SQL Database** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar Azure SQL Database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Ondersteund verificatietype
Azure SQL Database-connector biedt ondersteuning voor basisverificatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een Azure SQL Database verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een Azure blob-opslag naar een Azure SQL database kopiëren wilt, maakt u twee gekoppelde services als u wilt uw Azure storage-account en de Azure SQL database koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure SQL Database zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
3. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de blob-container en de map waarin de invoergegevens. En maken van een andere gegevensset opgeven van de SQL-tabel in de Azure SQL-database waarin de gegevens die zijn gekopieerd uit de blobopslag. Zie voor eigenschappen van gegevensset die specifiek voor Azure Data Lake Store zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron- en SqlSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure SQL Database in Azure Blob-opslag kopiëren wilt, gebruikt u SqlSource en BlobSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor Azure SQL Database zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure SQL Database, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-sql-database) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure SQL Database: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een Azure SQL gekoppelde service wordt een Azure SQL database aan uw gegevensfactory. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Azure SQL gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **AzureSqlDatabase** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de eigenschap connectionString. Alleen basisverificatie wordt ondersteund. |Ja |

> [!IMPORTANT]
> Configureer [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) de databaseserver worden [toestaan van Azure Services voor toegang tot de server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Bovendien configureren als u gegevens naar Azure SQL Database kopiëren wilt van de externe Azure met inbegrip van on-premises gegevensbronnen met data factory-gateway, de juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL Database verzenden is.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Als u een gegevensset te vertegenwoordigen de invoer- of -gegevens in een Azure SQL database, die u stelt de eigenschap type van de gegevensset: **AzureSqlTable**. Stel de **linkedServiceName** eigenschap van de gegevensset op de naam van de Azure SQL gekoppelde service.  

Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureSqlTable** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en slechts één uitvoer produceert.

Dat eigenschappen beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Als u gegevens uit een Azure SQL-database verplaatst, u het brontype instellen in de kopieerbewerking naar **SqlSource**. Als u gegevens naar een Azure SQL database verplaatst, u stelt het sink-type in de kopieerbewerking naar **SqlSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SqlSource en SqlSink.

### <a name="sqlsource"></a>SqlSource
In de kopieeractiviteit, wanneer de bron van het type **SqlSource**, de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de bron van de Azure SQL Database om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie van de structuur van de gegevensset JSON gebruikt voor het samenstellen van een query (`select column1, column2 from mytable`) om uit te voeren op de Azure SQL-Database. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Als u werkt met **sqlReaderStoredProcedureName**, moet u toch een waarde opgeven voor de **tableName** eigenschap in de JSON van de gegevensset. Er zijn geen controles uitgevoerd voor deze tabel al.
>
>

### <a name="sqlsource-example"></a>Voorbeeld van SqlSource

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

**De definitie van de opgeslagen procedure:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a>SqlSink
**SqlSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. Zie voor meer informatie [herhaalbare kopiëren](#repeatable-copy). |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van een kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie [herhaalbare kopiëren](#repeatable-copy). |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die definieert hoe brongegevens in doeltabel, bijvoorbeeld toepast op komen upserts of transformeren met behulp van uw eigen zakelijke logica. <br/><br/>Houd er rekening mee deze opgeslagen procedure wordt **aangeroepen per batch**. Als u uitvoeren die slechts eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `sqlWriterCleanupScript` eigenschap. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Een typenaam van de tabel. |Nee |

#### <a name="sqlsink-example"></a>Voorbeeld van SqlSink

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-sql-database"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van de SQL-Database
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe om gegevens te kopiëren en naar Azure SQL Database en de Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure SQL Database naar Azure Blob
Dezelfde definieert de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [AzureSqlDatabase](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [SqlSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert timeseries gegevens (elk uur, dagelijks, enzovoort) uit een tabel in Azure SQL-database naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.  

**Azure SQL Database, gekoppelde service:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Zie de [Azure SQL gekoppelde Service](#linked-service) sectie voor een lijst van eigenschappen die worden ondersteund door deze gekoppelde service.

**Azure Blob-opslag gekoppelde service:**

```JSON
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
Zie de [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel voor een lijst van eigenschappen die ondersteund worden door deze gekoppelde service.


**Azure SQL invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in Azure SQL en bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens.

Instelling 'extern': 'true' informeert de Azure Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

Zie de [type-eigenschappen van Azure SQL gegevensset](#dataset) sectie voor een lijst van eigenschappen die worden ondersteund door dit type dataset.  

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
Zie de [type-eigenschappen van Azure Blob-gegevensset](data-factory-azure-blob-connector.md#dataset-properties) sectie voor een lijst van eigenschappen die worden ondersteund door dit type dataset.  

**Een kopieeractiviteit in een pijplijn met de SQL-bron en sink van Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
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
In het voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron van de Azure SQL Database om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie van de structuur van de gegevensset JSON gebruikt voor het samenstellen van een query uitvoeren op de Azure SQL-Database. Bijvoorbeeld: `select column1, column2 from mytable`. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

Zie de [Sql-bron](#sqlsource) sectie en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die ondersteund worden door SqlSource en BlobSink.

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure SQL Database
Het voorbeeld worden de volgende Data Factory-entiteiten gedefinieerd:  

1. Een gekoppelde service van het type [AzureSqlDatabase](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [SqlSink](#copy-activity-properties).

De voorbeeld-kopieën timeseries gegevens (elk uur, dagelijks, enzovoort) van Azure-blob naar een tabel in Azure SQL database om het uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL gekoppelde service:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Zie de [Azure SQL gekoppelde Service](#linked-service) sectie voor een lijst van eigenschappen die worden ondersteund door deze gekoppelde service.

**Azure Blob-opslag gekoppelde service:**

```JSON
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
Zie de [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel voor een lijst van eigenschappen die ondersteund worden door deze gekoppelde service.


**Azure Blob invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat deze tabel extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
Zie de [type-eigenschappen van Azure Blob-gegevensset](data-factory-azure-blob-connector.md#dataset-properties) sectie voor een lijst van eigenschappen die worden ondersteund door dit type dataset.

**Azure SQL Database uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam "MijnTabel" in Azure SQL. De tabel maken in Azure SQL met hetzelfde aantal kolommen, zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
Zie de [type-eigenschappen van Azure SQL gegevensset](#dataset) sectie voor een lijst van eigenschappen die worden ondersteund door dit type dataset.

**Een kopieeractiviteit in een pijplijn met Blob-bron en sink SQL:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

```JSON
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
            "name": "AzureSqlOutput"
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
Zie de [Sql Sink](#sqlsink) sectie en [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die ondersteund worden door SqlSink en BlobSource.

## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase
Deze sectie bevat een voorbeeld voor het kopiëren van gegevens uit een brontabel zonder een identiteitskolom een doeltabel met een identiteitskolom.

**Brontabel:**

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Doeltabel:**

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
U ziet dat de doeltabel een identiteitskolom.

**Bron gegevensset JSON-definitie**

```JSON
{
    "name": "SampleSource",
    "properties": {
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
**Doel-dataset JSON-definitie**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
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

Merk op dat als de bron en doel-tabel verschillende schema's zijn (doel heeft een extra kolom met de identiteit). In dit scenario moet u opgeven **structuur** eigenschap in de definitie van de gegevensset doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van opgeslagen procedure uit SQL-sink
Zie voor een voorbeeld van een opgeslagen procedure van SQL-sink in een kopieeractiviteit van een pijplijn wordt aangeroepen, [opgeslagen procedure voor SQL-sink aanroepen in de kopieerbewerking](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel. 

## <a name="type-mapping-for-azure-sql-database"></a>Toewijzing van het type voor Azure SQL Database
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar en van Azure SQL Database, worden de volgende toewijzingen gebruikt vanuit de SQL-type aan .NET-type en vice versa. De toewijzing is hetzelfde als de SQL Server gegevenstypetoewijzing voor ADO.NET.

| SQL Server Database Engine-type | .NET framework-type |
| --- | --- |
| bigint |Int64 |
| Binaire |Byte[] |
| bits |Boole-waarde |
| CHAR |Tekenreeks, Char] |
| datum |Datum en tijd |
| Datum en tijd |Datum en tijd |
| datetime2 |Datum en tijd |
| Datetimeoffset |DateTimeOffset |
| Decimale |Decimale |
| FILESTREAM-kenmerk (varbinary(max)) |Byte[] |
| Float |Double |
| Afbeelding |Byte[] |
| int |Int32 |
| Money |Decimale |
| nchar |Tekenreeks, Char] |
| ntext |Tekenreeks, Char] |
| numerieke |Decimale |
| nvarchar |Tekenreeks, Char] |
| echte |Single |
| ROWVERSION |Byte[] |
| smalldatetime |Datum en tijd |
| smallint |Int16 |
| smallmoney |Decimale |
| sql_variant |Object * |
| Tekst |Tekenreeks, Char] |
| tijd |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Herhaalbare kopiëren
Bij het kopiëren van gegevens naar SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de tabel sink standaard. Om uit te voeren in plaats daarvan een UPSERT, Zie [Repeatable schrijven naar SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel. 

Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
