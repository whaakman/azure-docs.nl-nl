---
title: "Gegevens kopiëren van de HTTP-bron met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van een bron van de HTTP-cloud of on-premises naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.openlocfilehash: 232b9bed1ea719dfb76d639bc8d5274551cdab6f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Gegevens kopiëren van de HTTP-eindpunt met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-http-connector.md)
> * [Versie 2 - Preview](connector-http.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een HTTP-eindpunt. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [HTTP-connector in V1](v1/data-factory-http-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van HTTP-bron kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze HTTP-connector:

- Bij het ophalen van gegevens van HTTP/s-eindpunt met behulp van HTTP **ophalen** of **POST** methode.
- Bij het ophalen van gegevens met behulp van de volgende verificaties: **anoniem**, **Basic**, **Digest**, **Windows**, en  **ClientCertificate**.
- Kopiëren van het HTTP-antwoord als-is of bij het parseren van deze met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

Het verschil tussen deze connector en de [Web tabel connector](connector-web-table.md) de laatste wordt gebruikt om te tabelinhoud van de webpagina HTML uitpakken.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar HTTP-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HTTP-gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **HttpServer**. | Ja |
| url | Basis-URL voor de webserver | Ja |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen bij het verbinden met de HTTP-eindpunt. | Nee, de standaardwaarde is true |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn: **anoniem**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Raadpleeg de secties onder deze tabel over meer eigenschappen en voorbeelden voor deze verificatietypen JSON respectievelijk. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

### <a name="using-basic-digest-or-windows-authentication"></a>Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

Stel de eigenschap 'authenticationType' op **Basic**, **Digest**, of **Windows**, en geef de volgende eigenschappen samen met de algemene eigenschappen die worden beschreven in de vorige sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| userName | De naam van de gebruiker toegang tot het HTTP-eindpunt. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als SecureString. | Ja |

**Voorbeeld**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
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

### <a name="using-clientcertificate-authentication"></a>ClientCertificate-verificatie

ClientCertificate om verificatie te gebruiken, stelt u 'authenticationType'-eigenschap in op **ClientCertificate**, en geef de volgende eigenschappen samen met de algemene eigenschappen in de vorige sectie beschreven:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| embeddedCertData | Gegevens van Base64-gecodeerde certificaat. | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op uw Self-hosted integratie Runtime-machine-certificaatarchief. Geldt alleen als host zichzelf soort integratie Runtime is opgegeven in connectVia. | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| wachtwoord | Het wachtwoord is gekoppeld aan het certificaat. Dit veld markeren als SecureString. | Nee |

Als u 'certThumbprint' voor verificatie gebruiken en het certificaat in het persoonlijke archief van de lokale computer is geïnstalleerd, moet u de leesmachtiging verlenen aan de integratie Self-hosted Runtime:

1. Start Microsoft Management Console (MMC). Voeg de **certificaten** -module die gericht is op de **lokale Computer**.
2. Vouw **certificaten**, **persoonlijke**, en klik op **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief en selecteer **alle taken** -> **persoonlijke sleutels beheren...**
3. Op de **beveiliging** tabblad, het toevoegen van het gebruikersaccount waaronder integratie Runtime-Host-Service (DIAHostService) wordt uitgevoerd met leestoegang tot het certificaat.

**Voorbeeld 1: het gebruik van certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld 2: het gebruik van embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<base64 encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de HTTP-gegevensset.

Om gegevens te kopiëren van HTTP, stel de eigenschap type van de gegevensset **HttpFile**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **HttpFile** | Ja |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat. Als deze eigenschap niet is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. | Nee |
| requestMethod | HTTP-methode.<br/>Toegestane waarden zijn **ophalen** (standaard) of **Post**. | Nee |
| additionalHeaders | Aanvullende HTTP-aanvraagheaders. | Nee |
| requestBody | Instantie voor HTTP-aanvraag. | Nee |
| Indeling | Als u wilt **gegevens ophalen van HTTP-eindpunt als-is** overslaan zonder het parseren van het en kopiëren naar een winkel op basis van bestanden, de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u de inhoud van het HTTP-antwoord geparseerd tijdens het kopiëren wilt, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

**Voorbeeld 1: via Get-methode (standaard)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Voorbeeld 2: via Post-methode**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door HTTP-bron.

### <a name="http-as-source"></a>HTTP als bron

Om gegevens te kopiëren van HTTP, stelt u het brontype in de kopieerbewerking naar **HttpSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HttpSource** | Ja |
| httpRequestTimeout | De time-out (TimeSpan) voor de HTTP-aanvragen reageert. De time-out is reageert niet op de time-out antwoordgegevens lezen.<br/> Standaardwaarde: 00:01:40  | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).