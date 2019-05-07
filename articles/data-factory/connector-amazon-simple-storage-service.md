---
title: Gegevens kopiëren van Amazon Simple Storage-Service (S3) met Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u gegevens kopiëren van Amazon Simple Storage-Service (S3) naar een ondersteunde sink-gegevensopslag met behulp van Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: ca764c7e78f6ffe221386d1d320582e394d0a78a
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64875877"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Gegevens kopiëren van Amazon Simple Storage-Service met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
>
> * [Versie 1:](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Huidige versie](connector-amazon-simple-storage-service.md)

In dit artikel bevat een overzicht van hoe u gegevens kopiëren van Amazon Simple Storage-Service (Amazon S3). Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze Amazon S3-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Specifiek,-bestanden kopiëren als biedt ondersteuning voor deze connector Amazon S3-is of bij het parseren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md). Hierbij [AWS Signature versie 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) om te verifiëren van aanvragen voor S3.

>[!TIP]
>U kunt deze Amazon S3-connector gebruiken om te kopiëren van gegevens uit **een S3-compatibele opslagproviders** bijvoorbeeld [Google Cloud Storage](connector-google-cloud-storage.md). De bijbehorende service-URL opgeven in de configuratie van de gekoppelde service.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt kopiëren van gegevens vanaf Amazon S3, zorg ervoor dat u de volgende machtigingen zijn verleend:

- **Voor de uitvoering van activiteit kopiëren:**: `s3:GetObject` en `s3:GetObjectVersion` voor Amazon S3-Object bewerkingen.
- **Voor het ontwerpen van Data Factory-gebruikersinterface**: `s3:ListAllMyBuckets` en `s3:ListBucket` / `s3:GetBucketLocation` voor Amazon S3-Bucket bewerkingen machtigingen zijn ook vereist voor bewerkingen, zoals verbinding testen en navigeert u bladeren/bestand paden. Als u niet wilt dat deze machtiging te verlenen, overslaan van de testverbinding in de gekoppelde service-pagina voor het maken en geef het pad rechtstreeks in de gegevenssetinstellingen.

Zie voor meer informatie over de volledige lijst met machtigingen voor Amazon S3 [machtigingen op te geven in een beleid](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Amazon S3.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Amazon S3 gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Amazon S3**. | Ja |
| accessKeyId | ID van de geheime toegangssleutel. |Ja |
| secretAccessKey | De geheime toegangssleutel zelf. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| serviceUrl | Het aangepaste S3-eindpunt opgeven als u bij het kopiëren van gegevens uit een S3-compatibele opslagprovider dan de officiële Amazon S3-service. Bijvoorbeeld, om gegevens te kopiëren van Google Cloud Storage, geef `https://storage.googleapis.com`. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!TIP]
>Geef de aangepaste URL van de S3-service als u bij het kopiëren van gegevens uit een S3-compatibele opslag dan de officiële Amazon S3-service.

>[!NOTE]
>Toegangssleutels voor het kopiëren van gegevens vanaf Amazon S3 IAM-account voor deze connector vereist. [Tijdelijke beveiligingsreferentie](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) wordt niet ondersteund.
>

Hier volgt een voorbeeld:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. 

- Voor **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Voor andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Het kopiëren van gegevens vanaf Amazon S3 in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteund Instellingen. De volgende eigenschappen worden ondersteund voor Amazon S3 onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **AmazonS3Location**. | Ja      |
| bucketName | De naam van de S3-bucket.                                          | Ja      |
| folderPath | Het pad naar map onder de opgegeven bucket. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De bestandsnaam van het onder de opgegeven bucket + folderPath. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
> **AmazonS3Object** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup/de GET metadata activity voor compatibiliteit met eerdere versies, maar werkt niet met gegevensstroom toewijzen. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AmazonS3Location",
                "bucketName": "bucketname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Andere indeling-gegevensset

Het kopiëren van gegevens vanaf Amazon S3 in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AmazonS3Object** |Ja |
| bucketName | De naam van de S3-bucket. Filteren op jokerteken wordt niet ondersteund. |Ja voor kopiëren/Lookup-activiteit, niet voor de activiteit GetMetadata |
| key | De **naam of het jokerteken filter** van sleutel van de S3-object onder de opgegeven bucket. Geldt alleen wanneer de eigenschap 'voorvoegsel' is niet opgegeven. <br/><br/>Het filteren op jokerteken wordt ondersteund voor zowel maponderdeel en bestand de naam van onderdeel. Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Voorbeeld 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Zie meer voorbeeld in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). Gebruik `^` als escape voor als de naam van uw werkelijke map/bestand jokertekens of deze escape-teken in. |Nee |
| prefix | Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen als 'key'-eigenschap is niet opgegeven. |Nee |
| version | De versie van de S3-object, als S3 versiebeheer is ingeschakeld. Indien niet opgegeven, wordt de meest recente versie worden opgehaald. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **bucketName** voor bucket en **voorvoegsel** voor een maponderdeel.<br>Als u wilt kopiëren van één bestand met een specifieke naam, geef **bucketName** voor bucket en **sleutel** voor onderdeel plus bestand mapnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **bucketName** voor bucket en **sleutel** voor map onderdeel plus de jokerteken-filter.

**Voorbeeld: met behulp van voorvoegsel**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Voorbeeld: met behulp van sleutel en (optioneel)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Amazon S3-bron.

### <a name="amazon-s3-as-source"></a>Amazon S3 als bron

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens vanaf Amazon S3 in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Amazon S3 onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                                    |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AmazonS3ReadSetting**. | Ja                                                         |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                                          |
| prefix                   | Het voorvoegsel voor de sleutel van de S3-object onder de opgegeven bucket geconfigureerd in de gegevensset in de bron van filterobjecten. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. <br>Is van toepassing alleen wanneer `wildcardFolderPath` en `wildcardFileName` eigenschappen zijn niet opgegeven. | Nee                                                          |
| wildcardFolderPath       | Het pad met jokertekens onder de opgegeven bucket geconfigureerd in de gegevensset aan filter bronmappen. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                                          |
| wildcardFileName         | De naam van het bestand met jokertekens onder de opgegeven bucket + folderPath/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in.  Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` in de gegevensset en `prefix` zijn niet opgegeven |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd. | Nee                                                          |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                                          |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                                          |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **FileSystemSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AmazonS3ReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>De bron van andere indeling

Het kopiëren van gegevens vanaf Amazon S3 in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| Bucket | key | recursieve | Bron-map de structuur en het filter resultaat (bestanden vetgedrukt opgehaald)|
|:--- |:--- |:--- |:--- |
| Bucket | `Folder*/*` | false | Bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Bucket | `Folder*/*` | true | Bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Bucket | `Folder*/*.csv` | false | Bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| Bucket | `Folder*/*.csv` | true | Bucket<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit in Azure Data Factory worden ondersteund als bronnen en sinks, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
