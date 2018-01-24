---
title: "Gegevens kopiëren naar/van Azure SQL Database met behulp van de Data Factory | Microsoft Docs"
description: "Ontdek hoe gegevens van ondersteunde bron gegevensarchieven naar Azure SQL Database (of) van SQL-Database kopiëren naar gegevensarchieven ondersteunde sink met behulp van de Data Factory."
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
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: a0074bd68dc9714eed9064e42c6e1c6d708d1100
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure SQL Database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-sql-connector.md)
> * [Versie 2 - Preview](connector-azure-sql-database.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een Azure SQL Database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure SQL Database-connector in V1](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van/naar Azure SQL Database kopiëren naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar Azure SQL Database. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Azure SQL Database:

- Kopiëren van gegevens via SQL-verificatie.
- Als een bron ophalen van gegevens met behulp van SQL-query of een opgeslagen procedure.
- Als sink, het toevoegen van gegevens naar de doeltabel of een opgeslagen procedure met aangepaste logica wordt aangeroepen tijdens het kopiëren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure SQL Database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure SQL Database gekoppeld-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureSqlDatabase** | Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de eigenschap connectionString. Alleen basisverificatie wordt ondersteund. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

> [!IMPORTANT]
> Configureer [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) de databaseserver worden [toestaan van Azure Services voor toegang tot de server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Bovendien, als u gegevens naar Azure SQL Database van buiten Azure, met inbegrip van on-premises gegevensbronnen met data factory zelf gehoste integratie Runtime kopieert, juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL verzenden is configureren De database.

**Voorbeeld:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure SQL Database-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van/naar Azure SQL Database, **AzureSqlTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureSqlTable** | Ja |
| tableName |De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service verwijst. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure SQL Database-bron- en sink.

### <a name="azure-sql-database-as-source"></a>Azure SQL Database als bron

Om gegevens te kopiëren van Azure SQL Database, stelt u het brontype in de kopieerbewerking naar **SqlSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SqlSource** | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

**Verwijst naar Let op:**

- Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de bron van de Azure SQL Database om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).
- Als u geen 'sqlReaderQuery' of 'sqlReaderStoredProcedureName' opgeeft, kolommen die zijn gedefinieerd in de sectie 'structuur' van de gegevensset JSON worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) om uit te voeren op de Azure SQL-Database. Als de definitie van de gegevensset geen 'de structuur', worden alle kolommen uit de tabel geselecteerd.
- Als u werkt met **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de JSON van de gegevensset.

**Voorbeeld: met behulp van SQL-query**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
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
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
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

### <a name="azure-sql-database-as-sink"></a>Azure SQL Database als sink

Om gegevens te kopiëren naar Azure SQL Database, kunt u het sink-type instellen in de kopieeractiviteit om te **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **SqlSink** | Ja |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (de standaardwaarde is 10000) |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in Azure SQL Database. Er wordt slechts één keer worden aangeroepen per exemplaar uitvoeren. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die definieert hoe brongegevens in doeltabel, bijvoorbeeld toepast op komen upserts of transformeren met behulp van uw eigen zakelijke logica. <br/><br/>Houd er rekening mee deze opgeslagen procedure wordt **aangeroepen per batch**. Als u uitvoeren die slechts eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `preCopyScript` eigenschap. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Nee |

> [!TIP]
> Bij het kopiëren van gegevens naar Azure SQL Database met de kopieerbewerking worden gegevens toegevoegd aan de tabel sink standaard. Voor een UPSERT en aanvullende bedrijfslogica, gebruikt u de opgeslagen procedure in SqlSink. Meer informatie over meer details van [opgeslagen procedure wordt aangeroepen voor SQL Sink](#invoking-stored-procedure-for-sql-sink).

**Voorbeeld 1: gegevens toevoegen**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Voorbeeld 2: een opgeslagen procedure aanroepen tijdens het kopiëren voor upsert**

Meer informatie over meer details van [opgeslagen procedure wordt aangeroepen voor SQL Sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase

Deze sectie bevat een voorbeeld voor het kopiëren van gegevens uit een brontabel zonder een identiteitskolom een doeltabel met een identiteitskolom.

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

**Bron gegevensset JSON-definitie**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Doel-dataset JSON-definitie**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Merk op dat als de bron en doel-tabel verschillende schema's zijn (doel heeft een extra kolom met de identiteit). In dit scenario moet u opgeven **structuur** eigenschap in de definitie van de gegevensset doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van opgeslagen procedure uit SQL-sink

Bij het kopiëren van gegevens in Azure SQL Database, een opgegeven gebruiker kan de procedure worden geconfigureerd en aangeroepen met extra parameters.

Een opgeslagen procedure kan worden gebruikt wanneer de ingebouwde kopie mechanismen verzorgen geen het doel. Dit wordt doorgaans gebruikt wanneer upsert (invoegen + bijwerken) of een extra verwerking (samenvoegen kolommen, opzoeken van extra waarden invoegen in meerdere tabellen, enz.) moet worden uitgevoerd voordat de laatste invoeging van gegevens in de doeltabel.

Het volgende voorbeeld laat zien hoe het gebruik van een opgeslagen procedure voor een upsert in een tabel in de Azure SQL Database. Ervan uitgaande dat de invoergegevens en de sink 'Marketing' tabel heeft drie kolommen: ProfileID, status en categorie. Uitvoeren op basis van de kolom "ProfileID" upsert en gelden alleen voor een specifieke categorie.

**Uitvoergegevensset**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definieer de sectie 'SqlSink' als volgt in de kopieerbewerking.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Definieer de opgeslagen procedure met dezelfde naam als 'SqlWriterStoredProcedureName' in de database. Het ingevoerde gegevens uit de opgegeven bron- en merge in de uitvoertabel afgehandeld. U ziet dat de parameternaam van de opgeslagen procedure hetzelfde als de "tableName moet' gedefinieerd in de gegevensset.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Definieer het tabeltype met dezelfde naam als sqlWriterTableType in uw database. U ziet dat het schema van het type hetzelfde zijn als het schema geretourneerd door de invoergegevens moet.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

De opgeslagen procedure wordt gebruikgemaakt van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>De gegevenstypetoewijzing voor Azure SQL Database

Bij het kopiëren van gegevens van/naar Azure SQL Database, worden de volgende toewijzingen van Azure SQL Database-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Azure SQL Database-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
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

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).