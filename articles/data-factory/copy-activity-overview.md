---
title: Kopieeractiviteit in Azure Data Factory | Microsoft Docs
description: "Meer informatie over de kopieeractiviteit in Azure Data Factory waarmee u kunt gegevens van een ondersteunde brongegevensarchief kopiëren naar een ondersteunde sink-gegevensopslag."
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
ms.date: 12/15/2017
ms.author: jingwang
ms.openlocfilehash: 7786fc785afa745da28b1da644ec58568d0cf424
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="copy-activity-in-azure-data-factory"></a>De Kopieeractiviteit in Azure Data Factory

## <a name="overview"></a>Overzicht

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-data-movement-activities.md)
> * [Versie 2 - Preview](copy-activity-overview.md)

U kunt in Azure Data Factory Kopieeractiviteit gebruiken om gegevens tussen gegevens winkels zich lokaal en in de cloud te kopiëren. Nadat de gegevens worden gekopieerd, worden deze verder getransformeerd en geanalyseerd. U kunt ook Kopieeractiviteit gebruiken om de transformatie en analyseresultaten voor business intelligence (BI) en het verbruik van de toepassing te publiceren.

![Rol van kopieerbewerking](media/copy-activity-overview/copy-activity.png)

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

Kopieeractiviteit wordt uitgevoerd op een [integratie Runtime](concepts-integration-runtime.md). Voor het scenario voor het kopiëren van gegevens op verschillende kan andere versie van de Runtime-integratie worden gebruikt:

