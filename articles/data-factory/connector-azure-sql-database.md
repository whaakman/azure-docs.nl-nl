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
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: jingwang
ms.openlocfilehash: 5dbd739070b1f66fe5ff04f319a3818269d0bdaa
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449613"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure SQL Database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-azure-sql-connector.md)
> * [Huidige versie](connector-azure-sql-database.md)

In dit artikel bevat een overzicht van hoe u gegevens van en naar Azure SQL Database kopiëren. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze Azure SQL Database-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md) tabel
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Specifiek, ondersteunt deze Azure SQL Database-connector deze functies:

- Kopiëren van gegevens met behulp van SQL-verificatie en tokenverificatie van Azure Active Directory (Azure AD)-toepassing met een service-principal of beheerde identiteiten voor Azure-resources.
- Als een bron, het ophalen van gegevens met behulp van een SQL-query of een opgeslagen procedure.
- Als een sink, gegevens toevoegen aan een tabel van de bestemming of het aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren.

>[!NOTE]
>Azure SQL Database [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) wordt niet ondersteund door deze connector nu. Als u wilt omzeilen, kunt u een [algemene ODBC connector](connector-odbc.md) en een SQL Server ODBC-stuurprogramma via een zelf-hostende integratieruntime. Ga als volgt [deze richtlijnen](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current) met ODBC-stuurprogramma downloaden en connection string configuraties.

> [!IMPORTANT]
> Als u gegevens kopiëren met behulp van de Azure Data Factory integratieruntime, configureert u een [Azure SQL-serverfirewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) zodat Azure-services toegang hebben tot de server.
> Als u gegevens kopiëren met behulp van de zelf-hostende integratieruntime, configureert u de firewall van Azure SQL-Server zodat het juiste IP-adresbereik. Dit bereik omvat IP van de machine die wordt gebruikt om te verbinden met Azure SQL Database.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Azure Data Factory-entiteiten die specifiek op een Azure SQL Database-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

Deze eigenschappen worden ondersteund voor een gekoppelde Azure SQL Database-service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **AzureSqlDatabase**. | Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de **connectionString** eigenschap. <br/>Dit veld is gemarkeerd **SecureString** voor het veilig opslaan in Azure Data Factory. U kunt ook een wachtwoord of de service principal sleutel plaatsen in Azure Key Vault. Als het SQL-verificatie, halen de `password` configuratie buiten de verbindingsreeks. Zie voor meer informatie het volgende op de tabel JSON-voorbeeld en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja, wanneer u Azure AD-verificatie met een service-principal gebruiken |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld is gemarkeerd **SecureString** voor het veilig opslaan in Azure Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja, wanneer u Azure AD-verificatie met een service-principal gebruiken |
| tenant | Geef de tenant-gegevens, zoals de naam van het domein of tenant-ID, die uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja, wanneer u Azure AD-verificatie met een service-principal gebruiken |
| connectVia | Dit [integratieruntime](concepts-integration-runtime.md) wordt gebruikt voor verbinding met het gegevensarchief. U kunt de Azure integratieruntime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief bevindt zich in een particulier netwerk. Indien niet opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. | Nee |

Verwijzen respectievelijk naar de volgende secties over de vereisten en JSON-voorbeelden, voor andere verificatietypen:

