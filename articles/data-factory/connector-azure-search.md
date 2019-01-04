---
title: Gegevens kopiëren naar Search-index met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het pushen of gegevens kopiëren naar een Azure search-index met behulp van de Kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: aa6c6a35a66569d5db182e1871012b9697c2802c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023342"
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Gegevens kopiëren naar een Azure Search-index met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-search-connector.md)
> * [Huidige versie](connector-azure-search.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren in Azure Search-index. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een ondersteund brongegevensarchief kopiëren naar Azure Search-index. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Search-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de gekoppelde Azure-Search-service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureSearch** | Ja |
| url | De URL voor de Azure Search-service. | Ja |
| sleutel | Administrator-code voor de Azure Search-service. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

> [!IMPORTANT]
> Bij het kopiëren van gegevens uit een cloudgegevensopslag in Azure Search-index in Azure Search gekoppelde service, moet u een Azure Integration Runtime met expliciete-regio's in connactVia verwijzen. Instellen van de regio als het account dat uw Azure Search zich bevindt. Meer informatie uit [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime).

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Search-gegevensset.

Als u wilt kopiëren van gegevens in Azure Search, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Search-bron.

### <a name="azure-search-as-sink"></a>Azure Search als sink

Om gegevens te kopiëren in Azure Search, stelt u het brontype in de kopieeractiviteit naar **AzureSearchIndexSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureSearchIndexSink** | Ja |
| WriteBehavior | Hiermee geeft u op of u wilt samenvoegen of vervangen wanneer een document al in de index bestaat. Zie de [WriteBehavior eigenschap](#writebehavior-property).<br/><br/>Toegestane waarden zijn: **Samenvoegen** (standaard), en **uploaden**. | Nee |
| WriteBatchSize | Wanneer de buffergrootte writeBatchSize bereikt, uploadt u gegevens in de Azure Search-index. Zie de [WriteBatchSize eigenschap](#writebatchsize-property) voor meer informatie.<br/><br/>Toegestane waarden zijn: geheel getal van 1 naar 1000; standaardwaarde is 1000. | Nee |

### <a name="writebehavior-property"></a>De eigenschap WriteBehavior

AzureSearchSink upsert-bewerking bij het schrijven van gegevens. Bij het schrijven van een document, als de documentsleutel al in de Azure Search-index bestaat, werkt Azure Search met andere woorden, het bestaande document in plaats van die een conflict uitzondering veroorzaakt.

De AzureSearchSink biedt de volgende twee upsert gedrag (met behulp van SDK AzureSearch):

- **Samenvoegen**: alle kolommen in het nieuwe document met de bestaande combineren. Voor kolommen met een null-waarde in het nieuwe document, wordt de waarde in het bestaande bestand behouden.
- **Uploaden**: Het nieuwe document wordt het bestaande bestand vervangen. Voor de kolommen is niet opgegeven in het nieuwe document, wordt de waarde ingesteld op null of er een niet-null-waarde in het bestaande document of niet is.

Het standaardgedrag **samenvoegen**.

### <a name="writebatchsize-property"></a>De eigenschap WriteBatchSize

Azure Search-service ondersteunt documenten schrijven als een batch. Een batch kan 1 naar 1000 acties bevatten. Een actie verwerkt één document als het uploaden/merge-bewerking wilt uitvoeren.

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

| Azure Search-gegevenstype | Ondersteund in Azure Search-Sink |
| ---------------------- | ------------------------------ |
| Reeks | J |
| Int32 | J |
| Int64 | J |
| Double-waarde | J |
| Booleaans | J |
| DataTimeOffset | J |
| String-matrix | N |
| GeographyPoint | N |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
