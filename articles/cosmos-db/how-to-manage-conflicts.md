---
title: Informatie over het beheren van conflicten tussen regio’s in Azure Cosmos DB
description: Informatie over het beheren van conflicten in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/16/2019
ms.author: mjbrown
ms.openlocfilehash: fb9850548f0bfb71b797830eb0d5fdfddbc32306
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997016"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Conflictoplossingsbeleid beheren in Azure Cosmos DB

Wanneer meerdere clients naar hetzelfde item schrijven, kunnen met schrijfbewerkingen in meerdere regio's, veroorzaakt een conflict optreden. Wanneer er een conflict optreedt, kunt u het conflict kan oplossen met behulp van verschillende conflict resolutie beleid. In dit artikel wordt beschreven hoe u conflict resolutie beleidsregels beheren.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Het conflictoplossingsbeleid 'last writer wins' maken

Deze voorbeelden laten zien hoe u een container kunt instellen met het conflictoplossingsbeleid 'last writer wins'. Het standaardpad voor laatste schrijver wins is het tijdstempelveld of de `_ts` eigenschap. Dit kan ook worden ingesteld op een door de gebruiker gedefinieerde pad voor een numeriek type. Een conflict in de wins-de hoogste waarde. Als het pad is niet ingesteld of als deze waarde ongeldig is, wordt standaard `_ts`. Conflicten met dit beleid is opgelost, niet weergegeven in de feed conflict. Dit beleid kan worden gebruikt door alle API's.

### <a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>.NET SDK

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/myCustomId",
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-lww-java-async"></a>Java Async SDK

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>Java Sync SDK

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```

### <a id="create-custom-conflict-resolution-policy-lww-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/myCustomId"
    }
  }
);
```

### <a id="create-custom-conflict-resolution-policy-lww-python"></a>Python SDK

```python
udp_collection = {
                'id': self.udp_collection_name,
                'conflictResolutionPolicy': {
                    'mode': 'LastWriterWins',
                    'conflictResolutionPath': '/myCustomId'
                    }
                }
udp_collection = self.try_create_document_collection(create_client, database, udp_collection)
```

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Een aangepaste conflict resolutie beleid met behulp van een opgeslagen procedure maken

Deze voorbeelden laten zien hoe u een container kunt instellen met aangepast conflictoplossingsbeleid met een opgeslagen procedure om het conflict op te lossen. Deze conflicten worden niet weergegeven in de conflictfeed tenzij er een fout is in de opgeslagen procedure. Nadat het beleid is gemaakt met de container die u wilt maken van de opgeslagen procedure. De SDK voor .NET-voorbeeld hieronder toont een voorbeeld van deze. Dit beleid wordt alleen op Core (SQL) Api ondersteund.

### <a name="sample-custom-conflict-resolution-stored-procedure"></a>Voorbeeld van aangepaste conflictoplossing opgeslagen procedure

Aangepaste conflict resolutie opgeslagen procedures moeten worden geïmplementeerd met behulp van de functiehandtekening hieronder wordt weergegeven. Naam van de functie hoeft niet overeen met de naam die wordt gebruikt bij het registreren van de opgeslagen procedure met de container maar deze vereenvoudigen naamgeving. Hier volgt een beschrijving van de parameters die moeten worden geïmplementeerd voor deze opgeslagen procedure.

- **incomingItem**: Het item wordt ingevoegd of bijgewerkt in de doorvoer die een conflict tussen het genereert. Is null voor de bewerking verwijderen.
- **existingItem**: Het momenteel toegewezen item. Deze waarde is null in een update- en null voor het invoegen of verwijderen.
- **isTombstone**: Booleaanse waarde waarmee wordt aangegeven als de incomingItem is in conflict met een eerder verwijderde item. Indien waar, is ook existingItem null zijn.
- **conflictingItems**: Matrix van de doorgevoerde versie van alle items in de container die met incomingItem op id conflicteren of een andere unieke index-eigenschappen.

> [!IMPORTANT]
> Net zoals met alle opgeslagen procedures, kunt een aangepaste oplossing procedure krijgen tot gegevens met dezelfde partitiesleutel en kunt uitvoeren van een invoegen, bijwerken of verwijderen bewerking voor het oplossen van conflicten.


In dit opgeslagen voorbeeldprocedure wordt veroorzaakt een conflict opgelost door het selecteren van de laagste waarde van de `/myCustomId` pad.

