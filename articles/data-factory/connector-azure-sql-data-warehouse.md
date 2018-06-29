---
title: Gegevens kopiëren naar en van Azure SQL Data Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron winkels met Azure SQL Data Warehouse of van SQL Data Warehouse voor ondersteunde sink stores met behulp van Data Factory.
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
ms.date: 05/28/2018
ms.author: jingwang
ms.openlocfilehash: 42ffdbf117b3f522e27e6e46628231ddb8221018
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051624"
---
#  <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure SQL Data Warehouse met behulp van Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Version1 gedefinieerd ](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Huidige versie](connector-azure-sql-data-warehouse.md)

In dit artikel wordt uitgelegd hoe gebruiken Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar of van Azure SQL Data Warehouse. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Azure SQL Data Warehouse kopiëren naar een ondersteunde sink-gegevensarchief. En u kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure SQL Data Warehouse. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen of PUT door Kopieeractiviteit de [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Azure SQL Data Warehouse-connector deze functies:

- Gegevens kopiëren van SQL-verificatie en Azure Active Directory (Azure AD)-toepassing-tokenverificatie met een service-principal of beheerde Service identiteit (MSI).
- Als een bron ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink gegevens laden met PolyBase of een bulksgewijs invoegen. Beter PolyBase voor betere prestaties van de kopie.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase alleen SQL-verificatie, maar geen Azure AD-verificatie ondersteunt.

> [!IMPORTANT]
> Als u gegevens met behulp van Azure Data Factory-integratie Runtime kopieert, configureert u een [Azure SQL server-firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zodat Azure-services toegang heeft tot de server.
> Als u gegevens met behulp van een host zichzelf integratie runtime kopieert, configureert u de Azure SQL server-firewall zodat het juiste IP-adresbereik. Dit bereik omvat ook de machine IP die wordt gebruikt voor het verbinden met Azure SQL Database.

## <a name="get-started"></a>Aan de slag

> [!TIP]
> Gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse zodat de beste prestaties. De [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie bevat informatie. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die specifieke Data Factory-entiteiten met een Azure SQL Data Warehouse-connector definiëren.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor een Azure SQL Data Warehouse gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureSqlDW**. | Ja |
| connectionString | Geef de benodigde informatie om verbinding met het Azure SQL Data Warehouse-exemplaar voor de **connectionString** eigenschap. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Geef de toepassing client-ID. | Ja, als u Azure AD-verificatie met een service-principal. |
| servicePrincipalKey | De sleutel van de toepassing opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja, als u Azure AD-verificatie met een service-principal. |
| tenant | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja, als u Azure AD-verificatie met een service-principal. |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of een host zichzelf integratie-runtime gebruiken (als uw gegevensarchief bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee |

Voor andere verificatietypen, Zie de volgende secties over vereisten en JSON-voorbeelden, respectievelijk:

- [SQL-verificatie](#sql-authentication)
- Azure AD-toepassing tokenverificatie: [Service-principal](#service-principal-authentication)
- Azure AD-toepassing tokenverificatie: [beheerde Service-identiteit](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>SQL-verificatie

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van SQL-verificatie

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

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Volg deze stappen voor het gebruik van tokenverificatie voor service-principal gebaseerde Azure AD-toepassing:

1. **[Maken van een Azure Active Directory-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  vanuit de Azure-portal. Noteer de naam van de toepassing en de volgende waarden die definiëren van de gekoppelde service:

    - Toepassings-id
    - Sleutel van toepassing
    - Tenant-id

2. **[Inrichten van een Azure Active Directory-beheerder](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in de Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder kan dit een Azure AD-gebruiker of Azure AD-groep. Als u de groep met MSI een beheerdersrol verleent, slaat u stap 3 en 4. De beheerder heeft volledige toegang tot de database.

3. **[Ingesloten databasegebruikers maken](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de service-principal. Verbinding maken met het datawarehouse vanaf of waarnaar u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit die ten minste de machtiging ALTER elke gebruiker. Voer de volgende T-SQL:
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **De service-principal de vereiste machtigingen verlenen** zoals u dat gewend voor SQL-gebruikers of voor anderen bent. Voer de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Configureren van een Azure SQL Data Warehouse gekoppelde service** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van verificatie van de service-principal

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Een gegevensfactory kan worden gekoppeld aan een [beheerde Service-identiteit](data-factory-service-identity.md) die staat voor de specifieke factory. U kunt deze service-identiteit gebruiken voor verificatie met Azure SQL Data Warehouse. Toegang tot de aangewezen factory en kopiëren van of naar uw gegevens datawarehouse met behulp van deze identiteit.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase wordt momenteel niet ondersteund voor MSI-verificatie.

Volg deze stappen voor het gebruik van Azure AD op basis van het MSI-toepassing tokenverificatie:

1. **Een groep maken in Azure AD.** Maak de factory MSI lid van de groep.

    a. Zoek de identiteit van de data factory-service vanuit de Azure-portal. Ga naar uw gegevensfactory **eigenschappen**. Kopieer de identiteit van de-ID.

    b. Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u aan met behulp van de `Connect-AzureAD` opdracht. Voer de volgende opdrachten een groep maken en toevoegen van de gegevensfactory MSI als een lid.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Inrichten van een Azure Active Directory-beheerder](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in de Azure portal als u dat nog niet hebt gedaan.

3. **[Ingesloten databasegebruikers maken](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de Azure AD-groep. Verbinding maken met het datawarehouse vanaf of waarnaar u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit die ten minste de machtiging ALTER elke gebruiker. Voer de volgende T-SQL. 
    
    ```sql
    CREATE USER [your Azure AD group name] FROM EXTERNAL PROVIDER;
    ```

4. **De Azure AD-groep nodig machtigingen verlenen** zoals u dat gewend voor de SQL-gebruikers en andere bent. Bijvoorbeeld, voert u de volgende code.

    ```sql
    EXEC sp_addrolemember [role name], [your Azure AD group name];
    ```

5. **Configureren van een Azure SQL Data Warehouse gekoppelde service** in Azure Data Factory.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van MSI-verificatie

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Data Warehouse-gegevensset.

Om gegevens te kopiëren van of naar Azure SQL Data Warehouse, stel de **type** eigenschap van de gegevensset **AzureSqlDWTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **AzureSqlDWTable**. | Ja |
| tableName | De naam van de tabel of weergave in de Azure SQL Data Warehouse-instantie waarnaar de gekoppelde service naar verwijst. | Ja |

#### <a name="dataset-properties-example"></a>Voorbeeld van de DataSet-eigenschappen

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door de Azure SQL Data Warehouse-bron- en sink ondersteund.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Azure SQL Data Warehouse als de bron

Om gegevens te kopiëren van Azure SQL Data Warehouse, stel de **type** eigenschap in de bron van de Kopieeractiviteit naar **SqlDWSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **SqlDWSource**. | Ja |
| sqlReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. | Nee |
| sqlReaderStoredProcedureName | De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. | Nee |
| storedProcedureParameters | Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. | Nee |

### <a name="points-to-note"></a>Punten met

- Als de **sqlReaderQuery** is opgegeven voor de **SqlSource**, met de kopieerbewerking wordt deze query uitgevoerd op basis van de Azure SQL Data Warehouse-bron om de gegevens te verkrijgen. Of u kunt een opgeslagen procedure opgeven. Geef de **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u geen ofwel opgeeft **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de **structuur** sectie van de gegevensset JSON worden gebruikt om Hiermee maakt u een query. `select column1, column2 from mytable` wordt uitgevoerd op Azure SQL Data Warehouse. Als de definitie van de gegevensset beschikt niet over de **structuur**, alle kolommen uit de tabel zijn geselecteerd.
- Als u werkt met **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de JSON van de gegevensset.

#### <a name="sql-query-example"></a>Voorbeeld van de SQL-query

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

#### <a name="stored-procedure-example"></a>Voorbeeld van de opgeslagen procedure

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

### <a name="azure-sql-data-warehouse-as-sink"></a> Azure SQL Data Warehouse als sink

Om gegevens te kopiëren naar Azure SQL Data Warehouse, kunt u het sink-type instellen in Kopieeractiviteit naar **SqlDWSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit sink moet worden ingesteld op **SqlDWSink**. | Ja |
| allowPolyBase | Geeft aan of PolyBase, indien van toepassing, in plaats van het mechanisme voor BULKINSERT gebruikt. <br/><br/> Het is raadzaam om gegevens te laden in SQL Data Warehouse met behulp van PolyBase. Zie de [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie voor beperkingen en meer informatie.<br/><br/>Toegestane waarden zijn **True** en **False** (standaard).  | Nee |
| polyBaseSettings | Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **true**. | Nee |
| rejectValue | Hiermee geeft u het nummer of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt.<br/><br/>Meer informatie over de PolyBase Weiger de opties in de sectie argumenten van [maken EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>Toegestane waarden zijn 0 (standaard), 1, 2, enzovoort. |Nee |
| rejectType | Hiermee geeft u op of de **rejectValue** optie is een letterlijke waarde of een percentage.<br/><br/>Toegestane waarden zijn **waarde** (standaard) en **Percentage**. | Nee |
| rejectSampleValue | Bepaalt het aantal rijen om op te halen voordat PolyBase berekent het percentage van de geweigerde rijen opnieuw.<br/><br/>Toegestane waarden zijn 1, 2, enzovoort. | Ja, als de **rejectType** is **percentage**. |
| useTypeDefault | Geeft aan hoe de ontbrekende waarden in tekstbestanden met scheidingstekens verwerken wanneer PolyBase gegevens uit het tekstbestand ophaalt.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [maken EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Toegestane waarden zijn **True** en **False** (standaard). | Nee |
| writeBatchSize | Gegevens in de SQL-tabel invoegen wanneer de buffergrootte bereikt **writeBatchSize**. Geldt alleen wanneer PolyBase wordt niet gebruikt.<br/><br/>De toegestane waarde is **integer** (aantal rijen). | Nee. De standaardwaarde is 10000. |
| writeBatchTimeout | Wachttijd voor de batch-insert-bewerking te voltooien voordat een time-out optreedt. Geldt alleen wanneer PolyBase wordt niet gebruikt.<br/><br/>De toegestane waarde is **timespan**. Voorbeeld: "00: 30:00 ' (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de Kopieeractiviteit uitvoeren voordat het schrijven van gegevens in Azure SQL Data Warehouse in elke uitvoering. Gebruik deze eigenschap om de vooraf geladen gegevens op te schonen. | Nee | (#repeatability-tijdens-copy). | Een query-instructie. | Nee |

#### <a name="sql-data-warehouse-sink-example"></a>SQL Data Warehouse sink-voorbeeld

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

Meer informatie over het gebruik van PolyBase efficiënt laden SQL Data Warehouse in de volgende sectie.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse

Met behulp van [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) is een efficiënte manier om een grote hoeveelheid gegevens in Azure SQL Data Warehouse laden met hoge doorvoer. Met behulp van PolyBase in plaats van het standaardmechanisme voor BULKINSERT ziet u een groot voordeel in de doorvoer. Zie [prestaties verwijzing](copy-activity-performance.md#performance-reference) voor een gedetailleerde vergelijking. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB in Azure SQL Data Warehouse laden](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-load-sql-data-warehouse).

* Als de brongegevens in Azure Blob storage of Azure Data Lake Store en de indeling is compatibel met PolyBase, direct kopiëren naar Azure SQL Data Warehouse met behulp van PolyBase. Zie voor meer informatie  **[kopie sturen via PolyBase](#direct-copy-by-using-polybase)**.
* Als uw brongegevensarchief en de indeling wordt niet oorspronkelijk ondersteund door PolyBase, gebruikt u de **[gefaseerde kopiëren met behulp van PolyBase](#staged-copy-by-using-polybase)** in plaats daarvan functie. De voorbereide kopieerfunctie biedt u ook betere doorvoer. Automatisch converteert de gegevens in PolyBase-compatibele indeling. En de gegevens worden opgeslagen in Azure Blob-opslag. Vervolgens laadt de gegevens in SQL Data Warehouse.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase wordt momenteel niet ondersteund voor Azure AD-toepassing op basis van het MSI-tokenverificatie.

### <a name="direct-copy-by-using-polybase"></a>Directe kopiëren door PolyBase

SQL Data Warehouse PolyBase biedt rechtstreeks ondersteuning voor Azure-Blob en Azure Data Lake Store. Het service-principal gebruikt als bron en vereisten voor een specifieke indeling heeft. Als de brongegevens voldoet aan de criteria die in deze sectie beschreven, gebruikt u PolyBase kopiëren direct vanuit het brongegevensarchief naar Azure SQL Data Warehouse. Gebruik anders [gefaseerde kopiëren met behulp van PolyBase](#staged-copy-by-using-polybase).

> [!TIP]
> Om gegevens te kopiëren efficiënt van Data Lake Store met SQL Data Warehouse, meer wilt weten van [Azure Data Factory kunt u zelfs gemakkelijker en handige om inzichten bloot te van gegevens bij het gebruik van Data Lake Store met SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Als de vereisten zijn niet voldaan, wordt Azure Data Factory controleert de instellingen en automatisch terugvalt op het BULKINSERT mechanisme voor de verplaatsing van gegevens.

1. De **bron gekoppelde service** type **AzureStorage** of **AzureDataLakeStore** met verificatie van de service-principal.
2. De **invoergegevensset** type **AzureBlob** of **AzureDataLakeStoreFile**. Het indelingstype onder `type` eigenschappen is **OrcFormat**, **ParquetFormat**, of **TextFormat**, met de volgende configuraties:

   1. `rowDelimiter` moet **\n**.
   2. `nullValue` is ingesteld op **lege tekenreeks** (""), of `treatEmptyAsNull` is ingesteld op **true**.
   3. `encodingName` is ingesteld op **utf-8**, dit is de standaardwaarde.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, en `skipLineCount` zijn niet opgegeven.
   5. `compression` kan **geen compressie**, **GZip**, of **Deflate**.

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

3. Er is geen `skipHeaderLineCount` onder **BlobSource** of **AzureDataLakeStore** voor de Kopieeractiviteit in de pijplijn.
4. Er is geen `sliceIdentifierColumnName` onder **SqlDWSink** voor de Kopieeractiviteit in de pijplijn. PolyBase zorgt ervoor dat alle gegevens worden bijgewerkt of niet in een enkel uitvoering bijgewerkt wordt. Als u de **herhaalbaarheid**, gebruik `sqlWriterCleanupScript`.

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

### <a name="staged-copy-by-using-polybase"></a>Gefaseerde kopiëren door PolyBase

Wanneer de brongegevens niet voldoet aan de criteria in de vorige sectie, schakel gegevens kopiëren via een exemplaar tussentijdse staging Azure Blob storage. Azure Premium-opslag kan niet. Azure Data Factory worden in dit geval automatisch de transformaties uitgevoerd op de gegevens om te voldoen aan de vereisten voor de indeling van gegevens van PolyBase. Vervolgens wordt PolyBase gegevens laadt in SQL Data Warehouse. Ten slotte opschonen het van de tijdelijke gegevens van de blob-opslag. Zie [kopie gefaseerde](copy-activity-performance.md#staged-copy) voor meer informatie over het kopiëren van gegevens via een gefaseerde installatie Azure Blob storage-exemplaar.

Deze functie wilt gebruiken, maakt u een [gekoppelde Azure Storage-service](connector-azure-blob-storage.md#linked-service-properties) die verwijst naar de Azure storage-account met de tussentijdse blobopslag. Geef vervolgens de `enableStaging` en `stagingSettings` eigenschappen voor de Kopieeractiviteit, zoals wordt weergegeven in de volgende code:

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

## <a name="best-practices-for-using-polybase"></a>Aanbevolen procedures voor het gebruik van PolyBase

De volgende secties vindt u aanbevolen procedures naast de genoemde [aanbevolen procedures voor Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Vereiste databasemachtiging

Voor het gebruik van PolyBase, moet de gebruiker die gegevens in SQL Data Warehouse laadt hebben [machtiging "Beheer"](https://msdn.microsoft.com/library/ms191291.aspx) op de doeldatabase. Een manier om dat te bereiken is de gebruiker wilt toevoegen als lid van de **db_owner** rol. Meer informatie over hoe dat de [SQL Data Warehouse overzicht](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Typ limieten rijgrootte en gegevens

PolyBase-belastingen zijn beperkt tot rijen die kleiner is dan 1 MB. Ze kunnen niet laden VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX). Zie voor meer informatie [SQL Data Warehouse service Capaciteitslimieten](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Wanneer de brongegevens rijen die groter is dan 1 MB bevat, wilt u mogelijk verticaal gesplitst de brontabellen in verschillende kleine netwerken. Zorg ervoor dat de maximale grootte van elke rij de limiet niet overschrijdt. De kleinere tabellen kunnen vervolgens worden geladen met behulp van PolyBase en samengevoegd in Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>De bronklasse SQL Data Warehouse

Toewijzen voor de best mogelijke doorvoer, een grotere bronklasse aan de gebruiker waarin gegevens worden geladen in SQL Data Warehouse met PolyBase.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** in Azure SQL Data Warehouse

De volgende tabel worden voorbeelden gegeven van het opgeven van de **tableName** eigenschap in de JSON-gegevensset. Er worden verschillende combinaties van schema- en tabelnamen.

| DB Schema | Tabelnaam | **tableName** JSON-eigenschap |
| --- | --- | --- |
| dbo | MyTable | MyTable of dbo. MyTable of [dbo]. [MijnTabel] |
| dbo1 | MyTable | dbo1. MyTable of [dbo1]. [MijnTabel] |
| dbo | My.Table | [My.Table] of [dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Als u de volgende fout ziet, is het probleem mogelijk de waarde die u hebt opgegeven voor de **tableName** eigenschap. Zie de voorgaande tabel voor de juiste manier waarden opgeven voor de **tableName** JSON-eigenschap.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolommen met standaardwaarden

Op dit moment wordt accepteert de PolyBase-functie in de Data Factory alleen een hetzelfde aantal kolommen in de doeltabel. Een voorbeeld is een tabel met vier kolommen waarvoor een ervan is gedefinieerd met een standaardwaarde. De invoergegevens moet nog steeds vier kolommen hebben. Een invoergegevensset drie kolommen levert een fout die vergelijkbaar is met het volgende bericht:

```
All columns of the table must be specified in the INSERT BULK statement.
```

De NULL-waarde is een speciale vorm van de standaardwaarde. Als de kolom waarvoor null is toegestaan, kan de invoergegevens in de blob voor die kolom niet leeg zijn. Maar deze ontbreekt in de invoergegevensset mag niet. PolyBase voegt NULL voor de ontbrekende waarden in Azure SQL Data Warehouse.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>De gegevenstypetoewijzing voor Azure SQL Data Warehouse

Wanneer u gegevens van of naar Azure SQL Data Warehouse kopiëren, worden de volgende toewijzingen van Azure SQL Data Warehouse-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in Kopieeractiviteit worden toegewezen aan de sink.

| Azure SQL Data Warehouse-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint | Int64 |
| Binaire | Byte[] |
| bits | Boole-waarde |
| CHAR | Tekenreeks, Char] |
| datum | DateTime |
| Datum en tijd | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| decimale | decimale |
| FILESTREAM-kenmerk (varbinary(max)) | Byte[] |
| Float | Double |
| installatiekopie | Byte[] |
| int | Int32 |
| Money | decimale |
| nchar | Tekenreeks, Char] |
| ntext | Tekenreeks, Char] |
| numerieke | decimale |
| nvarchar | Tekenreeks, Char] |
| echte | Enkelvoudig |
| ROWVERSION | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | decimale |
| sql_variant | Object * |
| tekst | Tekenreeks, Char] |
| tijd | TimeSpan |
| tijdstempel | Byte[] |
| tinyint | Byte |
| uniqueidentifier | GUID |
| varbinary | Byte[] |
| varchar | Tekenreeks, Char] |
| xml | Xml |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de Kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).
