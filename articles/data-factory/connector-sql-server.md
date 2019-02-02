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
ms.date: 02/01/2019
ms.author: jingwang
ms.openlocfilehash: 10ec490a6fe2044e1845efca94762b4ae1a42752
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657355"
---
# <a name="copy-data-to-and-from-sql-server-using-azure-data-factory"></a>Gegevens kopiëren naar en van SQL Server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
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
| Gebruikersnaam |Geef de gebruikersnaam op als u van Windows-verificatie gebruikmaakt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Nee |
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

Om gegevens te kopiëren van/naar SQL Server-database, stel de eigenschap type van de gegevensset in **SqlServerTable**. De volgende eigenschappen worden ondersteund:

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

Om gegevens te kopiëren naar SQL Server, stelt u het sink-type in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **SqlSink** | Ja |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in SQL Server. Er wordt slechts één keer worden aangeroepen per exemplaar uitvoeren. U kunt deze eigenschap gebruiken voor het opschonen van de vooraf geladen gegevens. |Nee |
| sqlWriterStoredProcedureName |De naam van de opgeslagen procedure waarmee wordt gedefinieerd hoe gegevens in doeltabel, bijvoorbeeld toepast op upsert-bewerking of transformeren met behulp van uw eigen bedrijfslogica. <br/><br/>Houd er rekening mee worden deze opgeslagen procedure **per batch aangeroepen**. Als u uitvoeren die alleen wordt eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `preCopyScript` eigenschap. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Nee |

> [!TIP]
> Het kopiëren van gegevens naar SQL Server, de kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Als u een UPSERT of extra zakelijke logica, gebruikt u de opgeslagen procedure in SqlSink. Meer details bekijken van [wordt aangeroepen opgeslagen procedure voor het SQL-Sink](#invoking-stored-procedure-for-sql-sink).

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

**Voorbeeld 2: een opgeslagen procedure aanroepen tijdens het kopiëren van upsert**

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

U ziet dat als de bron en doel-tabel hebben verschillende schema (doel heeft een extra kolom met de identiteit). In dit scenario, moet u opgeven **structuur** eigenschap in de definitie van doel, waaronder de identiteitskolom niet.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aanroepen van de opgeslagen procedure van SQL-sink

Het kopiëren van gegevens in SQL Server-database, wordt in een gebruiker opgeslagen procedure kan worden geconfigureerd en aangeroepen met extra parameters opgegeven.

Een opgeslagen procedure kan worden gebruikt bij het kopiëren van ingebouwde mechanismen, verzorgen geen daar het doel. Dit wordt meestal gebruikt wanneer upsert (insert en update) of extra verwerking (samenvoegen kolommen, opzoeken van extra waarden invoegen in meerdere tabellen, enz.) moet worden uitgevoerd voordat u de laatste invoeging van gegevens in de doeltabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in de SQL Server-database. Ervan uitgaande dat de invoergegevens en de sink-tabel "Marketing" heeft drie kolommen: ProfileID, status en categorie. Op basis van de kolom "ProfileID" upsert uitvoeren en alleen van toepassing voor een specifieke categorie.

**Uitvoergegevensset**

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

Definieer de sectie SqlSink als volgt in de kopieeractiviteit.

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

In de database, definieert u de opgeslagen procedure met dezelfde naam als SqlWriterStoredProcedureName. Het ingevoerde gegevens uit de opgegeven bron- en samenvoegen in de uitvoertabel afgehandeld. De parameternaam van het tabeltype in de opgeslagen procedure moet gelijk zijn aan de 'tableName' gedefinieerd in de gegevensset.

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

In de database, het tabeltype met dezelfde naam als sqlWriterTableType te definiëren. U ziet dat het schema van het type dezelfde zijn als het schema dat is geretourneerd door de invoergegevens moet.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

De opgeslagen procedure-functie maakt gebruik van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Als u naar Money/Smallmoney-gegevenstype door aanroepen opgeslagen Procedure schrijft, kunnen waarden worden afgerond. Geef het bijbehorende gegevenstype in TVP als decimaal in plaats van geld/Smallmoney te beperken.

## <a name="data-type-mapping-for-sql-server"></a>Toewijzing voor SQL server-gegevenstype

Bij het kopiëren van gegevens van/naar SQL Server, worden de volgende toewijzingen van SQL Server-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SQL Server-gegevenstype | Data factory tussentijdse gegevenstype |
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
