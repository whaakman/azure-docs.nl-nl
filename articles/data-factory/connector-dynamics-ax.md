---
title: Gegevens kopiëren van Dynamics AX met behulp van Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Dynamics AX naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 11/28/2018
ms.author: jingwang
ms.openlocfilehash: 94358ffde697b8122e65aefcbe1dd97385ca5b3a
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621807"
---
# <a name="copy-data-from-dynamics-ax-by-using-azure-data-factory-preview"></a>Gegevens kopiëren van Dynamics AX met behulp van Azure Data Factory (Preview)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Dynamics AX-bron. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Dynamics AX kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

Met name kopiëren van gegevens uit met behulp van Dynamics AX biedt ondersteuning voor deze connector Dynamics AX **OData-protocol** met **Service-Principal verificatie**.

>[!TIP]
>U kunt ook deze connector gebruiken om te kopiëren van gegevens uit **Dynamics 365 Finance and Operations**. Raadpleeg de Dynamics 365 [OData-ondersteuning](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/odata) en [verificatiemethode](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor Dynamics AX-connector zijn.

## <a name="prerequisites"></a>Vereisten

Volg deze stappen voor het gebruik van service-principal verificatie:

1. Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) door [uw toepassing registreren bij een Azure AD-tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

2. Ga naar Dynamics AX en verleent deze service principal juiste machtiging voor toegang tot uw Dynamics AX.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Dynamics AX gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **DynamicsAX**. |Ja |
| url | Het Dynamics AX (of Dynamics 365 Finance and Operations)-exemplaar OData-eindpunt. |Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| aadResourceId | Geef de resource die u aanvraagt autorisatie. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt Azure Integration Runtime of een zelf-hostende Integration Runtime (als het gegevensarchief bevindt zich in een particulier netwerk). Indien niet opgegeven, wordt de standaard Azure Integration Runtime wordt gebruikt. |Nee |

**Voorbeeld**

```json
{
    "name": "DynamicsAXLinkedService",
    "properties": {
        "type": "DynamicsAX",
        "typeProperties": {
            "url": "<Dynamics AX instance OData endpoint>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<Dynamics AX resource url>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}

```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de Dynamics AX-gegevensset.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). 

Om gegevens te kopiëren van Dynamics AX, stel de **type** eigenschap van de gegevensset in **DynamicsAXResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **DynamicsAXResource**. | Ja |
| pad | Het pad naar de Dynamics AX OData-entiteit. | Ja |

**Voorbeeld**

```json
{
    "name": "DynamicsAXResourceDataset",
    "properties": {
        "type": "DynamicsAXResource",
        "typePoperties": {     
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics AX linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de Dynamics AX-bron.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md). 

### <a name="dynamics-ax-as-source"></a>Dynamics AX als bron

Om gegevens te kopiëren van Dynamics AX, stel de **bron** type in de Kopieeractiviteit naar **DynamicsAXSource**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **DynamicsAXSource**. | Ja |
| query | OData-queryopties voor het filteren van gegevens. Voorbeeld: `"?$select=Name,Description&$top=5"`.<br/><br/>**Houd er rekening mee**: de connector worden gegevens gekopieerd van de gecombineerde URL: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Zie voor meer informatie, [OData-URL-onderdelen](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Nee |

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyFromDynamicsAX",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics AX input dataset name>",
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
                "type": "DynamicsAXSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md##supported-data-stores-and-formats).