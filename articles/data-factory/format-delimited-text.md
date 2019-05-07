---
title: Gescheiden tekstopmaak in Azure Data Factory | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u omgaat met gescheiden tekstopmaak in Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 407b8ba2fda35d3acbf1b425bb15fe20778613d7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146005"
---
# <a name="delimited-text-format-in-azure-data-factory"></a>Gescheiden tekstopmaak in Azure Data Factory

Ga als volgt in dit artikel als u wilt **parseren van de tekstbestanden met scheidingstekens of de gegevens schrijven naar gescheiden tekstopmaak**. 

Gescheiden tekstopmaak wordt ondersteund voor de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestandssysteem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), en [ SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de tekst met scheidingstekens-gegevensset.

| Eigenschap         | Description                                                  | Vereist |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | De eigenschap type van de gegevensset moet worden ingesteld op **DelimitedText**. | Ja      |
| location         | De instellingen van de locatie van de bestanden. Elke connector op basis van het bestand heeft een eigen locatietype en eigenschappen onder ondersteund `location`. **Zie voor meer informatie artikel connector eigenschappengedeelte van gegevensset ->**. | Ja      |
| columnDelimiter  | De tekens die wordt gebruikt voor het scheiden van kolommen in een bestand. Scheidingsteken voor meerdere char is op dit moment alleen ondersteund voor de gegevensstroom toewijzen, maar niet de kopieeractiviteit. <br>De standaardwaarde is **met door komma's `,`** , wanneer het kolomscheidingsteken wordt gedefinieerd als lege tekenreeks, wat betekent dat er geen scheidingsteken, de volledige regel wordt het opgevat als één kolom. | Nee       |
| rowDelimiter     | Het teken of '\r\n' gebruikt voor het scheiden van rijen in een bestand.<br>De standaardwaarde is een van de volgende waarden **op lezen: ['\r\n', '\r', '\n']**, en **'\n' of '\r\n' schrijfwaarde** toewijzingsgegevens Flow en respectievelijk Copy-activiteit. <br>Wanneer `rowDelimiter` is ingesteld op geen scheidingsteken (lege tekenreeks), de `columnDelimiter` moet worden ingesteld als er geen scheidingsteken (lege tekenreeks) als het goed is, wat betekent dat de hele inhoud behandelen als één waarde. | Nee       |
| quoteChar        | Het teken kolomwaarden citeren als het scheidingsteken voor kolommen bevat. <br>De standaardwaarde is **dubbele aanhalingstekens** `"`. <br>Voor het toewijzen van gegevensstroom `quoteChar` mag geen lege tekenreeks. <br>Voor de kopieeractiviteit, wanneer `quoteChar` is gedefinieerd als lege tekenreeks is, betekent dit er is geen aanhalingsteken en waarde in de kolom niet wordt vermeld, en `escapeChar` wordt gebruikt als escapeteken voor het kolomscheidingsteken en zichzelf. | Nee       |
| escapeChar       | De enkele escape-teken aanhalingstekens in een waarde tussen aanhalingstekens.<br>De standaardwaarde is **backslash `\`** . <br>Voor het toewijzen van gegevensstroom `escapeChar` mag geen lege tekenreeks. <br/>Voor de kopieeractiviteit, wanneer `escapeChar` wordt gedefinieerd als lege tekenreeks is, de `quoteChar` moet worden ingesteld als ook lege tekenreeks, in welk geval Zorg ervoor dat alle waarden van de kolom niet scheidingstekens bevatten. | Nee       |
| firstRowAsHeader | Hiermee geeft u op of moet worden behandeld/om de eerste rij als een headerregel door de namen van kolommen.<br>Toegestane waarden zijn **waar** en **false** (standaard). | Nee       |
| nullValue        | Hiermee geeft u de tekenreeksweergave van null-waarde. <br>De standaardwaarde is **lege tekenreeks**. | Nee       |
| encodingName     | Type codering gebruikt om te lezen/schrijven testbestanden. <br>Toegestane waarden zijn als volgt: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", 'US-ASCII-', 'UTF-7', "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", " IBM852 ","IBM855","IBM857","IBM860","IBM861","IBM863","IBM864","IBM865","IBM869","IBM870","IBM01140","IBM01141","IBM01142","IBM01143","IBM01144","IBM01145","IBM01146","IBM01147","IBM01148","IBM01149","ISO-2022-JP","ISO-2022-KR ","ISO-8859-1', ' ISO-8859-2", 'ISO-8859-3',"ISO-8859-4", 'ISO-8859-5', 'ISO-8859-6', 'ISO-8859-7',"ISO-8859-8","ISO-8859-9","ISO-8859-13", 'ISO-8859-15', 'WINDOWS-874',"WINDOWS 1250","WINDOWS-1251","WINDOWS-1252","WINDOWS-1253"," WINDOWS-1254 ","WINDOWS-1255","WINDOWS-1256","WINDOWS-1257", 'WINDOWS-1258'.<br>Houd er rekening mee dat gegevensstroom toewijzing biedt geen ondersteuning voor UTF-7-codering. | Nee       |
| compressionCodec | De compressiecodec gebruikt om te lezen/schrijven tekstbestanden. <br>Toegestane waarden zijn **bzip2**, **gzip**, **deflate**, **ZipDeflate**, **snappy**, of **lz4**. om te gebruiken wanneer u het bestand opslaat. <br>Let op dat moment Copy activity biedt geen ondersteuning voor 'snappy' & 'lz4' en gegevensstroom toewijzing biedt geen ondersteuning voor 'ZipDeflate'. | Nee       |
| compressionLevel | De compressieverhouding. <br>Toegestane waarden zijn **optimale** of **snelst**.<br>- **Snelste:** De compressie-bewerking moet zo snel mogelijk worden voltooid, zelfs als het resulterende bestand is niet optimaal gecomprimeerd.<br>- **Optimale**: De compressie-bewerking moet optimaal zijn gecomprimeerd, zelfs als de bewerking duurt het langer om te voltooien. Zie voor meer informatie, [compressieniveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) onderwerp. | Nee       |

Hieronder volgt een voorbeeld van een tekstbestand met scheidingstekens gegevensset op Azure Blob-opslag:

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de tekst met scheidingstekens bron en sink.

### <a name="delimited-text-as-source"></a>Tekst met scheidingstekens als bron 

De volgende eigenschappen worden ondersteund in de kopieeractiviteit ***\*bron\**** sectie.

| Eigenschap       | Description                                                  | Vereist |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **DelimitedTextSource**. | Ja      |
| formatSettings | Een groep met eigenschappen. Raadpleeg **gescheiden tekst instellingen lezen** in de volgende tabel. | Nee       |
| storeSettings  | Een groep met eigenschappen voor het lezen van gegevens uit een gegevensarchief. Elke connector op basis van het bestand heeft een eigen ondersteunde lezen instellingen onder `storeSettings`. **Zie voor meer informatie artikel connector sectie met eigenschappen activiteit kopiëren ->**. | Nee       |

Ondersteund **tekst met scheidingstekens instellingen lezen** onder `formatSettings`:

| Eigenschap      | Description                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Het type formatSettings moet worden ingesteld op **DelimitedTextReadSetting**. | Ja      |
| skipLineCount | Geeft het aantal **niet-lege** rijen dat moet worden overgeslagen bij het lezen van gegevens in invoerbestanden. <br>Als zowel skipLineCount als firstRowAsHeader is opgegeven, worden de regels eerst overgeslagen en wordt de headerinformatie gelezen uit het invoerbestand. | Nee       |

### <a name="delimited-text-as-sink"></a>Tekst met scheidingstekens als sink

De volgende eigenschappen worden ondersteund in de kopieeractiviteit ***\*sink\**** sectie.

| Eigenschap       | Description                                                  | Vereist |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **DelimitedTextSink**. | Ja      |
| formatSettings | Een groep met eigenschappen. Raadpleeg **gescheiden tekst instellingen schrijven** in de volgende tabel. |          |
| storeSettings  | Een groep met eigenschappen voor het schrijven van gegevens naar een gegevensarchief. Elke connector op basis van het bestand heeft een eigen ondersteunde schrijven-instellingen onder `storeSettings`. **Zie voor meer informatie artikel connector sectie met eigenschappen activiteit kopiëren ->**. | Nee       |

Ondersteund **tekst met scheidingstekens instellingen schrijven** onder `formatSettings`:

| Eigenschap      | Description                                                  | Vereist                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | Het type formatSettings moet worden ingesteld op **DelimitedTextWriteSetting**. | Ja                                                   |
| fileExtension | De extensie gebruikt om de naam van de uitvoerbestanden bijvoorbeeld `.csv`, `.txt`. Het moet worden opgegeven wanneer de `fileName` niet is opgegeven in de uitvoer DelimitedText gegevensset. | Ja, als de bestandsnaam is niet opgegeven in de uitvoergegevensset |

## <a name="mapping-data-flow-properties"></a>Eigenschappen van fouttoewijzing gegevensstroom

Informatie over de details van [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de gegevensstroom toewijzen.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)