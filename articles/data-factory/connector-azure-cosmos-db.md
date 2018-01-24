---
title: "Gegevens kopiëren naar/van Azure Cosmos DB met Data Factory | Microsoft Docs"
description: "Ontdek hoe gegevens van ondersteunde bron gegevensarchieven bij Azure Cosmos-DB (of) van de Cosmos-database kopiëren voor gebruik van Data Factory ondersteunde sink-stores."
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 1114fd272bd16d17fbeb382b0f374a69dc74916a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Cosmos DB met Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-documentdb-connector.md)
> * [Versie 2 - Preview](connector-azure-cosmos-db.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Azure Cosmos-DB (SQL-API). Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure Cosmos DB connnector in V1](v1/data-factory-azure-documentdb-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Azure DB die Cosmos naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar Azure Cosmos DB. Zie voor een lijst met gegevensarchieven als bronnen/put wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Azure DB die Cosmos-connector:

- Cosmos DB [SQL-API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- JSON-documenten als importeren/exporteren-is of kopiëren van gegevens van/naar tabular gegevensset bijvoorbeeld SQL-database, CSV-bestanden, enzovoort.

Documenten kopiëren als-is naar/van de JSON-bestanden of een andere verzameling van de Cosmos-DB, Zie [voor importeren/exporteren JSON-documenten](#importexport-json-documents).

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke bij Azure Cosmos DB.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Cosmos DB gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **CosmosDb**. | Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure DB die Cosmos-database. Houd er rekening mee moet u gegevens van de database in de verbindingsreeks opgeven als het onderstaande voorbeeld. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure DB die Cosmos-gegevensset.

Om gegevens te kopiëren van/naar Azure Cosmos DB, stel de eigenschap type van de gegevensset **DocumentDbCollection**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **DocumentDbCollection** |Ja |
| CollectionName |De naam van de Cosmos-DB-document-verzameling. |Ja |

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

Voor gegevens zonder schema winkels zoals Azure Cosmos DB infereert kopieeractiviteit het schema in een van de volgende manieren. Daarom, tenzij u wilt [als JSON-documenten voor importeren/exporteren-is](#importexport-json-documents), de beste manier is om op te geven van de structuur van de gegevens in de **structuur** sectie.

1. Als u de structuur van gegevens met behulp van opgeven de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt in dit geval een null-waarde opgegeven voor deze.
2. Als u de structuur van gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service infereert het schema met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij niet het volledige schema bevat, sommige kolommen worden ontbreekt in het resultaat van de kopieerbewerking.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure DB die Cosmos-bron- en sink.

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB als bron

Om gegevens te kopiëren van Azure Cosmos DB, stelt u het brontype in de kopieeractiviteit om te **DocumentDbCollectionSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **DocumentDbCollectionSource** |Ja |
| query |Geef de Cosmos-DB-query om te lezen van gegevens.<br/><br/>Voorbeeld:`SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest en hoe flattern het resultaat ingesteld.<br/><br/>Bijvoorbeeld, als een Cosmos-DB-query retourneert een geneste resultaat `"Name": {"First": "John"}`, kopieeractiviteit wordt geïdentificeerd kolomnaam als 'Name.First' met de waarde 'John' wanneer de nestedSeparator punt. |Nee (standaard is dit punt `.`) |

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

Om gegevens te kopiëren van Azure Cosmos DB, stelt u het sink-type in de kopieerbewerking te **DocumentDbCollectionSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **DocumentDbCollectionSink** |Ja |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Bijvoorbeeld: `Name.First` de uitvoergegevensset structuur genereert de volgende JSON-structuur in het document DB Cosmos:`"Name": {"First": "[value maps to this column from source]"}` wanneer de nestedSeparator punt is. |Nee (standaard is dit punt `.`) |
| writeBatchTimeout |Wachttijd voor de bewerking te voltooien voordat er een optreedt time-out.<br/><br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

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
                "type": "DocumentDbCollectionSink"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>Import/Export JSON-documenten

Met deze Cosmos-DB-connector kunt u eenvoudig

* JSON-documenten uit diverse bronnen in Cosmos-DB, waaronder Azure Blob, Azure Data Lake Store en andere bestandsgebaseerde winkels ondersteund door Azure Data Factory importeren.
* JSON-documenten uit Cosmos DB gewijzigd exporteren in verschillende winkels op basis van bestanden.
* Documenten tussen twee Cosmos DB verzamelingen op als kopiëren-is.

Als u dit schema networkdirect-exemplaar:

- In Cosmos DB gegevensset (s), geef het gedeelte 'structuur'; en in kopieeractiviteit Cosmos DB bron/sink geeft niet de eigenschap 'nestingSeparator'.
- Wanneer van importeren / exporteren naar JSON-bestanden in de bijbehorende bestand store dataset notatietype opgeven als 'JsonFormat' en config 'filePattern' goed (Zie [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format) sectie voor meer informatie), geeft u niet de structuur' 'sectie en overslaan van de instellingen van de rest-indeling.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).