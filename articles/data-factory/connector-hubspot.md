---
title: Gegevens kopiëren van HubSpot met Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens kopiëren van HubSpot naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.openlocfilehash: 15ebace0be90813f568bdf04b14b48d68a239c8c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634554"
---
# <a name="copy-data-from-hubspot-using-azure-data-factory"></a>Gegevens kopiëren van HubSpot met Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van HubSpot. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uitproberen en ons feedback te geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit HubSpot kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen, dus hoeft u stuurprogramma voor gebruik van deze connector handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar HubSpot-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor HubSpot gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Hubspot** | Ja |
| ClientId | De client-ID die is gekoppeld aan uw toepassing Hubspot.  | Ja |
| ClientSecret | Het clientgeheim die zijn gekoppeld aan uw toepassing Hubspot. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| accessToken | Het toegangstoken verkregen bij het verifiëren van uw OAuth-integratie in eerste instantie. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| refreshToken | Het vernieuwingstoken dat is verkregen bij het verifiëren van uw OAuth-integratie in eerste instantie. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| useEncryptedEndpoints | Hiermee geeft u op of de eindpunten van de gegevensbron zijn versleuteld met behulp van HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Hiermee geeft u op of de hostnaam van de in het certificaat van de server zodat deze overeenkomen met de hostnaam van de server wanneer u verbinding maakt via SSL vereist. De standaardwaarde is true.  | Nee |
| usePeerVerification | Hiermee geeft u op of u wilt controleren of de identiteit van de server wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "HubspotLinkedService",
    "properties": {
        "type": "Hubspot",
        "typeProperties": {
            "clientId" : "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "accessToken": {
                 "type": "SecureString",
                 "value": "<accessToken>"
            },
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refreshToken>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HubSpot gegevensset.

Om gegevens te kopiëren van HubSpot, stel de eigenschap type van de gegevensset in **HubspotObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type gegevensset.

**Voorbeeld**

```json
{
    "name": "HubspotDataset",
    "properties": {
        "type": "HubspotObject",
        "linkedServiceName": {
            "referenceName": "<Hubspot linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door HubSpot bron.

### <a name="hubspotsource-as-source"></a>HubspotSource als bron

Om gegevens te kopiëren van HubSpot, stelt u het brontype in de kopieeractiviteit naar **HubspotSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **HubspotSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM Companies where Company_Id = xxx"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromHubspot",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Hubspot input dataset name>",
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
                "type": "HubspotSource",
                "query": "SELECT * FROM Companies where Company_Id = xxx"
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
