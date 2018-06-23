---
title: Gegevens kopiëren van Amazon Marketplace-webservice met behulp van Azure Data Factory (Preview) | Microsoft Docs
description: Ontdek hoe u gegevens kopiëren van Amazon Marketplace-webservice naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 06/15/2018
ms.author: jingwang
ms.openlocfilehash: add0f9185ac94a8b673e0c05de9de919286ffa3a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337952"
---
# <a name="copy-data-from-amazon-marketplace-web-service-using-azure-data-factory-preview"></a>Gegevens kopiëren van Amazon Marketplace-webservice met behulp van Azure Data Factory (Preview)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Amazon Marketplace-webservice. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uit te proberen en ons feedback te geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Amazon Marketplace-webservice kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Amazon Marketplace Web Service-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Amazon Marketplace Web Service gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AmazonMWS** | Ja |
| eindpunt | Het eindpunt van de Amazon MWS-server (dat wil zeggen, mws.amazonservices.com)  | Ja |
| marketplaceID | De Amazon Marketplace-ID die u gegevens wilt ophalen uit. Voor informatie over het ophalen van gegevens uit meerdere Marketplace-id's scheiden met komma's (`,`). (dat wil zeggen, A2EUQ1WTGCTBG2)  | Ja |
| sellerID | De Amazon verkoper-ID.  | Ja |
| mwsAuthToken | Het verificatietoken Amazon MWS. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| accessKeyId | De toegang sleutel-ID die wordt gebruikt voor toegang tot gegevens.  | Ja |
| secretKey | De geheime sleutel die wordt gebruikt voor toegang tot gegevens. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| useEncryptedEndpoints | Geeft aan of de eindpunten van de gegevensbron zijn versleuteld via HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Geeft aan of de hostnaam in het certificaat van de server overeenkomen met de hostnaam van de server om verbinding te maken via SSL vereisen. De standaardwaarde is true.  | Nee |
| usePeerVerification | Geeft aan of de identiteit van de server te verifiëren wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "AmazonMWSLinkedService",
    "properties": {
        "type": "AmazonMWS",
        "typeProperties": {
            "endpoint" : "mws.amazonservices.com",
            "marketplaceID" : "A2EUQ1WTGCTBG2",
            "sellerID" : "<sellerID>",
            "mwsAuthToken": {
                 "type": "SecureString",
                 "value": "<mwsAuthToken>"
            },
            "accessKeyId" : "<accessKeyId>",
            "secretKey": {
                 "type": "SecureString",
                 "value": "<secretKey>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de webservice voor Amazon Marketplace gegevensset.

Om gegevens te kopiëren van Amazon Marketplace-webservice, stel de eigenschap type van de gegevensset **AmazonMWSObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "AmazonMWSDataset",
    "properties": {
        "type": "AmazonMWSObject",
        "linkedServiceName": {
            "referenceName": "<AmazonMWS linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door de bron Amazon Marketplace-webservice.

### <a name="amazonmwssource-as-source"></a>AmazonMWSSource als bron

Om gegevens te kopiëren van Amazon Marketplace-webservice, stelt u het brontype in de kopieerbewerking naar **AmazonMWSSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AmazonMWSSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM Orders where  Amazon_Order_Id = 'xx'"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAmazonMWS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AmazonMWS input dataset name>",
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
                "type": "AmazonMWSSource",
                "query": "SELECT * FROM Orders where Amazon_Order_Id = 'xx'"
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
