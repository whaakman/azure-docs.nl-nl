---
title: Gegevens kopiëren naar en van Azure SQL Database Managed Instance met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het verplaatsen van gegevens en naar Azure SQL Database Managed Instance met behulp van Azure Data Factory.
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
ms.openlocfilehash: e68b522d5a0fe7c359d83fc436aa7a1fd2159198
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048591"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van Azure SQL Database Managed Instance met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure SQL Database Managed Instance. Dit is gebaseerd op de [activiteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Azure SQL Database Managed Instance kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren met het beheerde exemplaar. Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit worden ondersteund als bronnen en sinks, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek, ondersteunt deze Azure SQL Database Managed Instance-connector:

- Kopiëren van gegevens met behulp van SQL-verificatie.
- Als een bron, het ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink, gegevens toevoegen aan een tabel van de bestemming of het aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren.

SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wordt niet ondersteund. 

## <a name="prerequisites"></a>Vereisten

Voor toegang tot Azure SQL Database Managed Instance  **[openbaar eindpunt](../sql-database/sql-database-managed-instance-public-endpoint-securely.md)** , kunt u Azure IR ADF beheerd Zorg ervoor dat u niet alleen het openbare eindpunt inschakelen, maar ook openbaar eindpunt verkeer toestaan in de netwerkbeveiligingsgroep zodat de token wordt geen verbinding maken met uw database, door ADF [deze richtlijnen](../sql-database/sql-database-managed-instance-public-endpoint-configure.md).

