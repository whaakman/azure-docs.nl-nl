---
title: Gegevens kopiëren naar of van Azure Cosmos DB met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens uit ondersteunde bron-gegevensarchieven naar of van Azure Cosmos DB voor ondersteunde sink stores met behulp van Data Factory.
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
ms.openlocfilehash: 9a75ae8645503366a490dbc0ea65d2fdc73d7c61
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167287"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Cosmos DB met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-documentdb-connector.md)
> * [Huidige versie](connector-azure-cosmos-db.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Azure Cosmos DB (SQL-API). Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Azure Cosmos DB naar een ondersteunde sink-gegevensopslag, of gegevens kopiëren van een ondersteund brongegevensarchief naar een Azure Cosmos DB. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

U kunt de Azure Cosmos DB-connector te gebruiken:

- Gegevens kopiëren van en naar de Azure Cosmos DB [SQL API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Schrijven naar Azure Cosmos DB als **invoegen** of **upsert**.
- Importeren en exporteren van JSON-documenten als-is, of gegevens te kopiëren van of naar een gegevensset in tabelvorm. Voorbeelden zijn een SQL-database en een CSV-bestand. Documenten om te kopiëren als- of naar JSON-bestanden of naar of van een andere Azure Cosmos DB-verzameling, Zie [importeren of exporteren van JSON-documenten](#importexport-json-documents).

Data Factory kan worden geïntegreerd met de [Azure Cosmos DB bulksgewijs executor bibliotheek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) voor de beste prestaties bij het schrijven naar Azure Cosmos DB.

> [!TIP]
> De [gegevensmigratie video](https://youtu.be/5-SRNiC_qOU) begeleidt u stapsgewijs door de procedure voor het kopiëren van gegevens uit Azure Blob-opslag met Azure Cosmos DB. Prestaties afstemmen overwegingen voor het ophalen van gegevens met Azure Cosmos DB in het algemeen wordt ook beschreven in de video.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Cosmos DB zijn.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Azure Cosmos DB is gekoppeld:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **CosmosDb**. | Ja |
| connectionString |Geef informatie op die nodig is om verbinding met de Azure Cosmos DB-database.<br /><br />**Houd er rekening mee**: moet u database-informatie in de verbindingsreeks opgeven zoals wordt weergegeven in de voorbeelden. Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt de Azure Integration Runtime of een zelf-hostende integratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als deze eigenschap niet is opgegeven, wordt de standaard Azure Integration Runtime wordt gebruikt. |Nee |

**Voorbeeld**

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

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de Azure Cosmos DB-gegevensset. 

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). 

Om gegevens te kopiëren van of naar Azure Cosmos DB, stel de **type** eigenschap van de gegevensset in **DocumentDbCollection**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **DocumentDbCollection**. |Ja |
| collectionName |De naam van de Azure Cosmos DB-documentverzameling. |Ja |

**Voorbeeld**

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

Voor de winkels schemavrije gegevens, zoals Azure Cosmos DB bepaalt Copy Activity in het schema in een van de manieren beschreven in de volgende lijst. Tenzij u wilt [importeren of exporteren van JSON-documenten als-is](#import-or-export-json-documents), de aanbevolen procedure is om op te geven van de structuur van de gegevens in de **structuur** sectie.

* Als u de structuur van gegevens met behulp van de **structuur** eigenschap in de definitie van de Data Factory zich houdt aan deze structuur als het schema. 

    Als een rij geen waarde voor een kolom bevat, wordt een null-waarde opgegeven voor de waarde in de kolom.
* Als u de structuur van de gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de Data Factory-service bepaalt welk schema met behulp van de eerste rij in de gegevens. 

    Als de eerste rij niet het volledige schema bevat, worden sommige kolommen ontbreken in het resultaat van de kopieerbewerking.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning bieden voor de Azure Cosmos DB-bron en sink.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB als bron

Om gegevens te kopiëren van Azure Cosmos DB, stel de **bron** type in de Kopieeractiviteit naar **DocumentDbCollectionSource**. 

De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **DocumentDbCollectionSource**. |Ja |
| query |Geef de Azure Cosmos DB-query voor het lezen van gegevens.<br/><br/>Voorbeeld:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Als niet is opgegeven, wordt deze SQL-instructie uitgevoerd: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Een speciaal teken die aangeeft dat het document is genest en het samenvoegen van de resultatenset.<br/><br/>Bijvoorbeeld, als een Azure Cosmos DB-query retourneert het resultaat van de geneste `"Name": {"First": "John"}`, Copy-activiteit identificeert de naam van de kolom als `Name.First`, met de waarde 'John', wanneer de **nestedSeparator** waarde is **.** (punt). |Nee<br />(de standaardwaarde is **.** (punt)) |

**Voorbeeld**

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

Om gegevens te kopiëren naar Azure Cosmos DB, stel de **sink** type in de Kopieeractiviteit naar **DocumentDbCollectionSink**. 

De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Copy-activiteit-sink moet zijn ingesteld op **DocumentDbCollectionSink**. |Ja |
| WriteBehavior |Beschrijft hoe u gegevens naar Azure Cosmos DB te schrijven. Toegestane waarden: **invoegen** en **upsert**.<br/><br/>Het gedrag van **upsert** wordt vervangen door het document als een document met dezelfde ID al bestaat; anders wordt het document invoegen.<br /><br />**Houd er rekening mee**: Data Factory genereert automatisch een ID voor een document als een ID niet is opgegeven in het oorspronkelijke document of door in de kolomtoewijzing. Dit betekent dat u moet ervoor zorgen dat voor **upsert** om te werken zoals verwacht, heeft uw document een ID. |Nee<br />(de standaardwaarde is **invoegen**) |
| WriteBatchSize | Gegevensfactory maakt gebruik van de [Azure Cosmos DB bulksgewijs executor bibliotheek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) gegevens naar Azure Cosmos DB te schrijven. De **writeBatchSize** eigenschap bepaalt de grootte van documenten die wij aan de bibliotheek verstrekken. U kunt proberen te verhogen van de waarde voor **writeBatchSize** om prestaties te verbeteren. |Nee<br />(de standaardwaarde is **10.000**) |
| nestingSeparator |Een speciaal teken in de **bron** kolomnaam die aangeeft dat een geneste document is vereist. <br/><br/>Bijvoorbeeld, `Name.First` in de uitvoergegevensset structuur genereert de volgende JSON-structuur in de Azure Cosmos DB document wanneer de **nestedSeparator** is **.** (punt): `"Name": {"First": "[value maps to this column from source]"}`  |Nee<br />(de standaardwaarde is **.** (punt)) |

**Voorbeeld**

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

## <a name="import-or-export-json-documents"></a>Importeren of exporteren van JSON-documenten

U kunt eenvoudig deze Azure Cosmos DB-connector te gebruiken:

* JSON-documenten uit verschillende bronnen met Azure Cosmos DB, met inbegrip van Azure Blob storage, Azure Data Lake Store en andere bestandsgebaseerde winkels die ondersteuning biedt voor Azure Data Factory importeren.
* JSON-documenten uit een Azure Cosmos DB-verzameling exporteren naar verschillende winkels op basis van bestanden.
* Documenten tussen twee Azure Cosmos DB-verzamelingen als kopiëren-is.

Voor een schema-agnostische kopiëren:

* Wanneer u het hulpprogramma Copy Data gebruiken, selecteert u de **als exporteren-JSON-bestanden of Cosmos DB-verzameling** optie.
* Wanneer u activiteit ontwerpen, geen opgeeft de **structuur** (ook wel genoemd *schema*) sectie in de Azure Cosmos DB-gegevensset. Geef ook niet op de **nestingSeparator** eigenschap in de Azure Cosmos DB-gegevensbron of de sink in de Kopieeractiviteit. Wanneer u van importeren of exporteren naar JSON-bestanden, opgeslagen in het bijbehorende bestand gegevensset, geef de **indeling** typt als **JsonFormat** en configureer de **filePattern** als beschreven in de [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format) sectie. Vervolgens geeft u geen opgeeft de **structuur** sectie en slaat u de rest van de instellingen bestandsindelingen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
