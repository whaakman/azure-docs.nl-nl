---
title: Gegevens kopiëren van/naar Azure File Storage met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Azure File Storage om ondersteunde sink-gegevensopslag (of) van de ondersteunde bron-gegevensopslag met Azure File Storage met behulp van Azure Data Factory.
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
ms.openlocfilehash: 3caa30ed0607992960151484fe4711736c304775
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228589"
---
# <a name="copy-data-from-or-to-azure-file-storage-by-using-azure-data-factory"></a>Gegevens kopiëren van of naar Azure File Storage met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het kopiëren van gegevens naar en van Azure File Storage. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze connector Azure File Storage wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Specifiek,-bestanden kopiëren als biedt ondersteuning voor deze connector Azure File Storage-is of het parseren van/genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Azure File Storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure File Storage gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **FileServer**. | Ja |
| host | Hiermee geeft u het eindpunt van de Azure File Storage als: <br/>-Met UI: Geef `\\<storage name>.file.core.windows.net\<file service name>`<br/>-Met behulp van JSON: `"host": "\\\\<storage name>.file.core.windows.net\\<file service name>"`. | Ja |
| userid | Geef de gebruiker voor toegang tot de Azure File Storage als: <br/>-Met UI: Geef `AZURE\<storage name>`<br/>-Met behulp van JSON: `"userid": "AZURE\\<storage name>"`. | Ja |
| password | Geef de toegangssleutel voor opslag. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee voor bron, Ja voor sink |

>[!IMPORTANT]
> - Gegevens te kopiëren naar Azure File Storage met behulp van Azure Integration Runtime expliciet [maken van een Azure IR](create-azure-integration-runtime.md#create-azure-ir) door de locatie van uw bestandsopslag en koppelen in de gekoppelde service als in het volgende voorbeeld.
> - Houd er rekening mee om gegevens te kopiëren van/naar Azure File Storage met behulp van zelfgehoste Cloudintegratieruntime buiten Azure, uitgaande TCP-poort 445 in uw lokale netwerk openen.

>[!TIP]
>Wanneer u ADF UI gebruikt voor het ontwerpen, vindt u de specifieke vermelding van 'Azure File Storage' voor het maken van de gekoppelde service, welke eronder weergegeven type genereert `FileServer` object.

**Voorbeeld:**

```json
{
    "name": "AzureFileStorageLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "\\\\<storage name>.file.core.windows.net\\<file service name>",
            "userid": "AZURE\\<storage name>",
            "password": {
                "type": "SecureString",
                "value": "<storage access key>"
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

Het kopiëren van gegevens naar en van Azure File Storage in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van opmaak en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure File Storage onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **FileServerLocation**. | Ja      |
| folderPath | Het pad naar map. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De naam van het bestand onder het opgegeven mappad. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
>
> **Bestandsshare** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor de activiteit Lookup-exemplaar/GetMetadata voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure File Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FileServerLocation",
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

Het kopiëren van gegevens naar en van Azure File Storage in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **FileShare** |Ja |
| folderPath | Pad naar de map. <br/><br/>Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: rootfolder/submap/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Ja |
| fileName | **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ", bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'; Als u vanuit in tabelvorm bron met de tabelnaam van de in plaats van de query kopieert, het naampatroon is '*[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ", bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken.

>[!NOTE]
>Als u 'fileFilter'-eigenschap voor bestandsfilter, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe filter mogelijkheid toegevoegd aan 'fileName' voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "AzureFileStorageDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<Azure File Storage linked service name>",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure File Storage-bron- en sinkblobpaden.

### <a name="azure-file-storage-as-source"></a>Azure File Storage als bron

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens van Azure File Storage in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de indeling op basis van kopieeractiviteit bron- en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure File Storage onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **FileServerReadSetting**. | Ja                                           |
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
        "name": "CopyFromAzureFileStorage",
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
                    "type": "FileServerReadSetting",
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

Het kopiëren van gegevens van Azure File Storage in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure File Storage input dataset name>",
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

### <a name="azure-file-storage-as-sink"></a>Azure File Storage als sink

- Voor kopiëren naar **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Voor het kopiëren naar andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Gegevens kopiëren naar Azure File Storage in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel voor kopiëren-indeling op basis van activiteit-sink en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure File Storage onder `storeSettings` instellingen in op basis van opmaak kopiëren sink:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **FileServerWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **FileSystemSink** type activiteit-sink voor kopiëren die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "FileServerWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Andere indeling-sink

Gegevens kopiëren naar Azure File Storage in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **FileSystemSink** |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron-bestanden van gegevensopslag op basis van bestanden.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden uit de bronmap van het zich in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><b>-MergeFiles</b>: alle bestanden uit de bronmap naar één bestand worden samengevoegd. Als de naam van de bestands-/ Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. anders zou worden automatisch gegenereerde naam. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToAzureFileStorage",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure File Storage output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName | recursive | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="recursive-and-copybehavior-examples"></a>recursieve en copyBehavior voorbeelden

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursive | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + 5-bestand inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