```javascript
function resolver(incomingItem, existingItem, isTombstone, conflictingItems) {
  var collection = getContext().getCollection();

  if (!incomingItem) {
      if (existingItem) {

          collection.deleteDocument(existingItem._self, {}, function (err, responseOptions) {
              if (err) throw err;
          });
      }
  } else if (isTombstone) {
      // delete always wins.
  } else {
      if (existingItem) {
          if (incomingItem.myCustomId > existingItem.myCustomId) {
              return; // existing item wins
          }
      }

      var i;
      for (i = 0; i < conflictingItems.length; i++) {
          if (incomingItem.myCustomId > conflictingItems[i].myCustomId) {
              return; // existing conflict item wins
          }
      }

      // incoming item wins - clear conflicts and replace existing with incoming.
      tryDelete(conflictingItems, incomingItem, existingItem);
  }

  function tryDelete(documents, incoming, existing) {
      if (documents.length > 0) {
          collection.deleteDocument(documents[0]._self, {}, function (err, responseOptions) {
              if (err) throw err;

              documents.shift();
              tryDelete(documents, incoming, existing);
          });
      } else if (existing) {
          collection.replaceDocument(existing._self, incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      } else {
          collection.createDocument(collection.getSelfLink(), incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      }
  }
}
```

### <a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>.NET SDK

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });

//Create the stored procedure
await clients[0].CreateStoredProcedureAsync(
UriFactory.CreateStoredProcedureUri(this.databaseName, this.udpCollectionName, "resolver"), new StoredProcedure
{
    Id = "resolver",
    Body = File.ReadAllText(@"resolver.js")
});
```

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-async"></a>Java Async SDK

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

Nadat de container is gemaakt, moet u de opgeslagen procedure `resolver` maken.

### <a id="create-custom-conflict-resolution-policy-stored-proc-java-sync"></a>Java Sync SDK

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```

Nadat de container is gemaakt, moet u de opgeslagen procedure `resolver` maken.

### <a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Nadat de container is gemaakt, moet u de opgeslagen procedure `resolver` maken.

### <a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>Python SDK

```python
udp_collection = {
  'id': self.udp_collection_name,
  'conflictResolutionPolicy': {
      'mode': 'Custom',
      'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
      }
  }
udp_collection = self.try_create_document_collection(create_client, database, udp_collection)
```

Nadat de container is gemaakt, moet u de opgeslagen procedure `resolver` maken.


## <a name="create-a-custom-conflict-resolution-policy"></a>Aangepast conflictoplossingsbeleid maken

Deze voorbeelden laten zien hoe u een container kunt instellen met aangepast conflictoplossingsbeleid. Deze conflicten worden weergegeven in de conflictfeed.

### <a id="create-custom-conflict-resolution-policy-dotnet"></a>.NET SDK

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

### <a id="create-custom-conflict-resolution-policy-java-async"></a>Java Async SDK

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a id="create-custom-conflict-resolution-policy-java-sync"></a>Java Sync SDK

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```

### <a id="create-custom-conflict-resolution-policy-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a id="create-custom-conflict-resolution-policy-python"></a>Python SDK

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
                    'id': self.manual_collection_name,
                    'conflictResolutionPolicy': {
                          'mode': 'Custom'
                        }
                    }
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="read-from-conflict-feed"></a>Lezen uit conflictfeed

Deze voorbeelden laten zien hoe u kunt lezen uit de conflictfeed van een container. Veroorzaakt een conflict weergegeven in het conflict alleen als ze zijn niet automatisch opgelost of als een beleid voor aangepaste feed.

### <a id="read-from-conflict-feed-dotnet"></a>.NET SDK

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

### <a id="read-from-conflict-feed-java-async"></a>Java Async SDK

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-java-sync"></a>Java Sync SDK

```java
Iterator<Conflict> conflictsIterator = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterator.hasNext()) {
    Conflict conflict = conflictsIterator.next();
    /* Do something with conflict */
}
```

### <a id="read-from-conflict-feed-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a id="read-from-conflict-feed-python"></a>Python

```python
conflicts_iterator = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterator, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de volgende Azure Cosmos DB-concepten:

* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
* [Meerdere masters in uw toepassingen configureren](how-to-multi-master.md)
* [Clients configureren voor multihoming](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Toevoegen of verwijderen van regio's van uw Azure Cosmos DB-account](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Meerdere masters configureren in uw toepassingen](how-to-multi-master.md).
* [Partitionering en gegevensdistributie](partition-data.md)
* [Indexering in Azure Cosmos DB](indexing-policies.md)