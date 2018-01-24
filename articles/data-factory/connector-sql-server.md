---
title: "Gegevens kopiëren naar/van de SQL Server met behulp van Azure Data Factory | Microsoft Docs"
description: Meer informatie over het verplaatsen van gegevens uit SQL Server-database on-premises of in een virtuele machine in Azure met Azure Data Factory.
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
ms.openlocfilehash: d6e5b27493a786daa604124d4572f51bae4bcb20
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-and-from-sql-server-using-azure-data-factory"></a>Gegevens kopiëren naar en van SQL Server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-sqlserver-connector.md)
> * [Versie 2 - Preview](connector-sql-server.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een SQL Server-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [SQL Server-connector in V1](v1/data-factory-sqlserver-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van/naar SQL Server-database kopiëren naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar SQL Server-database. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze SQL Server-connector:

- SQL Server versie 2016, 2014 2012, 2008 R2, 2008 en 2005
- Kopiëren van gegevens met **SQL** of **Windows** verificatie.
- Als een bron ophalen van gegevens met behulp van SQL-query of een opgeslagen procedure.
- Als sink, het toevoegen van gegevens naar de doeltabel of een opgeslagen procedure met aangepaste logica wordt aangeroepen tijdens het kopiëren.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van gegevens kopiëren van een SQL Server-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Runtime-integratie biedt een ingebouwde stuurprogramma voor SQL Server-database, dus u hoeft niet te handmatig installeren van een stuurprogramma bij het kopiëren van gegevens van/naar SQL Server-database.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar de connector voor SQL Server-database.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SQL Server gekoppeld-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SqlServer** | Ja |
| connectionString |ConnectionString informatie die nodig zijn voor de verbinding met de SQL Server-database met behulp van de SQL-verificatie of de Windows-verificatie opgeven. Dit veld markeren als een SecureString. |Ja |
| userName |Geef de gebruikersnaam als u Windows-verificatie gebruikt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString. |Nee |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld 1: via SQL-verificatie**

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

**Voorbeeld 2: het gebruik van Windows-verificatie**

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SQL Server-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van/naar SQL Server-database, **SqlServerTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **SqlServerTable** | Ja |
| tableName |De naam van de tabel of weergave in de SQL Server-instantie waarnaar de gekoppelde service verwijst. | Ja |

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SQL Server-bron- en sink.

### <a name="sql-server-as-source"></a>SQL Server als de bron

Om gegevens te kopiëren uit SQL Server, stelt u het brontype in de kopieerbewerking naar **SqlSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SqlSource** | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

**Verwijst naar Let op:**

- Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de SQL Server-bron om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).
- Als u geen 'sqlReaderQuery' of 'sqlReaderStoredProcedureName' opgeeft, kolommen die zijn gedefinieerd in de sectie 'structuur' van de gegevensset JSON worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) om uit te voeren op de SQL Server. Als de definitie van de gegevensset geen 'de structuur', worden alle kolommen uit de tabel geselecteerd.
- Als u werkt met **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de JSON van de gegevensset.

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

**Voorbeeld: het gebruik van opgeslagen procedure**

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

### <a name="sql-server-as-sink"></a>SQL Server as sink

Om gegevens te kopiëren naar SQL Server, stelt u het sink-type in de kopieerbewerking naar **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **SqlSink** | Ja |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in SQL Server. Er wordt slechts één keer worden aangeroepen per exemplaar uitvoeren. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die definieert hoe brongegevens in doeltabel, bijvoorbeeld toepast op komen upserts of transformeren met behulp van uw eigen zakelijke logica. <br/><br/>Houd er rekening mee deze opgeslagen procedure wordt **aangeroepen per batch**. Als u uitvoeren die slechts eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `preCopyScript` eigenschap. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn: naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Nee |

