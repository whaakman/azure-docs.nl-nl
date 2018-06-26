---
title: Kopiëren van gegevens of naar Azure SQL Database met behulp van de Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit gegevensarchieven ondersteunde bron naar Azure SQL Database of SQL-Database tot gegevensarchieven ondersteunde sink met behulp van Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/05/2018
ms.author: jingwang
ms.openlocfilehash: 9b2acf622f33f5d1748c503ab4765b72c3d921e2
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751575"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure SQL Database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Versie 1, GA](v1/data-factory-azure-sql-connector.md)
> * [Versie 2, Preview](connector-azure-sql-database.md)

In dit artikel wordt uitgelegd hoe gebruiken Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van of naar Azure SQL Database. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel, hetgeen een algemeen overzicht van de Kopieeractiviteit toont.

> [!NOTE]
> In dit artikel is van toepassing op versie 2 van de Data Factory momenteel in preview. Als u versie 1 van de Data Factory-service, algemeen beschikbaar (GA), Zie [Azure SQL Database-connector in V1](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van of naar Azure SQL Database kopiëren naar een ondersteunde sink-gegevensarchief. En u kunt gegevens uit alle ondersteunde brongegevensarchief kopiëren naar Azure SQL Database. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen of PUT door Kopieeractiviteit de [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Azure SQL Database deze functies:

- Gegevens kopiëren van SQL-verificatie en Azure Active Directory (Azure AD)-toepassing-tokenverificatie met een service-principal of beheerde Service identiteit (MSI).
- Als een bron ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink gegevens toevoegen aan een doeltabel of een opgeslagen procedure met aangepaste logica aanroepen tijdens het kopiëren.

> [!IMPORTANT]
> Als u gegevens met behulp van Azure Data Factory-integratie Runtime kopieert, configureert u een [Azure SQL server-firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zodat Azure-Services toegang heeft tot de server.
> Als u gegevens met behulp van een host zichzelf integratie runtime kopieert, configureert u de Azure SQL server-firewall zodat het juiste IP-adresbereik. Dit bereik omvat ook de machine IP die wordt gebruikt voor het verbinden met Azure SQL Database.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor een Azure SQL Database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

Deze eigenschappen worden ondersteund voor een Azure SQL Database gekoppeld-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **AzureSqlDatabase**. | Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de **connectionString** eigenschap. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Geef de toepassing client-ID. | Ja, als u Azure AD-verificatie met een service-principal. |
| servicePrincipalKey | De sleutel van de toepassing opgeven. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja, als u Azure AD-verificatie met een service-principal. |
| tenant | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja, als u Azure AD-verificatie met een service-principal. |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of een host zichzelf integratie-runtime gebruiken als uw gegevensarchief in een particulier netwerk bevindt zich. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee |

Voor andere verificatietypen, Zie de volgende secties over vereisten en JSON-voorbeelden, respectievelijk:

- [SQL-verificatie](#sql-authentication)
- [Azure AD-toepassing tokenverificatie: Service-principal](#service-principal-authentication)
- [Azure AD-toepassing tokenverificatie: Service-identiteit beheerd](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>SQL-verificatie

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van SQL-verificatie

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

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Volg deze stappen voor het gebruik van een tokenverificatie voor service-principal gebaseerde Azure AD-toepassing:

1. **[Maken van een Azure Active Directory-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  vanuit de Azure-portal. Noteer de naam van de toepassing en de volgende waarden die definiëren van de gekoppelde service:

    - Toepassings-id
    - Sleutel van toepassing
    - Tenant-id

2. **[Inrichten van een Azure Active Directory-beheerder](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in de Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder moet een Azure AD-gebruiker of de Azure AD-groep, maar dit kan een service-principal niet. Deze stap is uitgevoerd, zodat in de volgende stap u een Azure AD-identiteit gebruiken kunt voor het maken van een ingesloten database-gebruiker voor de service principal.

3. **[Ingesloten databasegebruikers maken](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de service-principal. Verbinding maken met de database vanaf of waarnaar u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit die ten minste de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **De service-principal de vereiste machtigingen verlenen** zoals u dat gewend voor SQL-gebruikers of voor anderen bent. Voer de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Configureren van een service van Azure SQL Database gekoppeld** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van verificatie van de service-principal

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-service-identity-authentication"></a>Beheerde Service-identiteit verificatie

Een gegevensfactory kan worden gekoppeld aan een [beheerde Service-identiteit](data-factory-service-identity.md) die staat voor de specifieke gegevensfactory. U kunt deze service-identiteit gebruiken voor verificatie met Azure SQL Database. De aangewezen factory toegang tot en het kopiëren van gegevens van of naar de database met behulp van deze identiteit.

Volg deze stappen voor het gebruik van Azure AD op basis van het MSI-toepassing tokenverificatie:

1. **Een groep maken in Azure AD.** Maak de factory MSI lid van de groep.

    a. Zoek de identiteit van de data factory-service vanuit de Azure-portal. Ga naar uw gegevensfactory **eigenschappen**. Kopieer de identiteit van de-ID.

    b. Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u aan met behulp van de `Connect-AzureAD` opdracht. Voer de volgende opdrachten een groep maken en toevoegen van de gegevensfactory MSI als een lid.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Inrichten van een Azure Active Directory-beheerder](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in de Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder kan dit een Azure AD-gebruiker of Azure AD-groep. Als u de groep met MSI een beheerdersrol verleent, slaat u stap 3 en 4. De beheerder heeft volledige toegang tot de database.

3. **[Ingesloten databasegebruikers maken](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de Azure AD-groep. Verbinding maken met de database vanaf of waarnaar u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit die ten minste de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

4. **De Azure AD-groep nodig machtigingen verlenen** zoals u dat gewend voor de SQL-gebruikers en andere bent. Voer bijvoorbeeld de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

5. **Configureren van een service van Azure SQL Database gekoppeld** in Azure Data Factory.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van MSI-verificatie

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database-gegevensset.

Ingesteld om gegevens te kopiëren van of naar Azure SQL Database, de **type** eigenschap van de gegevensset **AzureSqlTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **AzureSqlTable**. | Ja |
| tableName | De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service naar verwijst. | Ja |

#### <a name="dataset-properties-example"></a>Voorbeeld van de DataSet-eigenschappen

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

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door de Azure SQL Database-bron- en sink ondersteund.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL Database als de bron

Ingesteld om gegevens te kopiëren van Azure SQL Database, de **type** eigenschap in de bron van de Kopieeractiviteit naar **SqlSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **SqlSource**. | Ja |
| sqlReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. | Nee |
| sqlReaderStoredProcedureName | De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. | Nee |
| storedProcedureParameters | Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. | Nee |

### <a name="points-to-note"></a>Punten met

- Als de **sqlReaderQuery** is opgegeven voor de **SqlSource**, Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron van de Azure SQL Database om de gegevens te verkrijgen. Of u kunt een opgeslagen procedure opgeven. Geef **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u geen ofwel opgeeft **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de **structuur** sectie van de gegevensset JSON worden gebruikt om Hiermee maakt u een query. `select column1, column2 from mytable` wordt uitgevoerd op Azure SQL Database. Als de definitie van de gegevensset beschikt niet over de **structuur**, alle kolommen uit de tabel zijn geselecteerd.
- Als u werkt met **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de JSON van de gegevensset.

#### <a name="sql-query-example"></a>Voorbeeld van de SQL-query

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

#### <a name="stored-procedure-example"></a>Voorbeeld van de opgeslagen procedure

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

### <a name="stored-procedure-definition"></a>Definitie van de opgeslagen procedure

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL Database als de sink

Ingesteld om gegevens te kopiëren naar Azure SQL Database, de **type** eigenschap in de kopieerbewerking sink naar **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit sink moet worden ingesteld op **SqlSink**. | Ja |
| writeBatchSize | Gegevens in de SQL-tabel invoegen wanneer de buffergrootte bereikt **writeBatchSize**.<br/> De toegestane waarde is **integer** (aantal rijen). | Nee. De standaardwaarde is 10000. |
| writeBatchTimeout | De wachttijd voor de batch invoegen bewerking is voltooid voordat er een optreedt time-out.<br/> De toegestane waarde is **timespan**. Voorbeeld: "00: 30:00 ' (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de Kopieeractiviteit uitvoeren voordat het schrijven van gegevens in Azure SQL Database. Er wordt slechts één keer aangeroepen per exemplaar uitvoeren. Gebruik deze eigenschap om de vooraf geladen gegevens op te schonen. | Nee |
| sqlWriterStoredProcedureName | De naam van de opgeslagen procedure die definieert het toepassen van gegevens in een doeltabel. Een voorbeeld is upserts of transformeren met behulp van uw eigen zakelijke logica. <br/><br/>Deze opgeslagen procedure is **aangeroepen per batch**. Voor bewerkingen die alleen keer uit te voeren en hebben niets te maken met de brongegevens, gebruikt u de `preCopyScript` eigenschap. Voorbeeld van de bewerkingen zijn verwijderen en afkappen. | Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn naam / waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. | Nee |
| sqlWriterTableType | Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. | Nee |

> [!TIP]
> Wanneer u gegevens naar Azure SQL Database kopiëren, Kopieeractiviteit worden gegevens toegevoegd aan de tabel sink standaard. Een upsert of aanvullende bedrijfslogica gebruikt u de opgeslagen procedure in **SqlSink**. Meer informatie over meer details van [aangeroepen opgeslagen procedure uit SQL Sink](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Voorbeeld van gegevens toevoegen

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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Een opgeslagen procedure aanroepen tijdens het kopiëren van upsert bijvoorbeeld

Meer informatie over meer details van [aangeroepen opgeslagen procedure uit SQL Sink](#invoking-stored-procedure-for-sql-sink).

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

Deze sectie wordt beschreven hoe u gegevens kopiëren van een brontabel zonder een identiteitskolom naar een doeltabel met een identiteitskolom.

#### <a name="source-table"></a>Brontabel

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Doeltabel

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> De doeltabel heeft een identiteitskolom.

#### <a name="source-dataset-json-definition"></a>Bron gegevensset JSON-definitie

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

#### <a name="destination-dataset-json-definition"></a>Doel-dataset JSON-definitie

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

> [!NOTE]
> De bron en doel-tabel verschillende schema's zijn. 

Het doel heeft een extra kolom met een identiteit. In dit scenario moet u de **structuur** eigenschap in de definitie van de gegevensset doel, waaronder de identiteitskolom niet.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aanroepen van opgeslagen procedure uit SQL-sink

Wanneer u gegevens naar Azure SQL Database kopiëren, kunt u ook configureren en een gebruiker is opgegeven, opgeslagen procedure met extra parameters worden aangeroepen.

U kunt een opgeslagen procedure ingebouwde kopie mechanismen niet hebben het doel. Ze doorgaans worden gebruikt wanneer een upsert, insert plus update of extra verwerking moet worden uitgevoerd voordat de laatste invoeging van gegevens in de doeltabel. Enkele voorbeelden van extra verwerking zijn samenvoegen kolommen, zoekt u naar aanvullende waarden en plaatst deze in meer dan één tabel.

Het volgende voorbeeld laat zien hoe het gebruik van een opgeslagen procedure voor een upsert in een tabel in Azure SQL Database. Wordt ervan uitgegaan dat de invoer- en de sink **Marketing** tabel elke drie kolommen hebben: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

#### <a name="output-dataset"></a>Uitvoergegevensset

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

Definieer de **SqlSink** sectie in Kopieeractiviteit:

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

In de database, definieert u de opgeslagen procedure met dezelfde naam als de **SqlWriterStoredProcedureName**. Deze verwerkt invoergegevens van de opgegeven bron en de uitvoertabel samengevoegd. De parameternaam van de opgeslagen procedure moet hetzelfde zijn als de **tableName** gedefinieerd in de gegevensset.

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

Definieer het tabeltype met dezelfde naam als in uw database, de **sqlWriterTableType**. Het schema van het type moet hetzelfde zijn als het schema geretourneerd door de invoergegevens zijn.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

De opgeslagen procedure wordt gebruikgemaakt van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>De gegevenstypetoewijzing voor Azure SQL Database

Wanneer u gegevens van of naar Azure SQL Database kopiëren, worden de volgende toewijzingen van Azure SQL Database-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in Kopieeractiviteit worden toegewezen aan de sink.

| Azure SQL Database-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint |Int64 |
| Binaire |Byte[] |
| bits |Boole-waarde |
| CHAR |Tekenreeks, Char] |
| datum |Datum en tijd |
| Datum en tijd |Datum en tijd |
| datetime2 |Datum en tijd |
| Datetimeoffset |DateTimeOffset |
| decimale |decimale |
| FILESTREAM-kenmerk (varbinary(max)) |Byte[] |
| Float |Double |
| installatiekopie |Byte[] |
| int |Int32 |
| Money |decimale |
| nchar |Tekenreeks, Char] |
| ntext |Tekenreeks, Char] |
| numerieke |decimale |
| nvarchar |Tekenreeks, Char] |
| echte |Enkelvoudig |
| ROWVERSION |Byte[] |
| smalldatetime |Datum en tijd |
| smallint |Int16 |
| smallmoney |decimale |
| sql_variant |Object * |
| tekst |Tekenreeks, Char] |
| tijd |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de Kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).
