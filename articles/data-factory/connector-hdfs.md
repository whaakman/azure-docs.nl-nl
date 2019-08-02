---
title: Gegevens kopiëren van HDFS met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een cloud of on-premises HDFS-bron naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 2cac2b350da5ca8738e40f9a288ecf4059e81060
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673909"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Gegevens uit HDFS met Azure Data Factory kopiëren
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-hdfs-connector.md)
> * [Huidige versie](connector-hdfs.md)

In dit artikel bevat een overzicht van hoe u gegevens uit HDFS-server te kopiëren. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze HDFS-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)

Om precies ondersteunt deze HDFS-connector:

- Het kopiëren van bestanden met behulp van **Windows** (Kerberos) of **anoniem** verificatie.
- Het kopiëren van bestanden met behulp van **webhdfs** protocol of **ingebouwde DistCp** ondersteunen.
- Kopiëren van bestanden als-is of het parseren van/genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van een HDFS die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](concepts-integration-runtime.md) artikel voor meer informatie.

> [!NOTE]
> Zorg ervoor dat de Integration Runtime toegang tot **alle** de [naam van knooppunt server]: [knooppunt poort name] en [servers voor het knooppunt van gegevens]: [knooppunt gegevenspoort] van het Hadoop-cluster. Standaard [naam van knooppunt poort] is 50070 en standaard [knooppunt gegevenspoort] 50075 is.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op HDFS.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HDFS gekoppelde service:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Hdfs**. | Ja |
| url |URL naar het HDFS |Ja |
| authenticationType | Toegestane waarden zijn: **Anonieme**, of **Windows**. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector, raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| userName |Gebruikersnaam voor Windows-verificatie. Geef voor Kerberos-verificatie, `<username>@<domain>.com`. |Ja (voor Windows-verificatie) |
| password |Wachtwoord voor Windows-verificatie. Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory, of [verwijs naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja (voor Windows-verificatie) |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld: met behulp van anonieme verificatie**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: met behulp van Windows-verificatie**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Het kopiëren van gegevens uit HDFS in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteund Instellingen. De volgende eigenschappen worden ondersteund voor HDFS onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Verplicht |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **HdfsLocation**. | Ja      |
| folderPath | Het pad naar map. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De naam van het bestand onder het opgegeven mappad. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
> **Bestandsshare** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup-activiteit voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
                "folderPath": "root/folder/subfolder"
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

Het kopiëren van gegevens uit HDFS in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **FileShare** |Ja |
| folderPath | Pad naar de map. Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: rootfolder/submap/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Ja |
| fileName |  **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u bestanden met een specifieke indeling parseren wilt, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken.

**Voorbeeld:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
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

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HDFS-bron.

### <a name="hdfs-as-source"></a>HDFS als bron

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens uit HDFS in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor HDFS onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Verplicht                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **HdfsReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens voor het filteren van bronmappen. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder het opgegeven mappad/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in.  Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **FileSystemSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
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
                    "type": "HdfsReadSetting",
                    "recursive": true
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

Het kopiëren van gegevens uit HDFS in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HdfsSource** |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |
| distcpSettings | Eigenschappengroep wanneer HDFS DistCp gebruiken. | Nee |
| resourceManagerEndpoint | De resourcemanager Yarn-eindpunt | Ja als DistCp gebruiken |
| tempScriptPath | Pad naar een map gebruikt voor het opslaan van tijdelijke DistCp opdrachtscript. Het scriptbestand wordt gegenereerd door Data Factory en wordt verwijderd nadat de taak voor het kopiëren is voltooid. | Ja als DistCp gebruiken |
| distcpOptions | Extra opties opgegeven voor de opdracht DistCp. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld: HDFS-bron in de kopieeractiviteit DistCp gebruiken**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

Meer informatie over het gebruik van DistCp gegevens uit HDFS efficiënt in de volgende sectie te kopiëren.

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName             | recursive | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (leeg, gebruikt u standaard) | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | (leeg, gebruikt u standaard) | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>DistCp gebruiken om te kopiëren van gegevens uit HDFS

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) is een systeemeigen Hadoop-opdrachtregelprogramma voor het gedistribueerde kopiëren in een Hadoop-cluster. Wanneer een opdracht Distcp uitvoert, wordt eerst alle bestanden worden gekopieerd, maakt u meerdere taken van de kaart in het Hadoop-cluster wordt een lijst, en elke kaart taak binaire kopie van bron naar het sink-doet.

