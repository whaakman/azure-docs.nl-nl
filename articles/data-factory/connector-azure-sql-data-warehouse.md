---
title: "Gegevens kopiëren naar/van Azure SQL Data Warehouse met behulp van de Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron winkels met Azure SQL Data Warehouse (of) van SQL Data Warehouse voor ondersteunde sink stores met behulp van de Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: jingwang
ms.openlocfilehash: 9360c0ee90f9a4ffdffd7649505699f656833bbe
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure SQL Data Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Versie 2 - Preview](connector-azure-sql-data-warehouse.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van een Azure SQL Data Warehouse. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure SQL Data Warehouse-connector in V1](v1/data-factory-azure-sql-data-warehouse-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van/naar Azure SQL Data Warehouse kopiëren naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar Azure SQL Data Warehouse. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Azure SQL Data Warehouse-connector:

- Kopiëren van gegevens via SQL-verificatie.
- Als een bron ophalen van gegevens met behulp van SQL-query of een opgeslagen procedure.
- Als sink, bij het laden van gegevens met **PolyBase** of bulksgewijs invoegen. De voormalige is **aanbevolen** voor betere prestaties van de kopie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure SQL Data Warehouse-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure SQL Data Warehouse gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureSqlDW** | Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de eigenschap connectionString. Alleen basisverificatie wordt ondersteund. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |


> [!IMPORTANT]
> Configureer [Azure SQL Data Warehouse Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) en de databaseserver worden [toestaan van Azure Services voor toegang tot de server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Bovendien als u gegevens naar Azure SQL Data Warehouse van buiten Azure met inbegrip van on-premises gegevensbronnen met Self-hosted integratie Runtime kopieert, juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL-gegevens verzenden is configureren Het magazijn.

**Voorbeeld:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure SQL Data Warehouse-gegevensset.

Om gegevens te kopiëren van/naar Azure SQL Data Warehouse, stel de eigenschap type van de gegevensset **AzureSqlDWTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureSqlDWTable** | Ja |
| tableName |De naam van de tabel of weergave in de Azure SQL Data Warehouse-exemplaar waarnaar de gekoppelde service verwijst. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure SQL Data Warehouse-bron- en sink.

### <a name="azure-sql-data-warehouse-as-source"></a>Azure SQL Data Warehouse als bron

Om gegevens te kopiëren van Azure SQL Data Warehouse, stelt u het brontype in de kopieerbewerking naar **SqlDWSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SqlDWSource** | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

**Verwijst naar Let op:**

- Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de Azure SQL Data Warehouse-bron om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).
- Als u geen 'sqlReaderQuery' of 'sqlReaderStoredProcedureName' opgeeft, kolommen die zijn gedefinieerd in de sectie 'structuur' van de gegevensset JSON worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) worden uitgevoerd op basis van de Azure SQL Data Warehouse. Als de definitie van de gegevensset geen 'de structuur', worden alle kolommen uit de tabel geselecteerd.
- Als u werkt met **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de JSON van de gegevensset.

**Voorbeeld: met behulp van SQL-query**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Voorbeeld: het gebruik van opgeslagen procedure**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlDWSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**De definitie van de opgeslagen procedure:**

