---
title: Gegevens kopiëren van HTTP-bron met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een HTTP-cloud of on-premises bron naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: jingwang
ms.openlocfilehash: 5afb2fccd5c7b8ca306079941837d854c0b21349
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091714"
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Gegevens kopiëren van HTTP-eindpunt met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-http-connector.md)
> * [Huidige versie](connector-http.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een HTTP-eindpunt. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van HTTP-bron naar elke ondersteunde sink-gegevensopslag kopiëren. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze HTTP-connector:

- Het ophalen van gegevens van HTTP/s-eindpunt met behulp van HTTP **ophalen** of **POST** methode.
- Bij het ophalen van gegevens met behulp van de volgende verificaties: **anoniem**, **Basic**, **Digest**, **Windows**, en  **ClientCertificate**.
- Het HTTP-antwoord als kopiëren-is of parseren met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

Het verschil tussen deze connector en de [Web-connector voor tabel](connector-web-table.md) is de laatste wordt gebruikt om op te halen van tabelinhoud van HTML-webpagina.

>[!TIP]
>Als u wilt testen HTTP-aanvraag voor gegevens ophalen voor het configureren van HTTP-connector in ADF, kunt u meer van de API-specificatie koptekst en hoofdtekst van de vereisten en hulpprogramma's zoals Postman of web browser gebruiken om te valideren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor HTTP-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HTTP-gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **HttpServer**. | Ja |
| url | Basis-URL naar de webserver | Ja |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen bij het verbinden met HTTP-eindpunt. Wanneer uw HTTPS-server zelf-ondertekend certificaat, stel dit in op false. | Nee, de standaardinstelling is true |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn: **anoniem**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Zie de secties onder deze tabel op meer eigenschappen en JSON-voorbeelden voor deze verificatietypen respectievelijk. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

### <a name="using-basic-digest-or-windows-authentication"></a>Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

Stel de eigenschap 'authenticationType' in op **Basic**, **Digest**, of **Windows**, en geeft u de volgende eigenschappen samen met de algemene eigenschappen die worden beschreven in de vorige sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| Gebruikersnaam | De naam van de gebruiker voor toegang tot het HTTP-eindpunt. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |

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

### <a name="using-clientcertificate-authentication"></a>Met behulp van ClientCertificate verificatie

ClientCertificate om verificatie te gebruiken, kunt u 'authenticationType'-eigenschap instellen op **ClientCertificate**, en geeft u de volgende eigenschappen samen met de algemene eigenschappen die worden beschreven in de vorige sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| embeddedCertData | Gegevens van Base64-gecodeerd certificaat. | Opgeven of de `embeddedCertData` of `certThumbprint`. |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op het certificaatarchief van de machine zelfgehoste Cloudintegratieruntime. Geldt alleen wanneer de zelf-hostende Integration Runtime-type is opgegeven in connectVia. | Opgeven of de `embeddedCertData` of `certThumbprint`. |
| wachtwoord | Het wachtwoord dat is gekoppeld aan het certificaat. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |

Als u "certThumbprint" voor verificatie gebruiken en het certificaat in het persoonlijke archief van de lokale computer is geïnstalleerd, moet u de leesmachtiging verlenen aan de zelfgehoste Cloudintegratieruntime:

1. Start Microsoft Management Console (MMC). Voeg de **certificaten** -module die is gericht op de **lokale Computer**.
2. Vouw **certificaten**, **persoonlijke**, en klikt u op **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief, en selecteer **alle taken** -> **persoonlijke sleutels beheren...**
3. Op de **Security** tabblad, voegt u het gebruikersaccount waarmee hostservice van Integration Runtime (DIAHostService) wordt uitgevoerd met de leestoegang voor het certificaat.

**Voorbeeld 1: met behulp van certThumbprint**

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

**Voorbeeld 2: met behulp van embeddedCertData**

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor HTTP-gegevensset.

Als u wilt kopiëren van gegevens van HTTP, stel de eigenschap type van de gegevensset in **HttpFile**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **HttpFile** | Ja |
| relativeurl bevatten | Een relatieve URL naar de resource die de gegevens bevat. Als deze eigenschap niet is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. | Nee |
| requestMethod | HTTP-methode.<br/>Toegestane waarden zijn **ophalen** (standaard) of **Post**. | Nee |
| additionalHeaders | Extra kopteksten die HTTP-aanvraag. | Nee |
| RequestBody | De hoofdtekst van HTTP-aanvraag. | Nee |
| Indeling | Als u wilt **ophalen van gegevens uit de HTTP-eindpunt als-is** overslaan zonder het parseren van het en kopiëren naar een winkel op basis van bestanden, het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u de inhoud van de HTTP-reactie parseren tijdens het kopiëren wilt, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

>[!NOTE]
>De ondersteunde grootte van HTTP-verzoek nettolading is ongeveer 500KB. Als de grootte van de nettolading die u wilt doorgeven aan uw webeindpunt groter dan dit is, kunt u overwegen naar batch in kleinere chunks.

**Voorbeeld 1: met behulp van Get-methode (standaard)**

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

**Voorbeeld 2: met behulp van Post-methode**

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HTTP-bron.

### <a name="http-as-source"></a>HTTP als bron

Om gegevens te kopiëren van HTTP, stelt u het brontype in de kopieeractiviteit naar **HttpSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HttpSource** | Ja |
| httpRequestTimeout | De time-out (duur) voor de HTTP-aanvraag reageert. De time-out is een antwoord, niet de time-out voor lezen van gegevens van de reactie.<br/> Standaardwaarde: 00:01:40  | Nee |

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
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