DistCp gebruiken om te kopiëren van bestanden als ondersteuning voor activiteit kopiëren-is in Azure Blob (met inbegrip van [gefaseerd kopiëren](copy-activity-performance.md)) of Azure Data Lake Store, in welk geval het kan volledig benut de kracht van uw cluster in plaats van die worden uitgevoerd op de zelfgehoste Integration Runtime. Met name als uw cluster is een zeer krachtig en biedt betere doorvoer van de kopie. Op basis van uw configuratie in Azure Data Factory, kopieeractiviteit automatisch maken van een opdracht distcp indienen bij uw Hadoop-cluster en de kopiestatus controleren.

### <a name="prerequisites"></a>Vereisten

DistCp gebruiken om te kopiëren bestanden als-is van HDFS tot Azure-Blob (met inbegrip van gefaseerd kopiëren) of Azure Data Lake Store, zorg ervoor dat uw Hadoop-cluster voldoet aan de volgende vereisten:

1. MapReduce en Yarn-services zijn ingeschakeld.
2. Yarn-versie is 2.5 of hoger.
3. HDFS-server is geïntegreerd met uw gegevensopslag doel - Azure Blob of Azure Data Lake Store:

    - Azure Blob-bestandssysteem systeem zelf wordt ondersteund sinds Hadoop 2.7. U hoeft alleen jar pad opgeven in Hadoop env-configuratie.
    - Azure Data Lake Store-bestandssysteem is verpakt vanaf Hadoop 3.0.0-alpha1. Als uw Hadoop-cluster lager dan deze versie is, moet u handmatig importeren ADLS gerelateerde jar-pakketten (azure-datalake-store.jar) in het cluster op basis van [hier](https://hadoop.apache.org/releases.html), en de jar-pad opgeven in Hadoop env config.

4. Bereid een tijdelijke map in HDFS. Deze tijdelijke map wordt gebruikt voor het opslaan van DistCp shell-script, zodat het KB-niveau ruimte wordt innemen.
5. Zorg ervoor dat het gebruikersaccount dat is opgegeven in de gekoppelde Service HDFS gemachtigd om te een)-toepassing in Yarn; verzenden (b) de machtiging submap maken, lezen/schrijven bestanden onder de tijdelijke map hebben.

### <a name="configurations"></a>Configuraties

