---
title: Gegevens kopiëren naar/van Azure SQL Data Warehouse | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar/van Azure SQL Data Warehouse met behulp van Azure Data Factory
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 72a666db6157300942b966b88d9c3369495b9fd4
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331231"
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Gegevens kopiëren naar en van Azure SQL Data Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-sql-data-warehouse-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-sql-data-warehouse.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure SQL Data Warehouse-connector in V2](../connector-azure-sql-data-warehouse.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen naar/van Azure SQL Data Warehouse. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

> [!TIP]
> Voor het bereiken van optimale prestaties, gebruikt u PolyBase om gegevens te laden in Azure SQL Data Warehouse. De [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie vindt u informatie. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **van Azure SQL Data Warehouse** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar Azure SQL Data Warehouse**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!TIP]
> Bij het kopiëren van gegevens uit SQL Server of Azure SQL Database naar Azure SQL Data Warehouse, als de tabel niet in het doelarchief bestaat, kan Data Factory automatisch maken in de tabel in SQL Data Warehouse met behulp van het schema van de tabel in de bron-gegevensopslag. Zie [automatisch van het maken van tabellen](#auto-table-creation) voor meer informatie.

## <a name="supported-authentication-type"></a>Ondersteunde verificatietype
Azure SQL Data Warehouse-connector ondersteuning voor basisverificatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een Azure SQL Data Warehouse verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn waarmee gegevens naar/van Azure SQL Data Warehouse worden gekopieerd te maken is met de wizard kopiëren. Zie [zelfstudie: Gegevens laden in SQL Data Warehouse met Data Factory](../../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens van een Azure blob-opslag met een Azure SQL datawarehouse kopieert, u twee gekoppelde services om uw Azure storage-account en Azure SQL datawarehouse koppelt aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure SQL Data Warehouse zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de blob-container en map die de invoergegevens bevat. En u maakt een andere gegevensset om op te geven van de tabel in Azure SQL datawarehouse waarin de gegevens die zijn gekopieerd uit de blob-opslag. Zie voor de gegevensseteigenschappen die specifiek voor Azure SQL Data Warehouse zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als bron- en SqlDWSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure SQL Data Warehouse naar Azure Blob Storage kopiëren wilt, gebruikt u SqlDWSource en BlobSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor Azure SQL Data Warehouse zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure SQL Data Warehouse [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure SQL Data Warehouse:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Azure SQL Data Warehouse gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **AzureSqlDW** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de connectionString-eigenschap. Alleen basisverificatie wordt ondersteund. |Ja |

> [!IMPORTANT]
> Configureer [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) en de database-server [Azure-Services toegang tot de server toestaan](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Daarnaast configureren als u gegevens naar Azure SQL Data Warehouse kopieert van buiten Azure met inbegrip van on-premises gegevensbronnen met data factory-gateway, juiste IP-adresbereik voor de machine die gegevens te naar Azure SQL Data Warehouse verzenden is.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureSqlDWTable** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de Azure SQL Data Warehouse-database waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

### <a name="sqldwsource"></a>SqlDWSource
Wanneer de bron is van het type **SqlDWSource**, de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlDWSource de Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron van de Azure SQL Data Warehouse de gegevens op te halen.

U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de structuur van de gegevensset JSON gebruikt voor het bouwen van een query wilt uitvoeren op basis van de Azure SQL Data Warehouse. Voorbeeld: `select column1, column2 from mytable`. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

#### <a name="sqldwsource-example"></a>Voorbeeld van de SqlDWSource

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

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. Zie voor meer informatie, [herhaalbaarheid sectie](#repeatability-during-copy). |Een query-instructie. |Nee |
| allowPolyBase |Hiermee wordt aangegeven of het gebruik van PolyBase (indien van toepassing) in plaats van BULKINSERT mechanisme. <br/><br/> **Met PolyBase is de aanbevolen manier om gegevens te laden in SQL Data Warehouse.** Zie [gebruik PolyBase om gegevens te laden in Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) sectie voor beperkingen en meer informatie. |True <br/>False (standaard) |Nee |
| polyBaseSettings |Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **waar**. |&nbsp; |Nee |
| rejectValue |Hiermee geeft u het getal of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt. <br/><br/>Meer informatie over van de PolyBase-weigeringsopties in de **argumenten** sectie van [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) onderwerp. |0 (standaardinstelling), 1, 2... |Nee |
| rejectType |Hiermee geeft u op of de optie rejectValue is opgegeven als een letterlijke waarde of een percentage. |Waarde (standaard), Percentage |Nee |
| rejectSampleValue |Bepaalt het aantal rijen om op te halen voordat de PolyBase berekent het percentage van geweigerde rijen opnieuw. |1, 2, … |Ja, als **rejectType** is **percentage** |
| useTypeDefault |Hiermee geeft u ontbrekende waarden in de tekstbestanden verwerken als PolyBase worden gegevens opgehaald uit het tekstbestand.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |True, False (standaard) |Nee |
| WriteBatchSize |Gegevens invoegen in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |timespan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

#### <a name="sqldwsink-example"></a>Voorbeeld van de SqlDWSink

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Gebruik van PolyBase om gegevens te laden in Azure SQL Data Warehouse
Met behulp van **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is een efficiënte manier om grote hoeveelheden gegevens in Azure SQL Data Warehouse laden met hoge doorvoer. Met behulp van PolyBase in plaats van het standaardmechanisme voor BULKINSERT ziet u een grote toename in de doorvoer. Zie [prestaties referentienummer kopiëren](data-factory-copy-activity-performance.md#performance-reference) met gedetailleerde vergelijking. Zie voor een overzicht met een use case [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md).

* Als de brongegevens bevinden zich in **Azure Blob of Azure Data Lake Store**, en de indeling is compatibel met PolyBase, kunt u rechtstreeks naar Azure SQL Data Warehouse met PolyBase kopiëren. Zie **[Direct kopiëren met behulp van PolyBase](#direct-copy-using-polybase)** met details.
* Als uw bron-gegevensopslag en -indeling is oorspronkelijk niet ondersteund in polybase, kunt u de **[gefaseerd kopiëren met behulp van PolyBase](#staged-copy-using-polybase)** functie in plaats daarvan. Het biedt u ook betere doorvoer door automatisch de gegevens in PolyBase-indeling converteren en opslaan van gegevens in Azure Blob-opslag. Vervolgens worden de gegevens geladen in SQL Data Warehouse.

Stel de `allowPolyBase` eigenschap **waar** zoals wordt weergegeven in het volgende voorbeeld voor Azure Data Factory met PolyBase gegevens te kopiëren naar Azure SQL Data Warehouse. Wanneer u allowPolyBase ingesteld op true, kunt u PolyBase specifieke eigenschappen met behulp van de `polyBaseSettings` eigenschappengroep. Zie de [SqlDWSink](#SqlDWSink) sectie voor meer informatie over de eigenschappen die u met polyBaseSettings gebruiken kunt.

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

### <a name="direct-copy-using-polybase"></a>Directe kopiëren met behulp van PolyBase
SQL Data Warehouse PolyBase ondersteuning rechtstreeks voor Azure BLOB Storage en Azure Data Lake Store (met behulp van service-principal) als bron en met vereisten voor een specifieke indeling. Als de brongegevens voldoet aan de criteria die in deze sectie beschreven, kunt u rechtstreeks vanuit de bron-gegevensopslag kopiëren naar Azure SQL Data Warehouse gebruik van PolyBase. Anders kunt u [gefaseerd kopiëren met behulp van PolyBase](#staged-copy-using-polybase).

> [!TIP]
> Meer informatie om gegevens te kopiëren van Data Lake Store met SQL Data Warehouse efficiënt, [Azure Data Factory kunt u nog gemakkelijker en handige om inzichten te verwerven in gegevens bij het gebruik van Data Lake Store met SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Als niet aan de vereisten wordt voldaan, wordt Azure Data Factory controleert of de instellingen en automatisch terugvalt op het mechanisme BULKINSERT voor de verplaatsing van gegevens.

1. **Bron gekoppelde service** is van het type: **AzureStorage** of **AzureDataLakeStore met service-principal verificatie**.
2. De **invoergegevensset** is van het type: **AzureBlob** of **AzureDataLakeStore**, en de indeling typt u onder `type` eigenschappen is **OrcFormat**, **ParquetFormat**, of **TextFormat** met de volgende configuraties:

    1. `rowDelimiter` moet **\n**.
    2. `nullValue` is ingesteld op **lege tekenreeks** (""), of `treatEmptyAsNull` is ingesteld op **waar**.
    3. `encodingName` is ingesteld op **utf-8**, die is **standaard** waarde.
    4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, en `skipLineCount` zijn niet opgegeven.
    5. `compression` kan **geen compressie**, **GZip**, of **Deflate**.

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

3. Er is geen `skipHeaderLineCount` bij **BlobSource** of **AzureDataLakeStore** voor de kopieeractiviteit in de pijplijn.
4. Er is geen `sliceIdentifierColumnName` bij **SqlDWSink** voor de kopieeractiviteit in de pijplijn. (PolyBase zorgt ervoor dat alle gegevens worden bijgewerkt of niet in een enkele uitvoeren bijgewerkt wordt. Om te realiseren **herhaalbaarheid**, kunt u `sqlWriterCleanupScript`).
5. Er is geen `columnMapping` wordt gebruikt in de bijbehorende in kopie activiteit.

### <a name="staged-copy-using-polybase"></a>Gefaseerd kopiëren met behulp van PolyBase
Als de brongegevens niet voldoet aan de criteria die zijn geïntroduceerd in de vorige sectie, kunt u kopiëren van gegevens via een tussentijdse staging Azure Blob-opslag (Premium-opslag kan niet worden) inschakelen. In dit geval voert Azure Data Factory automatisch transformaties op de gegevens om te voldoen aan de vereisten voor het opmaken van PolyBase gegevens en gebruik vervolgens PolyBase om gegevens te laden in SQL Data Warehouse en op de laatste opschoning uw tijdelijke gegevens uit de Blob-opslag. Zie [gefaseerd kopiëren](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over de werking kopiëren van gegevens via een gefaseerde installatie Azure-Blob in het algemeen.

> [!NOTE]
> Wanneer kopiëren van gegevens vanuit een on-premises gegevens opslaan in Azure SQL Data Warehouse met PolyBase en fasering, als uw versie van Data Management Gateway lager dan 2,4 is Java Runtime Environment (Java Runtime Environment) is vereist op de gatewaycomputer die wordt gebruikt om uw brongegevens te transformeren in de juiste indeling. Stelt dat u de gateway naar de nieuwste versie om te voorkomen dat deze afhankelijkheid upgraden.
>

Deze functie wilt gebruiken, maakt u een [gekoppelde Azure Storage-service](data-factory-azure-blob-connector.md#azure-storage-linked-service) die verwijst naar de Azure Storage-Account waarvoor de tussentijdse blob-opslag, geeft u de `enableStaging` en `stagingSettings` eigenschappen voor de Kopieeractiviteit, zoals wordt weergegeven in de volgende code:

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

## <a name="best-practices-when-using-polybase"></a>Aanbevolen procedures bij het gebruik van PolyBase
De volgende secties vindt u aanvullende aanbevolen procedures voor de velden die worden vermeld in [aanbevolen procedures voor Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Machtiging vereist database
Voor het gebruik van PolyBase, moet de gebruiker wordt gebruikt om gegevens te laden in SQL Data Warehouse heeft de ["bevoegdheid"](https://msdn.microsoft.com/library/ms191291.aspx) voor de doeldatabase. Een manier om te realiseren dat is het toevoegen van die gebruiker als lid van ' db_owner '. Leer hoe u dit doen door het volgende [in deze sectie](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limitation"></a>Typ beperking rijgrootte en gegevens
Polybase-loads zijn beperkt tot het laden van rijen beide kleiner is dan **1 MB** en kan niet worden geladen naar VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX). Raadpleeg [hier](../../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Als u de brongegevens met rijen groter zijn dan 1 MB hebt, kunt u de brontabellen verticaal gesplitst in verschillende kleine netwerken waarbij de grootste rijgrootte van elk van deze de limiet niet overschrijden. De kleinere tabellen kunnen vervolgens worden geladen met PolyBase en samengevoegd in Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>SQL Data Warehouse resourceklasse
Als u wilt bereiken best mogelijke doorvoer, kunt u grotere resourceklasse toewijzen aan de gebruiker die wordt gebruikt om gegevens te laden in SQL Data Warehouse met PolyBase. Leer hoe u dit doen door het volgende [wijzigen van een voorbeeld van een gebruiker resource klasse](../../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md).

### <a name="tablename-in-azure-sql-data-warehouse"></a>tableName in Azure SQL Data Warehouse
De volgende tabel bevat voorbeelden om op te geven over de **tableName** eigenschap in de gegevensset JSON voor de verschillende combinaties van schema en de tabelnaam.

| DB-Schema | Tabelnaam | tableName JSON-eigenschap |
| --- | --- | --- |
| dbo |MyTable |MyTable of dbo. MyTable of [dbo]. [MyTable] |
| dbo1 |MyTable |dbo1. MyTable of [dbo1]. [MyTable] |
| dbo |My.Table |[My.Table] of [dbo]. [My.Table] |
| dbo1 |My.Table |[dbo1]. [My.Table] |

Als u de volgende fout ziet, is het mogelijk een probleem met de waarde die u hebt opgegeven voor de eigenschap tableName. Zie de tabel voor de juiste manier waarden voor de eigenschap tableName JSON op te geven.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Kolommen met standaardwaarden
PolyBase-functie in Data Factory accepteert momenteel alleen hetzelfde aantal kolommen in de doeltabel. Stel, u hebt een tabel met vier kolommen en een van deze met de standaardwaarde is gedefinieerd. De ingevoerde gegevens moet nog steeds vier kolommen bevatten. Een invoergegevensset 3 kolommen bieden zou resulteert in een fout die vergelijkbaar is met het volgende bericht:

```
All columns of the table must be specified in the INSERT BULK statement.
```
NULL-waarde is een speciale vorm van de standaardwaarde. Als de kolom null-waarden is, de ingevoerde gegevens (in blob) voor die kolom kan niet leeg zijn (kan niet worden ontbreekt in de invoergegevensset). PolyBase voegt null zijn voor deze in de Azure SQL Data Warehouse.

## <a name="auto-table-creation"></a>Automatische tabel maken
Als u Wizard kopiëren gebruikt voor gegevens van SQL Server of Azure SQL Database kopiëren naar Azure SQL Data Warehouse en de tabel die overeenkomt met de brontabel niet in het doelarchief bestaat, kan Data Factory automatisch maken in de tabel in het datawarehouse door u het bron-tabelschema sing.

Data Factory maakt de tabel in het doelarchief met de naam van de dezelfde tabel in de bron-gegevensopslag. De gegevenstypen voor kolommen worden gekozen op basis van de toewijzing van het volgende type zijn. Indien nodig, voert het typeconversies om op te lossen eventuele incompatibiliteiten tussen bron- en doelserver. Round Robin-distributie voor tabel worden ook gebruikt.

| Kolomtype voor bron SQL-Database | Doel-SQL DW-kolomtype (de maximale grootte) |
| --- | --- |
| Int | Int |
| BigInt | BigInt |
| SmallInt | SmallInt |
| TinyInt | TinyInt |
| Bit | Bit |
| Decimaal | Decimaal |
| Numeriek | Decimaal |
| Float | Float |
| geld | geld |
| Real | Real |
| SmallMoney | SmallMoney |
| Binair bestand | Binair bestand |
| varbinary | Varbinary (maximaal 8000) |
| Date | Date |
| DateTime | DateTime |
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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische conversie van de typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar en van Azure SQL Data Warehouse, worden de volgende toewijzingen gebruikt van SQL-type naar .NET-type en vice versa.

De toewijzing is hetzelfde als de [SQL Server gegevenstypetoewijzing voor ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).

| SQL Server Database Engine-type | .NET framework-type |
| --- | --- |
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
| sql_variant |Object * |
| tekst |Tekenreeks, Char] |
| time |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

Ook kunt u kolommen uit de brongegevensset op kolommen uit de sink-gegevensset in het definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van SQL Data Warehouse
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens naar en van Azure SQL Data Warehouse en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure SQL Data Warehouse naar Azure Blob
Het voorbeeld definieert de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [AzureSqlDW](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlDWTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [SqlDWSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld time series (per uur, dagelijks, enzovoort) worden gegevens gekopieerd van een tabel in Azure SQL Data Warehouse-database naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

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
**Gekoppelde Azure Blob storage-service:**

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

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Azure SQL Data Warehouse en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **SqlDWSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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
> In het voorbeeld **sqlReaderQuery** is opgegeven voor de SqlDWSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron van de Azure SQL Data Warehouse de gegevens op te halen.
>
> U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).
>
> Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de structuur van de gegevensset JSON gebruikt voor het bouwen van een query (Selecteer Kolom1, Kolom2 van mytable) om te worden uitgevoerd op basis van de Azure SQL Data Warehouse. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure SQL Data Warehouse
Het voorbeeld definieert de volgende Data Factory-entiteiten:

1. Een gekoppelde service van het type [AzureSqlDW](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlDWTable](#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [SqlDWSink](#copy-activity-properties).

De voorbeeld-kopieën time series-gegevens (elk uur, dagelijks, enzovoort) van Azure blob naar een tabel in Azure SQL Data Warehouse-database om het uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

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
**Gekoppelde Azure Blob storage-service:**

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
**Azure Blob-invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling de Data Factory-service wordt geïnformeerd dat deze tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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
**Azure SQL Data Warehouse-uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam 'MyTable' in Azure SQL Data Warehouse. Als u verwacht de Blob-CSV-bestand dat bevat, moet u de tabel maken in Azure SQL Data Warehouse met hetzelfde aantal kolommen. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

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

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlDWSink**.

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
Voor een overzicht, Zie [1 TB laden in Azure SQL Data Warehouse minder dan 15 minuten met Azure Data Factory](data-factory-load-sql-data-warehouse.md) en [gegevens laden met Azure Data Factory](../../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) artikel in de documentatie van Azure SQL Data Warehouse.

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
