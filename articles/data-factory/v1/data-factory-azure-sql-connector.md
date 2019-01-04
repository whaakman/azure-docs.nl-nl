---
title: Gegevens kopiëren naar/van Azure SQL Database | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar/van Azure SQL Database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9b7104d27e7f8c384c742a3988ad7400c232d162
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023036"
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Gegevens kopiëren naar en van Azure SQL Database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-sql-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-sql-database.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure SQL Database-connector in V2](../connector-azure-sql-database.md).

In dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory gebruiken om gegevens te verplaatsen naar en van Azure SQL Database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.  

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een Azure SQL Database** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar Azure SQL Database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Ondersteunde verificatietype
Azure SQL Database-connector biedt ondersteuning voor basisverificatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een Azure SQL-Database verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens van een Azure blob-opslag met een Azure SQL-database kopieert, u twee gekoppelde services om uw Azure storage-account en Azure SQL-database koppelen aan uw data factory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure SQL Database zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de blob-container en map die de invoergegevens bevat. En u maakt een andere gegevensset om op te geven van de SQL-tabel in de Azure SQL-database waarin de gegevens die zijn gekopieerd uit de blob-opslag. Zie voor de gegevensseteigenschappen die specifiek voor Azure Data Lake Store zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron en SqlSink als sink voor de kopieeractiviteit. Op dezelfde manier als u uit een Azure SQL Database naar Azure Blob Storage kopiëren wilt, gebruikt u SqlSource en BlobSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor Azure SQL Database zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure SQL Database, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-sql-database) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure SQL Database: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een Azure SQL gekoppelde service koppelt een Azure SQL database aan uw data factory. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor gekoppelde Azure SQL-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **AzureSqlDatabase** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de connectionString-eigenschap. Alleen basisverificatie wordt ondersteund. |Ja |