```sql
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

### <a name="azure-sql-data-warehouse-as-sink"></a>Azure SQL Data Warehouse als sink

Om gegevens te kopiëren naar Azure SQL Data Warehouse, stelt u het sink-type in de kopieerbewerking naar **SqlDWSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **SqlDWSink** | Ja |
| allowPolyBase |Geeft aan of PolyBase (indien van toepassing) gebruiken in plaats van BULKINSERT mechanisme. <br/><br/> **Met PolyBase is de aanbevolen manier om gegevens te laden in SQL Data Warehouse.** Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie voor beperkingen en meer informatie.<br/><br/>Toegestane waarden zijn: **True** (standaard), en **False**.  |Nee |
| polyBaseSettings |Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **true**. |Nee |
| rejectValue |Hiermee geeft u het nummer of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt.<br/><br/>Meer informatie over opties voor het weigeren van de PolyBase in de **argumenten** sectie van [maken EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) onderwerp. <br/><br/>Toegestane waarden zijn: 0 (standaard), 1, 2,... |Nee |
| rejectType |Hiermee geeft u op of de optie rejectValue is opgegeven als een letterlijke waarde of een percentage.<br/><br/>Toegestane waarden zijn: **waarde** (standaard), en **Percentage**. |Nee |
| rejectSampleValue |Bepaalt het aantal rijen om op te halen voordat de PolyBase berekent het percentage van de geweigerde rijen opnieuw.<br/><br/>Toegestane waarden zijn: 1, 2,... |Ja, als **rejectType** is **percentage** |
| useTypeDefault |Geeft aan hoe de ontbrekende waarden in tekstbestanden met scheidingstekens verwerken wanneer PolyBase gegevens uit het tekstbestand ophaalt.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [maken EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Toegestane waarden zijn: **True**, **False** (standaard). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. Geldt alleen als PolyBase wordt niet gebruikt.<br/><br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (de standaardwaarde is 10000) |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. Geldt alleen als PolyBase wordt niet gebruikt.<br/><br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in Azure SQL Data Warehouse in elke uitvoering. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |(#repeatability-tijdens-copy). |Een query-instructie. |Nee |

**Voorbeeld:**

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

Meer informatie over het gebruik van PolyBase in SQL Data Warehouse efficiënt uit de volgende sectie worden geladen.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse

Met behulp van  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  is een efficiënte manier van het laden van grote hoeveelheid gegevens in Azure SQL Data Warehouse met hoge doorvoer. Met behulp van PolyBase in plaats van het standaardmechanisme voor BULKINSERT ziet u een groot voordeel in de doorvoer. Zie [prestaties referentienummer kopiëren](copy-activity-performance.md#performance-reference) met gedetailleerde vergelijking. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](connector-azure-sql-data-warehouse.md).

* Als de brongegevens **Azure Blob- of Azure Data Lake Store**, en de indeling is compatibel met PolyBase, kunt u rechtstreeks kopiëren naar Azure SQL Data Warehouse met PolyBase. Zie  **[Direct kopiëren met PolyBase](#direct-copy-using-polybase)**  met details.
* Als uw brongegevensarchief en de indeling wordt oorspronkelijk niet ondersteund door PolyBase, kunt u de  **[gefaseerde kopiëren met PolyBase](#staged-copy-using-polybase)**  in plaats daarvan functie. Dit biedt u ook betere doorvoer door automatisch converteren van de gegevens naar de PolyBase-compatibele indeling en opslaan van gegevens in Azure Blob-opslag. Vervolgens worden gegevens geladen in SQL Data Warehouse.

### <a name="direct-copy-using-polybase"></a>Directe kopiëren met PolyBase

SQL Data Warehouse PolyBase ondersteuning rechtstreeks voor Azure-Blob en Azure Data Lake Store (met behulp van de service-principal) als bron en met vereisten voor een specifieke indeling. Als de brongegevens voldoet aan de criteria die in deze sectie beschreven, kunt u rechtstreeks van brongegevensarchief kopiëren naar Azure SQL Data Warehouse met PolyBase. Anders kunt u [gefaseerde kopiëren met PolyBase](#staged-copy-using-polybase).

> [!TIP]
> Om gegevens te kopiëren van Data Lake Store met SQL Data Warehouse efficiënt, meer wilt weten van [Azure Data Factory kunt u zelfs gemakkelijker en handige om inzichten bloot te van gegevens bij het gebruik van Data Lake Store met SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Als niet aan de vereisten wordt voldaan, wordt Azure Data Factory controleert de instellingen en automatisch terugvalt op het BULKINSERT mechanisme voor de verplaatsing van gegevens.

1. **Bron gekoppelde service** is van het type: **AzureStorage** of **AzureDataLakeStore**.
2. De **invoergegevensset** is van het type: **AzureBlob** of **AzureDataLakeStoreFile**, en de indeling Typ onder `type` eigenschappen is **OrcFormat** , **ParquetFormat**, of **TextFormat** met de volgende configuraties:

   1. `rowDelimiter`moet  **\n** .
   2. `nullValue`is ingesteld op **lege tekenreeks** (""), of `treatEmptyAsNull` is ingesteld op **true**.
   3. `encodingName`is ingesteld op **utf-8**, namelijk **standaard** waarde.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, en `skipLineCount` zijn niet opgegeven.
   5. `compression`kan **geen compressie**, **GZip**, of **Deflate**.

    ```json
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",
           "nullValue": "",
           "encodingName": "utf-8"
       },
       "compression": {
           "type": "GZip",
           "level": "Optimal"
       }
    },
    ```

3. Er is geen `skipHeaderLineCount` onder **BlobSource** of **AzureDataLakeStore** voor de kopieeractiviteit in de pijplijn.
4. Er is geen `sliceIdentifierColumnName` onder **SqlDWSink** voor de kopieeractiviteit in de pijplijn. (PolyBase zorgt ervoor dat alle gegevens worden bijgewerkt of niet in een enkel uitvoering bijgewerkt wordt. Als u de **herhaalbaarheid**, kunt u `sqlWriterCleanupScript`).

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-using-polybase"></a>Gefaseerde kopiëren met PolyBase

Als de brongegevens niet voldoet aan de criteria die zijn geïntroduceerd in de vorige sectie, kunt u kopiëren van gegevens via een tussentijdse staging Azure Blob Storage (kan niet voor Premium-opslag) inschakelen. In dit geval voert Azure Data Factory automatisch transformaties op de gegevens die voldoen aan de vereisten van de indeling van PolyBase gegevens vervolgens PolyBase gebruiken om gegevens te laden in SQL Data Warehouse en vervolgens opschonen uw tijdelijke gegevens van de Blob-opslag. Zie [kopie gefaseerde](copy-activity-performance.md#staged-copy) voor meer informatie over de werking kopiëren van gegevens via een gefaseerde installatie Azure-Blob in het algemeen.

Deze functie wilt gebruiken, maakt u een [gekoppelde Azure Storage-service](connector-azure-blob-storage.md#linked-service-properties) die verwijst naar de Azure Storage-Account met de tussentijdse blobopslag, geeft u de `enableStaging` en `stagingSettings` eigenschappen voor de Kopieeractiviteit, zoals wordt weergegeven in de volgende code:

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-when-using-polybase"></a>Aanbevolen procedures voor met PolyBase

De volgende secties bevatten aanvullende aanbevolen procedures voor de waarden die worden vermeld in [aanbevolen procedures voor Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Vereiste databasemachtiging

Voor het gebruik van PolyBase, moet de gebruiker wordt gebruikt om gegevens te laden in SQL Data Warehouse heeft de [machtiging "Beheer"](https://msdn.microsoft.com/library/ms191291.aspx) op de doeldatabase. Een manier om dat te bereiken is het toevoegen van die gebruiker als lid van de rol 'db_owner'. Meer informatie over hoe dat door de volgende [in deze sectie](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Typ de beperking rijgrootte en gegevens

Polybase-belastingen zijn beperkt tot laden rijen beide kleiner is dan **1 MB** en kan niet worden geladen VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX). Raadpleeg [hier](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Als u hebt de brongegevens met rijen van de omvang is groter dan 1 MB, wilt u mogelijk de brontabellen verticaal gesplitst in verschillende kleine netwerken waarbij de grootste rijgrootte van elk van de limiet niet overschrijdt. De tabellen voor kleinere kunnen vervolgens worden geladen met PolyBase en samengevoegd in Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>De bronklasse SQL Data Warehouse

Overweeg om de best mogelijke doorvoer behalen, grotere bronklasse toewijzen aan de gebruiker wordt gebruikt om gegevens te laden in SQL Data Warehouse met PolyBase. Meer informatie over hoe dat door de volgende [wijzigen van een voorbeeld van een gebruiker resource klasse](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).

### <a name="tablename-in-azure-sql-data-warehouse"></a>tableName in Azure SQL Data Warehouse

De volgende tabel bevat voorbeelden op het opgeven van de **tableName** eigenschap in de gegevensset JSON voor verschillende combinaties van schema en de tabelnaam.

| DB Schema | Tabelnaam | JSON-eigenschap tableName |
| --- | --- | --- |
| dbo |MyTable |MyTable of dbo. MyTable of [dbo]. [MijnTabel] |
| dbo1 |MyTable |dbo1. MyTable of [dbo1]. [MijnTabel] |
| dbo |My.Table |[My.Table] of [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Als u de volgende fout ziet, is het mogelijk een probleem met de waarde die u hebt opgegeven voor de eigenschap tableName. Zie de tabel voor de juiste manier waarden opgeven voor de eigenschap tableName JSON.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolommen met standaardwaarden

PolyBase-functie in de Data Factory accepteert alleen op dit moment is hetzelfde aantal kolommen in de doeltabel. Stel, u hebt een tabel met vier kolommen en een ervan is gedefinieerd met een standaardwaarde. De invoergegevens moet nog steeds vier kolommen bevatten. Een invoergegevensset 3 kolommen bieden zou resulteert in een fout die vergelijkbaar is met het volgende bericht:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL-waarde is een speciale vorm van de standaardwaarde. Als de kolom waarvoor null is toegestaan, kan de invoergegevens (in blob) voor de kolom leeg zijn (kan niet worden ontbreekt uit de invoer gegevensset). PolyBase voegt NULL zijn voor deze in de Azure SQL Data Warehouse.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>De gegevenstypetoewijzing voor Azure SQL Data Warehouse

Bij het kopiëren van gegevens van/naar Azure SQL Data Warehouse, worden de volgende toewijzingen van Azure SQL Data Warehouse-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Azure SQL Data Warehouse-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint |Int64 |
| Binaire |Byte] |
| bits |Boole-waarde |
| CHAR |Tekenreeks, Char] |
| datum |Datum en tijd |
| Datum en tijd |Datum en tijd |
| datetime2 |Datum en tijd |
| DateTimeOffset |DateTimeOffset |
| Decimale |Decimale |
| FILESTREAM-kenmerk (varbinary(max)) |Byte] |
| Float |dubbele |
| Afbeelding |Byte] |
| int |Int32 |
| Money |Decimale |
| nchar |Tekenreeks, Char] |
| ntext |Tekenreeks, Char] |
| numerieke |Decimale |
| nvarchar |Tekenreeks, Char] |
| echte |Single |
| ROWVERSION |Byte] |
| smalldatetime |Datum en tijd |
| smallint |Int16 |
| smallmoney |Decimale |
| sql_variant |Object * |
| Tekst |Tekenreeks, Char] |
| tijd |TimeSpan |
| tijdstempel |Byte] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte] |
| varchar |Tekenreeks, Char] |
| xml |XML |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).