* Bij het kopiëren van gegevens tussen gegevens opslaat in beide openbaar toegankelijk zijn, kopieeractiviteit kan worden gemachtigd door **Azure integratie Runtime**, die is beveiligd, betrouwbaar, schaalbare en [wereldwijd beschikbare](concepts-integration-runtime.md#integration-runtime-location).
* Wanneer lokale kopiëren van gegevens van/naar gegevensarchieven bevindt of in een netwerk met toegangsbeheer (bijvoorbeeld Azure Virtual Network), moet u voor het instellen van een **zelf gehost geïntegreerde Runtime** zorgen dat gegevens opnieuw te kopiëren.

Integratie Runtime moet worden gekoppeld aan elke bron- en sink-gegevensarchief. Meer informatie over het kopieeractiviteit [bepaalt welke IR gebruiken](concepts-integration-runtime.md#determining-which-ir-to-use).

Kopieeractiviteit gaat via de volgende fasen gegevens kopiëren van een bron naar een sink. De service die ook door de Kopieeractiviteit:

1. Leest de gegevens uit een bron-gegevensarchief.
2. Voert serialisatie/deserialisatie, compressie/decompressie, kolomtoewijzing, enzovoort. Deze bewerkingen op basis van de configuraties van de invoergegevensset, uitvoergegevensset en Kopieeractiviteit gebeurt.
3. Schrijft gegevens naar het gegevensarchief sink/het doel.

![Overzicht kopieeractiviteit](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Ondersteunde gegevensarchieven en indelingen

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Ondersteunde bestandsindelingen

U kunt een Kopieeractiviteit om te **kopiëren van bestanden als-is** tussen twee bestandsgebaseerde gegevensarchieven, in dat geval de gegevens is gekopieerd efficiënt zonder een serialisatie/deserialisatie.

Kopieeractiviteit biedt ook ondersteuning voor lezen van en schrijven naar de bestanden in de opgegeven indelingen: **tekst, JSON Avro, ORC en parketvloeren**, en compressiecodec **GZip, Deflate, BZip2 en ZipDeflate** worden ondersteund. Zie [ondersteunde indelingen voor bestands- en compressie](supported-file-formats-and-compression-codecs.md) met details.

U kunt bijvoorbeeld de volgende kopie activiteiten doen:

* Gegevens kopiëren in on-premises SQL-Server en schrijven naar Azure Data Lake Store ORC-indeling.
* Kopieer de bestanden in tekstindeling (CSV) van de on-premises bestandssysteem en schrijven naar Azure Blob Avro-indeling.
* ZIP-bestanden kopiëren van on-premises bestandssysteem en decomprimeren vervolgens land naar Azure Data Lake Store.
* Kopiëren van gegevens in de GZip gecomprimeerde tekst (CSV)-indeling van Azure Blob en schrijven naar Azure SQL Database.

## <a name="supported-regions"></a>Ondersteunde regio’s

De service die ook door de kopieerbewerking is beschikbaar globaal in de regio's en locaties die worden vermeld in [Azure integratie Runtime locaties](concepts-integration-runtime.md#integration-runtime-location). De topologie wereldwijd beschikbare zorgt ervoor dat efficiënt gegevensverplaatsing die meestal regio-overschrijdende hops voorkomt. Zie [-Services per regio](https://azure.microsoft.com/regions/#services) voor beschikbaarheid van de Gegevensfactory en de verplaatsing van gegevens in een regio.

## <a name="configuration"></a>Configuratie

Als u de kopieeractiviteit in Azure Data Factory, moet u:

1. **Gekoppelde services voor gegevensopslag van de bron en sink-gegevensarchief maken.** Raadpleeg de connector-artikel 'Gekoppelde service-eigenschappen' gedeelte van het configureren en de ondersteunde eigenschappen. U vindt de lijst met ondersteunde connector in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats) sectie.
2. **Gegevenssets voor de bron- en sink maken.** Raadpleeg de bron en sink van connector-artikelen 'Eigenschappen van gegevensset' sectie voor het configureren en de ondersteunde eigenschappen.
3. **Een pijplijn maken met de kopieeractiviteit.** De volgende sectie biedt een voorbeeld.  

### <a name="syntax"></a>Syntaxis

De volgende sjabloon van een kopieeractiviteit bevat een uitgebreide lijst met ondersteunde eigenschappen. Geef de waarden die uw scenario past.

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
            "cloudDataMovementUnits": <number>,
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

### <a name="syntax-details"></a>Details van de syntaxis

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van een kopieeractiviteit moet worden ingesteld op: **kopiëren** | Ja |
| Invoer | Geef de gegevensset die u hebt gemaakt die verwijst naar de brongegevens. Kopieeractiviteit ondersteunt alleen een één invoer. | Ja |
| uitvoer | Geef de gegevensset die u hebt gemaakt die verwijst naar de sink-gegevens. Kopieeractiviteit ondersteunt slechts één uitvoer. | Ja |
| typeProperties | Een groep met eigenschappen voor het configureren van de kopieeractiviteit. | Ja |
| bron | Het brontype van het exemplaar en de bijbehorende eigenschappen opgeven voor het ophalen van gegevens.<br/><br/>Meer informatie over de details van de sectie 'activiteitseigenschappen kopiëren' in de connector artikelen in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats). | Ja |
| sink | Geef het exemplaar sink-type en de bijbehorende eigenschappen op het schrijven van gegevens.<br/><br/>Meer informatie over de details van de sectie 'activiteitseigenschappen kopiëren' in de connector artikelen in [ondersteunde gegevensarchieven en indelingen](#supported-data-stores-and-formats). | Ja |
| Translator | Geef expliciete kolomtoewijzingen van bron opvangen. Wanneer u het standaardgedrag voor het exemplaar kan niet voldoen aan uw behoeften past.<br/><br/>Meer informatie over de details van [Schema en de gegevens Typetoewijzing](copy-activity-schema-and-type-mapping.md). | Nee |
| cloudDataMovementUnits | Geef de powerfulness van [Azure integratie Runtime](concepts-integration-runtime.md) zorgen dat gegevens opnieuw te kopiëren.<br/><br/>Meer informatie over de details van [Cloud data movement eenheden](copy-activity-performance.md). | Nee |
| parallelCopies | Geef de parallelle uitvoering die u wilt dat de Kopieeractiviteit sink gebruikt bij het lezen van gegevens van de bron- en schrijven van gegevens.<br/><br/>Meer informatie over de details van [kopie parallelle](copy-activity-performance.md#parallel-copy). | Nee |
| enableStaging<br/>stagingSettings | Kies fase van de tijdelijke gegevens in aa blob storage in plaats van rechtstreeks gegevens kopiëren van bron opvangen.<br/><br/>Informatie over de nuttig scenario's en de configuratiedetails van [kopie gefaseerde](copy-activity-performance.md#staged-copy). | Nee |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Kies hoe niet-compatibele rijen verwerkt bij het kopiëren van gegevens van bron opvangen.<br/><br/>Meer informatie over de details van [fouttolerantie](copy-activity-fault-tolerance.md). | Nee |

## <a name="monitoring"></a>Bewaking

De activiteitsgegevens uitvoering kopiëren en prestatiekenmerken worden geretourneerd in het resultaat van uitgevoerde Kopieeractiviteit -> sectie uitvoer. Hieronder volgt een lijst met uitgeput. Informatie over het bewaken van de activiteit die wordt uitgevoerd van [Quick Start sectie bewaking](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run). U kunt vergelijken de prestaties en de configuratie van uw scenario om door te kopiëren van de activiteit [prestaties verwijzing](copy-activity-performance.md#performance-reference) van het intern testen.

| Naam van eigenschap  | Beschrijving | Eenheid |
|:--- |:--- |:--- |
| dataRead | Gegevensgrootte van het lezen van bron | Waarde voor Int64 in bytes |
| dataWritten | Gegevensgrootte opvangen geschreven | Waarde voor Int64 in bytes |
| rowsCopied | Het aantal rijen (niet van toepassing voor binaire kopie) wordt gekopieerd. | Waarde voor Int64 (geen unit) |
| rowsSkipped | Aantal niet-compatibele rijen wordt overgeslagen. U kunt de functie inschakelen door de set 'enableSkipIncompatibleRow' op true. | Waarde voor Int64 (geen unit) |
| Doorvoer | Verhouding waarmee gegevens worden overgedragen | Drijvendekommagetal in KB per seconde |
| copyDuration | De duur van de kopie | Waarde voor Int32 (in seconden) |
| sqlDwPolyBase | Als bij het kopiëren van gegevens in SQL Data Warehouse PolyBase gebruikt. | Boole-waarde |
| redshiftUnload | Als uit het geheugen laden wordt gebruikt bij het kopiëren van gegevens van Redshift. | Boole-waarde |
| hdfsDistcp | Als DistCp wordt gebruikt bij het kopiëren van gegevens uit HDFS. | Boole-waarde |
| effectiveIntegrationRuntime | Kunt u zien welk integratie Runtime(s) wordt gebruikt om te zorgen dat de activiteit die wordt uitgevoerd in de indeling van `<IR name> (<region if it's Azure IR>)`. | Tekst (tekenreeks) |
| usedCloudDataMovementUnits | De effectieve cloud data movement eenheden tijdens het kopiëren van. | Int32-waarde |
| redirectRowPath | Pad naar het logboek van overgeslagen incompatibel rijen in de blobopslag die u onder 'redirectIncompatibleRowSettings configureert'. Zie onderstaand voorbeeld. | Tekst (tekenreeks) |
| billedDuration | De duur wordt gefactureerd voor het verplaatsen van gegevens. | Waarde voor Int32 (in seconden) |

```json
"output": {
    "dataRead": 1024,
    "dataWritten": 2048,
    "rowsCopies": 100,
    "rowsSkipped": 2,
    "throughput": 1024.0,
    "copyDuration": 3600,
    "redirectRowPath": "https://<account>.blob.core.windows.net/<path>/<pipelineRunID>/",
    "redshiftUnload": true,
    "sqlDwPolyBase": true,
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 8,
    "billedDuration": 28800
}
```

## <a name="schema-and-data-type-mapping"></a>Schema- en gegevenstypetoewijzing

Zie de [Schema en de gegevens Typetoewijzing](copy-activity-schema-and-type-mapping.md), waarin wordt beschreven hoe de kopieerbewerking de brongegevens opvangen toegewezen.

## <a name="fault-tolerance"></a>Fouttolerantie

Standaard kopieeractiviteit stopt met het kopiëren van gegevens en retourneert fout zodra het niet-compatibele gegevens tussen de bron- en sink. U kunt expliciet configureren voor overslaan en meld u de niet-compatibele rijen en alleen kopiëren die compatibel gegevens om te kopiëren is voltooid. Zie de [Kopieeractiviteit fouttolerantie](copy-activity-fault-tolerance.md) op meer informatie.

## <a name="performance-and-tuning"></a>Prestaties en afstemmen

Zie de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](copy-activity-performance.md), waarin belangrijke factoren die van invloed op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory wordt beschreven. Ook hier worden de waargenomen prestaties tijdens interne tests en worden verschillende manieren om te optimaliseren van de prestaties van de Kopieeractiviteit besproken.

## <a name="incremental-copy"></a>Incrementele kopie 
Data Factory versie 2 ondersteunt scenario's voor het kopiëren van deltagegevens stapsgewijs uit een gegevensopslag van bron naar een doelgegevensopslagplaats. Zie [zelfstudie: gegevens stapsgewijs kopiëren](tutorial-incremental-copy-overview.md). 

## <a name="read-and-write-partitioned-data"></a>Gepartitioneerde gegevens lezen en schrijven
Azure Data Factory ondersteund in versie 1, lezen of schrijven van gepartitioneerde gegevens met behulp van de SliceStart/SliceEnd/WindowStart/WindowEnd systeemvariabelen. In versie 2, kunt u dit gedrag kunt bereiken met behulp van een pipeline-parameter en van de trigger geplande tijd/begintijd als een waarde van de parameter. Zie voor meer informatie [het lezen of schrijven gepartitioneerd gegevens](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende snelstartgidsen, zelfstudies en voorbeelden:

- [Gegevens kopiëren van de ene locatie naar een andere locatie in de dezelfde Azure-blobopslag](quickstart-create-data-factory-dot-net.md)
- [Gegevens kopiëren van Azure Blob-opslag naar Azure SQL Database](tutorial-copy-data-dot-net.md)
- [Gegevens kopiëren van on-premises SQL-Server naar Azure](tutorial-hybrid-copy-powershell.md)
