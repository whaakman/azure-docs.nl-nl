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
ms.date: 03/13/2019
ms.author: jingwang
ms.openlocfilehash: 782027f19d4e82f26fc1265f25b86223386d7182
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57903382"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van Azure SQL Database Managed Instance met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure SQL Database Managed Instance. Dit is gebaseerd op de [activiteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Azure SQL Database Managed Instance kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren met het beheerde exemplaar. Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit worden ondersteund als bronnen en sinks, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek, ondersteunt deze Azure SQL Database Managed Instance-connector:

- Kopiëren van gegevens met behulp van SQL- of Windows-verificatie.
- Als een bron, het ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink, gegevens toevoegen aan een tabel van de bestemming of het aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren.

SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wordt niet ondersteund. 

## <a name="prerequisites"></a>Vereisten

Instellen van een zelf-hostende integratieruntime die toegang heeft tot de database voor het gebruik van gegevens kopiëren van een Azure SQL Database Managed Instance die zich in een virtueel netwerk. Zie voor meer informatie, [zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md).

Als u uw zelf-hostende integratieruntime in hetzelfde virtuele netwerk bevinden als uw beheerde exemplaar inricht, zorg ervoor dat uw integratie-runtime-machine in een ander subnet dan uw beheerde exemplaar. Als u uw zelf-hostende integratieruntime in een ander virtueel netwerk dan uw beheerde exemplaar inricht, kunt u een peering op virtueel netwerk of een virtueel netwerk met virtuele netwerk. Zie voor meer informatie, [verbinding maken met uw toepassing naar Azure SQL Database Managed Instance](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met de Azure SQL Database Managed Instance-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Azure SQL Database Managed Instance gekoppeld:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SqlServer**. | Ja. |
| connectionString |Deze eigenschap geeft u de connectionString-informatie die nodig is om verbinding met het beheerde exemplaar met behulp van SQL-verificatie of Windows-verificatie. Zie voor meer informatie de volgende voorbeelden. <br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook wachtwoord plaatsen in Azure Key Vault, en als het SQL-verificatie pull de `password` configuratie buiten de verbindingsreeks. Zie het JSON-voorbeeld onder de tabel en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja. |
| Gebruikersnaam |Deze eigenschap geeft de naam van een gebruiker als u Windows-verificatie gebruiken. Een voorbeeld is **domainname\\gebruikersnaam**. |Nee. |
| wachtwoord |Deze eigenschap geeft u een wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de naam van de gebruiker. Selecteer **SecureString** de connectionString-gegevens veilig opslaan in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Nee. |
| connectVia | Dit [integratieruntime](concepts-integration-runtime.md) wordt gebruikt voor verbinding met het gegevensarchief. Richt de zelf-hostende integratieruntime in hetzelfde virtuele netwerk bevinden als uw beheerde exemplaar. |Ja. |

>[!TIP]
>U ziet de foutcode 'UserErrorFailedToConnectToSqlServer' met een bericht, zoals "de sessielimiet voor de database is XXX en is bereikt." Als deze fout optreedt, voegt u toe `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

**Voorbeeld 1: SQL-verificatie gebruiken**

```json
{
    "name": "AzureSqlMILinkedService",
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

**Voorbeeld 3: Windows-verificatie gebruiken**

```json
{
    "name": "AzureSqlMILinkedService",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor gebruik voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database Managed Instance-gegevensset.

Om gegevens te kopiëren naar en van Azure SQL Database Managed Instance, stel de eigenschap type van de gegevensset in **SqlServerTable**. De volgende eigenschappen worden ondersteund:

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

Om gegevens te kopiëren naar Azure SQL Database Managed Instance, stelt u het sink-type in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de sectie kopiëren activiteit-sink:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **SqlSink**. | Ja. |
| WriteBatchSize |Deze eigenschap voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn gehele getallen voor het aantal rijen. |Nee (standaard: 10,000). |
| writeBatchTimeout |Deze eigenschap geeft u de wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn voor de tijdsduur. Een voorbeeld hiervan is "00: 30:00, ' die is 30 minuten. |Nee. |
| preCopyScript |Deze eigenschap geeft u een SQL-query voor de kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in het beheerde exemplaar. De toepassing wordt aangeroepen slechts één keer per exemplaar uitvoeren. U kunt deze eigenschap gebruiken om vooraf geladen gegevens op te schonen. |Nee. |
| sqlWriterStoredProcedureName |Deze naam is voor de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in de doeltabel van toepassing. Voorbeelden van procedures zijn upsert-bewerking of transformaties doen met behulp van uw eigen bedrijfslogica. <br/><br/>Deze opgeslagen procedure is *per batch aangeroepen*. Een bewerking die wordt slechts één keer worden uitgevoerd en heeft niets te met brongegevens, bijvoorbeeld verwijderen of afbreken, gebruikt u de `preCopyScript` eigenschap. |Nee. |
| storedProcedureParameters |Deze parameters worden gebruikt voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van de parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee. |
| sqlWriterTableType |Deze eigenschap geeft u een tabel typenaam die moet worden gebruikt in de opgeslagen procedure. De kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens die wordt gekopieerd met bestaande gegevens samenvoegen. |Nee. |

> [!TIP]
> Wanneer gegevens worden gekopieerd naar Azure SQL Database Managed Instance, de kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Als u een upsert of extra zakelijke logica, gebruikt u de opgeslagen procedure in SqlSink. Zie voor meer informatie, [aanroepen van een opgeslagen procedure uit een SQL-sink](#invoke-a-stored-procedure-from-a-sql-sink).

**Voorbeeld 1: Gegevens toevoegen**

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

**Voorbeeld 2: Een opgeslagen procedure aanroepen tijdens het kopiëren van upsert**

Meer details bekijken van [aanroepen van een opgeslagen procedure uit een SQL-sink](#invoke-a-stored-procedure-from-a-sql-sink).

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

## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase

Het volgende voorbeeld worden gegevens gekopieerd van een tabel met geen id-kolom in een doeltabel met een id-kolom.

**Brontabel**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```

**Doeltabel**

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
        "type": " SqlServerTable",
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

**Bestemming gegevensset JSON-definitie**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "SqlServerTable",
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

U ziet dat de bron en doel-tabel verschillende schema hebben. De doeltabel is een identiteitskolom. Geef de eigenschap "structuur" in de definitie van doel, waaronder de identiteitskolom niet in dit scenario.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aanroepen van een opgeslagen procedure uit een SQL-sink

Wanneer gegevens worden gekopieerd naar Azure SQL Database Managed Instance, kunt u een opgeslagen procedure geconfigureerd en aangeroepen met extra parameters die u opgeeft.

U kunt een opgeslagen procedure gebruiken bij het kopiëren van ingebouwde mechanismen niet voldoen aan het doel. Dit wordt doorgaans gebruikt wanneer een upsert (update + invoegen) of een extra verwerking moet worden uitgevoerd voordat de laatste invoeging van gegevens in de doeltabel. Extra verwerking kan taken zoals het samenvoegen van kolommen, opzoeken van aanvullende waarden en plaatst deze in meerdere tabellen bevatten.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in het beheerde exemplaar. Het voorbeeld wordt ervan uitgegaan dat invoergegevens en de 'Marketing'-sink-tabel drie kolommen: ProfileID, status en categorie. Uitvoeren van de upsert op basis van de kolom ProfileID en toepassen voor alleen een specifieke categorie.

**Uitvoergegevensset**

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

Definieer de sectie SqlSink als volgt in een kopieeractiviteit:

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

In de database, definieert u de opgeslagen procedure met dezelfde naam als SqlWriterStoredProcedureName. Het ingevoerde gegevens van de opgegeven bron worden verwerkt en voegt deze samen in de uitvoertabel. De parameternaam van het tabeltype in de opgeslagen procedure is hetzelfde als 'tableName' die gedefinieerd in de gegevensset.

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
| sql_variant |Object |
| tekst |Tekenreeks, Char] |
| time |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

>[!NOTE]
> Voor de gegevenstypen die zijn toegewezen aan de tijdelijke decimaal, ondersteunt Azure Data Factory momenteel precisie maximaal 28. Als u gegevens die groter zijn dan 28 precisie nodig hebt, kunt u overwegen converteren naar een tekenreeks in een SQL-query.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
