---
title: "Gegevens kopiëren naar de Search-index met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over het push of gegevens kopiëren naar een Azure search-index met behulp van de Kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.openlocfilehash: 63081e2e5a2c792c8e688e7b8aaff0eca40e48a1
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Gegevens kopiëren naar een Azure Search-index met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-search-connector.md)
> * [Versie 2 - Preview](connector-azure-search.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar Azure Search-index. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure Search-connector in V1](v1/data-factory-azure-search-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure Search-index. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Search-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Search gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureSearch** | Ja |
| url | De URL voor de Azure Search-service. | Ja |
| sleutel | Administrator-code voor de Azure Search-service. Dit veld markeren als een SecureString. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

> [!IMPORTANT]
> Wanneer gegevens uit een cloud-gegevensarchief kopiëren naar Azure Search-index in Azure Search service gekoppelde, moet u een Azure-integratie Runtime met expliciete regio in connactVia verwijzen. Instellen van de regio als uw Azure Search zich bevindt. Klik hier als u meer wilt weten van [Azure integratie Runtime](concepts-integration-runtime.md#azure-integration-runtime).

**Voorbeeld:**

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Search-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren naar Azure Search, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureSearchIndex** | Ja |
| indexName | De naam van de Azure Search-index. Data Factory maakt niet de index. De index moet bestaan in Azure Search. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": {
            "referenceName": "<Azure Search linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties" : {
            "indexName": "products"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Azure Search-bron.

### <a name="azure-search-as-sink"></a>Azure Search als sink

Om gegevens te kopiëren naar Azure Search, stelt u het brontype in de kopieerbewerking naar **AzureSearchIndexSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureSearchIndexSink** | Ja |
| writeBehavior | Geeft aan of samenvoegen of wanneer een document al in de index bestaat te vervangen. Zie de [WriteBehavior eigenschap](#writebehavior-property).<br/><br/>Toegestane waarden zijn: **samenvoegen** (standaard), en **uploaden**. | Nee |
| writeBatchSize | Gegevens geüpload naar de Azure Search-index wanneer de buffergrootte writeBatchSize bereikt. Zie de [WriteBatchSize eigenschap](#writebatchsize-property) voor meer informatie.<br/><br/>Toegestane waarden zijn: geheel getal van 1-1000; standaardwaarde is 1000. | Nee |

### <a name="writebehavior-property"></a>De eigenschap WriteBehavior

AzureSearchSink upserts bij het schrijven van gegevens. Bij het schrijven van een document, als de documentsleutel al in de Azure Search-index bestaat, updates Azure Search met andere woorden, het bestaande document in plaats van er een conflict uitzondering is opgetreden.

De AzureSearchSink biedt de volgende twee upsert problemen (met behulp van AzureSearch SDK):

- **Samenvoegen**: alle kolommen in het nieuwe document met de huidige combineren. Voor kolommen met null-waarde in het nieuwe document, wordt de waarde in de bestaande waarde bewaard.
- **Uploaden**: nieuw document vervangt de bestaande waarde. Voor de kolommen is niet opgegeven in het nieuwe document, wordt de waarde ingesteld op null of er een niet-null-waarde in een bestaand document of niet is.

Het standaardgedrag **samenvoegen**.

### <a name="writebatchsize-property"></a>De eigenschap WriteBatchSize

Azure Search-service ondersteunt documenten schrijven als een batch. Een batch kunt 1-1000 acties bevatten. Een actie verwerkt één document als de upload/merge-bewerking wilt uitvoeren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

### <a name="data-type-support"></a>Ondersteuning voor gegevenstype

De volgende tabel geeft aan of een Azure Search-gegevenstype of niet wordt ondersteund.

| Azure Search-gegevenstype | Ondersteund in Azure Search Sink |
| ---------------------- | ------------------------------ |
| Tekenreeks | J |
| Int32 | J |
| Int64 | J |
| Double | J |
| Boole-waarde | J |
| DataTimeOffset | J |
| Tekenreeksmatrix | N |
| GeographyPoint | N |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
