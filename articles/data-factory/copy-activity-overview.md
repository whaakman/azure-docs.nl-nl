---
title: Kopieeractiviteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over de kopieeractiviteit in Azure Data Factory die u gebruiken kunt om gegevens te kopiëren van een ondersteund brongegevensarchief naar een ondersteunde sink-gegevensarchief.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: jingwang
ms.openlocfilehash: 154e0dcefab6d5bcdfc9532ba4258d09593f0970
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311128"
---
# <a name="copy-activity-in-azure-data-factory"></a>De Kopieeractiviteit in Azure Data Factory

## <a name="overview"></a>Overzicht

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-data-movement-activities.md)
> * [Huidige versie](copy-activity-overview.md)

In Azure Data Factory, kunt u Kopieeractiviteit om gegevens tussen gegevens winkels zich on-premises en in de cloud te kopiëren. Nadat de gegevens worden gekopieerd, kan worden verder verwerkt en geanalyseerd. U kunt ook een Kopieeractiviteit gebruiken om transformatie en analyseresultaten voor business intelligence (BI) en het verbruik van de toepassing te publiceren.

![Rol van Kopieeractiviteit](media/copy-activity-overview/copy-activity.png)

Kopieeractiviteit wordt uitgevoerd op een [Integration Runtime](concepts-integration-runtime.md). Verschillende gegevens kopiëren scenario voor een kan andere versie van Integration Runtime worden gebruikt:

