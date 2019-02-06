---
title: Gegevens kopiëren naar of van Azure Data Explorer met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens naar of van Azure Data Explorer met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 02/01/2019
ms.author: orspod
ms.openlocfilehash: 8f2a7a953ce2964645c281d9454a73b0cf1a8ff6
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747185"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Explorer met behulp van Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar of van [Azure Data Explorer](../data-explorer/data-explorer-overview.md). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een ondersteund brongegevensarchief kopiëren naar Azure Data Explorer. U kunt ook gegevens uit Azure Data Explorer kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md) tabel.

>[!NOTE]
>Op dit moment is kopiëren van gegevens naar/van Azure Data Explorer van/naar on-premises gegevensarchief gebruiken zelfgehoste Integration Runtime nog niet ondersteund.

De Azure Data Explorer-connector kunt u het volgende doen:

* Gegevens kopiëren met behulp van Azure Active Directory (Azure AD)-toepassing voor token verificatie met een **service-principal**.
* Als een bron ophalen van gegevens met behulp van een query KQL (Kusto).
* Als een sink, moet u gegevens toevoegen aan een doeltabel.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Azure Data Explorer-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Data Explorer gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **AzureDataExplorer** | Ja |
| endpoint | Eindpunt-URL van het cluster Azure Data Explorer, klikt u met de indeling als `https://<clusterName>.<regionName>.kusto.windows.net `. | Ja |
| database | De naam van de database. | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muiswijzer met de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |

**Voorbeeld van de gekoppelde Service-eigenschappen:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure Data Explorer-gegevensset.

Om gegevens te kopiëren naar Azure Data Explorer, stel de eigenschap type van de gegevensset in **AzureDataExplorerTable**.

De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **AzureDataExplorerTable** | Ja |
| tabel | De naam van de tabel waarnaar de gekoppelde service verwijst. | Ja voor sink; Nee voor bron |

**Voorbeeld van de gegevensset-eigenschappen**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "table": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Explorer-bron- en sinkblobpaden.

### <a name="azure-data-explorer-as-source"></a>Azure Data Explorer als bron

Instellen om gegevens te kopiëren van Azure Data Explorer, de **type** eigenschap in de bron van de activiteit kopiëren naar het **AzureDataExplorerSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureDataExplorerSource** | Ja |
| query | Een alleen-lezen-aanvraag die in een [KQL indeling](/azure/kusto/query/). Gebruik de aangepaste KQL query als uitgangspunt. | Ja |
| queryTimeout | Er is een time-out opgetreden voor de wachttijd voordat de queryaanvraag. Standaardwaarde is 10 minuten (00: 10:00); toegestane maximale waarde is 1 uur (01: 00:00). | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Azure Data Explorer als sink

Om gegevens te kopiëren naar Azure Data Explorer, stelt u de eigenschap type in de activiteit-sink kopiëren naar **AzureDataExplorerSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de kopie-activiteit-sink moet zijn ingesteld op: **AzureDataExplorerSink** | Ja |
| ingestionMappingName | Naam van een vooraf gemaakte [csv toewijzing](/azure/kusto/management/mappings#csv-mapping) op een Kusto-tabel. Als u wilt toewijzen van de kolommen van bron naar het Azure-gegevens verkennen, kunt u ook de Copy-activiteit gebruiken [kolomtoewijzing](copy-activity-schema-and-type-mapping.md). | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional csv mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).