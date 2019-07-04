---
title: Gegevens kopiëren naar en van SQL Server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens naar en vanuit SQL Server-database die zich on-premises of in een Azure-VM met behulp van Azure Data Factory.
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
ms.openlocfilehash: a6767c7c8931898c44fd748dbe4299b8ed23eb9c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443279"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van SQL Server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-sqlserver-connector.md)
> * [Huidige versie](connector-sql-server.md)

In dit artikel bevat een overzicht over het gebruik van de kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een SQL Server-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt de gegevens van en naar een SQL Server-database kopiëren naar een ondersteunde sink-gegevensopslag. Of u kunt gegevens kopiëren van een ondersteund brongegevensarchief naar een SQL Server-database. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze SQL Server-connector ondersteunt name:

- Versies van SQL Server 2016, 2014, 2012, 2008 R2, 2008 en 2005.
- Kopiëren van gegevens met behulp van SQL- of Windows-verificatie.
- Als een bron, het ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink, gegevens toevoegen aan een tabel van de bestemming of het aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren.

>[!NOTE]
>SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wordt niet ondersteund door deze connector nu. Als u wilt omzeilen, kunt u een [algemene ODBC connector](connector-odbc.md) en een SQL Server ODBC-stuurprogramma. Ga als volgt [deze richtlijnen](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-2017) met ODBC-stuurprogramma downloaden en connection string configuraties.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van kopiëren van gegevens uit een SQL Server-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelf-hostende integratieruntime. Zie voor meer informatie, [zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md). De integratieruntime biedt een ingebouwde stuurprogramma voor SQL Server-database. U hoeft niet te handmatig een stuurprogramma installeren wanneer u gegevens van of naar de SQL Server-database kopiëren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor de SQL Server-database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de gekoppelde SQL Server-service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SqlServer**. | Ja |
| connectionString |Geef **connectionString** informatie die nodig is om verbinding maken met de SQL Server-database met behulp van SQL-verificatie of Windows-verificatie. Raadpleeg de volgende voorbeelden.<br/>Dit veld is gemarkeerd **SecureString** voor het veilig opslaan in Azure Data Factory. U kunt ook een wachtwoord plaatsen in Azure Key Vault. Als het SQL-verificatie, halen de `password` configuratie buiten de verbindingsreeks. Zie voor meer informatie het volgende op de tabel JSON-voorbeeld en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| userName |Geef een gebruikersnaam op als u Windows-verificatie gebruiken. Een voorbeeld is **domainname\\gebruikersnaam**. |Nee |
| password |Geef een wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de naam van de gebruiker. Dit veld is gemarkeerd **SecureString** voor het veilig opslaan in Azure Data Factory. U kunt [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Nee |
| connectVia | Dit [integratieruntime](concepts-integration-runtime.md) wordt gebruikt voor verbinding met het gegevensarchief. U kunt een zelf-hostende integratieruntime of de Azure integratieruntime gebruiken als uw gegevensarchief openbaar toegankelijk is. Indien niet opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. |Nee |

>[!TIP]
>Als u een fout opgetreden met de foutcode 'UserErrorFailedToConnectToSqlServer' en een bericht bereikt, zoals "XXX is in de sessielimiet voor de database en het is bereikt", voegt u toe `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

**Voorbeeld 1: SQL-verificatie gebruiken**

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

**Voorbeeld 2: SQL-verificatie gebruiken met een wachtwoord in Azure Key Vault**

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

**Voorbeeld 3: Windows-verificatie gebruiken**

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de SQL Server-gegevensset.

Om gegevens te kopiëren van en naar een SQL Server-database, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **SqlServerTable**. | Ja |
| tableName |Deze eigenschap is de naam van de tabel of weergave in de SQL Server-database-instantie waarnaar de gekoppelde service naar verwijst. | Nee voor bron, Ja voor sink |

**Voorbeeld**

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor gebruik voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de SQL Server-bron en sink.

### <a name="sql-server-as-a-source"></a>SQL Server als een bron

Als u wilt kopiëren van gegevens uit SQL Server, stelt u het brontype in de kopieeractiviteit naar **SqlSource**. De volgende eigenschappen worden ondersteund in de sectie voor bron activiteit kopiëren:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **SqlSource**. | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Deze eigenschap is de naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Deze parameters zijn voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

**Die u moet weten:**

- Als **sqlReaderQuery** is opgegeven voor **SqlSource**, de kopieeractiviteit deze query wordt uitgevoerd op basis van de SQL Server-bron, de gegevens op te halen. Ook kunt u een opgeslagen procedure door op te geven **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u niet of opgeven **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de sectie "structuur" van de gegevensset JSON worden gebruikt om een query samen te stellen. De query `select column1, column2 from mytable` wordt uitgevoerd op de SQL-Server. Als de definitie van de gegevensset geen "structuur", worden alle kolommen uit de tabel geselecteerd.

**Voorbeeld: Gebruik SQL-query**

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

**Voorbeeld: Gebruik een opgeslagen procedure**

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

### <a name="sql-server-as-a-sink"></a>SQL Server als een sink

> [!TIP]
> Meer informatie over de ondersteunde schrijven gedrag, configuraties en best practices van [Best practice om gegevens te laden in SQL Server](#best-practice-for-loading-data-into-sql-server).

Om gegevens te kopiëren naar SQL Server, stelt u het sink-type in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de sectie kopiëren activiteit-sink:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **SqlSink**. | Ja |
| writeBatchSize |Aantal rijen in de SQL-tabel moet worden ingevoegd *per batch*.<br/>Toegestane waarden zijn gehele getallen voor het aantal rijen. Standaard bepaalt Azure Data Factory dynamisch de batchgrootte van de juiste op basis van de rijgrootte. |Nee |
| writeBatchTimeout |Deze eigenschap geeft u de wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn voor de periode. Een voorbeeld hiervan is "00: 30:00" gedurende 30 minuten. |Nee |
| preCopyScript |Deze eigenschap geeft u een SQL-query voor de kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in SQL Server. De toepassing wordt aangeroepen slechts één keer per exemplaar uitvoeren. U kunt deze eigenschap gebruiken voor het opschonen van de vooraf geladen gegevens. |Nee |
| sqlWriterStoredProcedureName |Deze naam is voor de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in de doeltabel van toepassing.<br/>Deze opgeslagen procedure is *per batch aangeroepen*. Een bewerking die wordt slechts één keer worden uitgevoerd en heeft niets te met brongegevens, bijvoorbeeld verwijderen of afbreken, gebruikt u de `preCopyScript` eigenschap. |Nee |
| storedProcedureParameters |Deze parameters worden gebruikt voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van de parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Deze eigenschap geeft u een tabel typenaam die moet worden gebruikt in de opgeslagen procedure. De kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens die wordt gekopieerd met bestaande gegevens samenvoegen. |Nee |

**Voorbeeld 1: Gegevens toevoegen**

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

**Voorbeeld 2: Een opgeslagen procedure aanroepen tijdens het kopiëren van**

Meer details bekijken van [aanroepen van een opgeslagen procedure uit een SQL-sink](#invoke-a-stored-procedure-from-a-sql-sink).

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

Wanneer u gegevens naar SQL Server kopiëren, kunt u verschillende schrijfeigenschappen mogelijk nodig hebt:

- [Toevoeg-](#append-data): Mijn brongegevens heeft alleen nieuwe records.
- [Upsert](#upsert-data): Mijn brongegevens heeft zowel invoegingen en updates.
- [Overschrijven](#overwrite-the-entire-table): Ik wil de gehele dimensietabel elke keer laden.
- [Schrijf met aangepaste logica](#write-data-with-custom-logic): Moet ik extra verwerking vóór de laatste invoeging in de doeltabel.

Zie de betreffende secties voor informatie over het configureren in Azure Data Factory en best practices.

### <a name="append-data"></a>Gegevens toevoegen

Gegevens toevoegen, is het standaardgedrag van deze SQL Server-sink-connector. Azure Data Factory biedt een bulksgewijs invoegen te schrijven naar de tabel efficiënt. U kunt configureren van de bron en sink-dienovereenkomstig in de kopieeractiviteit.

### <a name="upsert-data"></a>Upsert-gegevens

**Optie 1:** Wanneer u een grote hoeveelheid gegevens wilt kopiëren, gebruikt u de volgende benadering te doen een upsert hebt: 

- Gebruik eerst een [tijdelijke tabel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) voor bulksgewijs laden alle records met behulp van de kopieeractiviteit. Omdat bewerkingen op tijdelijke tabellen worden niet geregistreerd, kunt u miljoenen records laden in een paar seconden.
- Een opgeslagen procedure-activiteit uitvoeren in Azure Data Factory om toe te passen een [samenvoegen](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) of invoegen/bijwerken-instructie. Gebruik de tijdelijke tabel als een bron voor het uitvoeren van alle bijwerkt of als één transactie invoegt. Op deze manier wordt het aantal retouren en logboekbewerkingen verminderd. Aan het einde van de opgeslagen procedure-activiteit, kan de tijdelijke tabel worden afgekapt als u wilt deze klaar is voor de volgende upsert-cyclus.

Als u bijvoorbeeld in Azure Data Factory, kunt u een pijplijn met een **Kopieeractiviteit** gekoppeld met een **Stored Procedure-activiteit**. De vorige worden gegevens gekopieerd van de brongegevensopslag naar een tijdelijke tabel van de database, bijvoorbeeld **##UpsertTempTable**, als de naam van de tabel in de gegevensset. De laatste vervolgens roept een opgeslagen procedure voor het samenvoegen van brongegevens van de tijdelijke tabel in de doeltabel en het opschonen van de tijdelijke tabel.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

In uw database, definieert u een opgeslagen procedure met de logica van samenvoegen, zoals het volgende voorbeeld, waarin wordt aangewezen uit de vorige opgeslagen procedure-activiteit. Wordt ervan uitgegaan dat het doel is de **Marketing** een tabel met drie kolommen: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom.

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

**Optie 2:** U kunt desgewenst ook [aanroepen van een opgeslagen procedure in de kopieeractiviteit](#invoke-a-stored-procedure-from-a-sql-sink). Deze aanpak wordt elke rij in de brontabel in plaats van bulksgewijs invoegen als de aanpak van standaard in de kopieeractiviteit, is niet geschikt voor grootschalige upsert uitgevoerd.

### <a name="overwrite-the-entire-table"></a>De hele tabel overschrijven

U kunt configureren dat de **preCopyScript** eigenschap in een kopie activiteit-sink. In dit geval voor elke kopieeractiviteit die wordt uitgevoerd, voert Azure Data Factory het script eerst. Vervolgens wordt de kopie voor het invoegen van de gegevens uitgevoerd. Bijvoorbeeld als u wilt overschrijven de hele tabel met de meest recente gegevens, Geef een script voor het eerst alle records verwijderen voordat u bulksgewijs laden van de nieuwe gegevens uit de bron.

### <a name="write-data-with-custom-logic"></a>Schrijven van gegevens met aangepaste logica

De stappen voor het schrijven van gegevens met aangepaste logica zijn vergelijkbaar met die wordt beschreven in de [Upsert gegevens](#upsert-data) sectie. Wanneer u nodig hebt om toe te passen extra verwerking vóór de laatste invoegen van de brongegevens in de doeltabel voor grootschalige, kunt u doen twee dingen: 

- Laden naar een tijdelijke tabel en klik vervolgens aanroepen van een opgeslagen procedure. 
- Een opgeslagen procedure tijdens het kopiëren van aanroepen.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aanroepen van een opgeslagen procedure uit een SQL-sink

Wanneer u gegevens naar een SQL Server-database kopieert, kunt ook u configureren en aanroepen van een gebruiker opgegeven opgeslagen procedure met extra parameters.

> [!TIP]
> Aanroepen van een opgeslagen procedure, verwerkt de gegevens per rij in plaats van via een bulkbewerking, wordt niet voor grootschalige kopiëren aanbevolen. Meer informatie uit [Best practice om gegevens te laden in SQL Server](#best-practice-for-loading-data-into-sql-server).

U kunt een opgeslagen procedure gebruiken bij het kopiëren van ingebouwde mechanismen niet voldoen aan het doel. Een voorbeeld daarvan is wanneer u wilt toepassen extra verwerking vóór de laatste invoeging van gegevens in de doeltabel. Voorbeelden van de extra verwerking zijn als u wilt samenvoegen van kolommen, aanvullende waarden opzoeken en gegevens invoegen in meer dan één tabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in de SQL Server-database. Wordt ervan uitgegaan dat de invoergegevens en de sink **Marketing** tabel elke drie kolommen bevatten: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

**Uitvoergegevensset:** De 'tableName' is de hetzelfde type parameter tabelnaam in uw opgeslagen procedure, zoals wordt weergegeven in het volgende script in de opgeslagen procedure:

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

Definieer de **SQL-sink** sectie in de kopieeractiviteit als volgt:

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

In de database, definieert u de opgeslagen procedure met dezelfde naam als **SqlWriterStoredProcedureName**. Het ingevoerde gegevens van de opgegeven bron worden verwerkt en samengevoegd met de uitvoertabel. De parameternaam van het tabeltype in de opgeslagen procedure is hetzelfde als **tableName** gedefinieerd in de gegevensset.

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

In de database, definieert het tabeltype met dezelfde naam als **sqlWriterTableType**. Het schema van het tabeltype is hetzelfde als het schema dat is geretourneerd door de invoergegevens.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

De opgeslagen procedure-functie maakt gebruik van [tabelwaardeparameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-sql-server"></a>De gegevenstypetoewijzing voor SQL Server

Wanneer u gegevens van en naar SQL Server kopiëren, worden de volgende toewijzingen van SQL Server-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieerbewerking het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| SQL Server-gegevenstype | Azure Data Factory tussentijdse gegevenstype |
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

## <a name="troubleshoot-connection-issues"></a>Verbindingsproblemen oplossen

1. Configureer uw SQL Server-exemplaar om externe verbindingen te accepteren. Start **SQL Server Management Studio**, met de rechtermuisknop op **server**, en selecteer **eigenschappen**. Selecteer **verbindingen** uit de lijst en selecteer de **externe verbindingen met deze server toestaan** selectievakje.

    ![Externe verbindingen inschakelen](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Zie voor gedetailleerde stappen [configureren van de RAS-server-configuratieoptie](https://msdn.microsoft.com/library/ms191464.aspx).

2. Start **SQL Server Configuration Manager**. Vouw **SQL Server-netwerkconfiguratie** voor het exemplaar dat u wilt gebruiken en selecteer **protocollen voor MSSQLSERVER**. Protocollen worden weergegeven in het rechter deelvenster. Schakel TCP/IP in met de rechtermuisknop op **TCP/IP** en selecteren **inschakelen**.

    ![Schakel TCP/IP in](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Zie voor meer informatie en alternatieve manieren van het inschakelen van TCP/IP-protocol, [in- of uitschakelen van een server network protocol](https://msdn.microsoft.com/library/ms191294.aspx).

3. Dubbelklik in het venster dezelfde **TCP/IP** starten de **TCP/IP-eigenschappen** venster.
4. Schakel over naar de **IP-adressen** tabblad. Schuif omlaag naar Zie de **IPAll** sectie. Noteer de **TCP-poort**. De standaardwaarde is **1433**.
5. Maak een **regel voor de Windows-Firewall** op de computer waarmee inkomend verkeer via deze poort. 
6. **Verbinding controleren**: Als u wilt verbinding maken met SQL Server met behulp van een volledig gekwalificeerde naam, SQL Server Management Studio uit een andere computer te gebruiken. Een voorbeeld is `"<machine>.<domain>.corp.<company>.com,1433"`.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
