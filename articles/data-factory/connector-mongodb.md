---
title: Gegevens kopiëren van MongoDB met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Mongo DB naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: ca6040bb74839f30a2f1b13297f6037f05240c67
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55562205"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Gegevens kopiëren van MongoDB met Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een MongoDB-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

>[!IMPORTANT]
>ADF het uitbrengen van deze nieuwe versie van de MongoDB-connector biedt betere systeemeigen ondersteuning voor MongoDB. Als u in uw oplossing die wordt ondersteund als de vorige MongoDB-connector-is voor achterwaartse compatibiliteit verwijzen naar [MongoDB-connector (verouderd)](connector-mongodb-legacy.md) artikel.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MongoDB-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies deze MongoDB-connector ondersteunt **tot 3.4**.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren uit een MongoDB-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor MongoDB-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MongoDB gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **MongoDbV2** |Ja |
| connectionString |Geef de MongoDB-verbindingsreeks bijvoorbeeld `mongodb://[username:password@]host[:port][/[database][?options]]`. Raadpleeg [MongoDB handmatig op verbindingsreeks](https://docs.mongodb.com/manual/reference/connection-string/) voor meer informatie. <br/><br />Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| database | De naam van de database die u wilt openen. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDbV2",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://[username:password@]host[:port][/[database][?options]]"
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

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). De volgende eigenschappen worden ondersteund voor MongoDB-gegevensset:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **MongoDbV2Collection** | Ja |
| collectionName |De naam van de verzameling in de MongoDB-database. |Ja |

**Voorbeeld:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbV2Collection",
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<Collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MongoDB-bron.

### <a name="mongodb-as-source"></a>MongoDB als bron

De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **MongoDbV2Source** | Ja |
| filter | Hiermee geeft u een selectie filteren met behulp van de standaardoperators voor query's. Als u wilt alle documenten in een verzameling, deze parameter weglaat of een leeg document doorgeven ({}). | Nee |
| cursorMethods.project | Hiermee geeft u de velden weer in de documenten voor projectie. Als u wilt alle velden in de overeenkomende documenten retourneren, moet u deze parameter weglaat. | Nee |
| cursorMethods.sort | Hiermee geeft u de volgorde waarin de query overeenkomende documenten retourneert. Raadpleeg [cursor.sort()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Nee |
| cursorMethods.limit | Hiermee geeft u het maximum aantal documenten de server retourneert. Raadpleeg [cursor.limit()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Nee |
| cursorMethods.skip | Hiermee geeft u het aantal documenten om over te slaan en uit waar MongoDB begint om resultaten te retourneren. Raadpleeg [cursor.skip()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Nee |
| batchSize | Hiermee geeft u het aantal documenten te retourneren in elke batch van het antwoord van de MongoDB-exemplaar. In de meeste gevallen de batchgrootte wijzigen heeft geen invloed op de gebruiker of de toepassing. Cosmos DB-limieten elke batch mag niet meer dan 40MB in grootte, die de som van het aantal batchSize van de grootte van documenten, verlaag zodat deze waarde als de documentgrootte van uw groot wordt. | Nee<br/>(de standaardwaarde is **100**) |

>[!TIP]
>Ondersteuning voor ADF verbruikt BSON-document in **strikte modus**. Zorg ervoor dat uw filterquery in de strikte modus in plaats van Shell-modus. Beschrijving van meer kan worden gevonden op [MongoDB handmatig](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbV2Source",
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

## <a name="export-json-documents-as-is"></a>JSON-documenten als exporteren-is

U kunt deze MongoDB-connector gebruiken voor het exporteren van JSON-documenten als-afkomstig is van een MongoDB-verzameling voor verschillende bestanden zijn gebaseerd stores of Azure Cosmos DB. Voor het bereiken van deze kopie van de schema-agnostische overslaan 'de structuur' (ook wel genoemd *schema*) sectie in de gegevensset en schematoewijzing in de kopieeractiviteit.

## <a name="schema-mapping"></a>Schematoewijzing

Om gegevens te kopiëren van MongoDB naar tabellaire sink, verwijzen naar [schematoewijzing](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
