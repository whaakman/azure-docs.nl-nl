---
title: "Gegevens kopiëren naar/van Azure SQL Data Warehouse | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Azure SQL Data Warehouse met behulp van Azure Data Factory"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 97782d1437f47a5ec403a98464d38961874d7575
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Gegevens kopiëren van en naar Azure SQL Data Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-sql-data-warehouse-connector.md)
> * [Versie 2 - Preview](../connector-azure-sql-data-warehouse.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure SQL Data Warehouse-connector in V2](../connector-azure-sql-data-warehouse.md).

Dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van Azure SQL Data Warehouse. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.  

> [!TIP]
> Gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse zodat de beste prestaties. De [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie bevat informatie. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van Azure SQL Data Warehouse** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met Azure SQL Data Warehouse**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!TIP]
> Wanneer gegevens uit SQL Server of Azure SQL Database kopiëren naar Azure SQL Data Warehouse, als de tabel niet in het doelarchief bestaat, maken Data Factory automatisch in de tabel in SQL Data Warehouse met behulp van het schema van de tabel in het gegevensarchief van de bron. Zie [automatisch maken van de tabel](#auto-table-creation) voor meer informatie.

## <a name="supported-authentication-type"></a>Ondersteund verificatietype
Azure SQL Data Warehouse connector ondersteuning voor basisverificatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens worden verplaatst van een Azure SQL Data Warehouse met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn waarmee gegevens van Azure SQL Data Warehouse worden gekopieerd maken is met de wizard kopiëren. Zie [zelfstudie: gegevens laden in SQL Data Warehouse met Data Factory](../../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een Azure blob-opslag met een Azure SQL datawarehouse kopieert, maakt u twee gekoppelde services om te koppelen van uw Azure storage-account en de Azure SQL datawarehouse aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure SQL Data Warehouse zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
3. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de blob-container en de map waarin de invoergegevens. En maken van een andere dataset om op te geven van de tabel in het Azure SQL datawarehouse waarin de gegevens die zijn gekopieerd uit de blobopslag. Zie voor eigenschappen van gegevensset die specifiek voor Azure SQL Data Warehouse zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron- en SqlDWSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure SQL Data Warehouse naar Azure Blob Storage kopiëren wilt, gebruikt u SqlDWSource en BlobSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor Azure SQL Data Warehouse zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure SQL Data Warehouse, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure SQL Data Warehouse:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Azure SQL Data Warehouse gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **AzureSqlDW** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de eigenschap connectionString. Alleen basisverificatie wordt ondersteund. |Ja |

> [!IMPORTANT]
> Configureer [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) en de databaseserver worden [toestaan van Azure Services voor toegang tot de server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Bovendien configureren als u gegevens naar Azure SQL Data Warehouse kopiëren wilt van buiten Azure met inbegrip van on-premises gegevensbronnen met data factory-gateway, de juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL Data Warehouse verzenden is.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureSqlDWTable** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel of weergave in de Azure SQL Data Warehouse-database waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en slechts één uitvoer produceert.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

### <a name="sqldwsource"></a>SqlDWSource
Wanneer de bron is van het type **SqlDWSource**, de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * from MijnTabel. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlDWSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de Azure SQL Data Warehouse-bron om de gegevens te verkrijgen.

U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie van de structuur van de gegevensset JSON worden gebruikt voor het bouwen van een query uitvoeren op basis van de Azure SQL Data Warehouse. Voorbeeld: `select column1, column2 from mytable`. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

#### <a name="sqldwsource-example"></a>Voorbeeld van SqlDWSource

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
**De definitie van de opgeslagen procedure:**

```SQL
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

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. Zie voor meer informatie [herhaalbaarheid sectie](#repeatability-during-copy). |Een query-instructie. |Nee |
| allowPolyBase |Geeft aan of PolyBase (indien van toepassing) gebruiken in plaats van BULKINSERT mechanisme. <br/><br/> **Met PolyBase is de aanbevolen manier om gegevens te laden in SQL Data Warehouse.** Zie [gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie voor beperkingen en meer informatie. |True <br/>False (standaard) |Nee |
| polyBaseSettings |Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **true**. |&nbsp; |Nee |
| rejectValue |Hiermee geeft u het nummer of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt. <br/><br/>Meer informatie over opties voor het weigeren van de PolyBase in de **argumenten** sectie van [maken EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) onderwerp. |0 (standaard), 1, 2... |Nee |
| rejectType |Hiermee geeft u op of de optie rejectValue is opgegeven als een letterlijke waarde of een percentage. |In waarde (standaard), Percentage |Nee |
| rejectSampleValue |Bepaalt het aantal rijen om op te halen voordat de PolyBase berekent het percentage van de geweigerde rijen opnieuw. |1, 2, … |Ja, als **rejectType** is **percentage** |
| useTypeDefault |Geeft aan hoe de ontbrekende waarden in tekstbestanden met scheidingstekens verwerken wanneer PolyBase gegevens uit het tekstbestand ophaalt.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [maken EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |True, False (standaard) |Nee |
| writeBatchSize |Gegevens invoegen in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

#### <a name="sqldwsink-example"></a>Voorbeeld van SqlDWSink

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Gebruik PolyBase gegevens laadt in Azure SQL Data Warehouse
Met behulp van  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  is een efficiënte manier van het laden van grote hoeveelheid gegevens in Azure SQL Data Warehouse met hoge doorvoer. Met behulp van PolyBase in plaats van het standaardmechanisme voor BULKINSERT ziet u een groot voordeel in de doorvoer. Zie [prestaties referentienummer kopiëren](data-factory-copy-activity-performance.md#performance-reference) met gedetailleerde vergelijking. Zie voor een overzicht met een gebruiksvoorbeeld [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

* Als de brongegevens **Azure Blob- of Azure Data Lake Store**, en de indeling is compatibel met PolyBase, kunt u rechtstreeks kopiëren naar Azure SQL Data Warehouse met PolyBase. Zie  **[Direct kopiëren met PolyBase](#direct-copy-using-polybase)**  met details.
* Als uw brongegevensarchief en de indeling wordt oorspronkelijk niet ondersteund door PolyBase, kunt u de  **[gefaseerde kopiëren met PolyBase](#staged-copy-using-polybase)**  in plaats daarvan functie. Dit biedt u ook betere doorvoer door automatisch converteren van de gegevens naar de PolyBase-compatibele indeling en opslaan van gegevens in Azure Blob-opslag. Vervolgens worden gegevens geladen in SQL Data Warehouse.

Stel de `allowPolyBase` eigenschap **true** zoals weergegeven in het volgende voorbeeld voor Azure Data Factory PolyBase gebruiken om gegevens te kopiëren naar Azure SQL Data Warehouse. Wanneer u allowPolyBase ingesteld op true, kunt u PolyBase specifieke eigenschappen met behulp van de `polyBaseSettings` eigenschappengroep. Zie de [SqlDWSink](#SqlDWSink) sectie voor meer informatie over de eigenschappen die u met polyBaseSettings gebruiken kunt.

```JSON
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

### <a name="direct-copy-using-polybase"></a>Directe kopiëren met PolyBase
SQL Data Warehouse PolyBase ondersteuning rechtstreeks voor Azure-Blob en Azure Data Lake Store (met behulp van de service-principal) als bron en met vereisten voor een specifieke indeling. Als de brongegevens voldoet aan de criteria die in deze sectie beschreven, kunt u rechtstreeks van brongegevensarchief kopiëren naar Azure SQL Data Warehouse met PolyBase. Anders kunt u [gefaseerde kopiëren met PolyBase](#staged-copy-using-polybase).

> [!TIP]
> Om gegevens te kopiëren van Data Lake Store met SQL Data Warehouse efficiënt, meer wilt weten van [Azure Data Factory kunt u zelfs gemakkelijker en handige om inzichten bloot te van gegevens bij het gebruik van Data Lake Store met SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Als niet aan de vereisten wordt voldaan, wordt Azure Data Factory controleert de instellingen en automatisch terugvalt op het BULKINSERT mechanisme voor de verplaatsing van gegevens.

1. **Bron gekoppelde service** is van het type: **AzureStorage** of **AzureDataLakeStore met verificatie van de service-principal**.  
2. De **invoergegevensset** is van het type: **AzureBlob** of **AzureDataLakeStore**, en de indeling Typ onder `type` eigenschappen is **OrcFormat**, **ParquetFormat**, of **TextFormat** met de volgende configuraties:

   1. `rowDelimiter`moet  **\n** .
   2. `nullValue`is ingesteld op **lege tekenreeks** (""), of `treatEmptyAsNull` is ingesteld op **true**.
   3. `encodingName`is ingesteld op **utf-8**, namelijk **standaard** waarde.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, en `skipLineCount` zijn niet opgegeven.
   5. `compression`kan **geen compressie**, **GZip**, of **Deflate**.

    ```JSON
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

3. Er is geen `skipHeaderLineCount` onder **BlobSource** of **AzureDataLakeStore** voor de kopieeractiviteit in de pijplijn.
4. Er is geen `sliceIdentifierColumnName` onder **SqlDWSink** voor de kopieeractiviteit in de pijplijn. (PolyBase zorgt ervoor dat alle gegevens worden bijgewerkt of niet in een enkel uitvoering bijgewerkt wordt. Als u de **herhaalbaarheid**, kunt u `sqlWriterCleanupScript`).
5. Er is geen `columnMapping` wordt gebruikt in de bijbehorende in kopie activiteit.

### <a name="staged-copy-using-polybase"></a>Gefaseerde kopiëren met PolyBase
Als de brongegevens niet voldoet aan de criteria die zijn geïntroduceerd in de vorige sectie, kunt u kopiëren van gegevens via een tussentijdse staging Azure Blob Storage (kan niet voor Premium-opslag) inschakelen. In dit geval voert Azure Data Factory automatisch transformaties op de gegevens die moeten voldoen aan de vereisten van de indeling van PolyBase gegevens en gebruik vervolgens PolyBase om gegevens te laden in SQL Data Warehouse en op de laatste opschoning uw tijdelijke gegevens van de Blob-opslag. Zie [kopie gefaseerde](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over de werking kopiëren van gegevens via een gefaseerde installatie Azure-Blob in het algemeen.

> [!NOTE]
> Wanneer het kopiëren van gegevens uit een on-premises gegevens opslaan in Azure SQL Data Warehouse met PolyBase en fasering, als uw Data Management Gateway-versie lager dan 2.4 is Java Runtime Environment (Java Runtime Environment) is vereist op uw computer met de gateway die wordt gebruikt voor het omzetten van de brongegevens naar de juiste indeling. Stelt dat u een upgrade van uw gateway uit naar de meest recente uitvoeren om te voorkomen dat deze afhankelijkheid.
>

Deze functie wilt gebruiken, maakt u een [gekoppelde Azure Storage-service](data-factory-azure-blob-connector.md#azure-storage-linked-service) die verwijst naar de Azure Storage-Account met de tussentijdse blobopslag, geeft u de `enableStaging` en `stagingSettings` eigenschappen voor de Kopieeractiviteit, zoals wordt weergegeven in de volgende code:

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a>Aanbevolen procedures voor met PolyBase
De volgende secties bevatten aanvullende aanbevolen procedures voor de waarden die worden vermeld in [aanbevolen procedures voor Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Vereiste databasemachtiging
Voor het gebruik van PolyBase, moet de gebruiker wordt gebruikt om gegevens te laden in SQL Data Warehouse heeft de [machtiging "Beheer"](https://msdn.microsoft.com/library/ms191291.aspx) op de doeldatabase. Een manier om dat te bereiken is het toevoegen van die gebruiker als lid van de rol 'db_owner'. Meer informatie over hoe dat door de volgende [in deze sectie](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Typ de beperking rijgrootte en gegevens
Polybase-belastingen zijn beperkt tot laden rijen beide kleiner is dan **1 MB** en kan niet worden geladen VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX). Raadpleeg [hier](../../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Als u hebt de brongegevens met rijen van de omvang is groter dan 1 MB, wilt u mogelijk de brontabellen verticaal gesplitst in verschillende kleine netwerken waarbij de grootste rijgrootte van elk van de limiet niet overschrijdt. De tabellen voor kleinere kunnen vervolgens worden geladen met PolyBase en samengevoegd in Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>De bronklasse SQL Data Warehouse
Overweeg om de best mogelijke doorvoer behalen, grotere bronklasse toewijzen aan de gebruiker wordt gebruikt om gegevens te laden in SQL Data Warehouse met PolyBase. Meer informatie over hoe dat door de volgende [wijzigen van een voorbeeld van een gebruiker resource klasse](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).

### <a name="tablename-in-azure-sql-data-warehouse"></a>tableName in Azure SQL Data Warehouse
De volgende tabel bevat voorbeelden op het opgeven van de **tableName** eigenschap in de gegevensset JSON voor verschillende combinaties van schema en de tabelnaam.

| DB Schema | Tabelnaam | JSON-eigenschap tableName |
| --- | --- | --- |
| dbo |MyTable |MyTable of dbo. MyTable of [dbo]. [MijnTabel] |
| dbo1 |MyTable |dbo1. MyTable of [dbo1]. [MijnTabel] |
| dbo |My.Table |[My.Table] of [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1].[My.Table] |

Als u de volgende fout ziet, is het mogelijk een probleem met de waarde die u hebt opgegeven voor de eigenschap tableName. Zie de tabel voor de juiste manier waarden opgeven voor de eigenschap tableName JSON.  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolommen met standaardwaarden
PolyBase-functie in de Data Factory accepteert alleen op dit moment is hetzelfde aantal kolommen in de doeltabel. Stel, u hebt een tabel met vier kolommen en een ervan is gedefinieerd met een standaardwaarde. De invoergegevens moet nog steeds vier kolommen bevatten. Een invoergegevensset 3 kolommen bieden zou resulteert in een fout die vergelijkbaar is met het volgende bericht:

```
All columns of the table must be specified in the INSERT BULK statement.
```
NULL-waarde is een speciale vorm van de standaardwaarde. Als de kolom waarvoor null is toegestaan, kan de invoergegevens (in blob) voor de kolom leeg zijn (kan niet worden ontbreekt uit de invoer gegevensset). PolyBase voegt NULL zijn voor deze in de Azure SQL Data Warehouse.  

## <a name="auto-table-creation"></a>Maken van de tabel automatisch
Als u Wizard kopiëren gebruikt voor gegevens van SQL Server of Azure SQL Database kopiëren naar Azure SQL Data Warehouse en de tabel die overeenkomt met de brontabel niet in het doelarchief bestaat, maken Data Factory automatisch in de tabel in het datawarehouse met behulp van de bron-tabelschema.

Data Factory maakt in de tabel in het doelarchief met dezelfde tabelnaam in het gegevensarchief van de bron. De gegevenstypen voor kolommen worden gekozen op basis van de toewijzing van het volgende type zijn. Indien nodig, voert deze typeconversies om op te lossen eventuele incompatibiliteiten tussen bron- en doelserver stores. Round Robin tabeldistributie worden ook gebruikt.

| Type gegevensbron SQL Database-kolom | Doel-SQL DW-kolomtype (de maximale grootte) |
| --- | --- |
| Int | Int |
| BigInt | BigInt |
| SmallInt | SmallInt |
| TinyInt | TinyInt |
| bits | bits |
| Decimale | Decimale |
| Numeriek | Decimale |
| Float | Float |
| Money | Money |
| Real | Real |
| SmallMoney | SmallMoney |
| Binair bestand | Binair bestand |
| varbinary | Varbinary (maximaal 8000) |
| Date | Date |
| Datum en tijd | Datum en tijd |
| DateTime2 | DateTime2 |
| Time | Time |
| DateTimeOffset | DateTimeOffset |
| SmallDateTime | SmallDateTime |
| Tekst | Varchar (maximaal 8000) |
| NText | NVarChar (maximaal 4000) |
| Installatiekopie | VarBinary (maximaal 8000) |
| UniqueIdentifier | UniqueIdentifier |
| CHAR | CHAR |
| NChar | NChar |
| VarChar | VarChar (maximaal 8000) |
| NVarChar | NVarChar (maximaal 4000) |
| Xml | Varchar (maximaal 8000) |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a>Toewijzing van het type voor Azure SQL Data Warehouse
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering uitgevoerd:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar & van Azure SQL Data Warehouse, worden de volgende toewijzingen gebruikt vanuit de SQL-type aan .NET-type en vice versa.

De toewijzing is hetzelfde als de [SQL Server gegevenstypetoewijzing voor ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).

| SQL Server Database Engine-type | .NET framework-type |
| --- | --- |
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
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

U kunt ook kolommen uit de bron-gegevensset naar kolommen uit sink gegevensset in de definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van SQL Data Warehouse
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe om gegevens te kopiëren naar en van Azure SQL Data Warehouse en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure SQL Data Warehouse naar Azure Blob
Het voorbeeld worden de volgende Data Factory-entiteiten gedefinieerd:

1. Een gekoppelde service van het type [AzureSqlDW](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlDWTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [SqlDWSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

De voorbeeld-tijdreeks (elk uur, dagelijks, enz.) worden gegevens gekopieerd van een tabel in Azure SQL Data Warehouse-database naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL Data Warehouse gekoppelde service:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob-opslag gekoppelde service:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure SQL Data Warehouse invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in Azure SQL Data Warehouse en bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens.

Instelling 'extern': 'true' informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**De kopieeractiviteit in een pijplijn met SqlDWSource en BlobSink:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlDWSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
> [!NOTE]
> In het voorbeeld **sqlReaderQuery** is opgegeven voor de SqlDWSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de Azure SQL Data Warehouse-bron om de gegevens te verkrijgen.
>
> U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).
>
> Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie van de structuur van de gegevensset JSON worden gebruikt voor het bouwen van een query (Selecteer column1, column2 from MijnTabel) als u wilt uitvoeren op Azure SQL Data Warehouse. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure SQL Data Warehouse
Het voorbeeld worden de volgende Data Factory-entiteiten gedefinieerd:

1. Een gekoppelde service van het type [AzureSqlDW](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlDWTable](#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [SqlDWSink](#copy-activity-properties).

De voorbeeld-kopieën timeseries gegevens (elk uur, dagelijks, etc.) van Azure-blob naar een tabel in Azure SQL Data Warehouse-database om het uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL Data Warehouse gekoppelde service:**

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Azure Blob-opslag gekoppelde service:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Blob invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat deze tabel extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL Data Warehouse uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam "MijnTabel" in Azure SQL Data Warehouse. De tabel maken in Azure SQL Data Warehouse met hetzelfde aantal kolommen, zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**De kopieeractiviteit in een pijplijn met BlobSource en SqlDWSink:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlDWSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
Voor een overzicht, Zie de [1 TB laden in Azure SQL Data Warehouse onder 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md) en [gegevens laden met Azure Data Factory](../../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) artikel in de documentatie van Azure SQL Data Warehouse.

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
