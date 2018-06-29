---
title: Gegevens kopiëren van Concur met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van Concur naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.openlocfilehash: 9414107e92bfb48bbf28348aa45c8ec6795dbd3f
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045405"
---
# <a name="copy-data-from-concur-using-azure-data-factory"></a>Gegevens kopiëren van Concur met behulp van Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens van Concur kopiëren. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uit te proberen en ons feedback te geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Concur kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

> [!NOTE]
> Partner-account is momenteel niet ondersteund.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Concur-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Concur gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Concur** | Ja |
| clientId | Toepassing client_id geleverd door instemming van de App-beheer.  | Ja |
| gebruikersnaam | De gebruikersnaam die u gebruikt voor toegang tot Service overeenstemmen.  | Ja |
| wachtwoord | Het wachtwoord dat overeenkomt met de naam van de gebruiker die u hebt opgegeven in het veld username. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| useEncryptedEndpoints | Geeft aan of de eindpunten van de gegevensbron zijn versleuteld via HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Geeft aan of de hostnaam in het certificaat van de server overeenkomen met de hostnaam van de server om verbinding te maken via SSL vereisen. De standaardwaarde is true.  | Nee |
| usePeerVerification | Geeft aan of de identiteit van de server te verifiëren wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "ConcurLinkedService",
    "properties": {
        "type": "Concur",
        "typeProperties": {
            "clientId" : "<clientId>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Concur dataset.

Om gegevens te kopiëren van Concur, stel de eigenschap type van de gegevensset **ConcurObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "ConcurDataset",
    "properties": {
        "type": "ConcurObject",
        "linkedServiceName": {
            "referenceName": "<Concur linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Concur bron.

### <a name="concursource-as-source"></a>ConcurSource als bron

Om gegevens te kopiëren van Concur, stelt u het brontype in de kopieerbewerking naar **ConcurSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **ConcurSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM Opportunities where Id = xxx "`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromConcur",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Concur input dataset name>",
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
                "type": "ConcurSource",
                "query": "SELECT * FROM Opportunities where Id = xxx"
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
