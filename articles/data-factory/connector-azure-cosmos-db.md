---
title: Gegevens kopiëren naar/van Azure Cosmos DB met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron-gegevensopslag met Azure Cosmos DB (of) van Cosmos DB voor ondersteunde sink stores met behulp van Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: jingwang
ms.openlocfilehash: eb290dffd7bdd86b9776d4f30b6fd09a2a952a46
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720018"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Cosmos DB met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-documentdb-connector.md)
> * [Huidige versie](connector-azure-cosmos-db.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Azure Cosmos DB (SQL-API). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Azure Cosmos DB naar een ondersteunde sink-gegevensopslag, of gegevens kopiëren van een ondersteund brongegevensarchief naar een Azure Cosmos DB. Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit ondersteund als bronnen/sinks, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze connector Azure Cosmos DB ondersteunt name:

- Gegevens kopiëren van/naar Cosmos DB [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Schrijven naar Cosmos DB als UPSERT of invoegen.
- JSON-documenten als importeren/exporteren-is of kopiëren van gegevens van/naar de gegevensset in tabelvorm zoals SQL-database, CSV-bestanden, enzovoort. Documenten om te kopiëren als-is naar/van JSON-bestanden of een andere Cosmos DB-verzameling, Zie [Import/Export-JSON-documenten](#importexport-json-documents).

Data Factory kan worden geïntegreerd met [Cosmos DB bulksgewijs executor-bibliotheek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) voor de beste prestaties schrijven in Cosmos DB.

>[!TIP]
>Bekijk [in deze video](https://www.youtube.com/watch?v=5-SRNiC_qOU&t=760s) die helpt bij het kopiëren van gegevens uit Azure Blob-opslag met Cosmos DB en een beschrijving van overwegingen voor het ophalen van gegevens in het algemeen in Cosmos DB afstemmen van de prestaties.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Azure Cosmos DB.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Cosmos DB gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **CosmosDb**. | Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure Cosmos DB-database. Houd er rekening mee moet u database-informatie in de verbindingsreeks opgeven als het onderstaande voorbeeld. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Cosmos DB-gegevensset.

Om gegevens te kopiëren van/naar Azure Cosmos DB, stel de eigenschap type van de gegevensset in **DocumentDbCollection**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **DocumentDbCollection** |Ja |
| collectionName |De naam van de Cosmos DB-documentverzameling. |Ja |

**Voorbeeld:**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema door Data Factory

Voor de winkels schemavrije gegevens, zoals Azure Cosmos DB bepaalt copy activity in het schema in een van de volgende manieren. Daarom, tenzij u wilt [JSON-documenten als voor importeren/exporteren-is](#importexport-json-documents), de aanbevolen procedure is om op te geven van de structuur van de gegevens in de **structuur** sectie.

*. Als u de structuur van gegevens met behulp van de **structuur** eigenschap in de definitie van de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt er in dit geval een null-waarde worden opgegeven voor deze.
*. Als u de structuur van de gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de Data Factory-service bepaalt welk schema met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij niet het volledige schema bevat, sommige kolommen worden ontbreekt in het resultaat van de kopieerbewerking.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Cosmos DB-bron- en sinkblobpaden.

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB als bron

Om gegevens te kopiëren van Azure Cosmos DB, stelt u het brontype in de kopieeractiviteit naar **DocumentDbCollectionSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **DocumentDbCollectionSource** |Ja |
| query |Specificeer de Cosmos DB-query voor het lezen van gegevens.<br/><br/>Voorbeeld: `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest en hoe aan flattern het resultaat ingesteld.<br/><br/>Bijvoorbeeld, als een Cosmos DB-query retourneert een geneste resultaat `"Name": {"First": "John"}`, copy-activiteit identificeert kolomnaam als "Name.First" met de waarde "John" wanneer de nestedSeparator punt. |Nee (de standaardwaarde is punt `.`) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Azure Cosmos DB als sink

Om gegevens te kopiëren naar Azure Cosmos DB, stelt u het sink-type in de kopieeractiviteit naar **DocumentDbCollectionSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **DocumentDbCollectionSink** |Ja |
| WriteBehavior |Beschreven hoe u gegevens schrijft naar Cosmos DB. Toegestane waarden zijn: `insert` en `upsert`.<br/>Het gedrag van **upsert** wordt vervangen door het document als een document met dezelfde id al bestaat; anders is het invoegen. Opmerking ADF genereert automatisch een id voor het document als deze niet is opgegeven in het oorspronkelijke document of door in de kolomtoewijzing), wat betekent dat u nodig hebt om te controleren of uw document heeft een 'id' zodat upsert werken zoals verwacht. |Nee, de standaardwaarde is invoegen |
| WriteBatchSize | Gebruik van Data Factory [Cosmos DB bulksgewijs executor-bibliotheek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) om gegevens te schrijven in Cosmos DB. "writeBatchSize" bepaalt de grootte van documenten die we elke tijd aan de bibliotheek biedt. U kunt proberen te vergroten writeBatchSize om prestaties te verbeteren. |Nee, de standaardwaarde is 10.000 |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Bijvoorbeeld, `Name.First` in de uitvoergegevensset structuur genereert de volgende JSON-structuur in de Cosmos DB-document:`"Name": {"First": "[value maps to this column from source]"}` wanneer de nestedSeparator punt is. |Nee (de standaardwaarde is punt `.`) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>Import/Export-JSON-documenten

Met deze connector Cosmos DB kunt u eenvoudig

* JSON-documenten uit verschillende bronnen importeren in Cosmos DB, met inbegrip van Azure Blob-, Azure Data Lake Store- en andere bestandsgebaseerde winkels die wordt ondersteund door Azure Data Factory.
* JSON-documenten uit Cosmos DB gewijzigd in verschillende archieven op basis van een bestand exporteren.
* Documenten tussen twee Cosmos DB-verzamelingen als kopiëren-is.

Voor dergelijke schema-agnostische kopiëren:

* Als u het kopieerprogramma van gegevens, moet u de **' exporteren als-JSON-bestanden of Cosmos DB-verzameling "** optie.
* Wanneer met behulp van activiteit ontwerpen, geen de sectie 'structuur' (ook wel schema) opgeeft in Cosmos DB gegevensset (s), noch de eigenschap 'nestingSeparator' op Cosmos DB bron/sink in de kopieeractiviteit. Wanneer van importeren / exporteren naar JSON-bestanden in het bijbehorende bestand store gegevensset opgeven type-indeling als 'JsonFormat' en het configuratiebestand 'filePattern' goed (Zie [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format) sectie voor meer informatie), klikt u vervolgens de structuur' niet opgeven ' (ook wel schema) sectie en overslaan van de instellingen van de rest-indeling.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
