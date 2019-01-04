---
title: Gegevens kopiëren van Amazon Simple Storage-Service met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u gegevens kopiëren van Amazon Simple Storage-Service (S3) naar een ondersteunde sink-gegevensopslag met behulp van Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 7373cc23654e2168963a364e4b4069331bf196c5
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969927"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Gegevens kopiëren van Amazon Simple Storage-Service met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Huidige versie](connector-amazon-simple-storage-service.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Amazon S3. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens Amazon S3 kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek,-bestanden kopiëren als biedt ondersteuning voor deze connector Amazon S3-is of bij het parseren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

>[!TIP]
>U kunt deze Amazon S3-connector gebruiken om te kopiëren van gegevens uit **een S3-compatibele opslagproviders** bijvoorbeeld [Google Cloud Storage](#copy-from-google-cloud-storage). De bijbehorende service-URL opgeven in de configuratie van de gekoppelde service.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt kopiëren van gegevens vanaf Amazon S3, zorg ervoor dat u de volgende machtigingen zijn verleend:

- **Voor de uitvoering van activiteit kopiëren:**: `s3:GetObject` en `s3:GetObjectVersion` voor Amazon S3-Object bewerkingen.
- **Voor het ontwerpen van Data Factory-gebruikersinterface**: `s3:ListAllMyBuckets` en `s3:ListBucket` / `s3:GetBucketLocation` voor Amazon S3-Bucket bewerkingen machtigingen zijn ook vereist voor bewerkingen, zoals verbinding testen en navigeert u bladeren/bestand paden. Als u niet wilt dat deze machtiging te verlenen, gaat u verder testverbinding in de gekoppelde service-pagina voor het maken en speicify het pad rechtstreeks in de gegevenssetinstellingen.

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
| serviceUrl | Het aangepaste S3-eindpunt opgeven als u bij het kopiëren van gegevens uit een S3-compatibele opslagprovider dan de officiële Amazon S3-service. Bijvoorbeeld, voor [gegevens kopiëren van Google Cloud Storage](#copy-from-google-cloud-storage), geef `https://storage.googleapis.com`. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!TIP]
>Geef de aangepaste URL van de S3-service als u bij het kopiëren van gegevens uit een S3-compatibele opslag dan de officiële Amazon S3-service.

>[!NOTE]
>Toegangssleutels voor het kopiëren van gegevens vanaf Amazon S3 IAM-account voor deze connector vereist. [Tijdelijke beveiligingsreferentie](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) wordt niet ondersteund.
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Amazon S3-gegevensset.

Als u wilt kopiëren van gegevens vanaf Amazon S3, stel de eigenschap type van de gegevensset in **AmazonS3Object**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AmazonS3Object** |Ja |
| bucketName | De naam van de S3-bucket. Filteren op jokerteken wordt niet ondersteund. |Ja voor kopiëren/Lookup-activiteit, niet voor de activiteit GetMetadata |
| sleutel | De **naam of het jokerteken filter** van sleutel van de S3-object onder de opgegeven bucket. Geldt alleen wanneer de eigenschap 'voorvoegsel' is niet opgegeven. <br/><br/>Het filteren op jokerteken wordt alleen ondersteund voor het bestand de naam van onderdeel, maar geen map deel uitmaakt. Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"key": "rootfolder/subfolder/*.csv"`<br/>-Voorbeeld 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in. |Nee |
| voorvoegsel | Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen als 'key'-eigenschap is niet opgegeven. |Nee |
| versie | De versie van de S3-object, als S3 versiebeheer is ingeschakeld. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| Indeling | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

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

Als u wilt kopiëren van gegevens vanaf Amazon S3, stelt u het brontype in de kopieeractiviteit naar **FileSystemSource** (waaronder Amazon S3). De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |

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

## <a name="copy-from-google-cloud-storage"></a>Kopiëren van Google Cloud Storage

Omdat Google Cloud Storage S3-compatibele interoperabiliteit biedt, kunt u Amazon S3-connector gebruiken om gegevens te kopiëren van Google Cloud Storage naar een [sinkgegevensopslag ondersteund](copy-activity-overview.md#supported-data-stores-and-formats). 

U vindt de specifieke Google Cloud Storage-vermelding in de ADF authoring UI-connector-galerie, die automatisch wordt ingevuld in de service-URL als `https://storage.googleapis.com`. Als u wilt de toegangssleutel en het geheim gevonden, gaat u naar **Google Cloud Storage** > **instellingen** > **interoperabiliteit**. Raadpleeg dit artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van de S3-connector om gegevens te kopiëren.

**Voorbeeld van de gekoppelde service:**

```json
{
    "name": "GoogleCloudStorageLinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                "type": "SecureString",
                "value": "<secret access key>"
            },
            "serviceUrl": "https://storage.googleapis.com"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit in Azure Data Factory worden ondersteund als bronnen en sinks, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
