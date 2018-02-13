---
title: "Gegevens kopiëren van Google BigQuery met behulp van Azure Data Factory (bèta) | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van Google BigQuery naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een data factory-pijplijn."
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
ms.date: 02/12/2018
ms.author: jingwang
ms.openlocfilehash: 35f61f6bd38b59a2df0613ba2506d047c1daeaaa
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory-beta"></a>Gegevens kopiëren van Google BigQuery met behulp van Azure Data Factory (bèta)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens van Google BigQuery kopiëren. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service, die in het algemeen beschikbaar is, Zie [Kopieeractiviteit in versie 1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Deze connector is momenteel in de bètaversie. U kunt uit te proberen en ons feedback te geven. Gebruik deze niet in een productieomgeving.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de Google BigQuery kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

 Data Factory biedt een ingebouwde stuurprogramma's zodat de verbinding. Daarom hoeft u niet handmatig een stuurprogramma voor het gebruik van deze connector te installeren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar de Google BigQuery-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Google BigQuery service gekoppelde.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **GoogleBigQuery**. | Ja |
| project | De project-ID van het standaard BigQuery project aan de query op.  | Ja |
| additionalProjects | Een door komma's gescheiden lijst met project-id van de openbare BigQuery projecteert om toegang te hebben.  | Nee |
| requestGoogleDriveScope | Of u wilt toegang vragen tot Google Drive. Google Drive toegang toe te staan, schakelt u ondersteuning voor federatieve tabellen die gegevens met gegevens uit de Google Drive BigQuery combineren. De standaardwaarde is **false**.  | Nee |
| authenticationType | Het OAuth 2.0-verificatiemechanisme gebruikt voor verificatie. ServiceAuthentication kan alleen worden gebruikt voor Self-hosted integratie Runtime. <br/>Toegestane waarden zijn **UserAuthentication** en **ServiceAuthentication**. Raadpleeg de secties onder deze tabel over meer eigenschappen en voorbeelden voor deze verificatietypen JSON respectievelijk. | Ja |

### <a name="using-user-authentication"></a>Met behulp van verificatie van gebruiker

Stel de eigenschap 'authenticationType' op **UserAuthentication**, en geef de volgende eigenschappen samen met de algemene eigenschappen die in de vorige sectie beschreven:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| clientId | ID van de toepassing die wordt gebruikt voor het genereren van het vernieuwingstoken. | Nee |
| clientSecret | Geheim van de toepassing die wordt gebruikt voor het genereren van het vernieuwingstoken. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| refreshToken | Het vernieuwingstoken dat is verkregen van Google toegang verlenen aan BigQuery gebruikt. Meer informatie over het ophalen van [verkrijgen van OAuth 2.0-toegangstokens](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens). Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |

**Voorbeeld:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Service-verificatie

Stel de eigenschap 'authenticationType' op **ServiceAuthentication**, en geef de volgende eigenschappen samen met de algemene eigenschappen die in de vorige sectie beschreven. Dit verificatietype kan alleen op Self-hosted integratie Runtime worden gebruikt.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| e-mail | De service-account e-mail-ID die wordt gebruikt voor ServiceAuthentication. Deze kan alleen worden gebruikt voor Self-hosted integratie Runtime.  | Nee |
| keyFilePath | Het volledige pad naar het sleutelbestand .p12 die wordt gebruikt voor verificatie van het e-mailadres van de service-account. | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met vertrouwde CA-certificaten gebruikt om te controleren of de server wanneer u verbinding via SSL maakt. Deze eigenschap kan alleen worden ingesteld als u SSL op Self-hosted integratie-Runtime gebruiken. De standaardwaarde is het cacerts.pem-bestand met de integratie-runtime is geïnstalleerd.  | Nee |
| useSystemTrustStore | Hiermee bepaalt u of een CA-certificaat uit het archief van de vertrouwensrelatie system of vanuit een opgegeven .pem-bestand. De standaardwaarde is **false**.  | Nee |

**Voorbeeld:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
} 
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de gegevensset Google BigQuery.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van Google BigQuery, **GoogleBigQueryObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het Google BigQuery brontype.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource als een type gegevensbron

Om gegevens te kopiëren van Google BigQuery, stelt u het brontype in de kopieerbewerking naar **GoogleBigQuerySource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **GoogleBigQuerySource**. | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