- [SQL-verificatie](#sql-authentication)
- [Azure AD-toepassing-token verificatie: Service principal](#service-principal-authentication)
- [Azure AD-toepassing-token verificatie: beheerde identiteiten voor Azure-resources](#managed-identity)

>[!TIP]
>Als u een fout opgetreden met de foutcode 'UserErrorFailedToConnectToSqlServer' en een bericht bereikt, zoals "XXX is in de sessielimiet voor de database en het is bereikt", voegt u toe `Pooling=false` met de verbindingstekenreeks en probeer het opnieuw.

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

**Wachtwoord in Azure Key Vault** 

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Volg deze stappen voor het gebruik van een tokenverificatie voor service-principal op basis van Azure AD-toepassing:

1. [Maak een Azure Active Directory-toepassing](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) vanuit Azure portal. Noteer de naam van de toepassing en de volgende waarden voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

2. [Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) voor uw Azure SQL-Server in Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder moet een Azure AD-gebruiker of de Azure AD-groep, maar mag niet een service-principal. Deze stap wordt uitgevoerd, zodat in de volgende stap u een Azure AD-identiteit gebruiken kunt een ingesloten databasegebruiker voor de service-principal maken.

3. [Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) voor de service-principal. Verbinding maken met de database vanaf of waarnaar u wilt kopiëren van gegevens met behulp van hulpprogramma's zoals SQL Server Management Studio, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Verleen dat de service-principal machtigingen nodig, zoals u gewend voor SQL-gebruikers of voor anderen bent. Voer de volgende code. Zie voor meer opties [dit document](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. Een gekoppelde Azure SQL Database-service configureren in Azure Data Factory.


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

### <a name="managed-identity"></a> Beheerde identiteiten voor verificatie van de Azure-resources

Een data factory, kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md) die staat voor de specifieke data factory. U kunt deze beheerde identiteit gebruiken voor Azure SQL Database-verificatie. Toegang heeft tot de aangewezen factory en gegevens kopiëren van of naar de database met behulp van deze identiteit.

Volg deze stappen voor het gebruik van verificatie van de beheerde identiteit.

1. [Een Azure Active Directory-beheerder inrichten](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) voor uw Azure SQL-Server in Azure portal als u dat nog niet hebt gedaan. De Azure AD-beheerder kan een Azure AD-gebruiker of een Azure AD-groep zijn. Als u de groep met beheerde identiteit een beheerdersrol toewijst, moet u de stappen 3 en 4 overslaan. De beheerder heeft volledige toegang tot de database.

2. [Maak ingesloten databasegebruikers](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) beheerde identiteit voor de Azure Data Factory. Verbinding maken met de database vanaf of waarnaar u wilt kopiëren van gegevens met behulp van hulpprogramma's zoals SQL Server Management Studio, met een Azure AD-identiteit ten minste heeft de machtiging ALTER elke gebruiker. Voer de volgende T-SQL: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Verleen dat de Data Factory beheerde identiteit vereist machtigingen als u gewend bent voor de SQL-gebruikers en anderen. Voer de volgende code. Zie voor meer opties [dit document](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

4. Een gekoppelde Azure SQL Database-service configureren in Azure Data Factory.

**Voorbeeld**

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, [gegevenssets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database-gegevensset.

Om gegevens te kopiëren van of naar Azure SQL Database, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **AzureSqlTable**. | Ja |
| tableName | De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service naar verwijst. | Nee voor bron, Ja voor sink |

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
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, [pijplijnen](concepts-pipelines-activities.md). Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure SQL Database-bron en sink.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL-Database als de bron

Instellen om gegevens te kopiëren uit een Azure SQL Database, de **type** eigenschap in de bron van de activiteit kopiëren naar het **SqlSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **SqlSource**. | Ja |
| sqlReaderQuery | Deze eigenschap maakt gebruik van de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `select * from MyTable`. | Nee |
| sqlReaderStoredProcedureName | De naam van de opgeslagen procedure die gegevens uit de brontabel leest. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. | Nee |
| storedProcedureParameters | Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De naam en het hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |

**Die u moet weten:**

- Als **sqlReaderQuery** is opgegeven voor **SqlSource**, de kopieeractiviteit deze query wordt uitgevoerd op basis van de bron van de Azure SQL Database de gegevens op te halen. Ook kunt u een opgeslagen procedure door op te geven **sqlReaderStoredProcedureName** en **storedProcedureParameters** als de opgeslagen procedure parameters nodig heeft.
- Als u niet of opgeven **sqlReaderQuery** of **sqlReaderStoredProcedureName**, de kolommen die zijn gedefinieerd in de sectie "structuur" van de gegevensset JSON worden gebruikt om een query samen te stellen. De query `select column1, column2 from mytable` wordt uitgevoerd op Azure SQL Database. Als de definitie van de gegevensset geen "structuur", worden alle kolommen uit de tabel geselecteerd.

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

> [!TIP]
> Meer informatie over de ondersteunde schrijven gedrag, configuraties en best practices van [Best practice om gegevens te laden in Azure SQL Database](#best-practice-for-loading-data-into-azure-sql-database).

Instellen om gegevens te kopiëren naar Azure SQL Database, de **type** sink-eigenschap in de kopieeractiviteit naar **SqlSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de kopie-activiteit-sink moet zijn ingesteld op **SqlSink**. | Ja |
| writeBatchSize | Aantal rijen in de SQL-tabel moet worden ingevoegd *per batch*.<br/> De toegestane waarde is **geheel getal** (aantal rijen). Standaard bepaalt Azure Data Factory dynamisch de batchgrootte van de juiste op basis van de rijgrootte. | Nee |
| writeBatchTimeout | De wachttijd voor de batch invoegen bewerking is voltooid voordat er een optreedt time-out.<br/> De toegestane waarde is **timespan**. Een voorbeeld hiervan is "00: 30:00 ' (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in Azure SQL Database. De toepassing wordt aangeroepen slechts één keer per exemplaar uitvoeren. Gebruik deze eigenschap voor het opschonen van de vooraf geladen gegevens. | Nee |
| sqlWriterStoredProcedureName | De naam van de opgeslagen procedure die over het toepassen van gegevens in een doeltabel definieert. <br/>Deze opgeslagen procedure is *per batch aangeroepen*. Voor bewerkingen die slechts één keer worden uitgevoerd en hebben niets te met brongegevens, bijvoorbeeld verwijderen of afbreken, gebruikt u de `preCopyScript` eigenschap. | Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn naam / waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |
| sqlWriterTableType | Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. De kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens die wordt gekopieerd met bestaande gegevens samenvoegen. | Nee |

**Voorbeeld 1: Gegevens toevoegen**

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

**Voorbeeld 2: Een opgeslagen procedure aanroepen tijdens het kopiëren van**

Meer details bekijken van [aanroepen van een opgeslagen procedure uit een SQL-sink](#invoke-a-stored-procedure-from-a-sql-sink).

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

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Aanbevolen procedure voor het laden van gegevens in Azure SQL Database

Wanneer u gegevens naar Azure SQL Database kopiëren, kunt u verschillende schrijfeigenschappen mogelijk nodig hebt:

- [Toevoeg-](#append-data): Mijn brongegevens heeft alleen nieuwe records.
- [Upsert](#upsert-data): Mijn brongegevens heeft zowel invoegingen en updates.
- [Overschrijven](#overwrite-the-entire-table): Ik wil een hele dimensietabel elke keer laden.
- [Schrijf met aangepaste logica](#write-data-with-custom-logic): Moet ik extra verwerking vóór de laatste invoeging in de doeltabel.

Raadpleeg de betreffende secties over het configureren in Azure Data Factory en best practices.

### <a name="append-data"></a>Gegevens toevoegen

Gegevens toevoegen, is het standaardgedrag van deze Azure SQL Database-sink-connector. Azure Data Factory biedt een bulksgewijs invoegen te schrijven naar de tabel efficiënt. U kunt configureren van de bron en sink-dienovereenkomstig in de kopieeractiviteit.

### <a name="upsert-data"></a>Upsert-gegevens

**Optie 1:** Wanneer u een grote hoeveelheid gegevens wilt kopiëren, gebruikt u de volgende benadering te doen een upsert hebt: 

- Gebruik eerst een [tijdelijke tabel in het databasebereik](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current#database-scoped-global-temporary-tables-azure-sql-database) voor bulksgewijs laden alle records met behulp van de kopieeractiviteit. Omdat operations op basis van database binnen het bereik van tijdelijke tabellen worden niet geregistreerd, kunt u miljoenen records laden in een paar seconden.
- Een opgeslagen procedure-activiteit uitvoeren in Azure Data Factory om toe te passen een [samenvoegen](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) of invoegen/bijwerken-instructie. Gebruik de tijdelijke tabel als de bron om uit te voeren op alle bijwerkt of als één transactie invoegt. Op deze manier wordt het aantal retouren en logboekbewerkingen verminderd. Aan het einde van de opgeslagen procedure-activiteit, kan de tijdelijke tabel worden afgekapt als u wilt deze klaar is voor de volgende upsert-cyclus.

Als u bijvoorbeeld in Azure Data Factory, kunt u een pijplijn met een **Kopieeractiviteit** gekoppeld met een **Stored Procedure-activiteit**. De vorige worden gegevens gekopieerd van de brongegevensopslag naar een tijdelijke tabel van Azure SQL Database, bijvoorbeeld **##UpsertTempTable**, als de naam van de tabel in de gegevensset. De laatste vervolgens roept een opgeslagen procedure voor het samenvoegen van brongegevens van de tijdelijke tabel in de doeltabel en het opschonen van de tijdelijke tabel.

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

U kunt configureren dat de **preCopyScript** eigenschap in de kopie-activiteit-sink. In dit geval voor elke kopieeractiviteit die wordt uitgevoerd, voert Azure Data Factory het script eerst. Vervolgens wordt de kopie voor het invoegen van de gegevens uitgevoerd. Bijvoorbeeld als u wilt overschrijven de hele tabel met de meest recente gegevens, Geef een script voor het eerst alle records verwijderen voordat u bulksgewijs laden van de nieuwe gegevens uit de bron.

### <a name="write-data-with-custom-logic"></a>Schrijven van gegevens met aangepaste logica

De stappen voor het schrijven van gegevens met aangepaste logica zijn vergelijkbaar met die wordt beschreven in de [Upsert gegevens](#upsert-data) sectie. Wanneer u nodig hebt om toe te passen extra verwerking vóór de laatste invoegen van de brongegevens in de doeltabel voor grootschalige, kunt u doen twee dingen:

- Laden naar een database-scoped tijdelijke tabel en klik vervolgens aanroepen van een opgeslagen procedure. 
- Een opgeslagen procedure tijdens het kopiëren van aanroepen.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aanroepen van een opgeslagen procedure uit een SQL-sink

Wanneer u gegevens naar Azure SQL Database kopiëren, kunt ook u configureren en aanroepen van een gebruiker opgegeven opgeslagen procedure met extra parameters.

> [!TIP]
> Aanroepen van een opgeslagen procedure, verwerkt de gegevens per rij in plaats van via een bulkbewerking, wordt niet voor grootschalige kopiëren aanbevolen. Meer informatie uit [Best practice om gegevens te laden in Azure SQL Database](#best-practice-for-loading-data-into-azure-sql-database).

U kunt een opgeslagen procedure gebruiken bij het kopiëren van ingebouwde mechanismen niet voldoen aan het doel. Een voorbeeld daarvan is wanneer u wilt toepassen extra verwerking vóór de laatste invoeging van gegevens in de doeltabel. Voorbeelden van de extra verwerking zijn als u wilt kolommen samenvoegen, aanvullende waarden opzoeken en invoegen in meer dan één tabel.

Het volgende voorbeeld laat zien hoe een opgeslagen procedure gebruiken om te doen van een upsert in een tabel in Azure SQL Database. Wordt ervan uitgegaan dat de invoergegevens en de sink **Marketing** tabel elke drie kolommen bevatten: **ProfileID**, **status**, en **categorie**. Voer de upsert op basis van de **ProfileID** kolom, en alleen toe te passen voor een specifieke categorie.

**Uitvoergegevensset:** De 'tableName' is de hetzelfde type parameter tabelnaam in uw opgeslagen procedure, zoals wordt weergegeven in het volgende script in de opgeslagen procedure:

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
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL
)
```

De opgeslagen procedure-functie maakt gebruik van [tabelwaardeparameters](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="mapping-data-flow-properties"></a>Eigenschappen van de gebruikersstroom toewijzing van gegevens

Informatie over de details van [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) bij het toewijzen van de gegevensstroom.

## <a name="data-type-mapping-for-azure-sql-database"></a>De gegevenstypetoewijzing voor Azure SQL Database

Wanneer gegevens worden gekopieerd vanuit of naar Azure SQL Database, worden de volgende toewijzingen van Azure SQL Database-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieerbewerking het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| Azure SQL Database-gegevenstype | Azure Data Factory tussentijdse gegevenstype |
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
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Voor de gegevenstypen die zijn toegewezen aan de tijdelijke decimaal, ondersteunt Azure Data Factory momenteel precisie maximaal 28. Als u gegevens met een nauwkeurigheid groter dan 28 hebt, kunt u overwegen converteren naar een tekenreeks in SQL-query.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).