Voor toegang tot Azure SQL Database Managed Instance **persoonlijke eindpunt**Stel een [zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md) die toegang heeft tot de database. Als u de zelf-hostende integratieruntime in hetzelfde virtuele netwerk bevinden als uw beheerde exemplaar inricht, zorg ervoor dat uw integratie-runtime-machine in een ander subnet dan uw beheerde exemplaar. Als u uw zelf-hostende integratieruntime in een ander virtueel netwerk dan uw beheerde exemplaar inricht, kunt u een peering op virtueel netwerk of een virtueel netwerk met virtuele netwerk. Zie voor meer informatie, [verbinding maken met uw toepassing naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met de Azure SQL Database Managed Instance-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Azure SQL Database Managed Instance gekoppeld:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SqlServer**. | Ja. |
| connectionString |Deze eigenschap geeft u de connectionString-informatie die nodig is om verbinding met het beheerde exemplaar met behulp van SQL-verificatie. Zie voor meer informatie de volgende voorbeelden. <br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook wachtwoord plaatsen in Azure Key Vault, en als het SQL-verificatie pull de `password` configuratie buiten de verbindingsreeks. Zie het JSON-voorbeeld onder de tabel en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja. |
| connectVia | Dit [integratieruntime](concepts-integration-runtime.md) wordt gebruikt voor verbinding met het gegevensarchief. U kunt zelfgehoste Cloudintegratieruntime of Azure Integration Runtime (als uw beheerde exemplaar openbaar eindpunt heeft en u ADF kunt voor toegang tot) gebruiken. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Ja. |

**Voorbeeld 1: SQL-verificatie gebruiken**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername:port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: SQL-verificatie gebruiken met een wachtwoord in Azure Key Vault**

```json
{
    "name": "AzureSqlMILinkedService",
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

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor gebruik voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database Managed Instance-gegevensset.

Om gegevens te kopiëren naar en van Azure SQL Database Managed Instance, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **SqlServerTable**. | Ja. |
| tableName |Deze eigenschap is de naam van de tabel of weergave in de databaseinstantie waarnaar de gekoppelde service naar verwijst. | Niet vereist voor de bron. Ja voor sink. |

**Voorbeeld**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor gebruik voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database Managed Instance-bron en sink.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Azure SQL Database Managed Instance als een bron

Om gegevens te kopiëren van Azure SQL Database Managed Instance, stelt u het brontype in de kopieeractiviteit naar **SqlSource**. De volgende eigenschappen worden ondersteund in de sectie voor bron activiteit kopiëren:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **SqlSource**. | Ja. |
| sqlReaderQuery |Deze eigenschap maakt gebruik van de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `select * from MyTable`. |Nee. |
| sqlReaderStoredProcedureName |Deze eigenschap is de naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee. |
| storedProcedureParameters |Deze parameters zijn voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van de parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee. |

Houd rekening met de volgende punten:

- Als **sqlReaderQuery** is opgegeven voor **SqlSource**, de kopieeractiviteit deze query wordt uitgevoerd op basis van de bron beheerd exemplaar van de gegevens op te halen. Ook kunt u een opgeslagen procedure door op te geven **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u niet of opgeven de **sqlReaderQuery** of **sqlReaderStoredProcedureName** eigenschap, de kolommen die zijn gedefinieerd in de sectie "structuur" van de gegevensset JSON worden gebruikt om een query samen te stellen. De query `select column1, column2 from mytable` wordt uitgevoerd op het beheerde exemplaar. Als de definitie van de gegevensset geen "structuur", worden alle kolommen uit de tabel geselecteerd.

**Voorbeeld: Gebruik een SQL-query**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**Voorbeeld: Gebruik een opgeslagen procedure**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**De definitie van de opgeslagen procedure**

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

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Azure SQL Database Managed Instance als een sink

> [!TIP]
> Meer informatie over de ondersteunde schrijven gedrag, configuraties en best practices van [Best practice voor het laden van gegevens in Azure SQL Database Managed Instance](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Om gegevens te kopiëren naar Azure SQL Database Managed Instance, stelt u het sink-type in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de sectie kopiëren activiteit-sink:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **SqlSink**. | Ja. |
| writeBatchSize |Aantal rijen dat moet worden ingevoegd in de SQL-tabel **per batch**.<br/>Toegestane waarden zijn gehele getallen voor het aantal rijen. Standaard bepalen Data Factory dynamisch van de juiste batchgrootte op basis van de rijgrootte.  |Nee |
| writeBatchTimeout |Deze eigenschap geeft u de wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn voor de tijdsduur. Een voorbeeld hiervan is "00: 30:00, ' die is 30 minuten. |Nee. |
| preCopyScript |Deze eigenschap geeft u een SQL-query voor de kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in het beheerde exemplaar. De toepassing wordt aangeroepen slechts één keer per exemplaar uitvoeren. U kunt deze eigenschap gebruiken om vooraf geladen gegevens op te schonen. |Nee. |
| sqlWriterStoredProcedureName |Deze naam is voor de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in de doeltabel van toepassing. <br/>Deze opgeslagen procedure is *per batch aangeroepen*. Een bewerking die wordt slechts één keer worden uitgevoerd en heeft niets te met brongegevens, bijvoorbeeld verwijderen of afbreken, gebruikt u de `preCopyScript` eigenschap. |Nee. |
| storedProcedureParameters |Deze parameters worden gebruikt voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van de parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee. |
| sqlWriterTableType |Deze eigenschap geeft u een tabel typenaam die moet worden gebruikt in de opgeslagen procedure. De kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens die wordt gekopieerd met bestaande gegevens samenvoegen. |Nee. |

**Voorbeeld 1: gegevens toevoegen**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

Meer details bekijken van [aanroepen van een opgeslagen procedure uit een SQL-sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database-managed-instance"></a>Best practices voor het laden van gegevens in Azure SQL Database Managed Instance

Wanneer u gegevens naar Azure SQL Database Managed Instance kopiëren, moet u wellicht gedrag verschillende schrijven:

- **[Toevoeg-](#append-data)** : Mijn brongegevens heeft alleen nieuwe records.
- **[Upsert](#upsert-data)** : Mijn brongegevens heeft zowel invoegingen en updates;
- **[Overschrijven](#overwrite-entire-table)** : Ik wil laden gehele dimensietabel telkens;
- **[Schrijf met aangepaste logica](#write-data-with-custom-logic)** : Moet ik extra verwerking vóór de laatste invoeging in de doeltabel.

Raadpleeg de respectievelijk de secties over het configureren van ADF en de aanbevolen procedures.

### <a name="append-data"></a>Gegevens toevoegen

Dit is het standaardgedrag van deze Azure SQL Database Managed Instance-sink-connector en ADF **bulksgewijs invoegen** efficiënt schrijven naar de tabel. U kunt gewoon configureren van de bron en sink-dienovereenkomstig in de kopieeractiviteit.

### <a name="upsert-data"></a>Upsert-gegevens

**Optie ik** (met name wanneer u grote hoeveelheden gegevens te kopiëren hebt aanbevolen): de **meeste biedt praktische aanpak** te doen van upsert, is het volgende: 

- Ten eerste, gebruikmaken van een [tijdelijke tabel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) voor bulksgewijs laden van alle records met activiteit kopiëren. Zolang er bewerkingen op tijdelijke tabellen worden niet geregistreerd, kunt u miljoenen records laden in een paar seconden.
- Een opgeslagen Procedure-activiteit uitvoeren in ADF om toe te passen een [samenvoegen](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) (of invoegen/bijwerken)-instructie en het gebruik van de tijdelijke tabel als bron voor het uitvoeren van alle bijwerkt of invoegt als één transactie, verminderen de hoeveelheid interactie en bewerkingen in een logboek. Aan het einde van de activiteit opgeslagen Procedure, kan de tijdelijke tabel als u wilt deze klaar is voor de volgende cyclus van upsert worden afgekapt. 

Als u bijvoorbeeld in Azure Data Factory, kunt u een pijplijn met een **Kopieeractiviteit** gekoppeld met een **Stored Procedure-activiteit** bij succes. De vorige worden gegevens gekopieerd van de brongegevensopslag naar een tijdelijke tabel bijvoorbeeld ' **##UpsertTempTable**' als de tabelnaam in uw gegevensset, de laatste wordt een opgeslagen Procedure voor het samenvoegen van brongegevens van de tijdelijke tabel in de doeltabel en opschonen tijdelijke tabel.

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

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aanroepen van een opgeslagen procedure uit een SQL-sink

Wanneer u gegevens naar Azure SQL Database Managed Instance kopiëren, kunt u ook configureren en aanroepen van een gebruiker opgegeven opgeslagen procedure met extra parameters.

> [!TIP]
> Aanroepen van opgeslagen procedure verwerkt de gegevens per rij in plaats van bulkbewerking, niet voor grootschalige kopiëren aangeraden wordt. Meer informatie uit [Best practice voor het laden van gegevens in Azure SQL Database Managed Instance](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

U kunt een opgeslagen procedure als ingebouwd kopiëren mechanismen niet aan het doel voldoen, bijvoorbeeld extra verwerking vóór de laatste invoeging van gegevens in de doeltabel van toepassing. Voorbeelden van de extra verwerking zijn kolommen samenvoegen, zoekt u aanvullende waarden en plaatst deze in meer dan één tabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in de SQL Server-database. Wordt ervan uitgegaan dat de invoer- en de sink **Marketing** tabel elke drie kolommen bevatten: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

**Uitvoergegevensset:** de 'tableName' moet dezelfde tabel type parameternaam in de opgeslagen procedure (Zie onderstaande script van de opgeslagen procedure).

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
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

In de database, het tabeltype met dezelfde naam als sqlWriterTableType te definiëren. Het schema van het tabeltype is hetzelfde als het schema dat is geretourneerd door de invoergegevens.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

De opgeslagen procedure-functie maakt gebruik van [tabelwaardeparameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>De gegevenstypetoewijzing voor Azure SQL Database Managed Instance

Wanneer gegevens worden gekopieerd en naar Azure SQL Database Managed Instance, worden de volgende toewijzingen van Azure SQL Database Managed Instance-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieeractiviteit wordt toegewezen vanuit de bron-schema en het gegevenstype voor de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| Azure SQL Database Managed Instance-gegevenstype | Azure Data Factory tussentijdse gegevenstype |
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
> Voor de gegevenstypen die zijn toegewezen aan de tijdelijke decimaal, ondersteunt Azure Data Factory momenteel precisie maximaal 28. Als u gegevens die groter zijn dan 28 precisie nodig hebt, kunt u overwegen converteren naar een tekenreeks in een SQL-query.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
