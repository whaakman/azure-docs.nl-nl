---
title: Gegevens verplaatsen van Amazon eenvoudige Storage-Service met behulp van de Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens vanaf Amazon eenvoudige Storage-Service (S3) met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: fb2b534955a2cd0e1294df5425550ac6958ff3c2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Gegevens verplaatsen van Amazon eenvoudige Storage-Service met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-amazon-simple-storage-service-connector.md)
> * [Versie 2 - Preview](../connector-amazon-simple-storage-service.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Amazon S3-connector in V2](../connector-amazon-simple-storage-service.md).

In dit artikel wordt uitgelegd hoe de kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van Amazon eenvoudige Storage-Service (S3) gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens vanaf Amazon S3 kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data Factory ondersteunt momenteel alleen zwevend gegevens vanaf Amazon S3 naar andere gegevensarchieven, maar niet verplaatsen van gegevens van andere gegevens worden opgeslagen op de Amazon S3.

## <a name="required-permissions"></a>Vereiste machtigingen
Als u wilt kopiëren van gegevens vanaf Amazon S3, moet dat u de volgende machtigingen hebben gekregen:

* `s3:GetObject`en `s3:GetObjectVersion` voor Amazon S3 Object bewerkingen.
* `s3:ListBucket`voor Amazon S3-Bucket bewerkingen. Als u de Data Factory-Wizard kopiëren, `s3:ListAllMyBuckets` is ook vereist.

Zie voor meer informatie over de volledige lijst met machtigingen voor Amazon S3 [machtigingen geven in een beleid](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens worden verplaatst van een bron Amazon S3 met behulp van verschillende hulpprogramma's of API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie voor een snel overzicht [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Of u hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een gegevensopslag Amazon S3, de [JSON-voorbeeld: gegevens kopiëren van Amazon S3 naar Azure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob-storage) sectie van dit artikel.

> [!NOTE]
> Zie voor meer informatie over ondersteunde indelingen voor de bestands- en compressie voor een kopieeractiviteit [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Amazon S3.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Een gekoppelde service gegevensopslag is gekoppeld aan een gegevensfactory. Maken van een gekoppelde service van het type **AwsAccessKey** uw Amazon S3 data store koppelen aan uw gegevensfactory. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Amazon S3 (AwsAccessKey) gekoppelde service.

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| accessKeyID |ID van de geheime toegangssleutel. |tekenreeks |Ja |
| secretAccessKey |De geheime toegangssleutel zelf. |Versleutelde geheime tekenreeks |Ja |

>[!NOTE]
>Sneltoetsen voor de IAM-account voor het kopiëren van gegevens vanaf Amazon S3 voor deze connector vereist. [Tijdelijke beveiligingsreferentie](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) wordt niet ondersteund.
>

Hier volgt een voorbeeld:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Als een dataset staat voor invoergegevens in Azure Blob-opslag opgeven, stelt u de eigenschap type van de gegevensset **AmazonS3**. Stel de **linkedServiceName** eigenschap van de gegevensset op de naam van de Amazon S3 gekoppelde service. Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets [gegevenssets maken](data-factory-create-datasets.md). 

Secties zoals structuur, beschikbaarheid en beleid zijn identiek voor alle typen van de gegevensset (zoals SQL-database, blob van Azure en Azure-tabel). De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor een gegevensset van het type **AmazonS3** (waaronder de gegevensset Amazon S3) heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| bucketName |De naam van de S3-bucket. |Tekenreeks |Ja |
| sleutel |De sleutel van het object S3. |Tekenreeks |Nee |
| voorvoegsel |Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen als sleutel is leeg. |Tekenreeks |Nee |
| versie |De versie van het object S3, als S3 versiebeheer is ingeschakeld. |Tekenreeks |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie de [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt kopiëren van bestanden als-is tussen bestandsgebaseerde winkels (binaire kopiëren), de indeling-sectie in beide definities invoer en uitvoer gegevensset overslaan. |Nee | |
| Compressie | Geef het type en de compressie van de gegevens. De ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. De ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee | |


> [!NOTE]
> **bucketName + toets** geeft de locatie van het object S3, waarbij bucket de hoofdcontainer voor S3-objecten en -sleutel is het volledige pad naar het S3-object.

### <a name="sample-dataset-with-prefix"></a>Voorbeeldgegevensset met voorvoegsel

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a>Voorbeeldgegevensset (met versie)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>Dynamische paden voor S3
Het vorige voorbeeld gebruikmaakt van vaste waarden voor de **sleutel** en **bucketName** eigenschappen in de Amazon S3-gegevensset.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

U kunt deze eigenschappen tijdens runtime dynamisch berekenen met behulp van systeemvariabelen zoals SliceStart Gegevensfactory hebben.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

U kunt hetzelfde doen voor de **voorvoegsel** eigenschap van een gegevensset Amazon S3. Zie voor een lijst van ondersteunde functies en variabelen [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten [pijplijnen maken](data-factory-create-pipelines.md). Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren eigenschappen, afhankelijk van de typen van bronnen en Put. Wanneer een bron in de kopieerbewerking is van het type **FileSystemSource** (waaronder Amazon S3), de volgende eigenschap is beschikbaar in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee geeft u op of voor recursief S3 lijst objecten in de map. |waar/onwaar |Nee |

## <a name="json-example-copy-data-from-amazon-s3-to-azure-blob-storage"></a>JSON-voorbeeld: gegevens kopiëren van Amazon S3 naar Azure Blob-opslag
Dit voorbeeld laat zien hoe gegevens vanaf Amazon S3 kopiëren naar een Azure-blobopslag. Echter, gegevens rechtstreeks naar kunnen worden gekopieerd [een van de put die worden ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de kopieeractiviteit in Data Factory.

Het voorbeeld bevat JSON-definities voor de volgende Data Factory-entiteiten. U kunt deze definities gebruiken voor het maken van een pijplijn om gegevens te kopiëren vanaf Amazon S3 naar Blob storage met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* Een gekoppelde service van het type [AwsAccessKey](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AmazonS3](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [FileSystemSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd vanaf Amazon S3 met een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

### <a name="amazon-s3-linked-service"></a>Amazon S3 gekoppelde service

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a>Invoergegevensset Amazon S3

Instelling **'extern': true** informeert de Data Factory-service dat de dataset externe aan de gegevensfactory. Deze eigenschap instelt op true in een invoergegevensset die niet wordt geproduceerd door een activiteit in de pijplijn.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a>Azure Blob-uitvoergegevensset

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur delen van de begintijd.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>De kopieeractiviteit in een pijplijn met een bron Amazon S3 en een blob-sink

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **FileSystemSource**, en **sink** type is ingesteld op **BlobSink**.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> Zie het toewijzen van kolommen uit een gegevensset bron naar kolommen uit een gegevensset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).


## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* Zie voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (kopieeractiviteit) in de Data Factory en verschillende manieren om te optimaliseren, de [activiteit prestaties en prestatieafstemming handleiding kopiëren](data-factory-copy-activity-performance.md).

* Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
