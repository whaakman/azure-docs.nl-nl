---
title: Gegevens kopiëren naar of van Azure Cosmos DB (MongoDB-API) met behulp van Data Factory | Microsoft Docs
description: Leer hoe u gegevens van ondersteunde bron-gegevensopslag kopiëren naar of van Azure Cosmos DB (MongoDB-API) voor ondersteunde sink stores met behulp van Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: a28d0e819243810486179e7219ad3cb48487a299
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54107171"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-mongodb-api-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Cosmos DB (MongoDB-API) met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Azure Cosmos DB (MongoDB-API). Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

>[!NOTE]
>Alleen ondersteuning voor deze connector gegevens kopiëren van/naar Cosmos DB MongoDB-API. Raadpleeg voor SQL-API, [Cosmos DB SQL API-connector](connector-azure-cosmos-db.md). Andere API-typen worden nu niet ondersteund.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Azure Cosmos DB (MongoDB-API) naar een ondersteunde sink-gegevensopslag, of gegevens kopiëren van een ondersteund brongegevensarchief naar een Azure Cosmos DB (MongoDB-API). Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

U kunt de Azure Cosmos DB (MongoDB API)-connector te gebruiken:

- Gegevens kopiëren van en naar de Azure Cosmos DB [MongoDB-API](https://docs.microsoft.com/azure/cosmos-db/mongodb-introduction).
- Schrijven naar Azure Cosmos DB als **invoegen** of **upsert**.
- Importeren en exporteren van JSON-documenten als-is, of gegevens te kopiëren van of naar een gegevensset in tabelvorm. Voorbeelden zijn een SQL-database en een CSV-bestand. Documenten om te kopiëren als- of naar JSON-bestanden of naar of van een andere Azure Cosmos DB-verzameling, Zie [importeren of exporteren van JSON-documenten](#importexport-json-documents).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Cosmos DB (MongoDB-API zijn).

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Azure Cosmos DB (MongoDB-API) is gekoppeld:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **CosmosDbMongoDbApi**. | Ja |
| connectionString |Geef de verbindingsreeks voor uw Azure Cosmos DB MongoDB-API. U vindt deze in Azure portal -> uw blade Cosmos DB -> primaire of secundaire verbindingsreeks, met het patroon van `mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb`. <br/><br />Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| database | De naam van de database die u wilt openen. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt de Azure Integration Runtime of een zelf-hostende integratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als deze eigenschap niet is opgegeven, wordt de standaard Azure Integration Runtime wordt gebruikt. |Nee |

**Voorbeeld**

```json
{
    "name": "CosmosDbMongoDBAPILinkedService",
    "properties": {
        "type": "CosmosDbMongoDbApi",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://<cosmosdb-name>:<password>@<cosmosdb-name>.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
            },
            "database": "myDatabase"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). De volgende eigenschappen worden ondersteund voor Azure Cosmos DB (MongoDB API)-gegevensset:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **CosmosDbMongoDbApiCollection**. |Ja |
| collectionName |De naam van de Azure Cosmos DB-verzameling. |Ja |

**Voorbeeld**

```json
{
    "name": "CosmosDbMongoDBAPIDataset",
    "properties": {
        "type": "CosmosDbMongoDbApiCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB MongoDB API linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning bieden voor de Azure Cosmos DB (MongoDB API)-bron en sink.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md).

### <a name="azure-cosmos-db-mongodb-api-as-source"></a>Azure Cosmos DB (MongoDB-API) als bron

De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **CosmosDbMongoDbApiSource**. |Ja |
| filter | Hiermee geeft u een selectie filteren met behulp van de standaardoperators voor query's. Als u wilt alle documenten in een verzameling, deze parameter weglaat of een leeg document doorgeven ({}). | Nee |
| cursorMethods.project | Hiermee geeft u de velden weer in de documenten voor projectie. Als u wilt alle velden in de overeenkomende documenten retourneren, moet u deze parameter weglaat. | Nee |
| cursorMethods.sort | Hiermee geeft u de volgorde waarin de query overeenkomende documenten retourneert. Raadpleeg [cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Nee |
| cursorMethods.limit | Hiermee geeft u het maximum aantal documenten de server retourneert. Raadpleeg [cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Nee | 
| cursorMethods.skip | Hiermee geeft u het aantal documenten om over te slaan en uit waar MongoDB begint om resultaten te retourneren. Raadpleeg [cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Nee |
| batchSize | Hiermee geeft u het aantal documenten te retourneren in elke batch van het antwoord van de MongoDB-exemplaar. In de meeste gevallen de batchgrootte wijzigen heeft geen invloed op de gebruiker of de toepassing. Cosmos DB-limieten elke batch mag niet meer dan 40MB in grootte, die de som van het aantal batchSize van de grootte van documenten, verlaag zodat deze waarde als de documentgrootte van uw groot wordt. | Nee<br/>(de standaardwaarde is **100**) |

>[!TIP]
>Ondersteuning voor ADF verbruikt BSON-document in **strikte modus**. Zorg ervoor dat uw filterquery in de strikte modus in plaats van Shell-modus. Beschrijving van meer kan worden gevonden op [MongoDB handmatig](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBMongoDBAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB MongoDB API input dataset name>",
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
                "type": "CosmosDbMongoDbApiSource",
                "filter": "{datetimeData: {$gte: ISODate(\"2018-12-11T00:00:00.000Z\"),$lt: ISODate(\"2018-12-12T00:00:00.000Z\")}, _id: ObjectId(\"5acd7c3d0000000000000000\") }",
                "cursorMethods": {
                    "project": "{ _id : 1, name : 1, age: 1, datetimeData: 1 }",
                    "sort": "{ age : 1 }",
                    "skip": 3,
                    "limit": 3
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-mongodb-api-as-sink"></a>Azure Cosmos DB (MongoDB-API) als sink

De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Copy-activiteit-sink moet zijn ingesteld op **CosmosDbMongoDbApiSink**. |Ja |
| WriteBehavior |Beschrijft hoe u gegevens naar Azure Cosmos DB te schrijven. Toegestane waarden: **invoegen** en **upsert**.<br/><br/>Het gedrag van **upsert** wordt vervangen door het document als een document met dezelfde ID al bestaat; anders wordt het document invoegen.<br /><br />**Opmerking**: Data Factory genereert automatisch een ID voor een document als een ID niet is opgegeven in het oorspronkelijke document of door in de kolomtoewijzing. Dit betekent dat u moet ervoor zorgen dat voor **upsert** om te werken zoals verwacht, heeft uw document een ID. |Nee<br />(de standaardwaarde is **invoegen**) |
| WriteBatchSize | De **writeBatchSize** eigenschap bepaalt de grootte van documenten om te schrijven in elke batch. U kunt proberen te verhogen van de waarde voor **writeBatchSize** voor betere prestaties en het verminderen van de waarde als de documentgrootte van uw wordt groot. |Nee<br />(de standaardwaarde is **10.000**) |
| writeBatchTimeout | De wachttijd voor de batch invoegen bewerking is voltooid voordat er een optreedt time-out. De toegestane waarde is duur. | Nee<br/>(de standaardwaarde is **00:30:00** : 30 minuten) |

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyToCosmosDBMongoDBAPI",
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
                "type": "CosmosDbMongoDbApiSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

>[!TIP]
>JSON-documenten als importeren-is, raadpleegt u [importeren of exporteren van JSON-documenten](#import-or-export-json-documents) sectie; als u wilt kopiëren van gegevens in tabelvorm vorm, verwijzen naar [schematoewijzing](#schema-mapping).

## <a name="import-or-export-json-documents"></a>Importeren of exporteren van JSON-documenten

U kunt eenvoudig deze Azure Cosmos DB-connector te gebruiken:

* JSON-documenten uit verschillende bronnen met Azure Cosmos DB, met inbegrip van Azure Blob storage, Azure Data Lake Store en andere bestandsgebaseerde winkels die ondersteuning biedt voor Azure Data Factory importeren.
* JSON-documenten uit een Azure Cosmos DB-verzameling exporteren naar verschillende winkels op basis van bestanden.
* Documenten tussen twee Azure Cosmos DB-verzamelingen als kopiëren-is.

Voor het bereiken van deze kopie van de schema-agnostische overslaan 'de structuur' (ook wel genoemd *schema*) sectie in de gegevensset en schematoewijzing in de kopieeractiviteit.

## <a name="schema-mapping"></a>Schematoewijzing

Gegevens kopiëren van Cosmos DB MongoDB API naar tabellaire sink of omgekeerd, verwijzen naar [schematoewijzing](copy-activity-schema-and-type-mapping.md#schema-mapping).

Specifiek voor het schrijven naar Cosmos DB, om ervoor te zorgen dat voor het vullen van de Cosmos DB met het juiste object-ID van de gegevensbron, bijvoorbeeld, u hebt een kolom 'id' in SQL database-tabel en wilt gebruiken de waarde van die als de document-ID in MongoDB voor invoegen/upsert , moet u de schematoewijzing van het juiste op basis van de definitie van de MongoDB-strikte modus instellen (`_id.$oid`) als het volgende:

![Toewijzings-ID in de MongoDB-sink](./media/connector-azure-cosmos-db-mongodb-api/map-id-in-mongodb-sink.png)

Na het kopiëren wordt uitvoering van activiteit, hieronder BSON-object-id gegenereerd in sink:

```json
{
    "_id": ObjectId("592e07800000000000000000")
}
``` 

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
