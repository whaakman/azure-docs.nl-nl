---
title: Gegevens kopiëren naar en van Azure SQL Data Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens uit ondersteunde bron winkels naar Azure SQL Data Warehouse of SQL Data Warehouse voor ondersteunde sink stores kopiëren met behulp van Data Factory.
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
ms.date: 07/28/2018
ms.author: jingwang
ms.openlocfilehash: ef1bd613943543f78d358064f4abefc6fa31b63e
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842332"
---
#  <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure SQL Data Warehouse met behulp van Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Version1 ](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Huidige versie](connector-azure-sql-data-warehouse.md)

In dit artikel wordt uitgelegd hoe u gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar of van Azure SQL Data Warehouse. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Azure SQL Data Warehouse kopiëren naar een ondersteunde sink-gegevensopslag. En u kunt gegevens uit een ondersteund brongegevensarchief kopiëren naar Azure SQL Data Warehouse. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT door Copy-activiteit, de [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek, ondersteunt deze Azure SQL Data Warehouse-connector deze functies:

- Gegevens kopiëren met behulp van SQL-verificatie en tokenverificatie van Azure Active Directory (Azure AD)-toepassing met een service-principal of Managed Service Identity (MSI).
- Als een bron ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink gegevens laden met behulp van PolyBase of een bulksgewijs invoegen. We raden PolyBase voor betere kopieerprestaties.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase alleen SQL-verificatie, maar geen Azure AD-verificatie ondersteunt.

> [!IMPORTANT]
> Als u gegevens kopiëren met behulp van Azure Data Factory Integration Runtime, configureert u een [Azure SQL-serverfirewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zodat Azure-services toegang hebben tot de server.
> Als u gegevens kopiëren met behulp van de zelf-hostende integratieruntime, configureert u de firewall van de Azure SQL-server zodat het juiste IP-adresbereik. Dit bereik omvat IP van de machine die wordt gebruikt om te verbinden met Azure SQL Database.

## <a name="get-started"></a>Aan de slag

> [!TIP]
> Voor het bereiken van optimale prestaties, gebruikt u PolyBase om gegevens te laden in Azure SQL Data Warehouse. De [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie vindt u informatie. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die specifieke Data Factory-entiteiten met een Azure SQL Data Warehouse-connector definiëren.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor een Azure SQL Data Warehouse gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureSqlDW**. | Ja |
| connectionString | Geef de informatie die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de **connectionString** eigenschap. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja, als u Azure AD-verificatie met een service-principal. |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja, als u Azure AD-verificatie met een service-principal. |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja, als u Azure AD-verificatie met een service-principal. |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of een zelf-hostende integratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee |

Verwijzen respectievelijk naar de volgende secties over de vereisten en JSON-voorbeelden, voor andere verificatietypen:

- [SQL-verificatie](#sql-authentication)
- Azure AD-toepassing-tokenverificatie: [Service-principal](#service-principal-authentication)
- Azure AD-toepassing-tokenverificatie: [beheerde Service-identiteit](#managed-service-identity-authentication)

>[!TIP]
>Als u fout met foutcode als "UserErrorFailedToConnectToSqlServer" bereikt en wordt weergegeven, zoals 'de sessielimiet voor de database is XXX en is bereikt.', toe te voegen `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

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

Volg deze stappen voor het gebruik van service-principal op basis van Azure AD-toepassing-tokenverificatie:

1. **[Maak een Azure Active Directory-toepassing](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  vanuit Azure portal. Noteer de naam van de toepassing en de volgende waarden voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

1. **[Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Als u de groep met MSI-bestand op een beheerdersrol toewijst, moet u de stappen 3 en 4 overslaan. De beheerder heeft volledige toegang tot de database.

1. **[Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de service-principal. Verbinding maken met het datawarehouse uit of die u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL:
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. **De service-principal die nodig is machtigingen verlenen** zoals u gewend voor SQL-gebruikers of voor anderen bent. Voer de volgende code:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. **Configureren van een Azure SQL Data Warehouse gekoppelde service** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Voorbeeld van de gekoppelde service die gebruikmaakt van verificatie van service-principal

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

Een data factory, kan worden gekoppeld aan een [beheerde Service-identiteit](data-factory-service-identity.md) die staat voor de specifieke factory. U kunt deze service-identiteit gebruiken voor verificatie van Azure SQL Data Warehouse. De aangewezen factory kunt openen en gegevens kopiëren van of naar uw data warehouse met behulp van deze identiteit.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase wordt momenteel niet ondersteund voor MSI-verificatie.

Volg deze stappen voor het gebruik van Azure AD MSI-gebaseerde toepassing tokenverificatie:

1. **Een groep maken in Azure AD.** Controleer de factory MSI een lid van de groep.

    1. Zoek de data factory-service-identiteit vanuit Azure portal. Ga naar uw data factory **eigenschappen**. Kopieer de SERVICE-identiteit-ID.

    1. Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module. Meld u aan met behulp van de `Connect-AzureAD` opdracht. Voer de volgende opdrachten een groep maken en toevoegen van de data factory MSI-bestand als een lid.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

1. **[Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  voor uw Azure SQL-server in Azure portal als u dat nog niet hebt gedaan.

1. **[Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  voor de Azure AD-groep. Verbinding maken met het datawarehouse uit of die u kopiëren van gegevens wilt met behulp van hulpprogramma's zoals SSMS, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL. 
    
    ```sql
    CREATE USER [your Azure AD group name] FROM EXTERNAL PROVIDER;
    ```

1. **De Azure AD-groep die nodig is machtigingen verlenen** zoals u gewend voor de SQL-gebruikers en anderen bent. Voer bijvoorbeeld de volgende code.

    ```sql
    EXEC sp_addrolemember [role name], [your Azure AD group name];
    ```

1. **Configureren van een Azure SQL Data Warehouse gekoppelde service** in Azure Data Factory.

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Data Warehouse-gegevensset.

Om gegevens te kopiëren van of naar Azure SQL Data Warehouse, stel de **type** eigenschap van de gegevensset in **AzureSqlDWTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **AzureSqlDWTable**. | Ja |
| tableName | De naam van de tabel of weergave in de Azure SQL Data Warehouse-instantie waarnaar de gekoppelde service naar verwijst. | Ja |

#### <a name="dataset-properties-example"></a>Voorbeeld van de gegevensset-eigenschappen

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

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Data Warehouse-bron en sink.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Azure SQL Data Warehouse als de bron

Om gegevens te kopiëren van Azure SQL Data Warehouse, stel de **type** eigenschap in de bron van Kopieeractiviteit naar **SqlDWSource**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **SqlDWSource**. | Ja |
| sqlReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `select * from MyTable`. | Nee |
| sqlReaderStoredProcedureName | De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. | Nee |
| storedProcedureParameters | Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |

### <a name="points-to-note"></a>Die u moet weten

- Als de **sqlReaderQuery** is opgegeven voor de **SqlSource**, de Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron van de Azure SQL Data Warehouse de gegevens op te halen. Of u kunt een opgeslagen procedure opgeven. Geef de **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u niet of opgeven **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de **structuur** sectie van de gegevensset JSON worden gebruikt om een query maken. `select column1, column2 from mytable` wordt uitgevoerd op Azure SQL Data Warehouse. Als de definitie van de gegevensset niet over de **structuur**, alle kolommen zijn geselecteerd uit de tabel.
- Bij het gebruik **sqlReaderStoredProcedureName**, moet u nog steeds om op te geven van een dummy **tableName** eigenschap in de gegevensset JSON.

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

Om gegevens te kopiëren naar Azure SQL Data Warehouse, stelt u het sink-type in de Kopieeractiviteit naar **SqlDWSink**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Copy-activiteit-sink moet zijn ingesteld op **SqlDWSink**. | Ja |
| allowPolyBase | Hiermee wordt aangegeven of het gebruik van PolyBase, indien van toepassing, in plaats van het mechanisme voor BULKINSERT. <br/><br/> Het is raadzaam om gegevens te laden in SQL Data Warehouse met behulp van PolyBase. Zie de [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie voor beperkingen en meer informatie.<br/><br/>Toegestane waarden zijn **waar** en **False** (standaard).  | Nee |
| polyBaseSettings | Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **waar**. | Nee |
| rejectValue | Hiermee geeft u het getal of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt.<br/><br/>Meer informatie over de PolyBase-weigeringsopties in de sectie argumenten van [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>Toegestane waarden zijn 0 (standaardinstelling), 1, 2, enzovoort. |Nee |
| rejectType | Hiermee geeft u op of de **rejectValue** optie is een letterlijke waarde of een percentage.<br/><br/>Toegestane waarden zijn **waarde** (standaard) en **Percentage**. | Nee |
| rejectSampleValue | Bepaalt het aantal rijen om op te halen voordat PolyBase berekent het percentage van geweigerde rijen opnieuw.<br/><br/>Toegestane waarden zijn 1, 2, enzovoort. | Ja, als de **rejectType** is **percentage**. |
| useTypeDefault | Hiermee geeft u ontbrekende waarden in de tekstbestanden verwerken als PolyBase worden gegevens opgehaald uit het tekstbestand.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Toegestane waarden zijn **waar** en **False** (standaard). | Nee |
| WriteBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte bereikt **writeBatchSize**. Geldt alleen wanneer PolyBase wordt niet gebruikt.<br/><br/>De toegestane waarde is **geheel getal** (aantal rijen). | Nee. De standaardwaarde is 10000. |
| writeBatchTimeout | Wachttijd voor de bewerking voor het invoegen van batch worden voltooid voordat er een optreedt time-out. Geldt alleen wanneer PolyBase wordt niet gebruikt.<br/><br/>De toegestane waarde is **timespan**. Voorbeeld: "00: 30:00 ' (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de Kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in Azure SQL Data Warehouse in elke uitvoering. Gebruik deze eigenschap voor het opschonen van de vooraf geladen gegevens. | Nee | (#repeatability-tijdens-kopie). | Een query-instructie. | Nee |

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

Meer informatie over het gebruik van PolyBase te laden efficiënt SQL Data Warehouse in de volgende sectie.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Gebruik van PolyBase om gegevens te laden in Azure SQL Data Warehouse

Met behulp van [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) is een efficiënte manier om een grote hoeveelheid gegevens in Azure SQL Data Warehouse laden met hoge doorvoer. Met behulp van PolyBase in plaats van het standaardmechanisme voor BULKINSERT ziet u een grote toename in de doorvoer. Zie [prestaties verwijzing](copy-activity-performance.md#performance-reference) voor een gedetailleerde vergelijking. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-load-sql-data-warehouse).

* Als de brongegevens bevinden zich in Azure Blob storage of Azure Data Lake Store en de indeling compatibel met PolyBase, rechtstreeks van de kopie in Azure SQL Data Warehouse is met behulp van PolyBase. Zie voor meer informatie,  **[directe kopiëren met behulp van PolyBase](#direct-copy-by-using-polybase)**.
* Als de bron-gegevensopslag en -indeling wordt niet oorspronkelijk ondersteund door PolyBase, gebruikt u de **[gefaseerd kopiëren met behulp van PolyBase](#staged-copy-by-using-polybase)** functie in plaats daarvan. De functie gefaseerd kopiëren biedt u ook betere doorvoer. De gegevens wordt automatisch geconverteerd in PolyBase-compatibele indeling. En de gegevens worden opgeslagen in Azure Blob-opslag. Vervolgens worden de gegevens in SQL Data Warehouse geladen.

> [!IMPORTANT]
> Houd er rekening mee dat PolyBase wordt momenteel niet ondersteund voor Azure AD-toepassing op basis van een MSI-token verificatie.

### <a name="direct-copy-by-using-polybase"></a>Directe kopiëren met behulp van PolyBase

SQL Data Warehouse PolyBase biedt rechtstreeks ondersteuning voor Azure BLOB Storage en Azure Data Lake Store. Het service-principal gebruikt als een bron en vereisten voor een specifieke indeling heeft. Als de brongegevens voldoet aan de criteria die in deze sectie beschreven, moet u PolyBase gebruiken om te kopiëren direct van de bron-gegevensopslag naar Azure SQL Data Warehouse. Gebruik anders [gefaseerd kopiëren met behulp van PolyBase](#staged-copy-by-using-polybase).

> [!TIP]
> Meer informatie om gegevens te kopiëren efficiënt van Data Lake Store met SQL Data Warehouse, [Azure Data Factory kunt u nog gemakkelijker en handige om inzichten te verwerven in gegevens bij het gebruik van Data Lake Store met SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Als aan de vereisten zijn niet voldaan, wordt Azure Data Factory controleert of de instellingen en automatisch terugvalt op het mechanisme BULKINSERT voor de verplaatsing van gegevens.

1. De **bron gekoppelde service** type Azure Blob-opslag is (**Azure BLOB Storage**/**AzureStorage**) met account-sleutelverificatie of Azure Data Lake Opslag Gen1 (**AzureDataLakeStore**) met service-principal verificatie.
2. De **invoergegevensset** type **AzureBlob** of **AzureDataLakeStoreFile**. Het indelingstype onder `type` eigenschappen is **OrcFormat**, **ParquetFormat**, of **TextFormat**, met de volgende configuraties:

   1. `fileName` filteren op jokerteken bevat.
   2. `rowDelimiter` moet **\n**.
   3. `nullValue` is een ingesteld op **lege tekenreeks** ("") of als standaard, links en `treatEmptyAsNull` niet is ingesteld op false.
   4. `encodingName` is ingesteld op **utf-8**, dit is de standaardwaarde.
   5. `escapeChar`, `quoteChar` en `skipLineCount` zijn niet opgegeven. Ondersteuning voor PolyBase overslaan rij met koppen die kan worden geconfigureerd als `firstRowAsHeader` in ADF.
   6. `compression` kan **geen compressie**, **GZip**, of **Deflate**.

    ```json
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",
           "nullValue": "",
           "encodingName": "utf-8",
           "firstRowAsHeader": <any>
       },
       "compression": {
           "type": "GZip",
           "level": "Optimal"
       }
    },
    ```

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

### <a name="staged-copy-by-using-polybase"></a>Gefaseerd kopiëren met behulp van PolyBase

Wanneer de brongegevens niet voldoet aan de criteria in de vorige sectie, gegevens kopiëren via een exemplaar tussentijdse staging Azure Blob-opslag inschakelen Azure Premium Storage kan niet. In dit geval Azure Data Factory wordt automatisch uitgevoerd transformaties op de gegevens om te voldoen aan de vereisten voor het opmaken van gegevens van PolyBase. Vervolgens deze gebruikmaakt van PolyBase om gegevens te laden in SQL Data Warehouse. Ten slotte opschonen het van uw tijdelijke gegevens uit de blob-opslag. Zie [gefaseerd kopiëren](copy-activity-performance.md#staged-copy) voor meer informatie over het kopiëren van gegevens via een gefaseerde installatie exemplaar van de Azure Blob-opslag.

Deze functie wilt gebruiken, maakt u een [gekoppelde Azure Storage-service](connector-azure-blob-storage.md#linked-service-properties) die verwijst naar de Azure storage-account met de tussentijdse blob-opslag. Geef vervolgens de `enableStaging` en `stagingSettings` eigenschappen voor de Kopieeractiviteit, zoals wordt weergegeven in de volgende code:

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

De volgende secties vindt u aanbevolen procedures naast die worden vermeld in [aanbevolen procedures voor Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Machtiging vereist database

Voor het gebruik van PolyBase, moet de gebruiker die gegevens in SQL Data Warehouse laadt hebben ["bevoegdheid"](https://msdn.microsoft.com/library/ms191291.aspx) voor de doeldatabase. Één manier om te realiseren dat is het toevoegen van de gebruiker als lid van de **db_owner** rol. Leer hoe u dat doen in de [overzicht van SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Typ limieten rijgrootte en gegevens

PolyBase-loads zijn beperkt tot de rijen die kleiner is dan 1 MB. Ze kunnen niet laden voor VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX). Zie voor meer informatie, [capaciteitslimieten voor SQL Data Warehouse-service](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Wanneer de brongegevens rijen die groter is dan 1 MB bevat, is het raadzaam om te splitsen verticaal de brontabellen in verschillende kleine netwerken. Zorg ervoor dat de maximale grootte van elke rij de limiet niet overschrijdt. De kleinere tabellen kunnen vervolgens worden geladen met behulp van PolyBase en samengevoegd in Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>SQL Data Warehouse resourceklasse

Voor het bereiken van de best mogelijke doorvoer, een grotere resourceklasse aan de gebruiker die gegevens in SQL Data Warehouse met PolyBase laadt te toewijzen.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** in Azure SQL Data Warehouse

De volgende tabel bevat voorbeelden van hoe u de **tableName** eigenschap in de JSON-gegevensset. Hier ziet u verschillende combinaties van schema- en tabelnamen.

| DB-Schema | Tabelnaam | **tableName** JSON-eigenschap |
| --- | --- | --- |
| dbo | MyTable | MyTable of dbo. MyTable of [dbo]. [MyTable] |
| dbo1 | MyTable | dbo1. MyTable of [dbo1]. [MyTable] |
| dbo | My.Table | [My.Table] of [dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Als u de volgende fout ziet, is het probleem mogelijk de waarde die u hebt opgegeven voor de **tableName** eigenschap. Zie de voorgaande tabel voor de juiste manier om op te geven van de waarden voor de **tableName** JSON-eigenschap.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolommen met standaardwaarden

Op dit moment accepteert de PolyBase-functie in Data Factory alleen een hetzelfde aantal kolommen in de doeltabel. Een voorbeeld is een tabel met vier kolommen waarin een van deze met de standaardwaarde is gedefinieerd. De ingevoerde gegevens moet nog steeds vier kolommen hebben. Een invoergegevensset drie kolommen levert een fout die vergelijkbaar is met het volgende bericht:

```
All columns of the table must be specified in the INSERT BULK statement.
```

De waarde NULL is een speciale vorm van de standaardwaarde. Als de kolom toegestaan is, is de ingevoerde gegevens in de blob voor die kolom kan niet leeg zijn. Maar deze niet meer worden ontbreekt in de invoergegevensset. PolyBase voegt NULL voor ontbrekende waarden in Azure SQL Data Warehouse.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>De gegevenstypetoewijzing voor Azure SQL Data Warehouse

Wanneer u gegevens van of naar Azure SQL Data Warehouse kopieert, worden de volgende toewijzingen van Azure SQL Data Warehouse-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe Copy Activity in het schema en de gegevens van een brontype toegewezen aan de sink.

| Azure SQL Data Warehouse-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| bigint | Int64 |
| binaire bestanden | Byte[] |
| bits | Booleaans |
| CHAR | Tekenreeks, Char] |
| datum | DateTime |
| Datum en tijd | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| decimaal | decimaal |
| FILESTREAM-kenmerk (varbinary(max)) | Byte[] |
| Float | Double-waarde |
| image | Byte[] |
| int | Int32 |
| geld | decimaal |
| nchar | Tekenreeks, Char] |
| ntext | Tekenreeks, Char] |
| numerieke | decimaal |
| nvarchar | Tekenreeks, Char] |
| echte | Enkelvoudig |
| ROWVERSION | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | decimaal |
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
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de Kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).
