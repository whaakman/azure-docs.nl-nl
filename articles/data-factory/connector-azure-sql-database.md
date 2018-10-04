---
title: Gegevens kopiëren naar of van Azure SQL Database met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron-gegevensopslag naar Azure SQL Database of SQL-Database voor ondersteunde sink-gegevensopslag met behulp van Data Factory.
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
ms.date: 09/12/2018
ms.author: jingwang
ms.openlocfilehash: c513ef76174507f1ea78b265b1882266b8473737
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248938"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure SQL Database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Versie 1:](v1/data-factory-azure-sql-connector.md)
> * [Huidige versie](connector-azure-sql-database.md)

In dit artikel wordt uitgelegd hoe u gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van of naar Azure SQL Database. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel een algemeen overzicht van de Kopieeractiviteit geeft.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van of naar Azure SQL Database kopiëren naar een ondersteunde sink-gegevensopslag. En u kunt gegevens uit een ondersteund brongegevensarchief kopiëren naar Azure SQL Database. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT door Copy-activiteit, de [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek, ondersteunt deze Azure SQL Database-connector deze functies:

- Gegevens kopiëren met behulp van SQL-verificatie en tokenverificatie van Azure Active Directory (Azure AD)-toepassing met een service-principal of beheerde identiteiten voor Azure-resources.
- Als een bron ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink, gegevens toevoegen aan een doeltabel of een opgeslagen procedure met aangepaste logica aanroepen tijdens het kopiëren.

> [!IMPORTANT]
> Als u gegevens kopiëren met behulp van Azure Data Factory Integration Runtime, configureert u een [Azure SQL-serverfirewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zodat Azure-Services toegang hebben tot de server.
> Als u gegevens kopiëren met behulp van de zelf-hostende integratieruntime, configureert u de firewall van de Azure SQL-server zodat het juiste IP-adresbereik. Dit bereik omvat IP van de machine die wordt gebruikt om te verbinden met Azure SQL Database.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op een Azure SQL Database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

Deze eigenschappen worden ondersteund voor een gekoppelde Azure SQL Database-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **AzureSqlDatabase**. | Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de **connectionString** eigenschap. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja, als u Azure AD-verificatie met een service-principal. |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja, als u Azure AD-verificatie met een service-principal. |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja, als u Azure AD-verificatie met een service-principal. |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief bevindt zich in een particulier netwerk. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee |

Verwijzen respectievelijk naar de volgende secties over de vereisten en JSON-voorbeelden, voor andere verificatietypen:

- [SQL-verificatie](#sql-authentication)
- [Azure AD-toepassing-tokenverificatie: Service-principal](#service-principal-authentication)
- [Azure AD-toepassing-tokenverificatie: beheerde identiteiten voor een Azure-resources](#managed-service-identity-authentication)

>[!TIP]
>Als u fout met foutcode als "UserErrorFailedToConnectToSqlServer" bereikt en wordt weergegeven, zoals 'de sessielimiet voor de database is XXX en is bereikt.', toe te voegen `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

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

Volg deze stappen voor het gebruik van een tokenverificatie voor service-principal op basis van Azure AD-toepassing:

1. **[Maak een Azure Active Directory-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  vanuit Azure portal. Noteer de naam van de toepassing en de volgende waarden voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

1. **[Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder moet een Azure AD-gebruiker of de Azure AD-groep, maar mag niet een service-principal. Deze stap wordt uitgevoerd, zodat in de volgende stap u een Azure AD-identiteit gebruiken kunt een ingesloten databasegebruiker voor de service-principal maken.

1. **[Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de service-principal. Verbinding maken met de database vanaf of waarnaar u wilt kopiëren van gegevens met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. **De service-principal die nodig is machtigingen verlenen** zoals u gewend voor SQL-gebruikers of voor anderen bent. Voer de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. **Een gekoppelde Azure SQL Database-service configureren** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van verificatie van service-principal

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

### <a name="managed-identities-for-azure-resources-authentication"></a>Beheerde identiteiten voor verificatie van de Azure-resources

Een data factory, kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md) die staat voor de specifieke data factory. U kunt deze service-identiteit gebruiken voor Azure SQL Database-verificatie. Toegang heeft tot de aangewezen factory en gegevens kopiëren van of naar de database met behulp van deze identiteit.

Volg deze stappen voor het gebruik van Azure AD MSI-gebaseerde toepassing tokenverificatie:

1. **Een groep maken in Azure AD.** Controleer de factory MSI een lid van de groep.
    
    1. Zoek de data factory-service-identiteit vanuit Azure portal. Ga naar uw data factory **eigenschappen**. Kopieer de SERVICE-identiteit-ID.
    
    1. Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u aan met behulp van de `Connect-AzureAD` opdracht. Voer de volgende opdrachten een groep maken en toevoegen van de data factory MSI-bestand als een lid.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```
    
1. **[Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Als u de groep met MSI-bestand op een beheerdersrol toewijst, moet u de stappen 3 en 4 overslaan. De beheerder heeft volledige toegang tot de database.

1. **[Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de Azure AD-groep. Verbinding maken met de database vanaf of waarnaar u wilt kopiëren van gegevens met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

1. **De Azure AD-groep die nodig is machtigingen verlenen** zoals u gewend voor de SQL-gebruikers en anderen bent. Voer bijvoorbeeld de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

1. **Een gekoppelde Azure SQL Database-service configureren** in Azure Data Factory.

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database-gegevensset.

Instellen om gegevens te kopiëren van of naar Azure SQL Database, de **type** eigenschap van de gegevensset in **AzureSqlTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **AzureSqlTable**. | Ja |
| tableName | De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service naar verwijst. | Ja |

#### <a name="dataset-properties-example"></a>Voorbeeld van de gegevensset-eigenschappen

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database-bron en sink.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL-Database als de bron

Instellen om gegevens te kopiëren uit een Azure SQL Database, de **type** eigenschap in de bron van Kopieeractiviteit naar **SqlSource**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **SqlSource**. | Ja |
| sqlReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. | Nee |
| sqlReaderStoredProcedureName | De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. | Nee |
| storedProcedureParameters | Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |

### <a name="points-to-note"></a>Die u moet weten

- Als de **sqlReaderQuery** is opgegeven voor de **SqlSource**, Copy Activity in deze query wordt uitgevoerd op basis van de bron van de Azure SQL Database de gegevens op te halen. Of u kunt een opgeslagen procedure opgeven. Geef **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u niet of opgeven **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de **structuur** sectie van de gegevensset JSON worden gebruikt om een query maken. `select column1, column2 from mytable` wordt uitgevoerd op Azure SQL Database. Als de definitie van de gegevensset niet over de **structuur**, alle kolommen zijn geselecteerd uit de tabel.
- Bij het gebruik **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de gegevensset JSON.

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL-Database als de sink

Instellen om gegevens te kopiëren naar Azure SQL Database, de **type** sink-eigenschap in de Kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Copy-activiteit-sink moet zijn ingesteld op **SqlSink**. | Ja |
| WriteBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte bereikt **writeBatchSize**.<br/> De toegestane waarde is **geheel getal** (aantal rijen). | Nee. De standaardwaarde is 10000. |
| writeBatchTimeout | De wachttijd voor de batch invoegen bewerking is voltooid voordat er een optreedt time-out.<br/> De toegestane waarde is **timespan**. Voorbeeld: "00: 30:00 ' (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de Kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in Azure SQL Database. Deze wordt slechts één keer aangeroepen per exemplaar uitvoeren. Gebruik deze eigenschap voor het opschonen van de vooraf geladen gegevens. | Nee |
| sqlWriterStoredProcedureName | De naam van de opgeslagen procedure die over het toepassen van gegevens in een doeltabel definieert. Een voorbeeld is upsert-bewerking of transformeren met behulp van uw eigen bedrijfslogica. <br/><br/>Deze opgeslagen procedure is **per batch aangeroepen**. Voor bewerkingen die slechts één keer uitgevoerd en hebben niets te doen met de brongegevens, gebruikt u de `preCopyScript` eigenschap. Voorbeeld van de bewerkingen zijn verwijderen en afkappen. | Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn naam / waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |
| sqlWriterTableType | Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. | Nee |

> [!TIP]
> Wanneer u gegevens naar Azure SQL Database kopieert, Kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Hiertoe een upsert of extra zakelijke logica, gebruikt u de opgeslagen procedure in **SqlSink**. Meer details bekijken van [aanroepen van opgeslagen procedure van SQL-Sink](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Gegevens voorbeeld Append

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

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Een opgeslagen procedure aanroepen tijdens het kopiëren van upsert-voorbeeld

Meer details bekijken van [aanroepen van opgeslagen procedure van SQL-Sink](#invoking-stored-procedure-for-sql-sink).

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

Deze sectie leest u hoe u gegevens kopiëren van een brontabel zonder een id-kolom in een doeltabel met een id-kolom.

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
> De doeltabel is een identiteitskolom.

#### <a name="source-dataset-json-definition"></a>Source dataset-JSON-definitie

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

#### <a name="destination-dataset-json-definition"></a>Bestemming gegevensset JSON-definitie

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
> De bron en doel-tabel hebben verschillende schema. 

Het doel heeft een extra kolom met een identiteit. In dit scenario, moet u de **structuur** eigenschap in de definitie van doel, waaronder de identiteitskolom niet.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Aanroepen van de opgeslagen procedure van SQL-sink

Wanneer u gegevens naar Azure SQL Database kopiëren, kunt u ook configureren en aanroepen van een gebruiker opgegeven opgeslagen procedure met extra parameters.

U kunt een opgeslagen procedure gebruiken bij het kopiëren van ingebouwde mechanismen niet voldoen aan het doel. Ze doorgaans worden gebruikt wanneer een upsert, insert en update of extra verwerking moet worden uitgevoerd voordat de laatste invoeging van gegevens in de doeltabel. Voorbeelden van de extra verwerking zijn kolommen samenvoegen, zoekt u aanvullende waarden en plaatst deze in meer dan één tabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in Azure SQL Database. Wordt ervan uitgegaan dat de invoer- en de sink **Marketing** tabel elke drie kolommen bevatten: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

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

Definieer de **SqlSink** sectie in de Kopieeractiviteit:

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
      VALUES (source.ProfileID, source.State, source.Category)
END
```

In de database, definieert het tabeltype met dezelfde naam als de **sqlWriterTableType**. Het schema van het type moet dezelfde zijn als het schema dat is geretourneerd door de invoergegevens.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

De opgeslagen procedure-functie maakt gebruik van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).

>[!NOTE]
>Als u naar Money/Smallmoney-gegevenstype door aanroepen opgeslagen Procedure schrijft, kunnen waarden worden afgerond. Geef het bijbehorende gegevenstype in TVP als decimaal in plaats van geld/Smallmoney te beperken. 

## <a name="data-type-mapping-for-azure-sql-database"></a>De gegevenstypetoewijzing voor Azure SQL Database

Wanneer u gegevens van of naar Azure SQL Database kopieert, worden de volgende toewijzingen van Azure SQL Database-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe Copy Activity in het schema en de gegevens van een brontype toegewezen aan de sink.

| Azure SQL Database-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint |Int64 |
| binaire bestanden |Byte[] |
| bits |Booleaans |
| CHAR |Tekenreeks, Char] |
| datum |DateTime |
| Datum en tijd |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| decimaal |decimaal |
| FILESTREAM-kenmerk (varbinary(max)) |Byte[] |
| Float |Double-waarde |
| image |Byte[] |
| int |Int32 |
| geld |decimaal |
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
| tijd |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de Kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).
