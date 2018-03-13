---
title: "Gegevens kopiëren van ServiceNow met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van ServiceNow naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 02/22/2018
ms.author: jingwang
ms.openlocfilehash: 4d05d816b43e04c2c2e88cab33b8e4c623ab9185
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="copy-data-from-servicenow-using-azure-data-factory"></a>Gegevens kopiëren van Azure Data Factory met ServiceNow

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van ServiceNow. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit ServiceNow kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar ServiceNow-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor ServiceNow gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **ServiceNow** | Ja |
| endpoint | Het eindpunt van de server ServiceNow (`http://ServiceNowData.com`).  | Ja |
| authenticationType | Het verificatietype dat moet worden gebruikt. <br/>Toegestane waarden zijn: **Basic**, **OAuth2** | Ja |
| gebruikersnaam | De gebruikersnaam voor verbinding met de server ServiceNow voor Basic en OAuth2-verificatie gebruikt.  | Nee |
| wachtwoord | Het wachtwoord dat overeenkomt met de gebruikersnaam voor de verificatie Basic en OAuth2. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| clientId | De client-ID voor OAuth2-verificatie.  | Nee |
| clientSecret | Het clientgeheim voor OAuth2-verificatie. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| useEncryptedEndpoints | Geeft aan of de eindpunten van de gegevensbron zijn versleuteld via HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Geeft aan of de hostnaam in het certificaat van de server overeenkomen met de hostnaam van de server om verbinding te maken via SSL vereisen. De standaardwaarde is true.  | Nee |
| usePeerVerification | Geeft aan of de identiteit van de server te verifiëren wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://ServiceNowData.com",
            "authenticationType" : "Basic",
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door ServiceNow-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van ServiceNow, **ServiceNowObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door ServiceNow-bron.

### <a name="servicenow-as-source"></a>ServiceNow als bron

Om gegevens te kopiëren van ServiceNow, stelt u het brontype in de kopieerbewerking naar **ServiceNowSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **ServiceNowSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM Actual.alm_asset"`. | Ja |

Let op het volgende bij het opgeven van het schema en de kolom voor ServiceNow in query:

- **Schema:** Geef het schema als `Actual` of `Display` in de ServiceNow-query u dit als de parameter van beschouwen kunt `sysparm_display_value` als waar of onwaar bij het aanroepen van [ServiceNow restful-API's](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Kolom:** voor de werkelijke waarde onder de naam van de kolom `Actual` scehma is `[columne name]_value`, terwijl voor de weergegeven waarde onder `Display` schema is `[columne name]_display_value`. Opmerking naam van de kolom moet zijn toegewezen aan het schema wordt gebruikt in de query.

**Voorbeeldquery:** 
 `SELECT col_value FROM Actual.alm_asset` of `SELECT col_display_value FROM Display.alm_asset`

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
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