* Bij het kopiëren van gegevens tussen de gegevens worden opgeslagen dat beide openbaar toegankelijk zijn, kopieeractiviteit gemachtigd zijn door **Azure Integration Runtime**, die is beveiligd, betrouwbaar, schaalbare en [wereldwijd beschikbaar](concepts-integration-runtime.md#integration-runtime-location).
* Wanneer gegevens kopiëren van/naar de opgeslagen gegevens zich on-premises of in een netwerk met toegangsbeheer (bijvoorbeeld Azure Virtual Network), moet u voor het instellen van een **geïntegreerde Runtime zelf-hostend** om te kopiëren van gegevens.

Integratieruntime moet worden gekoppeld aan elke bron en sink-gegevensopslag. Meer informatie over hoe u kopieeractiviteit [bepaalt welke IR u moet gebruiken](concepts-integration-runtime.md#determining-which-ir-to-use).

Kopieeractiviteit verloopt via de volgende fasen om gegevens te kopiëren van een bron naar een sink. De service die wordt gebruikt door de Kopieeractiviteit:

1. Leest gegevens uit een bron-gegevensopslag.
2. Voert serialisatie/deserialisatie, compressie/decompressie, kolomtoewijzing, enzovoort. Dit gebeurt deze bewerkingen op basis van de configuraties van de invoergegevensset, de uitvoergegevensset en de Kopieeractiviteit.
3. Schrijft gegevens naar het sink/doelgegevensarchief.

![Overzicht kopieeractiviteit](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Ondersteunde gegevensarchieven en indelingen

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Ondersteunde bestandsindelingen

U kunt Copy-activiteit naar **bestanden als kopiëren-is** tussen twee bestanden zijn gebaseerd, in dat geval de gegevens worden gekopieerd efficiënt zonder een serialisatie/deserialisatie.

Copy Activity biedt ook ondersteuning voor lezen van en schrijven naar bestanden in een opgegeven indeling: **Tekst, JSON, Avro, ORC en Parquet**, en compressiecodec **GZip, Deflate, BZip2 en ZipDeflate** worden ondersteund. Zie [ondersteunde indelingen voor bestanden en compressie](supported-file-formats-and-compression-codecs.md) met details.

U kunt bijvoorbeeld de volgende kopieeractiviteiten doen:

* Kopiëren van gegevens in on-premises SQL Server en schrijven naar Azure Data Lake Store in ORC-indeling.
* Bestanden kopiëren in tekstindeling (CSV) van on-premises bestandssysteem en naar Azure-Blob in Avro-indeling wilt schrijven.
* ZIP-bestanden kopiëren van on-premises bestandssysteem en decomprimeren vervolgens land Azure Data Lake Store.
* Kopiëren van gegevens in de indeling van GZip gecomprimeerde tekstbestand (CSV) van Azure Blob en schrijven naar Azure SQL Database.

## <a name="supported-regions"></a>Ondersteunde regio’s

De service die wordt gebruikt door de Kopieeractiviteit is wereldwijd beschikbaar in de regio's en regio's die worden vermeld in [Azure Integration Runtime locaties](concepts-integration-runtime.md#integration-runtime-location). De algemeen beschikbare topologie zorgt u ervoor efficiënt gegevensverplaatsing die meestal regio-overschrijdende hops voorkomt. Zie [-Services per regio](https://azure.microsoft.com/regions/#services) voor beschikbaarheid van Data Factory en de verplaatsing van gegevens in een regio.

## <a name="configuration"></a>Configuratie

Voor het gebruik van kopieeractiviteit in Azure Data Factory, moet u naar:

1. **Maak gekoppelde services voor gegevensopslag van de bron en sink-gegevensopslag.** Raadpleeg het connector-artikel 'Gekoppelde service-eigenschappen' gedeelte over het configureren en de ondersteunde eigenschappen. U vindt de lijst met ondersteunde connector in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats) sectie.
2. **Gegevenssets voor de bron en sink maken.** Raadpleeg de bron en sink van de sectie 'Dependencies voor gegevensset' connector-artikelen over het configureren en de ondersteunde eigenschappen.
3. **Een pijplijn maken met de kopieeractiviteit.** De volgende sectie bevat een voorbeeld.  

### <a name="syntax"></a>Syntaxis

De volgende sjabloon van een kopieeractiviteit bevat een uitgebreide lijst met ondersteunde eigenschappen. Geef de waarden die geschikt zijn voor uw scenario.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Syntaxis van de details

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van een kopieeractiviteit moet worden ingesteld op: **kopiëren** | Ja |
| invoer | Geef op de gegevensset die u hebt gemaakt die verwijst naar de brongegevens. Kopieeractiviteit ondersteunt alleen een één invoer. | Ja |
| uitvoer | Geef op de gegevensset die u hebt gemaakt die verwijst naar de sink-gegevens. Kopieeractiviteit ondersteunt slechts één uitvoer. | Ja |
| typeProperties | Een groep met eigenschappen te configureren van de kopieeractiviteit. | Ja |
| source | Het brontype voor kopiëren en de bijbehorende eigenschappen opgeven over het ophalen van gegevens.<br/><br/>Meer details in de sectie 'Eigenschappen van de Kopieeractiviteit' connector-artikel die worden vermeld in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats). | Ja |
| sink | Geef de sink-type voor kopiëren en de bijbehorende eigenschappen voor het schrijven van gegevens.<br/><br/>Meer details in de sectie 'Eigenschappen van de Kopieeractiviteit' connector-artikel die worden vermeld in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats). | Ja |
| Translator | Geef expliciete kolomtoewijzingen van bron naar een sink. Is van toepassing wanneer het standaardgedrag van het exemplaar kan niet voldoen aan uw behoeften.<br/><br/>Informatie over de details van [toewijzing van het type Schema en gegevens](copy-activity-schema-and-type-mapping.md). | Nee |
| dataIntegrationUnits | Geef de powerfulness van [Azure Integration Runtime](concepts-integration-runtime.md) om te kopiëren van gegevens. Voorheen bekend als cloud Data Movement eenheden (DMU). <br/><br/>Informatie over de details van [integratie gegevenseenheden](copy-activity-performance.md#data-integration-units). | Nee |
| parallelCopies | Geef de parallelle uitvoering die u wilt dat Copy-activiteit naar het sink-wordt gebruikt bij het lezen van gegevens uit de bron en het schrijven van gegevens.<br/><br/>Informatie over de details van [kopie parallelle](copy-activity-performance.md#parallel-copy). | Nee |
| enableStaging<br/>stagingSettings | Wilt u de tussentijdse gegevens in aa blob-opslag in plaats van rechtstreeks gegevens kopiëren van bron naar het sink-fase.<br/><br/>Meer informatie over de handige scenario's en de configuratiedetails van de van [gefaseerd kopiëren](copy-activity-performance.md#staged-copy). | Nee |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Kiezen hoe u voor het afhandelen van incompatibele rijen tijdens het kopiëren van gegevens van bron naar een sink.<br/><br/>Informatie over de details van [fouttolerantie](copy-activity-fault-tolerance.md). | Nee |

## <a name="monitoring"></a>Bewaking

U kunt controleren met de kopieeractiviteit in Azure Data Factory 'Maken en controleren' gebruikersinterface of via een programma uitvoeren. U kunt vervolgens vergelijken de prestaties en configuratie van uw scenario aan Copy-activiteit [prestaties verwijzing](copy-activity-performance.md#performance-reference) van interne testen.

### <a name="monitor-visually"></a>Visueel bewaken

Als u wilt de copy-activiteit uitvoeren visueel te controleren, Ga naar uw data factory -> **Author & Monitor** -> **controletabblad**, ziet u een lijst van de pijplijn wordt uitgevoerd met een koppeling 'Uitvoeringen van activiteit weergeven' in de  **Acties** kolom. 

![Pijplijnuitvoeringen controleren](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Klik om te zien van de lijst met activiteiten in deze pijplijn-run. In de **acties** kolom, hebt u koppelingen naar de kopie activiteit invoer, uitvoer, fouten (als de uitvoering van activiteit kopiëren mislukt) en details.

![Uitvoering van activiteiten controleren](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Klik op de '**Details**' koppeling onder **acties** om te zien van de kopieeractiviteit uitvoeringsdetails en prestatiekenmerken. Hier ziet u informatie, inclusief volume/rijen/bestanden van de gegevens opgehaald uit de bron om op te vangen, doorvoer, stappen gaat door met de bijbehorende tijdsduur en gebruikt als deze configuraties voor uw scenario kopiëren. 

>[!TIP]
>Voor sommige scenario's, ziet u ook '**tips afstemmen van prestaties**"boven op het kopiëren van bewaking van de pagina die leest u het knelpunt geïdentificeerd en helpt u op wat u wilt wijzigen zodat deze kopie doorvoer te verhogen, Zie het voorbeeld met details [hier](#performance-and-tuning).

**Voorbeeld: kopiëren van Amazon S3 naar Azure Data Lake Store**
![details uitvoering van activiteiten controleren](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Voorbeeld: kopiëren uit een Azure SQL Database naar Azure SQL Data Warehouse met behulp van gefaseerd kopiëren**
![details uitvoering van activiteiten controleren](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Controleren via een programma

Details van uitvoering van activiteit kopiëren en prestatiekenmerken worden ook geretourneerd in de Kopieeractiviteit uitvoering uitvoer sectie ->. Hieronder vindt u een volledige lijst. alleen van toepassing die op uw scenario kopiëren worden weergegeven. Informatie over het bewaken van de uitvoering van activiteiten [snelstartgids sectie bewaking](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Naam van eigenschap  | Description | Eenheid |
|:--- |:--- |:--- |
| DataRead | Grootte van de gegevens lezen van bron | Waarde voor Int64 in **bytes** |
| Datawritten door | Grootte van gegevens naar een sink geschreven | Waarde voor Int64 in **bytes** |
| filesRead | Het aantal bestanden die bij het kopiëren van gegevens uit de opslag van bestanden worden gekopieerd. | Waarde voor Int64 (geen unit) |
| fileScanned | Het aantal bestanden worden gescand van de bron van file storage. | Waarde voor Int64 (geen unit) |
| filesWritten | Het aantal bestanden die bij het kopiëren van gegevens met file storage worden gekopieerd. | Waarde voor Int64 (geen unit) |
| rowsCopied | Het aantal rijen (niet van toepassing voor binaire kopie) wordt gekopieerd. | Waarde voor Int64 (geen unit) |
| rowsSkipped | Het aantal niet-compatibele rijen wordt overgeslagen. U kunt de functie inschakelen door de set 'enableSkipIncompatibleRow' op ' True '. | Waarde voor Int64 (geen unit) |
| Doorvoer | Verhouding waarmee gegevens worden overgedragen | Drijvendekommagetal in **KB/s** |
| copyDuration | De duur van de kopie | Int32-waarde in seconden |
| sqlDwPolyBase | Als u PolyBase is gebruikt bij het kopiëren van gegevens in SQL Data Warehouse. | Booleaans |
| redshiftUnload | Als uit het geheugen laden wordt gebruikt bij het kopiëren van gegevens van Redshift. | Booleaans |
| hdfsDistcp | Als DistCp wordt gebruikt bij het kopiëren van gegevens uit HDFS. | Booleaans |
| effectiveIntegrationRuntime | Laten zien welke integratie Runtime(s) wordt gebruikt voor het maken van de activiteit die wordt uitgevoerd, in de indeling van `<IR name> (<region if it's Azure IR>)`. | Tekst (tekenreeks) |
| usedDataIntegrationUnits | De effectieve gegevens integratie eenheden tijdens het kopiëren. | Int32-waarde |
| usedParallelCopies | De effectieve parallelCopies tijdens het kopiëren. | Int32-waarde|
| redirectRowPath | Pad naar het logboek van overgeslagen incompatibele rijen in de blob-opslag die u onder "redirectIncompatibleRowSettings" configureren. Zie onderstaande voorbeeld. | Tekst (tekenreeks) |
| executionDetails | Meer informatie over de fasen copy-activiteit doorloopt, en de bijbehorende stappen, duur, gebruikte configuraties, enzovoort. Het is niet raadzaam deze sectie parseren als kan worden gewijzigd. | Matrix |

```json
"output": {
    "dataRead": 107280845500,
    "dataWritten": 107280845500,
    "filesRead": 10,
    "filesWritten": 10,
    "copyDuration": 224,
    "throughput": 467707.344,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US 2)",
    "usedDataIntegrationUnits": 32,
    "usedParallelCopies": 8,
    "executionDetails": [
        {
            "source": {
                "type": "AmazonS3"
            },
            "sink": {
                "type": "AzureDataLakeStore"
            },
            "status": "Succeeded",
            "start": "2018-01-17T15:13:00.3515165Z",
            "duration": 221,
            "usedDataIntegrationUnits": 32,
            "usedParallelCopies": 8,
            "detailedDurations": {
                "queuingDuration": 2,
                "transferDuration": 219
            }
        }
    ]
}
```

## <a name="schema-and-data-type-mapping"></a>Schema en de toewijzing van het type gegevens

Zie de [toewijzing van het type Schema en gegevens](copy-activity-schema-and-type-mapping.md), waarin wordt beschreven hoe copy activity in uw brongegevens naar het sink-kaarten.

## <a name="fault-tolerance"></a>Fouttolerantie

Standaard kopieeractiviteit kopiëren van gegevens gestopt en fout retourneert wanneer er niet-compatibele gegevens tussen de bron en sink. U kunt expliciet configureren als u wilt overslaan en meld u de niet-compatibele rijen en alleen deze compatibel gegevens kopiëren naar de kopie is voltooid. Zie de [Kopieeractiviteit fouttolerantie](copy-activity-fault-tolerance.md) op meer informatie.

## <a name="performance-and-tuning"></a>Prestaties en afstemmen

Zie de [Kopieeractiviteit prestatie- en afstemmingshandleiding](copy-activity-performance.md), waarin belangrijke factoren die invloed hebben op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory wordt beschreven. Ook geeft een lijst van de waargenomen prestaties tijdens interne tests van en worden verschillende manieren om te optimaliseren de prestaties van Kopieeractiviteit besproken.

In sommige gevallen, als u een kopieeractiviteit in ADF uitvoert, rechtstreeks ziet u '**tips afstemmen van prestaties**"boven de [kopieeractiviteit bewaking pagina](#monitor-visually) zoals wordt weergegeven in het volgende voorbeeld. Het niet alleen leest u het knelpunt geïdentificeerd voor de uitvoering van het opgegeven exemplaar, maar ook helpt u op wat u wilt wijzigen zodat deze kopie doorvoer te verhogen. De prestaties van tips voor het afstemmen momenteel suggesties wilt bieden het kopiëren van gegevens in Azure SQL Data Warehouse PolyBase gebruiken om te vergroten Azure Cosmos DB RU of Azure SQL DB DTU wanneer de resource op gegevens opslaan aan clientzijde is het knelpunt, de overbodige gefaseerd verwijderen kopiëren, enzovoort. De afstemming van regels wordt geleidelijk ook uitgebreid zijn.

**Voorbeeld: kopiëren naar Azure SQL-database met tips afstemmen van prestaties**

In dit voorbeeld tijdens het kopiëren van uitvoert, ADF kennisgeving die de sink-Azure SQL DB bereikt hoog DTU-gebruik die de bewerkingen voor schrijven vertraagt, dus het voorstel is om te verhogen van de Azure SQL DB-laag met meer DTU. 

![Kopiëren met tips afstemmen van de prestaties controleren](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Incrementele kopie 
Data Factory biedt ondersteuning voor scenario's voor het incrementeel kopiëren van delta-gegevens van een brongegevensarchief naar een doelgegevensarchief. Zie [zelfstudie: gegevens stapsgewijs kopiëren](tutorial-incremental-copy-overview.md). 

## <a name="read-and-write-partitioned-data"></a>Gepartitioneerde gegevens lezen en schrijven
Azure Data Factory ondersteund in versie 1, lezen of schrijven van gepartitioneerde gegevens met behulp van de slicestart-waarde/SliceEnd/WindowStart/WindowEnd systeemvariabelen. In de huidige versie, kunt u dit gedrag bewerkstelligen met behulp van een pijplijnparameter en de tijd/geplande begintijd van trigger als een waarde van de parameter. Zie voor meer informatie, [lezen of schrijven gegevens gepartitioneerd](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende snelstartgidsen, zelfstudies en voorbeelden:

- [Gegevens van de ene locatie naar een andere locatie in dezelfde Azure Blob Storage kopiëren](quickstart-create-data-factory-dot-net.md)
- [Gegevens kopiëren van Azure Blob Storage naar Azure SQL Database](tutorial-copy-data-dot-net.md)
- [Gegevens kopiëren van on-premises SQL Server naar Azure](tutorial-hybrid-copy-powershell.md)