> [!TIP]
> Bij het kopiëren van gegevens naar SQL Server, de kopieeractiviteit worden gegevens toegevoegd aan de tabel sink standaard. Voor een UPSERT en aanvullende bedrijfslogica, gebruikt u de opgeslagen procedure in SqlSink. Meer informatie over meer details van [opgeslagen procedure wordt aangeroepen voor SQL Sink](#invoking-stored-procedure-for-sql-sink).

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

**Voorbeeld 2: een opgeslagen procedure aanroepen tijdens het kopiëren voor upsert**

Meer informatie over meer details van [opgeslagen procedure wordt aangeroepen voor SQL Sink](#invoking-stored-procedure-for-sql-sink).

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

Deze sectie bevat een voorbeeld waarin gegevens uit een brontabel met geen identiteitskolom naar een doeltabel met een identiteitskolom kopieert.

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

**Doel-dataset JSON-definitie**

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

Merk op dat als de bron en doel-tabel verschillende schema's zijn (doel heeft een extra kolom met de identiteit). In dit scenario moet u opgeven **structuur** eigenschap in de definitie van de gegevensset doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van opgeslagen procedure uit SQL-sink

Wanneer gegevens worden gekopieerd naar SQL Server-database, wordt in een gebruiker opgeslagen procedure kan worden geconfigureerd en worden aangeroepen met extra parameters opgegeven.

Een opgeslagen procedure kan worden gebruikt wanneer de ingebouwde kopie mechanismen verzorgen geen het doel. Dit wordt doorgaans gebruikt wanneer upsert (invoegen + bijwerken) of een extra verwerking (samenvoegen kolommen, opzoeken van extra waarden invoegen in meerdere tabellen, enz.) moet worden uitgevoerd voordat de laatste invoeging van gegevens in de doeltabel.

Het volgende voorbeeld laat zien hoe het gebruik van een opgeslagen procedure voor een upsert in een tabel in de SQL Server-database. Ervan uitgaande dat de invoergegevens en de sink 'Marketing' tabel heeft drie kolommen: ProfileID, status en categorie. Uitvoeren op basis van de kolom "ProfileID" upsert en gelden alleen voor een specifieke categorie.

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

Definieer de opgeslagen procedure met dezelfde naam als SqlWriterStoredProcedureName in uw database. Het ingevoerde gegevens uit de opgegeven bron- en merge in de uitvoertabel afgehandeld. U ziet dat de parameternaam van de opgeslagen procedure hetzelfde als de "tableName moet' gedefinieerd in de gegevensset.

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

## <a name="data-type-mapping-for-sql-server"></a>Toewijzing voor SQL server-gegevenstype

Bij het kopiëren van gegevens van/naar SQL Server, worden de volgende toewijzingen van SQL Server-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| SQL Server-gegevenstype | Data factory tussentijdse gegevenstype |
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
| tinyint |Int16 |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="troubleshooting-connection-issues"></a>Verbindingsproblemen oplossen

1. Configureer uw SQL Server om externe verbindingen te accepteren. Start **SQL Server Management Studio**, met de rechtermuisknop op **server**, en klik op **eigenschappen**. Selecteer **verbindingen** uit de lijst en het selectievakje **toestaan van externe verbindingen met de server**.

    ![Externe verbindingen inschakelen](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Zie [de serverconfiguratieoptie voor externe toegang configureren](https://msdn.microsoft.com/library/ms191464.aspx) voor gedetailleerde stappen.

2. Start **SQL Server Configuration Manager**. Vouw **SQL Server-netwerkconfiguratie** voor het exemplaar dat u wilt gebruiken en selecteer **protocollen voor MSSQLSERVER**. U ziet protocollen in het rechterdeelvenster. TCP/IP inschakelen met de rechtermuisknop op **TCP/IP** en te klikken op **inschakelen**.

    ![TCP/IP inschakelen](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Zie [in- of uitschakelen van een Server netwerkprotocol](https://msdn.microsoft.com/library/ms191294.aspx) voor meer informatie en alternatieve manieren TCP/IP-protocol in te schakelen.

3. Dubbelklik in het venster dezelfde **TCP/IP** starten **TCP/IP-eigenschappen** venster.
4. Overschakelen naar de **IP-adressen** tabblad. Schuif helemaal naar Zie **IPAll** sectie. Noteer de ** TCP-poort ** (standaardwaarde is **1433**).
5. Maak een **regel voor de Windows Firewall** op de computer waarmee binnenkomend verkeer via deze poort.  
6. **Verbinding controleren**: voor verbinding met de volledig gekwalificeerde naam SQL-Server, SQL Server Management Studio uit een andere computer te gebruiken. Bijvoorbeeld: `"<machine>.<domain>.corp.<company>.com,1433"`.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
