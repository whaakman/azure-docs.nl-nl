---
title: Parquet-indeling in Azure Data Factory | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u omgaat met Parquet-indeling in Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 5b711fac2a7bc41d11bcc80927f460390888cc3e
ms.sourcegitcommit: 2c09af866f6cc3b2169e84100daea0aac9fc7fd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64878056"
---
# <a name="parquet-format-in-azure-data-factory"></a>Parquet-indeling in Azure Data Factory

Ga als volgt in dit artikel als u wilt **de Parquet-bestanden parseren of de gegevens in Parquet-indeling wilt schrijven**. 

Parquet-indeling wordt ondersteund voor de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestandssysteem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), en [ SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Parquet-gegevensset.

| Eigenschap         | Description                                                  | Vereist |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | De eigenschap type van de gegevensset moet worden ingesteld op **Parquet**. | Ja      |
| location         | De instellingen van de locatie van de bestanden. Elke connector op basis van het bestand heeft een eigen locatietype en eigenschappen onder ondersteund `location`. **Zie voor meer informatie artikel connector eigenschappengedeelte van gegevensset ->**. | Ja      |
| compressionCodec | De compressiecodec te gebruiken bij het schrijven naar een Parquet-bestanden. Bij het lezen van Parquet-bestanden, wordt de compressiecodec op basis van metagegevens van het bestand automatisch bepalen door Data Factory.<br>Ondersteunde typen zijn '**geen**","**gzip**","**snappy**' (standaard), en "**lzo**'. Let op dat moment Copy activity biedt geen ondersteuning voor LZO. | Nee       |

> [!NOTE]
> Witruimte in de naam van kolom wordt niet ondersteund voor Parquet-bestanden.

Hieronder volgt een voorbeeld van de gegevensset Parquet op Azure Blob-opslag:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Parquet-bron en sink.

### <a name="parquet-as-source"></a>Parquet als bron

De volgende eigenschappen worden ondersteund in de kopieeractiviteit ***\*bron\**** sectie.

| Eigenschap      | Description                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **ParquetSource**. | Ja      |
| storeSettings | Een groep met eigenschappen voor het lezen van gegevens uit een gegevensarchief. Elke connector op basis van het bestand heeft een eigen ondersteunde lezen instellingen onder `storeSettings`. **Zie voor meer informatie artikel connector sectie met eigenschappen activiteit kopiëren ->**. | Nee       |

### <a name="parquet-as-sink"></a>Parquet als sink

De volgende eigenschappen worden ondersteund in de kopieeractiviteit ***\*sink\**** sectie.

| Eigenschap      | Description                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **ParquetSink**. | Ja      |
| storeSettings | Een groep met eigenschappen voor het schrijven van gegevens naar een gegevensarchief. Elke connector op basis van het bestand heeft een eigen ondersteunde schrijven-instellingen onder `storeSettings`. **Zie voor meer informatie artikel connector sectie met eigenschappen activiteit kopiëren ->**. | Nee       |

## <a name="mapping-data-flow-properties"></a>Eigenschappen van fouttoewijzing gegevensstroom

Informatie over de details van [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de gegevensstroom toewijzen.

## <a name="data-type-support"></a>Ondersteuning voor gegevenstype

Parquet complexe gegevenstypen momenteel niet zijn ondersteund (bijvoorbeeld MAP, LIST, STRUCT).

## <a name="using-self-hosted-integration-runtime"></a>Met de zelf-hostende Integratieruntime

> [!IMPORTANT]
> Voor kopiëren gemachtigd door zelfgehoste Cloudintegratieruntime bijvoorbeeld tussen on-premises en cloud gegevensarchieven, als u niet Parquet-bestanden kopieert **als-is**, moet u voor het installeren van de **64-bits JRE 8 (Java Runtime Environment) of OpenJDK** op uw computer IR. Zie de volgende alinea met meer informatie.

Voor het exemplaar wordt uitgevoerd in zelfgehoste IR en Parquet-bestand serialisatie/deserialisatie, ADF wordt gezocht naar de Java-runtime door te controleren in de eerste plaats het register *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* voor JRE, als dat niet wordt gevonden, ten tweede controle systeemvariabele *`JAVA_HOME`* voor OpenJDK.

- **Gebruik Java Runtime Environment**: De 64-bits-IR is 64-bits JRE vereist. U vindt deze in [hier](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Gebruik OpenJDK**: dit wordt ondersteund sinds IR versie 3.13. Pakket de jvm.dll met alle andere vereiste assembly's van OpenJDK in zelfgehoste IR-machine en set-systeemomgevingsvariabele JAVA_HOME dienovereenkomstig.

> [!TIP]
> Als u kopieert gegevens naar/vanuit Parquet-indeling met behulp van zelfgehoste Cloudintegratieruntime en druk op fout met de melding "Er is een fout opgetreden bij het aanroepen van java, bericht: **java.lang.OutOfMemoryError:Java heap ruimte**', kunt u een omgevingsvariabele toevoegen `_JAVA_OPTIONS` in de computer die als host fungeert voor de zelfgehoste IR om aan te passen de min/max heap-grootte voor JVM voor die te maken van dergelijke kopiëren, klikt u vervolgens de pijplijn opnieuw uitvoeren.

![JVM-heap-grootte instellen op zelfgehoste IR](C:/AzureContent/azure-docs-pr/articles/data-factory/media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Voorbeeld: set-variabele `_JAVA_OPTIONS` met waarde `-Xms256m -Xmx16g`. De vlag `Xms` Hiermee geeft u de eerste toewijzing van de geheugengroep voor een Java Virtual Machine (JVM), terwijl `Xmx` geeft de maximale hoeveelheid geheugen toegewezen groep. Dit betekent dat JVM wordt gestart met `Xms` hoeveelheid geheugen en kunnen maximaal gebruiken `Xmx` hoeveelheid geheugen. Standaard ADF gebruiken van 64MB min en max 1G.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)