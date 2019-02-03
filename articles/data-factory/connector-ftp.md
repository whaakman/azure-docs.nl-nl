---
title: Gegevens kopiëren van een FTP-server met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een FTP-server naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: jingwang
ms.openlocfilehash: 292d2da33edb5a0b48fbf138d8a45fc1c3c17d46
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663444"
---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Gegevens kopiëren van FTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-ftp-connector.md)
> * [Huidige versie](connector-ftp.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een FTP-server. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van FTP-server naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze FTP-connector:

- Het kopiëren van bestanden met behulp van **Basic** of **anoniem** verificatie.
- Kopiëren van bestanden als-is of bij het parseren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met FTP.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor FTP-gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **FtpServer**. | Ja |
| host | Geef de naam of IP-adres van de FTP-server. | Ja |
| poort | Geef de poort waarop de FTP-server luistert.<br/>Toegestane waarden zijn: geheel getal, standaardwaarde is **21**. | Nee |
| enableSsl | Geef op of u FTP via een SSL/TLS-kanaal.<br/>Toegestane waarden zijn: **waar** (standaard), **false**. | Nee |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen wanneer u FTP via SSL/TLS-kanaal.<br/>Toegestane waarden zijn: **waar** (standaard), **false**. | Nee |
| authenticationType | Geef het verificatietype.<br/>Toegestane waarden zijn: **Basic**, **anonieme** | Ja |
| Gebruikersnaam | De gebruiker die toegang tot de FTP-server heeft opgeven. | Nee |
| wachtwoord | Geef het wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>De FTP-connector biedt ondersteuning voor toegang tot FTP-server zonder versleuteling of een expliciete SSL/TLS-versleuteling; het biedt geen ondersteuning voor impliciete SSL/TLS-versleuteling.

**Voorbeeld 1: anonieme verificatie gebruikt**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: met behulp van basisverificatie**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de FTP-gegevensset.

Om gegevens te kopiëren van de FTP, stel de eigenschap type van de gegevensset in **bestandsshare**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **FileShare** |Ja |
| folderPath | Pad naar de map. Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: rootfolder/submap/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Ja |
| fileName | **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in. |Nee |
| Indeling | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u bestanden met een specifieke indeling parseren wilt, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |
| useBinaryTransfer | Geef op of de binaire overdrachtsmodus gebruiken. De waarden zijn true voor binaire modus (standaard), en ONWAAR voor tekenreeksen ASCII. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken.

>[!NOTE]
>Als u 'fileFilter'-eigenschap voor bestandsfilter, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe filter mogelijkheid toegevoegd aan 'fileName' voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de FTP-bron.

### <a name="ftp-as-source"></a>FTP-als bron

Om gegevens te kopiëren van de FTP, stelt u het brontype in de kopieeractiviteit naar **FileSystemSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
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

| folderPath | fileName | recursieve | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
