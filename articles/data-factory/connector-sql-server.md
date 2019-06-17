---
title: Gegevens kopiëren naar/van SQL Server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens naar/van SQL Server-database die zich on-premises of in een Azure-VM met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: jingwang
ms.openlocfilehash: 230fe94820a00c276238a7f5ff189ecc817f3f96
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074047"
---
# <a name="copy-data-to-and-from-sql-server-using-azure-data-factory"></a>Gegevens kopiëren naar en van SQL Server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-sqlserver-connector.md)
> * [Huidige versie](connector-sql-server.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een SQL Server-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van/naar SQL Server-database kopiëren naar een ondersteunde sink-gegevensopslag, of gegevens kopiëren van een ondersteund brongegevensarchief naar SQL Server-database. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze SQL Server-connector ondersteunt name:

- SQL Server-versie 2016, 2014, 2012, 2008 R2, 2008 en 2005
- Kopiëren van gegevens met **SQL** of **Windows** verificatie.
- Het ophalen van gegevens met behulp van SQL-query of een opgeslagen procedure als de bron.
- Als sink, doeltabel of aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren van het toevoegen van gegevens.

SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van kopiëren van gegevens uit een SQL Server-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Integration Runtime biedt een ingebouwde stuurprogramma voor SQL Server-database, dus u hoeft voor het installeren van een stuurprogramma handmatig bij het kopiëren van gegevens van/naar SQL Server-database.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op SQL Server-database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de gekoppelde SQL Server-service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SqlServer** | Ja |
| connectionString |Geef connectionString informatie die nodig zijn voor het verbinding maken met de SQL Server-database met behulp van SQL-verificatie of Windows-verificatie. Raadpleeg de volgende voorbeelden.<br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook wachtwoord plaatsen in Azure Key Vault, en als het SQL-verificatie pull de `password` configuratie buiten de verbindingsreeks. Zie het JSON-voorbeeld onder de tabel en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja |
| userName |Geef de gebruikersnaam op als u van Windows-verificatie gebruikmaakt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| password |Geeft het wachtwoord op voor het gebruikersaccount dat u hebt opgegeven voor de userName. Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory, of [verwijs naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!TIP]
>Als u fout met foutcode als "UserErrorFailedToConnectToSqlServer" bereikt en wordt weergegeven, zoals 'de sessielimiet voor de database is XXX en is bereikt.', toe te voegen `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

**Voorbeeld 1: met behulp van SQL-verificatie**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: met behulp van SQL-verificatie met een wachtwoord in Azure Key Vault**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 3: met behulp van Windows-verificatie**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SQL Server-gegevensset.

Om gegevens te kopiëren van/naar SQL Server-database, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **SqlServerTable** | Ja |
| tableName |De naam van de tabel of weergave in de SQL Server-exemplaar waarnaar de gekoppelde service verwijst. | Nee voor bron, Ja voor sink |

**Voorbeeld:**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SQL Server-bron- en sinkblobpaden.

### <a name="sql-server-as-source"></a>SQL Server als de bron

Als u wilt kopiëren van gegevens uit SQL Server, stelt u het brontype in de kopieeractiviteit naar **SqlSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SqlSource** | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

**Die u moet weten:**

- Als de **sqlReaderQuery** is opgegeven voor de SqlSource de Kopieeractiviteit deze query wordt uitgevoerd op basis van de SQL Server-bron, de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).
- Als u 'sqlReaderQuery' of 'sqlReaderStoredProcedureName' niet opgeeft, de kolommen die zijn gedefinieerd in de sectie "structuur" van de gegevensset JSON worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) op de SQL-Server uit te voeren. Als de definitie van de gegevensset geen 'de structuur', worden alle kolommen uit de tabel geselecteerd.

**Voorbeeld: met behulp van SQL-query**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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

**Voorbeeld: met behulp van opgeslagen procedure**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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

### <a name="sql-server-as-sink"></a>SQL Server als sink

> [!TIP]
> Meer informatie over de ondersteunde schrijven gedrag, configuraties en best practices van [Best practice om gegevens te laden in SQL Server](#best-practice-for-loading-data-into-sql-server).

Om gegevens te kopiëren naar SQL Server, stelt u het sink-type in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **SqlSink** | Ja |
| writeBatchSize |Aantal rijen dat moet worden ingevoegd in de SQL-tabel **per batch**.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). Standaard bepalen Data Factory dynamisch van de juiste batchgrootte op basis van de rijgrootte. |Nee |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in SQL Server. Er wordt slechts één keer worden aangeroepen per exemplaar uitvoeren. U kunt deze eigenschap gebruiken voor het opschonen van de vooraf geladen gegevens. |Nee |
| sqlWriterStoredProcedureName |De naam van de opgeslagen procedure die over het toepassen van de brongegevens in doeltabel definieert.<br/>Houd er rekening mee worden deze opgeslagen procedure **per batch aangeroepen**. Als u uitvoeren die alleen wordt eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `preCopyScript` eigenschap. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Nee |

**Voorbeeld 1: gegevens toevoegen**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
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

**Voorbeeld 2: een opgeslagen procedure aanroepen tijdens het kopiëren van**

Meer details bekijken van [wordt aangeroepen opgeslagen procedure voor het SQL-Sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
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

## <a name="best-practice-for-loading-data-into-sql-server"></a>Best practices voor het laden van gegevens in SQL Server

Wanneer u gegevens naar SQL Server kopiëren, moet u wellicht gedrag verschillende schrijven:

- **[Toevoeg-](#append-data)** : Mijn brongegevens heeft alleen nieuwe records.
- **[Upsert](#upsert-data)** : Mijn brongegevens heeft zowel invoegingen en updates;
- **[Overschrijven](#overwrite-entire-table)** : Ik wil laden gehele dimensietabel telkens;
- **[Schrijf met aangepaste logica](#write-data-with-custom-logic)** : Moet ik extra verwerking vóór de laatste invoeging in de doeltabel.

Raadpleeg de respectievelijk de secties over het configureren van ADF en de aanbevolen procedures.

### <a name="append-data"></a>Gegevens toevoegen

Dit is het standaardgedrag van deze SQL Server-sink-connector en ADF **bulksgewijs invoegen** efficiënt schrijven naar de tabel. U kunt gewoon configureren van de bron en sink-dienovereenkomstig in de kopieeractiviteit.

### <a name="upsert-data"></a>Upsert-gegevens

**Optie ik** (met name wanneer u grote hoeveelheden gegevens te kopiëren hebt aanbevolen): de **meeste biedt praktische aanpak** te doen van upsert, is het volgende: 

- Ten eerste, gebruikmaken van een [tijdelijke tabel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) voor bulksgewijs laden van alle records met activiteit kopiëren. Zolang er bewerkingen op tijdelijke tabellen worden niet geregistreerd, kunt u miljoenen records laden in een paar seconden.
- Een opgeslagen Procedure-activiteit uitvoeren in ADF om toe te passen een [samenvoegen](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) (of invoegen/bijwerken)-instructie en het gebruik van de tijdelijke tabel als bron voor het uitvoeren van alle bijwerkt of invoegt als één transactie, verminderen de hoeveelheid interactie en bewerkingen in een logboek. Aan het einde van de activiteit opgeslagen Procedure, kan de tijdelijke tabel als u wilt deze klaar is voor de volgende cyclus van upsert worden afgekapt. 

Als u bijvoorbeeld in Azure Data Factory, kunt u een pijplijn met een **Kopieeractiviteit** gekoppeld met een **Stored Procedure-activiteit** bij succes. De vorige worden gegevens gekopieerd van de brongegevensopslag naar een tijdelijke tabel van de database, bijvoorbeeld ' **##UpsertTempTable**' als de tabelnaam in uw gegevensset, de laatste wordt een opgeslagen Procedure voor het samenvoegen van brongegevens van de tijdelijke tabel in de doeltabel, en het opschonen van de tijdelijke tabel.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

In de database, definieert u een opgeslagen Procedure met de logica van samenvoegen, zoals het volgende voorbeeld, waarin wordt aangewezen uit de bovenstaande Stored Procedure-activiteit. Ervan uitgaande dat het doel **Marketing** een tabel met drie kolommen: **ProfileID**, **status**, en **categorie**, en voer de upsert op basis van de **ProfileID** kolom.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**Optie II:** ook u kunt [aanroepen van de opgeslagen procedure in de kopieeractiviteit](#invoking-stored-procedure-for-sql-sink), terwijl deze benadering wordt uitgevoerd voor elke rij in de brontabel in plaats van gebruik te maken van bulksgewijs Opmerking invoegen als de aanpak van standaard in de kopieeractiviteit, waardoor deze niet geschikt voor grootschalige upsert.

### <a name="overwrite-entire-table"></a>Hele tabel overschrijven

U kunt configureren **preCopyScript** sink-eigenschap in de kopieeractiviteit, in dat geval voor elke kopie-activiteit die wordt uitgevoerd, ADF voert het script eerst, voert u de kopie voor het invoegen van de gegevens. Bijvoorbeeld, als u wilt overschrijven de hele tabel met de meest recente gegevens, kunt u een script voor het eerst alle records verwijderen voordat u de nieuwe gegevens uit de bron voor bulksgewijs laden.

### <a name="write-data-with-custom-logic"></a>Schrijven van gegevens met aangepaste logica

Net zoals beschreven in [Upsert gegevens](#upsert-data) sectie, wanneer u moet extra verwerking vóór de laatste invoeging van gegevens in de doeltabel toepassen kunt u een) voor de grote schaal, naar een tijdelijke tabel laadt en aanroepen van een opgeslagen procedure of een b) een opgeslagen procedure tijdens het kopiëren van aanroepen.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aanroepen van de opgeslagen procedure van SQL-sink

Het kopiëren van gegevens in SQL Server-database, kunt u ook configureren en aanroepen van een gebruiker opgegeven opgeslagen procedure met extra parameters.

> [!TIP]
> Aanroepen van opgeslagen procedure verwerkt de gegevens per rij in plaats van bulkbewerking, niet voor grootschalige kopiëren aangeraden wordt. Meer informatie uit [Best practice om gegevens te laden in SQL Server](#best-practice-for-loading-data-into-sql-server).

U kunt een opgeslagen procedure als ingebouwd kopiëren mechanismen niet aan het doel voldoen, bijvoorbeeld extra verwerking vóór de laatste invoeging van gegevens in de doeltabel van toepassing. Voorbeelden van de extra verwerking zijn kolommen samenvoegen, zoekt u aanvullende waarden en plaatst deze in meer dan één tabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in de SQL Server-database. Wordt ervan uitgegaan dat de invoer- en de sink **Marketing** tabel elke drie kolommen bevatten: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

**Uitvoergegevensset:** de 'tableName' moet dezelfde tabel type parameternaam in de opgeslagen procedure (Zie onderstaande script van de opgeslagen procedure).

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definieer de **SQL-sink** sectie als volgt in de kopieeractiviteit.

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

In de database, definieert u de opgeslagen procedure met dezelfde naam als de **SqlWriterStoredProcedureName**. Het ingevoerde gegevens van de opgegeven bron worden verwerkt en samengevoegd met de uitvoertabel. De parameternaam van het tabeltype in de opgeslagen procedure moet gelijk zijn aan de **tableName** gedefinieerd in de gegevensset.

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
      VALUES (source.ProfileID, source.State, source.Category);
END
```

In de database, het tabeltype met dezelfde naam als sqlWriterTableType te definiëren. U ziet dat het schema van het type dezelfde zijn als het schema dat is geretourneerd door de invoergegevens moet.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

De opgeslagen procedure-functie maakt gebruik van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-sql-server"></a>Toewijzing voor SQL server-gegevenstype

Bij het kopiëren van gegevens van/naar SQL Server, worden de volgende toewijzingen van SQL Server-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SQL Server-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Voor gegevenstypen kaarten naar tussentijdse decimaal, ADF momenteel precisie maximaal 28. Als u gegevens met een nauwkeurigheid groter dan 28 hebt, kunt u overwegen om te converteren naar een tekenreeks in SQL-query.

## <a name="troubleshooting-connection-issues"></a>Verbindingsproblemen oplossen

1. Uw SQL Server configureren voor externe verbindingen kan accepteren. Start **SQL Server Management Studio**, met de rechtermuisknop op **server**, en klikt u op **eigenschappen**. Selecteer **verbindingen** in de lijst en Controleer **toestaan van externe verbindingen met de server**.

    ![Externe verbindingen inschakelen](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Zie [configureren van de RAS-Server-configuratieoptie](https://msdn.microsoft.com/library/ms191464.aspx) voor gedetailleerde stappen.

2. Start **SQL Server Configuration Manager**. Vouw **SQL Server-netwerkconfiguratie** voor het exemplaar dat u wilt gebruiken en selecteer **protocollen voor MSSQLSERVER**. Hier ziet u protocollen in het rechterdeelvenster. Schakel TCP/IP in met de rechtermuisknop op **TCP/IP** en te klikken op **inschakelen**.

    ![Schakel TCP/IP in](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Zie [in- of uitschakelen van een Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) voor meer informatie en alternatieve manieren van het inschakelen van TCP/IP-protocol.

3. Dubbelklik in het venster dezelfde **TCP/IP** starten **TCP/IP-eigenschappen** venster.
4. Schakel over naar de **IP-adressen** tabblad. Schuif omlaag naar Zie **IPAll** sectie. Noteer de **TCP-poort** (de standaardwaarde is **1433**).
5. Maak een **regel voor de Windows-Firewall** op de computer waarmee inkomend verkeer via deze poort.  
6. **Verbinding controleren**: Voor verbinding met de volledig gekwalificeerde naam SQL-Server, SQL Server Management Studio uit een andere computer te gebruiken. Bijvoorbeeld: `"<machine>.<domain>.corp.<company>.com,1433"`.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