Zie DistCp gerelateerde configuraties en voorbeelden in [HDFS als bron](#hdfs-as-source) sectie.

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Kerberos-verificatie voor HDFS-connector gebruiken

Er zijn twee opties voor het instellen van de on-premises-omgeving om Kerberos-verificatie gebruiken in HDFS-connector. U kunt dat de beter aansluit op uw situatie.
* Optie 1: [Zelfgehoste Cloudintegratieruntime machine toevoegen in Kerberos-realm](#kerberos-join-realm)
* Optie 2: [Wederzijdse vertrouwensrelatie tussen Windows-domein- en Kerberos-realm inschakelen](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Optie 1: Zelfgehoste Cloudintegratieruntime machine toevoegen in Kerberos-realm

#### <a name="requirements"></a>Vereisten

* De machine zelfgehoste Cloudintegratieruntime moet deelnemen aan het Kerberos-realm en kan niet deelnemen aan een Windows-domein.

#### <a name="how-to-configure"></a>Configureren

**Op de machine zelfgehoste Cloudintegratieruntime:**

1.  Voer de **Ksetup** hulpprogramma om de Kerberos KDC-server en de realm te configureren.

    De machine moet worden geconfigureerd als een lid van een werkgroep, omdat een Kerberos-realm af van een Windows-domein wijkt. Dit kan worden bereikt door het Kerberos-realm en als volgt een KDC-server toe te voegen. Vervang *REALM.COM* met uw eigen respectieve realm indien nodig.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Opnieuw opstarten** de machine na het uitvoeren van deze opdrachten 2.

2.  Controleer de configuratie met **Ksetup** opdracht. De uitvoer moet, zoals:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Configureer de HDFS-connector op basis **Windows-verificatie** samen met uw Kerberos principal-naam en het wachtwoord voor het verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie voor informatie over de configuratie.

### <a name="kerberos-mutual-trust"></a>Optie 2: Wederzijdse vertrouwensrelatie tussen Windows-domein- en Kerberos-realm inschakelen

#### <a name="requirements"></a>Vereisten

*   De machine zelfgehoste Cloudintegratieruntime moet een Windows-domein toevoegen.
*   U moet zijn gemachtigd om de instellingen van de domeincontroller te werken.

#### <a name="how-to-configure"></a>Configureren

> [!NOTE]
> Vervangen door REALM.COM en AD.COM in de volgende zelfstudie als u uw eigen respectieve realm en de domeincontroller naar behoefte.

**Op KDC-server:**

1. Bewerken in de configuratie van de KDC **krb5.conf** bestand om te laten KDC vertrouwt Windows-domein verwijst naar de volgende configuratiesjabloon. Standaard de configuratie bevindt zich in **/etc/krb5.conf**.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   **Opnieuw opstarten** de KDC-service na de configuratie.

2. Voorbereiden van een principal met de naam **krbtgt/REALM.COM\@AD.COM** in de KDC-server met de volgende opdracht uit:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. In **hadoop.security.auth_to_local** HDFS-serviceconfiguratie-bestand, toe te voegen `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`.

**Op de domeincontroller:**

1.  Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Vertrouwensrelatie van Windows-domein voor Kerberos-Realm. [wachtwoord] is het wachtwoord voor de principal **krbtgt/REALM.COM\@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Schakel versleuteling algoritme dat wordt gebruikt in Kerberos.

    1. Ga aan de Server Manager > Groepsbeleidsbeheer > domein > groepsbeleidsobjecten > standaard of beleid van het actieve domein en bewerken.

    2. In de **Editor voor Groepsbeleidsbeheer** pop-upvenster, Ga naar Computerconfiguratie > beleidsregels > Windows-instellingen > Beveiligingsinstellingen > lokaal beleid > beveiligingsopties, en configureer **netwerk beveiliging: Voor Kerberos toegestane versleutelingstypen configureren**.

    3. Selecteer het versleutelingsalgoritme die u wilt gebruiken wanneer verbinding met KDC. Over het algemeen kunt u gewoon alle opties selecteren.

        ![Configuratie-versleutelingstypen voor Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Gebruik **Ksetup** opdracht voor het opgeven van het versleutelingsalgoritme op de specifieke REALM moet worden gebruikt.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Maak de toewijzing tussen het domeinaccount en Kerberos-principal om Kerberos-principal gebruiken in Windows-domein.

    1. Start de beheerprogramma's > **Active Directory: gebruikers en Computers**.

    2. Geavanceerde functies configureren door te klikken op **weergave** > **geavanceerde functies**.

    3. Zoek de account die u wilt maken, toewijzingen en om weer te geven met de rechtermuisknop op **naamstoewijzingen** > klikt u op **Kerberos-namen** tabblad.

    4. Voeg een principal van de realm.

        ![Toewijzen van beveiligings-id](media/connector-hdfs/map-security-identity.png)

**Op de machine zelfgehoste Cloudintegratieruntime:**

* Voer de volgende **Ksetup** opdrachten een realm-vermelding toe te voegen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Configureer de HDFS-connector op basis **Windows-verificatie** samen met uw domeinaccount of Kerberos-Principal verbinding maken met de HDFS-gegevensbron. Controleer [eigenschappen van de gekoppelde Service HDFS](#linked-service-properties) sectie voor informatie over de configuratie.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