> [!IMPORTANT]
> Configureer [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) welke databaseserver moet worden [Azure-Services toegang tot de server toestaan](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Als u gegevens naar Azure SQL Database kopieert van buiten Azure met inbegrip van on-premises gegevensbronnen met data factory-gateway, Daarnaast juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL Database verzenden is configureren.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Als een gegevensset die de invoer- of -gegevens in een Azure SQL-database opgeven, stelt u de eigenschap type van de gegevensset in: **AzureSqlTable**. Stel de **linkedServiceName** eigenschap van de gegevensset in de naam van de Azure SQL gekoppelde service.  

Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureSqlTable** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Dat eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Als u gegevens uit een Azure SQL-database verplaatst, u het brontype instellen in de kopieeractiviteit naar **SqlSource**. Als u gegevens naar een Azure SQL database verplaatst, u stelt het sink-type in de kopieeractiviteit naar **SqlSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SqlSource en SqlSink.

### <a name="sqlsource"></a>SqlSource
In de kopieeractiviteit, wanneer de bron van het type **SqlSource**, de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource de Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron van de Azure SQL Database de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, de kolommen die zijn gedefinieerd in de structuur van de gegevensset JSON worden gebruikt om een query te maken (`select column1, column2 from mytable`) op de Azure SQL Database uit te voeren. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Bij het gebruik **sqlReaderStoredProcedureName**, u moet nog steeds een waarde opgeven voor de **tableName** eigenschap in de gegevensset JSON. Er zijn geen verdere controles uitgevoerd op deze tabel al.
>
>

### <a name="sqlsource-example"></a>Voorbeeld van de SqlSource

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

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. Zie voor meer informatie, [herhaalbare kopie](#repeatable-copy). |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van een kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie, [herhaalbare kopie](#repeatable-copy). |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |
| sqlWriterStoredProcedureName |De naam van de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in doeltabel, bijvoorbeeld toepast op upsert-bewerking of transformeren met behulp van uw eigen bedrijfslogica. <br/><br/>Houd er rekening mee worden deze opgeslagen procedure **per batch aangeroepen**. Als u uitvoeren die alleen wordt eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `sqlWriterCleanupScript` eigenschap. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Een typenaam van de tabel. |Nee |

#### <a name="sqlsink-example"></a>Voorbeeld van de SqlSink

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
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens naar en van Azure SQL Database en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure SQL Database naar Azure Blob
Dezelfde definieert de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [AzureSqlDatabase](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [SqlSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld time series worden gegevens gekopieerd (elk uur, dagelijks, enzovoort) uit een tabel in Azure SQL-database naar een blob eenmaal per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.  

**Gekoppelde service Azure SQL Database:**

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
Zie de [gekoppelde Azure SQL-Service](#linked-service) sectie voor een lijst van eigenschappen die worden ondersteund door deze gekoppelde service.

**Gekoppelde Azure Blob storage-service:**

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
Zie de [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel voor de lijst met eigenschappen die worden ondersteund door deze gekoppelde service.


**Azure SQL-invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Azure SQL en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens.

Instellen van "extern": "true" informeert de Azure Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

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

Zie de [typeproperties voor Azure SQL-gegevensset](#dataset) sectie voor een lijst van eigenschappen die worden ondersteund door dit gegevenssettype.  

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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
Zie de [typeproperties voor Azure Blob-gegevensset](data-factory-azure-blob-connector.md#dataset-properties) sectie voor een lijst van eigenschappen die worden ondersteund door dit gegevenssettype.  

**Een kopieeractiviteit in een pijplijn met de SQL-bron en sink voor Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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
In het voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron van de Azure SQL Database de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de structuur van de gegevensset JSON gebruikt voor het bouwen van een query op de Azure SQL Database uit te voeren. Bijvoorbeeld: `select column1, column2 from mytable`. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

Zie de [Sql-bron](#sqlsource) sectie en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door SqlSource en BlobSink.

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure SQL Database
Het voorbeeld definieert de volgende Data Factory-entiteiten:  

1. Een gekoppelde service van het type [AzureSqlDatabase](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [SqlSink](#copy-activity-properties).

De voorbeeld-kopieën time series-gegevens (elk uur, dagelijks, enzovoort) van Azure blob naar een tabel in Azure SQL database-uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

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
Zie de [gekoppelde Azure SQL-Service](#linked-service) sectie voor een lijst van eigenschappen die worden ondersteund door deze gekoppelde service.

**Gekoppelde Azure Blob storage-service:**

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
Zie de [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) artikel voor de lijst met eigenschappen die worden ondersteund door deze gekoppelde service.


**Azure Blob-invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling de Data Factory-service wordt geïnformeerd dat deze tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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
Zie de [typeproperties voor Azure Blob-gegevensset](data-factory-azure-blob-connector.md#dataset-properties) sectie voor een lijst van eigenschappen die worden ondersteund door dit gegevenssettype.

**Azure SQL Database-uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam 'MyTable' in Azure SQL. Als u verwacht de Blob-CSV-bestand dat bevat, moet u de tabel maken in Azure SQL met hetzelfde aantal kolommen. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

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
Zie de [typeproperties voor Azure SQL-gegevensset](#dataset) sectie voor een lijst van eigenschappen die worden ondersteund door dit gegevenssettype.

**Een kopieeractiviteit in een pijplijn met Blob-bron en sink voor SQL:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

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
Zie de [Sql-Sink](#sqlsink) sectie en [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door SqlSink en BlobSource.

## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase
Deze sectie bevat een voorbeeld voor het kopiëren van gegevens uit een brontabel zonder een id-kolom in een doeltabel met een id-kolom.

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

**Source dataset-JSON-definitie**

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
**Bestemming gegevensset JSON-definitie**

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

U ziet dat als de bron en doel-tabel hebben verschillende schema (doel heeft een extra kolom met de identiteit). In dit scenario, moet u opgeven **structuur** eigenschap in de definitie van doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van de opgeslagen procedure van SQL-sink
Zie voor een voorbeeld van een opgeslagen procedure van SQL-sink in een kopieeractiviteit van een pijplijn aanroepen [opgeslagen procedure voor SQL-sink aanroepen in de kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel. 

## <a name="type-mapping-for-azure-sql-database"></a>Toewijzing van het type voor Azure SQL Database
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar en van Azure SQL Database, worden de volgende toewijzingen gebruikt van SQL-type naar .NET-type en vice versa. De toewijzing is hetzelfde als de SQL Server gegevenstypetoewijzing voor ADO.NET.

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

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Herhaalbare kopiëren
Het kopiëren van gegevens naar SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Om uit te voeren in plaats daarvan een UPSERT, Zie [Repeatable schrijven naar SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel. 

Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
