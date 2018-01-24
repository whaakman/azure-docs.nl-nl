---
title: "Gegevens kopiëren van Amazon eenvoudige Storage-Service met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over hoe u gegevens vanaf Amazon eenvoudige Storage-Service (S3) kopiëren naar gegevensarchieven ondersteunde sink met behulp van Azure Data Factory."
services: data-factory
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 6df7d74d572a59c83105905fbe0a9e218aadc28f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Gegevens kopiëren van Amazon eenvoudige Storage-Service met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Versie 2 - Preview](connector-amazon-simple-storage-service.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Blob Storage. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Amazon S3 connnector in V1](v1/data-factory-amazon-simple-storage-service-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure Data Lake Store of gegevens kopiëren van Azure Data Lake Store met alle ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder kopiëren bestanden als biedt ondersteuning voor deze connector Amazon S3-is of bij het parseren van bestanden met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt kopiëren van gegevens vanaf Amazon S3, moet dat u de volgende machtigingen hebben gekregen:

- `s3:GetObject`en `s3:GetObjectVersion` voor Amazon S3 Object bewerkingen.
- `s3:ListBucket`voor Amazon S3-Bucket bewerkingen. Als u de Data Factory-Wizard kopiëren, `s3:ListAllMyBuckets` is ook vereist.

Zie voor meer informatie over de volledige lijst met machtigingen voor Amazon S3 [machtigingen geven in een beleid](http://docs.aws.amazon.com/amazons3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Amazon S3.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Amazon S3 gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AmazonS3**. | Ja |
| accessKeyId | ID van de geheime toegangssleutel. |Ja |
| secretAccessKey | De geheime toegangssleutel zelf. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

>[!NOTE]
>Sneltoetsen voor de IAM-account voor het kopiëren van gegevens vanaf Amazon S3 voor deze connector vereist. [Tijdelijke beveiligingsreferentie](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) wordt niet ondersteund.
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Amazon S3 dataset.

Om gegevens te kopiëren vanaf Amazon S3, stel de eigenschap type van de gegevensset **AmazonS3Object**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AmazonS3Object** |Ja |
| bucketName | De naam van de S3-bucket. |Ja |
| sleutel | De sleutel van het object S3. Geldt alleen als voorvoegsel is niet opgegeven. |Nee |
| voorvoegsel | Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen als sleutel is niet opgegeven. |Nee |
| versie | De versie van het object S3, als S3 versiebeheer is ingeschakeld. |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

> [!NOTE]
> **bucketName + toets** geeft de locatie van het object S3, waarbij bucket de hoofdcontainer voor S3-objecten en -sleutel is het volledige pad naar het S3-object.

**Voorbeeld: voorvoegsel gebruiken**

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

**Voorbeeld: met behulp van sleutel en versie (optioneel)**

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake-bron- en sink.

### <a name="amazon-s3-as-source"></a>Amazon S3 als bron

Om gegevens te kopiëren vanaf Amazon S3, stelt u het brontype in de kopieerbewerking naar **FileSystemSource** (inclusief Amazon S3). De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. Opmerking Wanneer recursieve is ingesteld op true en sink is bestandsgebaseerde opslag, lege map/subbewerkingen-folder niet worden gekopieerd/gemaakt op de sink.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |

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
## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).