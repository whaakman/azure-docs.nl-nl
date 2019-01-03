---
title: Gegevens kopiëren van een REST-bron met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een cloud of on-premises REST-gegevensbron naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 31f523dc8d171f62e814cd1a4225d2b25a972f36
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970318"
---
# <a name="copy-data-from-a-rest-endpoint-by-using-azure-data-factory"></a>Gegevens kopiëren van een REST-eindpunt met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een REST-eindpunt. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

Het verschil tussen deze connector REST [HTTP-connector](connector-http.md) en de [Web-connector voor tabel](connector-web-table.md) zijn:

- **REST-connector** specifiek ondersteuning kopiëren van gegevens uit RESTful-API's; 
- **HTTP-connector** is generiek gegevens ophalen van een HTTP-eindpunt, bijvoorbeeld om bestand te downloaden. Voordat deze REST-connector beschikbaar is, kan het geval zijn HTTP-connector gebruiken om gegevens te kopiëren van RESTful-API, die wordt ondersteund, maar minder functionele vergelijken met de REST-connector.
- **Web-connector voor tabel** extraheert inhoud van de webpagina van een HTML-tabel.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van een REST-bron naar elke ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

Om precies ondersteunt deze algemene REST-connector:

- Het ophalen van gegevens van een REST-eindpunt met behulp van de **ophalen** of **POST** methoden.
- Het ophalen van gegevens met behulp van een van de volgende verificaties: **Anonieme**, **Basic**, **AAD-service-principal**, en **beheerde identiteiten voor een Azure-resources**.
- **[Paginering](#pagination-support)**  in de REST-API's.
- Kopiëren van de REST-JSON-antwoord [as-is](#export-json-response-as-is) of parseren met behulp van [schematoewijzing](copy-activity-schema-and-type-mapping.md#schema-mapping). Alleen responslading in **JSON** wordt ondersteund.

> [!TIP]
> Als u wilt testen een aanvraag voor het ophalen van gegevens voordat u de REST-connector in Data Factory configureert, meer informatie over de API-specificatie voor de koptekst en hoofdtekst van de vereisten. U kunt hulpprogramma's zoals Postman of een webbrowser gebruiken om te valideren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor de REST-connector zijn.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de overige gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **HttpServer**. | Ja |
| url | De basis-URL van de REST-service. | Ja |
| enableServerCertificateValidation | Of u wilt valideren van server side SSL-certificaat bij het verbinden met het eindpunt. | Nee<br /> (de standaardwaarde is **waar**) |
| authenticationType | Het type verificatie gebruikt voor verbinding met de REST-service. Toegestane waarden zijn **anoniem**, **Basic**, **AadServicePrincipal** en **ManagedServiceIdentity**. Wordt verwezen naar de betreffende secties hieronder in meer eigenschappen en voorbeelden. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt de Azure Integration Runtime of een zelf-hostende integratie-Runtime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Indien niet opgegeven, wordt deze eigenschap gebruikt de standaard Azure Integration Runtime. |Nee |

### <a name="use-basic-authentication"></a>Gebruikmaken van basisverificatie

Stel de **authenticationType** eigenschap **Basic**. Naast de algemene eigenschappen die worden beschreven in de vorige sectie, moet u de volgende eigenschappen opgeven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| Gebruikersnaam | De gebruikersnaam gebruiken voor toegang tot het REST-eindpunt. | Ja |
| wachtwoord | Het wachtwoord voor de gebruiker (de **gebruikersnaam** waarde). Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |

**Voorbeeld**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<REST endpoint>",
            "userName": "<user name>",
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

### <a name="use-aad-service-principal-authentication"></a>Service-principal AAD-verificatie gebruiken

Stel de **authenticationType** eigenschap **AadServicePrincipal**. Naast de algemene eigenschappen die worden beschreven in de vorige sectie, moet u de volgende eigenschappen opgeven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| servicePrincipalId | Geef de Azure Active Directory-toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de Azure Active Directory-toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| aadResourceId | Geef de AAD-resource die u hebt aangevraagd voor autorisatie, bijvoorbeeld `https://management.core.windows.net`.| Ja |

**Voorbeeld**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Gebruik van beheerde identiteiten voor verificatie van de Azure-resources

Stel de **authenticationType** eigenschap **ManagedServiceIdentity**. Naast de algemene eigenschappen die worden beschreven in de vorige sectie, moet u de volgende eigenschappen opgeven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| aadResourceId | Geef de AAD-resource die u hebt aangevraagd voor autorisatie, bijvoorbeeld `https://management.core.windows.net`.| Ja |

**Voorbeeld**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "ManagedServiceIdentity",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de REST-gegevensset. 

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). 

Om gegevens te kopiëren van REST, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **RestResource**. | Ja |
| relativeurl bevatten | Een relatieve URL naar de resource die de gegevens bevat. Als deze eigenschap is niet opgegeven, wordt alleen de URL die opgegeven in de definitie van de gekoppelde service gebruikt. | Nee |
| requestMethod | De HTTP-methode. Toegestane waarden zijn **ophalen** (standaard) en **Post**. | Nee |
| additionalHeaders | Extra kopteksten die HTTP-aanvraag. | Nee |
| RequestBody | De hoofdtekst van de HTTP-aanvraag. | Nee |
| paginationRules | De paginering regels voor het opstellen van de volgende pagina-aanvragen. Raadpleeg [ondersteuning voor paginering](#pagination-support) sectie voor meer informatie. | Nee |

**Voorbeeld 1: Met behulp van de Get-methode met paginering**

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": {
                "x-user-defined": "helloworld"
            },
            "paginationRules": {
                "AbsoluteUrl": "$.paging.next"
            }
        }
    }
}
```

**Voorbeeld 2: Met behulp van de Post-methode**

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST REST request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de REST-bron.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md). 

### <a name="rest-as-source"></a>REST-als bron

De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **RestSource**. | Ja |
| httpRequestTimeout | De time-out (de **TimeSpan** waarde) voor de HTTP-aanvraag reageert. Deze waarde is de time-out voor een antwoord, niet de time-out voor lezen van gegevens van de reactie. De standaardwaarde is **00:01:40**.  | Nee |
| requestInterval | De tijd moet worden gewacht voordat de aanvraag voor de volgende pagina wordt verzonden. De standaardwaarde is **00:00:01** |  Nee |

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="pagination-support"></a>Ondersteuning voor paginering

Normaal gesproken beperken REST-API de grootte van de nettolading antwoord van een enkele aanvraag onder een redelijk aantal; tijd om terug te keren van grote hoeveelheden gegevens, het het resultaat splitst in meerdere pagina's en aanroepen voor het verzenden van opeenvolgende aanvragen voor het ophalen van de volgende pagina van het resultaat vereist. De aanvraag voor een pagina is meestal dynamische en samengesteld door de informatie die uit het antwoord van de vorige pagina.

Deze algemene REST-connector ondersteunt de volgende paginering patronen: 

* Absolute URL van de volgende aanvraag = waarde van de eigenschap in de hoofdtekst van het huidige antwoord
* Absolute URL van de volgende aanvraag = headerwaarde in de huidige antwoordheaders
* De queryparameter van de volgende aanvraag = waarde van de eigenschap in de hoofdtekst van het huidige antwoord
* De queryparameter van de volgende aanvraag = headerwaarde in de huidige antwoordheaders
* Volgende aanvraagheader = waarde van de eigenschap in de hoofdtekst van het huidige antwoord
* Volgende aanvraagheader = headerwaarde in de huidige antwoordheaders

**Regels voor paginering** worden gedefinieerd als een woordenlijst in de gegevensset die een of meer hoofdlettergevoelig sleutel-waardeparen bevatten. De configuratie wordt gebruikt voor het genereren van de aanvraag vanaf de tweede pagina. De connector wordt niet meer doorlopen wanneer het HTTP-statuscode 204 (geen inhoud) opgehaald, of een van de expressie JSONPath in 'paginationRules' wordt null geretourneerd.

**Ondersteunde sleutels** in paginering regels:

| Sleutel | Description |
|:--- |:--- |
| AbsoluteUrl | Geeft aan dat de URL voor het uitgeven van de volgende aanvraag. |
| QueryParameters. *request_query_parameter* of QueryParameters [request_query_parameter] | "request_query_parameter" is gebruiker gedefinieerd die verwijst naar een query parameter de naam in de volgende HTTP-aanvraag-URL. |
| Headers. *request_header* of Headers [request_header] | "request_header" is gebruiker gedefinieerd die verwijst naar een headernaam in de volgende HTTP-aanvraag. |

**Ondersteunde waarden** in paginering regels:

| Waarde | Description |
|:--- |:--- |
| Headers. *response_header* of Headers [response_header] | "response_header" is gebruiker gedefinieerd die verwijst naar een headernaam in de huidige HTTP-antwoord, de waarde die wordt gebruikt om uit te geven van de volgende aanvraag. |
| Een expressie voor een JSONPath beginnen met '$' (die vertegenwoordigt de hoofdmap van de antwoordtekst) | De antwoordtekst mag slechts één JSON-object. De expressie JSONPath moet één primitieve waarde, dat wordt gebruikt om uit te geven van de volgende aanvraag retourneren. |

**Voorbeeld:**

Facebook Graph API retourneert een antwoord in de volgende structuur, waarin de URL van volgende pagina de aanvraag wordt weergegeven in ***paging.next***:

```json
{
    "data": [
        {
            "created_time": "2017-12-12T14:12:20+0000",
            "name": "album1",
            "id": "1809938745705498_1809939942372045"
        },
        {
            "created_time": "2017-12-12T14:14:03+0000",
            "name": "album2",
            "id": "1809938745705498_1809941802371859"
        },
        {
            "created_time": "2017-12-12T14:14:11+0000",
            "name": "album3",
            "id": "1809938745705498_1809941879038518"
        }
    ],
    "paging": {
        "cursors": {
            "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
            "before": "NDMyNzQyODI3OTQw"
        },
        "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw",
        "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
    }
}
```

De configuratie van de bijbehorende REST-gegevensset met name de `paginationRules` is als volgt:

```json
{
    "name": "MyFacebookAlbums",
    "properties": {
            "type": "RestResource",
            "typeProperties": {
                "relativeUrl": "albums",
                "paginationRules": {
                    "AbsoluteUrl": "$.paging.next"
                }
            },
            "linkedServiceName": {
                "referenceName": "MyRestService",
                "type": "LinkedServiceReference"
            }
    }
}
```

## <a name="export-json-response-as-is"></a>JSON-antwoord als exporteren-is

U kunt deze REST-connector gebruiken voor het exporteren van REST-API-JSON-antwoord als-is aan verschillende winkels op basis van bestanden. Voor het bereiken van deze kopie van de schema-agnostische overslaan 'de structuur' (ook wel genoemd *schema*) sectie in de gegevensset en schematoewijzing in de kopieeractiviteit.

## <a name="schema-mapping"></a>Schematoewijzing

Als u wilt gegevens kopiëren van de REST-eindpunt naar sink in tabelvorm, verwijzen naar [schematoewijzing](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).
