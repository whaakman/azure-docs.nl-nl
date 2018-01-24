---
title: "Gegevens kopiëren van de SFTP-server met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over de MySQL-connector in Azure Data Factory waarmee u gegevens kopiëren van een SFTP-server naar een gegevensarchief ondersteund als een sink."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: ad008432b0e8f6ce9f9357cc539c982e878e2eba
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-sftp-server-using-azure-data-factory"></a>Gegevens kopiëren van de SFTP-server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-sftp-connector.md)
> * [Versie 2 - Preview](connector-sftp.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SFTP-server. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [SFTP-connector in V1](v1/data-factory-sftp-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de SFTP-server kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze SFTP-connector:

- Het kopiëren van bestanden met behulp van **Basic** of **SshPublicKey** verificatie.
- Kopiëren van bestanden als-is of bij het parseren van bestanden met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met SFTP.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SFTP gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Sftp**. |Ja |
| host | Naam of IP-adres van de SFTP-server. |Ja |
| poort | De poort waarop de SFTP-server luistert.<br/>Toegestane waarden zijn: integer, standaardwaarde is **22**. |Nee |
| skipHostKeyValidation | Geef op of validatie host-sleutel overslaan.<br/>Toegestane waarden zijn: **true**, **false** (standaard).  | Nee |
| hostKeyFingerprint | Geef de vingerafdruk van de hostsleutel. | Ja als u de 'skipHostKeyValidation' is ingesteld op false.  |
| authenticationType | Geef het verificatietype.<br/>Toegestane waarden zijn: **Basic**, **SshPublicKey**. Raadpleeg [Using basisverificatie](#using-basic-authentication) en [met behulp van SSH openbare-sleutelauthenticatie](#using-ssh-public-key-authentication) respectievelijk secties over meer eigenschappen en voorbeelden van JSON. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

### <a name="using-basic-authentication"></a>Met behulp van basisverificatie

Voor het gebruik van basisverificatie, stelt u 'authenticationType'-eigenschap in op **Basic**, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| userName | De gebruiker die toegang tot de SFTP-server heeft. |Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als een SecureString. | Ja |

**Voorbeeld:**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
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

### <a name="using-ssh-public-key-authentication"></a>Met behulp van SSH-verificatie voor openbare sleutel

Voor het gebruik van SSH-verificatie voor openbare sleutel, stelt u de eigenschap 'authenticationType' als **SshPublicKey**, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| userName | Gebruiker die toegang tot de SFTP-server heeft |Ja |
| privateKeyPath | Geef het absolute pad naar het persoonlijke sleutelbestand waartoe integratie Runtime toegang heeft. Geldt alleen als host zichzelf soort integratie Runtime is opgegeven in 'connectVia'. | Geef ofwel de `privateKeyPath` of `privateKeyContent`.  |
| privateKeyContent | Base64-gecodeerde SSH-inhoud met persoonlijke sleutel. Persoonlijke SSH-sleutel moet OpenSSH-indeling. Dit veld markeren als een SecureString. | Geef ofwel de `privateKeyPath` of `privateKeyContent`. |
| passPhrase | Geef de pass woordgroep en het wachtwoord voor het ontsleutelen van de persoonlijke sleutel als het sleutelbestand is beveiligd met een wachtwoordzin. Dit veld markeren als een SecureString. | Ja als u een bestand met de persoonlijke sleutel wordt beveiligd door een wachtwoordzin. |

> [!NOTE]
> SFTP-connector ondersteunt alleen OpenSSH-sleutel. Zorg ervoor dat uw sleutelbestand in de juiste indeling. U kunt Putty hulpprogramma ppk converteren naar OpenSSH-indeling.

**Voorbeeld 1: SshPublicKey verificatie met behulp van persoonlijke sleutels filePath**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: SshPublicKey verificatie met behulp van persoonlijke sleutels inhoud**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<sftp server>",
            "port": 22,
            "skipHostKeyValidation": true,
            "authenticationType": "SshPublicKey",
            "userName": "<username>",
            "privateKeyContent": {
                "type": "SecureString",
                "value": "<base64 string of the private key content>"
            },
            "passPhrase": {
                "type": "SecureString",
                "value": "<pass phrase>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de SFTP-gegevensset.

Om gegevens te kopiëren van SFTP, stel de eigenschap type van de gegevensset **FileShare**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **bestandsshare** |Ja |
| folderPath | Pad naar de map. Bijvoorbeeld: map/submap / |Ja |
| fileName | Geef de naam van het bestand in de **folderPath** als u wilt kopiëren uit een specifiek bestand. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de status van de gegevensset verwijst naar alle bestanden in de map als bron. |Nee |
| fileFilter | Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren. Geldt alleen als fileName is niet opgegeven. <br/><br/>Jokertekens zijn toegestaan: `*` (meerdere tekens) en `?` (willekeurig teken).<br/>-Voorbeeld 1:`"fileFilter": "*.log"`<br/>-Voorbeeld 2:`"fileFilter": 2017-09-??.txt"` |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren van bestanden met een specifieke indeling, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

**Voorbeeld:**

```json
{
    "name": "SFTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<SFTP linked service name>",
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door SFTP-bron.

### <a name="sftp-as-source"></a>SFTP als bron

Om gegevens te kopiëren van SFTP, stelt u het brontype in de kopieerbewerking naar **FileSystemSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. Opmerking Wanneer recursieve is ingesteld op true en sink is bestandsgebaseerde opslag, lege map/subbewerkingen-folder niet worden gekopieerd/gemaakt op de sink.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SFTP input dataset name>",
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