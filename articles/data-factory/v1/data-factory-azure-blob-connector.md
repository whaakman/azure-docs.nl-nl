---
title: "Gegevens kopiëren naar/van Azure Blob Storage | Microsoft Docs"
description: "Informatie over het kopiëren van blob-gegevens in Azure Data Factory. Gebruik onze voorbeeld: gegevens kopiëren naar en van Azure Blob Storage en Azure SQL Database."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f66ddecd6b999400b05a4b00aa781ffef3f7887d
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure Blob Storage met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-blob-connector.md)
> * [Versie 2 - Preview](../connector-azure-blob-storage.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure Blob Storage-connector in V2](../connector-azure-blob-storage.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Blob Storage gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

## <a name="overview"></a>Overzicht
U kunt gegevens kopiëren van alle ondersteunde brongegevensarchief naar Azure Blob Storage of Azure Blob Storage met alle ondersteunde sink-gegevensarchief. De volgende tabel geeft een lijst van gegevensarchieven ondersteund als bronnen of sinks door met de kopieerbewerking. U kunt bijvoorbeeld gegevens verplaatsen **van** een SQL Server-database of een Azure SQL database **naar** Azure blob storage. En u kunt gegevens kopiëren **van** Azure blob-opslag **naar** een Azure SQL Data Warehouse of een verzameling Azure Cosmos DB. 

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit Azure Blob Storage** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **naar Azure Blob Storage**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> Kopieeractiviteit ondersteunt kopiëren van gegevens van/naar zowel algemeen Azure Storage-accounts als Hot/Cool Blob-opslag. De activiteit ondersteunt **lezen van het blok, toevoegen of pagina-blobs**, maar ondersteunt **schrijven naar alleen blok-blobs**. Azure Premium-opslag wordt niet ondersteund als een sink omdat het wordt ondersteund door de pagina-blobs.
> 
> Kopieeractiviteit verwijdert geen gegevens van de bron nadat de gegevens is gekopieerd naar de bestemming. Als u brongegevens verwijderen na een geslaagde kopie moet, maakt u een [aangepaste activiteit](data-factory-use-custom-activities.md) verwijderen van de gegevens en de activiteit in de pijplijn gebruiken. Zie voor een voorbeeld de [verwijderen blob of de map voorbeeld op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity). 

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een Azure Blob-opslag verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Dit artikel bevat een [scenario](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) voor het maken van een pijplijn om gegevens te kopiëren van een Azure Blob Storage-locatie naar een andere locatie van de Azure Blob Storage. Zie voor een zelfstudie over het maken van een pijplijn om gegevens te kopiëren uit een Azure Blob Storage met Azure SQL Database [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een Azure blob-opslag naar een Azure SQL database kopiëren wilt, maakt u twee gekoppelde services als u wilt uw Azure storage-account en de Azure SQL database koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Azure Blob Storage zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de blob-container en de map waarin de invoergegevens. En maken van een andere gegevensset opgeven van de SQL-tabel in de Azure SQL-database waarin de gegevens die zijn gekopieerd uit de blobopslag. Zie voor eigenschappen van gegevensset die specifiek voor Azure Blob Storage zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u BlobSource als een bron- en SqlSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure SQL Database in Azure Blob-opslag kopiëren wilt, gebruikt u SqlSource en BlobSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor Azure Blob Storage zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink.  

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure Blob Storage, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-blob-storage  ) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Blob Storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Er zijn twee soorten gekoppelde services die kunt u een Azure Storage koppelen aan een Azure data factory. Ze zijn: **AzureStorage** gekoppelde service en **AzureStorageSas** gekoppelde service. De gekoppelde Azure Storage-service biedt de gegevensfactory met globale toegang tot Azure Storage. Terwijl de Azure Storage SAS (Shared Access Signature) gekoppelde biedt service de gegevensfactory met de toegang beperkt/tijdsgebonden naar Azure Storage. Er zijn geen andere verschillen tussen deze twee gekoppelde services. Kies de gekoppelde service die bij uw behoeften past. De volgende secties bevatten meer details over deze twee gekoppelde services.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Als u een gegevensset te vertegenwoordigen de invoer- of -gegevens in een Azure Blob Storage, die u stelt de eigenschap type van de gegevensset: **AzureBlob**. Stel de **linkedServiceName** eigenschap van de gegevensset op de naam van de Azure Storage of Azure Storage SAS gekoppelde service.  Geef de eigenschappen van het type van de gegevensset de **blob-container** en de **map** in blob storage.

Zie voor een volledige lijst van de JSON-secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

Data factory ondersteunt de volgende compatibel met CLS .NET op basis van type-waarden voor het ontwikkelen van type-informatie in 'structuur' voor het schema op lezen gegevensbronnen zoals Azure blob: Int16, Int32, Int64, één, Double, Decimal, Byte [], Bool, String, Guid, Datetime, DateTimeOffset, Timespan. Data Factory voert automatisch typeconversies bij het verplaatsen van gegevens uit een gegevensopslag bron naar een gegevensarchief sink.

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie opmaken enz., van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **AzureBlob** gegevensset heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de container en map in blob storage. Voorbeeld: myblobcontainer\myblobfolder\ |Ja |
| fileName |De naam van de blob. Bestandsnaam is optioneel en is hoofdlettergevoelig.<br/><br/>Als u een filename opgeeft, wordt de activiteit (inclusief kopiëren) werkt op de specifieke Blob.<br/><br/>FileName is opgegeven, bevat kopiëren alle Blobs in de folderPath voor invoergegevensset.<br/><br/>Wanneer **fileName** is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in activiteit sink, de naam van het gegenereerde bestand zou worden in de volgende indeling: Data.<Guid>. txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy is een optionele eigenschap. U kunt deze gebruiken om op te geven van een dynamische folderPath en de bestandsnaam voor de reeksgegevens. FolderPath kan bijvoorbeeld parameters worden gebruikt voor elk uur van gegevens. Zie de [partitionedBy eigenschap sectie](#using-partitionedBy-property) voor meer informatie en voorbeelden. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

### <a name="using-partitionedby-property"></a>Gebruik de eigenschap partitionedBy
Zoals vermeld in de vorige sectie, kunt u een dynamische folderPath en de bestandsnaam voor de time series-gegevens met de **partitionedBy** -eigenschap [Data Factory-functies en de systeemvariabelen](data-factory-functions-variables.md).

Zie voor meer informatie over tijd reeks gegevenssets, planning en segmenten [gegevenssets maken](data-factory-create-datasets.md) en [planning en uitvoering](data-factory-scheduling-and-execution.md) artikelen.

#### <a name="sample-1"></a>Voorbeeld 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

In dit voorbeeld {segment} is vervangen door de opgegeven waarde van de Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst voor het starten van de tijd van het segment. FolderPath verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104

#### <a name="sample-2"></a>Voorbeeld 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

In dit voorbeeld worden jaar, maand, dag en tijd van de SliceStart uitgepakt in verschillende variabelen die worden gebruikt door de eigenschappen voor folderPath en de bestandsnaam.

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle typen activiteiten. Dat eigenschappen beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put. Als u gegevens uit een Azure Blob-opslag verplaatst, u het brontype instellen in de kopieerbewerking naar **BlobSource**. Als u gegevens naar een Azure Blob Storage verplaatst, u stelt het sink-type in de kopieerbewerking naar **BlobSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door BlobSource en BlobSink.

**BlobSource** ondersteunt de volgende eigenschappen in de **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True (standaardwaarde), False |Nee |

**BlobSink** ondersteunt de volgende eigenschappen **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of het bestandssysteem. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><br/><b>MergeFiles</b>: alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand/de opgegeven Blob is opgegeven, zijn de samengevoegde bestandsnaam de opgegeven naam; anders zou worden automatisch gegenereerde naam. |Nee |

**BlobSource** biedt ook ondersteuning voor deze twee eigenschappen voor achterwaartse compatibiliteit.

* **treatEmptyAsNull**: Hiermee wordt aangegeven of null of lege tekenreeks behandelen als null-waarde.
* **skipHeaderLineCount** -geeft aan hoeveel lijnen moeten worden overgeslagen. Dit geldt alleen wanneer invoergegevensset gebruikmaakt TextFormat.

Op deze manier **BlobSink** ondersteunt de volgende eigenschap voor achterwaartse compatibiliteit.

* **blobWriterAddHeader**: Hiermee bepaalt u of een koptekst van de kolomdefinities toevoegen tijdens het schrijven naar een uitvoergegevensset.

Gegevenssets ondersteunen nu de volgende eigenschappen die dezelfde functionaliteit implementeren: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

De volgende tabel bevat richtlijnen over het gebruik van de eigenschappen van nieuwe gegevensset in plaats van deze eigenschappen van de bron/sink blob.

| Activiteitseigenschap kopiëren | Eigenschap DataSet |
|:--- |:--- |
| skipHeaderLineCount op BlobSource |skipLineCount en firstRowAsHeader. Regels eerst worden overgeslagen en wordt de eerste rij als een koptekst gelezen. |
| treatEmptyAsNull op BlobSource |treatEmptyAsNull op invoergegevensset |
| blobWriterAddHeader op BlobSink |firstRowAsHeader op uitvoergegevensset |

Zie [geven TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) sectie voor meer informatie over deze eigenschappen.    

### <a name="recursive-and-copybehavior-examples"></a>Voorbeelden van recursieve en copyBehavior
Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor verschillende combinaties van recursieve en copyBehavior waarden.

| Recursieve | copyBehavior | Resulterende gedrag |
| --- | --- | --- |
| waar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met dezelfde structuur als de bron<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| waar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 bestand2 + bestand3 + File4 + bestand 5 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| onwaar |preserveHierarchy |Voor een bronmap Map1 met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |flattenHierarchy |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |mergeFiles |Voor een bronmap Map1 met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a>Walkthrough: Wizard voor de kopie gebruiken om gegevens te kopiëren naar/van de Blob-opslag
Bekijk het snel kopiëren van gegevens uit een Azure blob storage. In dit overzicht bron- en doelserver gegevensopslag van het type: Azure Blob Storage. De pijplijn in dit scenario kopieert gegevens vanuit een map naar een andere map in de blobcontainer met dezelfde. In dit scenario is opzettelijk eenvoudig om weer te geven u eigenschappen of instellingen bij gebruik van Blob Storage als een bron- of sink. 

### <a name="prerequisites"></a>Vereisten
1. Maak een algemeen **Azure Storage-Account** als u nog niet hebt. U de blobopslag gebruiken als beide **bron** en **bestemming** gegevens opslaan in dit scenario. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account.
2. Maak een blobcontainer met de naam **adfblobconnector** in het opslagaccount. 
4. Maak een map met de naam **invoer** in de **adfblobconnector** container.
5. Maak een bestand met de naam **emp.txt** met de volgende inhoud en upload het naar de **invoer** map met hulpprogramma's zoals [Azure Opslagverkenner](https://azurestorageexplorer.codeplex.com/)
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-the-data-factory"></a>De gegevensfactory maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **+ nieuw** vanuit de linkerbovenhoek op **Intelligence en analyse**, en klik op **Data Factory**.
3. In de blade **Nieuwe gegevensfactory**:   
    1. Voer **ADFBlobConnectorDF** voor de **naam**. De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als u de foutmelding: `*Data factory name “ADFBlobConnectorDF” is not available`, wijzig de naam van de gegevensfactory (bijvoorbeeld yournameADFBlobConnectorDF) en probeert u het opnieuw. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    2. Selecteer uw Azure-**abonnement**.
    3. Selecteer voor de resourcegroep **gebruik bestaande** op een bestaande resourcegroep selecteren (of) selecteren **nieuw** een naam voor een resourcegroep in te voeren.
    4. Selecteer een **locatie** voor de gegevensfactory.
    5. Selecteer het selectievakje **Vastmaken aan dashboard** onderaan de blade.
    6. Klik op **Create**.
3. Nadat het maken voltooid is, ziet u de **Data Factory** blade zoals weergegeven in de volgende afbeelding: ![Data factory-startpagina](./media/data-factory-azure-blob-connector/data-factory-home-page.png)

### <a name="copy-wizard"></a>De wizard Kopiëren
1. Klik op de startpagina van de Data Factory de **kopiëren van gegevens [PREVIEW]** tegel starten **Data-Wizard kopiëren** op een afzonderlijke tabblad.    
    
    > [!NOTE]
    >    Als u ziet dat de webbrowser is vastgelopen bij 'Autoriseren...', schakelt **blokkeren van cookies van derden en sitegegevens** instellen (of) laat dit ingeschakeld en maakt een uitzondering voor **login.microsoftonline.com**en probeer het opnieuw starten van de wizard.
2. Op de pagina **Eigenschappen**:
    1. Voer **CopyPipeline** voor **taaknaam**. Naam van de taak is de naam van de pijplijn in uw gegevensfactory.
    2. Voer een **beschrijving** voor de taak (optioneel).
    3. Voor **taak uitgebracht of taakschema**, blijven de **regelmatig wordt uitgevoerd volgens schema** optie. Als u deze taak slechts één keer in plaats van uitvoeren volgens een schema herhaaldelijk uitvoeren wilt, selecteert u **eenmaal nu uitvoeren**. Als u selecteert, **eenmaal nu uitvoeren** optie, een [eenmalige pijplijn](data-factory-create-pipelines.md#onetime-pipeline) wordt gemaakt. 
    4. De instellingen voor bewaren **terugkerend patroon**. Deze taak wordt uitgevoerd dagelijks tussen de begin- en eindtijden die u in de volgende stap opgeeft.
    5. Wijzig de **begindatum tijd** naar **21/04/2017**. 
    6. Wijzig de **einddatum en-tijd** naar **25/04/2017**. U kunt naar het type van de datum in plaats van het bladeren door de kalender.     
    8. Klik op **Volgende**.
      ![Hulpprogramma voor kopiëren - pagina eigenschappen](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png) 
3. Op de pagina **Brongegevensarchief** klikt u op de tegel **Azure Blob Storage**. U gebruikt deze pagina om het brongegevensarchief op te geven voor de kopieertaak. U kunt een bestaande gekoppelde service van een gegevensarchief gebruiken of een nieuw gegevensarchief opgeven. Voor het gebruik van een bestaande gekoppelde service, selecteert u **van bestaande gekoppelde SERVICES** en selecteer de juiste gekoppelde service. 
    ![Hulpprogramma voor kopiëren - brongegevensarchief](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)
4. Op de pagina **Het Azure Blob Storage-account opgeven**:
   1. Houd de automatisch gegenereerde naam voor **verbindingsnaam**. Naam van de verbinding is de naam van de gekoppelde service van het type: Azure Storage. 
   2. Controleer of de optie **Van Azure-abonnementen** is geselecteerd als **accountselectiemethode**.
   3. Selecteer uw Azure-abonnement of houden **Alles selecteren** voor **Azure-abonnement**.   
   4. Selecteer een **Azure-opslagaccount** uit de lijst met Azure-opslagaccounts die beschikbaar is voor het abonnement dat u hebt geselecteerd. U kunt ook opslagaccountinstellingen handmatig door het selecteren van **handmatig invoeren** optie voor de **Accountselectiemethode**.
   5. Klik op **Volgende**. 
      ![Hulpprogramma voor kopiëren - het Azure Blob storage-account opgeven](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)
5. Op de pagina **Het invoerbestand of de invoermap kiezen**:
   1. Dubbelklik op **adfblobcontainer**.
   2. Selecteer **invoer**, en klik op **Kies**. In dit scenario maakt selecteren u de invoermap. U kunt ook selecteren de emp.txt-bestand in de map in plaats daarvan. 
      ![Hulpprogramma voor kopiëren - het invoerbestand of de invoermap kiezen](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)
6. Op de **het invoerbestand of de invoermap kiezen** pagina:
    1. Controleer of de **bestand of map** is ingesteld op **adfblobconnector/invoer**. Als de bestanden in submappen, bijvoorbeeld 04-01-2017, 2017/04/02 en enzovoort, en geef adfblobconnector/invoer / {year} / {month} / {day} voor bestand of map. Als u op TAB buiten het tekstvak drukt, ziet u drie vervolgkeuzelijsten indelingen selecteren voor jaar (jjjj), de maand (MM) en de dag (dd). 
    2. Stel geen **kopiëren van bestand recursief**. Selecteer deze optie om recursief bladeren door mappen voor bestanden worden gekopieerd naar de bestemming. 
    3. Niet de **binaire kopiëren** optie. Selecteer deze optie voor het uitvoeren van een binaire kopie van het bronbestand naar de bestemming. Selecteer niet voor dit scenario zodat u meer opties in de volgende pagina's kunt zien. 
    4. Controleer of de **compressietype** is ingesteld op **geen**. Selecteer een waarde voor deze optie als de bronbestanden in een van de ondersteunde indelingen zijn gecomprimeerd. 
    5. Klik op **Volgende**.
    ![Hulpprogramma voor kopiëren - het invoerbestand of de invoermap kiezen](./media/data-factory-azure-blob-connector/chose-input-file-folder.png) 
7. Op de pagina **Bestandsinstellingen** ziet u de scheidingstekens en het schema dat automatisch is gedetecteerd door de wizard tijdens het parseren van het bestand. 
    1. Bevestig de volgende opties: een. De **bestandsindeling** is ingesteld op **tekstindeling**. Hier ziet u de ondersteunde indelingen in de vervolgkeuzelijst. Bijvoorbeeld: JSON, Avro, ORC, parketvloeren.
        b. De **kolomscheidingsteken** is ingesteld op `Comma (,)`. Hier ziet u de andere kolom scheidingstekens die door Data Factory worden ondersteund in de vervolgkeuzelijst. U kunt ook een aangepaste scheidingsteken opgeven.
        c. De **rijscheidingsteken** is ingesteld op `Carriage Return + Line feed (\r\n)`. Hier ziet u de andere rij scheidingstekens die door Data Factory worden ondersteund in de vervolgkeuzelijst. U kunt ook een aangepaste scheidingsteken opgeven.
        d. De **aantal regels overslaan** is ingesteld op **0**. Als u een paar regels moet worden overgeslagen aan de bovenkant van het bestand wilt, voert u hier het nummer.
        e.  De **eerste gegevensrij kolomnamen bevat** is niet ingesteld. Selecteer deze optie als de bronbestanden kolomnamen in de eerste rij bevatten.
        f. De **leeg kolomwaarde behandelen als null** optie is ingesteld.
    2. Vouw **geavanceerde instellingen** om geavanceerde optie beschikbaar te zien.
    3. Aan de onderkant van de pagina, Zie de **preview** van gegevens van het bestand emp.txt.
    4. Klik op **SCHEMA** tabblad onderaan om te zien van het schema dat u de wizard kopiëren die zijn afgeleid door te kijken naar de gegevens in het bronbestand.
    5. Klik op **Volgende** nadat u de scheidingstekens hebt gecontroleerd en een voorbeeld van de gegevens hebt bekeken.
    ![Hulpprogramma voor kopiëren - bestandsindelingsinstellingen](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)  
8. Op de **bestemming gegevensarchief pagina**, selecteer **Azure Blob Storage**, en klik op **volgende**. Als zowel de bron- en doelserver gegevensarchieven in dit scenario gebruikt u de Azure Blob Storage.    
    ![Hulpprogramma voor kopiëren - gegevensarchief van de doelserver selecteren](media/data-factory-azure-blob-connector/select-destination-data-store.png)
9. Op **de Azure Blob storage-account opgeven** pagina:
   1. Voer **AzureStorageLinkedService** voor de **verbindingsnaam** veld.
   2. Controleer of de optie **Van Azure-abonnementen** is geselecteerd als **accountselectiemethode**.
   3. Selecteer uw Azure-**abonnement**.  
   4. Selecteer uw Azure storage-account. 
   5. Klik op **Volgende**.     
10. Op de **het uitvoerbestand of de invoermap kiezen** pagina: 
    6. Geef **mappad** als **adfblobconnector/uitvoer / {year} / {month} / {day}**. Voer **tabblad**.
    7. Voor de **jaar**, selecteer **jjjj**.
    8. Voor de **maand**, bevestig dat is ingesteld op **MM**.
    9. Voor de **dag**, bevestig dat is ingesteld op **dd**.
    10. Controleer of de **compressietype** is ingesteld op **geen**.
    11. Controleer of de **gedrag kopiëren** is ingesteld op **bestanden samenvoegen**. Als het uitvoerbestand met dezelfde naam al bestaat, wordt de nieuwe inhoud toegevoegd aan hetzelfde bestand aan het einde.
    12. Klik op **Volgende**.
    ![Hulpprogramma voor kopiëren - bestand voor uitvoer of de invoermap kiezen](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)
11. Op de **bestandsindelingsinstellingen** pagina, controleert u de instellingen en klik op **volgende**. Een van de aanvullende opties hier is een koptekst kunt toevoegen aan het uitvoerbestand. Als u deze optie selecteert, wordt een veldnamenrij toegevoegd met namen van de kolommen van het schema van de bron. Wanneer u het schema voor de bron weergeeft, kunt u de standaardkolomnamen wijzigen. U kan bijvoorbeeld de eerste kolom wijzigen voornaam en achternaam tweede kolom. Vervolgens wordt het bestand voor uitvoer gegenereerd met een header met deze namen als kolomnamen. 
    ![Hulpprogramma voor kopiëren - bestandsindelingsinstellingen voor bestemming](media/data-factory-azure-blob-connector/file-format-destination.png)
12. Op de **prestatie-instellingen** pagina, controleert u of **eenheden cloud** en **kopieën parallelle** zijn ingesteld op **automatisch**, en klik op volgende. Zie voor meer informatie over deze instellingen [activiteit prestaties en prestatieafstemming handleiding kopiëren](data-factory-copy-activity-performance.md#parallel-copy).
    ![Hulpprogramma voor kopiëren - prestatie-instellingen](media/data-factory-azure-blob-connector/copy-performance-settings.png) 
14. Op de **samenvatting** pagina, controleert u alle instellingen (taakeigenschappen, instellingen voor de bron- en doelserver en instellingen) en klik op **volgende**.
    ![Hulpprogramma voor kopiëren - pagina overzicht](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)
15. Lees de informatie op de pagina **Samenvatting** en klik op **Voltooien**. De wizard maakt twee gekoppelde services, twee gegevenssets (invoer en uitvoer) en één pijplijn in de gegevensfactory (van waaruit u de wizard Kopiëren hebt gestart).
    ![Hulpprogramma voor kopiëren - pagina van de implementatie](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)

### <a name="monitor-the-pipeline-copy-task"></a>Bewaken van de pijplijn (taak kopiëren)

1. Klik op de koppeling `Click here to monitor copy pipeline` op de **implementatie** pagina. 
2. U ziet de **bewaken en beheren van de toepassing** op een afzonderlijke tabblad.  ![Bewaken en beheren van App](media/data-factory-azure-blob-connector/monitor-manage-app.png)
3. Wijzig de **start** tijd boven aan `04/19/2017` en **end** tijd `04/27/2017`, en klik vervolgens op **toepassen**. 
4. Ziet u vijf activiteit windows in de **ACTIVITEITSVENSTERS** lijst. De **WindowStart** tijden moeten voldoende zijn voor alle dagen vanaf het begin van de pijplijn tot eindtijden pipeline. 
5. Klik op **vernieuwen** knop voor de **ACTIVITEITSVENSTERS** lijst een paar keer tot u de status van alle activiteitsvensters is ingesteld op gereed. 
6. Controleer nu of dat de uitvoerbestanden worden gegenereerd in de map voor uitvoer van de container adfblobconnector. Hier ziet u de volgende mapstructuur in de uitvoermap: 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
Zie voor gedetailleerde informatie over het controleren en beheren van gegevensfactory [bewaken en beheren van de Data Factory-pijplijn](data-factory-monitor-manage-app.md) artikel. 
 
### <a name="data-factory-entities"></a>Data Factory-entiteiten
Nu Ga terug naar het tabblad met de Data Factory-startpagina. U ziet dat er twee gekoppelde services, twee gegevenssets en een pijplijn in uw data factory nu zijn. 

![Data Factory-startpagina met entiteiten](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

Klik op **auteur en implementeren van** Data Factory-Editor te starten. 

![Data Factory Editor](media/data-factory-azure-blob-connector/data-factory-editor.png)

U ziet de volgende Data Factory-entiteiten in uw data factory: 

 - Twee gekoppelde services. Een voor de bron en de andere is voor de bestemming. De gekoppelde services verwijzen naar hetzelfde Azure-opslagaccount in dit scenario. 
 - Twee gegevenssets. Een invoergegevensset en een uitvoergegevensset. In dit scenario maakt gebruik van de blobcontainer met dezelfde beide maar verwijzen naar andere mappen (invoer en uitvoer).
 - Een pijplijn. De pijplijn bevat een kopieeractiviteit die gebruikmaakt van een blob-bron- en een blob-sink gegevens vanaf de locatie van een Azure-blob kopiëren naar een andere locatie van de Azure-blob. 

De volgende secties bevatten meer informatie over deze entiteiten. 

#### <a name="linked-services"></a>Gekoppelde services
Hier ziet u twee gekoppelde services. Een voor de bron en de andere is voor de bestemming. In dit overzicht beide definities er hetzelfde uitzien, met uitzondering van de namen. De **type** van de gekoppelde service is ingesteld op **AzureStorage**. Belangrijkste eigenschap van de definitie van de gekoppelde service is de **connectionString**, die wordt gebruikt door de Data Factory verbinding maken met uw Azure Storage-account tijdens runtime. De eigenschap hubName in de definitie negeren. 

##### <a name="source-blob-storage-linked-service"></a>Bron blob-opslag gekoppelde service
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a>Bestemming blob-opslag gekoppelde service

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

Zie voor meer informatie over Azure Storage gekoppelde service [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 

#### <a name="datasets"></a>Gegevenssets
Er zijn twee gegevenssets: een invoergegevensset en een uitvoergegevensset. Het type van de gegevensset is ingesteld op **AzureBlob** voor beide. 

Invoergegevensset verwijst naar de **invoer** map van de **adfblobconnector** blob-container. De **externe** eigenschap is ingesteld op **true** voor deze gegevensset omdat de gegevens niet wordt geproduceerd door de pijplijn met de kopieeractiviteit waarmee deze dataset als invoer. 

De uitvoergegevensset die verwijst naar de **uitvoer** map van de dezelfde blobcontainer. De uitvoergegevensset wordt ook gebruikgemaakt van het jaar, maand en dag van de **SliceStart** systeemvariabele dynamisch evalueren van het pad naar het uitvoerbestand. Zie voor een lijst met functies en ondersteund door Data Factory systeemvariabelen [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md). De **externe** eigenschap is ingesteld op **false** (standaardwaarde) omdat deze gegevensset wordt geproduceerd door de pijplijn. 

Zie voor meer informatie over de eigenschappen die worden ondersteund door Azure Blob-gegevensset [eigenschappen van gegevensset](#dataset-properties) sectie.

##### <a name="input-dataset"></a>Invoergegevensset

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a>Uitvoergegevensset

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a>Pijplijn
De pijplijn heeft slechts één activiteit. De **type** van de activiteit is ingesteld op **kopie**.  In de type-eigenschappen voor de activiteit zijn er twee secties, één voor de gegevensbron en de andere één voor sink. Het brontype is ingesteld op **BlobSource** als de activiteit van gegevens van een blob-opslag kopiëren. Het sink-type is ingesteld op **BlobSink** als de activiteit voor het kopiëren van gegevens naar een blob-opslag. De kopieeractiviteit duurt InputDataset-z4y als invoer en OutputDataset-z4y als uitvoer. 

Zie voor meer informatie over de eigenschappen die worden ondersteund door BlobSource en BlobSink [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van Blob-opslag  
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van en naar Azure Blob Storage en Azure SQL Database kopiëren. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a>JSON-voorbeeld: Gegevens kopiëren van Blob-opslag naar SQL-Database
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [BlobSource](#copy-activity-properties) en [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).

De voorbeeld-kopieën timeseries gegevens van een Azure-blob naar een Azure SQL tabel per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL gekoppelde service:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Gekoppelde Azure Storage-service:**

```json
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
Azure Data Factory ondersteunt twee soorten gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de sleutel van de account opgeven voor de eerste en voor de latere, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.  

**Azure Blob invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling Data Factory informeert dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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
**Azure SQL-uitvoergegevensset:**

De voorbeeldgegevens kopieert naar een tabel met de naam "MijnTabel" in een Azure SQL database. De tabel in uw Azure SQL database met hetzelfde aantal kolommen maken, zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
**Een kopieeractiviteit in een pijplijn met Blob-bron en sink SQL:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
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
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van Azure SQL naar Azure Blob
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) en [BlobSink](#copy-activity-properties).

Het voorbeeld kopieert tijdreeks gegevens van een Azure SQL-tabel naar een Azure-blob per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure SQL gekoppelde service:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Gekoppelde Azure Storage-service:**

```json
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
Azure Data Factory ondersteunt twee soorten gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de sleutel van de account opgeven voor de eerste en voor de latere, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.  

**Azure SQL invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in Azure SQL en bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens.

Instelling 'extern': 'true' informeert Data Factory-service dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
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

**Een kopieeractiviteit in een pijplijn met de SQL-bron en sink van Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
