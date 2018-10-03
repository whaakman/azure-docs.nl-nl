---
title: Meerdere masters conflictoplossing in Azure Cosmos DB
description: In dit artikel beschrijft de categorieën conflict en het conflict resolutiemodi zoals laatste schrijver Wins (LWW), aangepaste – Procedure door de gebruiker gedefinieerde, aangepaste – asynchroon in Azure Comsos DB meerdere masters.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 39fd393e78a2b66749c6aa34a758b185b38effdf
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041184"
---
# <a name="multi-master-conflict-resolution-in-azure-cosmos-db"></a>Meerdere masters conflictoplossing in Azure Cosmos DB 

Meerdere masters van Azure Cosmos DB biedt globale conflict resolutiemodi om ervoor te zorgen gegevens is consistent in alle regio's waar deze worden gerepliceerd.

## <a name="conflict-categories"></a>Conflict categorieën

Er zijn drie categorieën van conflicten die optreden kunnen bij het werken met Azure Cosmos DB-gegevens:

* **Invoegen van conflicten:** dit type conflict gebeurt er wanneer de toepassing Hiermee voegt u twee of meer documenten met dezelfde unieke index (bijvoorbeeld ID) van twee of meer regio's tegelijkertijd. In dit geval alle schrijfbewerkingen in eerste instantie mogelijk wel, maar op basis van het conflict resolutie beleid dat u kiest, slechts één document met de oorspronkelijke ID is doorgevoerd.

* **Vervang conflicten:** dit type conflict gebeurt er wanneer de toepassing één document tegelijk uit twee of meer regio's bijgewerkt.

* **Verwijderen van conflicten:** dit type conflict gebeurt er wanneer de toepassing wordt verwijderd van een document van de ene regio en wordt deze vanaf een of meer regio's bijgewerkt. 

## <a name="conflict-resolution-modes"></a>Conflict resolutiemodi

Er zijn drie conflict resolutiemodi die worden aangeboden door Azure Cosmos DB. Conflict resolutiemodi zijn gedefinieerd voor elke verzameling.

> [!NOTE]
> SQL-API-gebruikers kunnen kiezen uit drie verschillende conflict resolutiemodi. Beleidsconflicten worden opgelost met behulp van laatste schrijver Wins voor alle andere API-modellen (MongoDB, Cassandra, grafiek en tabel).

### <a name="last-writer-wins-lww"></a>Laatste schrijver Wins (LWW)

Laatste schrijver Wins is de standaardmodus voor de oplossing van conflicten. In deze modus conflicten worden opgelost op basis van een numerieke waarde die wordt doorgegeven in een eigenschap van het document.

Het volgende codefragment wordt een voorbeeld van het laatste schrijver Wins conflict resolutie beleid configureren bij gebruik van de .net SDK. De "ConflictResolutionPath" Hiermee wordt het pad naar de eigenschap die wordt gebruikt voor het conflict oplossen. In dit voorbeeld `/userDefinedId` is het pad van de oplossing conflict en het document met de allergrootste `userDefinedId` waarde win altijd het conflict. Voor het registreren van een modus voor het oplossen van laatste schrijver Wins, richt u de verzameling met de ConflictResolutionPolicy zoals hieronder wordt weergegeven.

```csharp
DocumentCollection lwwCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
   Id = "myCollection", 
   ConflictResolutionPolicy = new ConflictResolutionPolicy
   {
      Mode = ConflictResolutionMode.LastWriterWins,
      ConflictResolutionPath = "/userDefinedId"
   }
});
```

 Modus voor het oplossen van de laatste schrijver Wins wordt toegepast op verschillende conflict categorieën als volgt:

* **Invoeg- en conflicten:** als twee of meer documenten in conflict zijn bij het invoegen of vervangen, het document met de grootste waarde voor het pad van de oplossing conflict wordt de winnaar (dat wil zeggen, userDefinedId). Als meerdere documenten dezelfde numerieke waarde voor het pad van de oplossing conflict hebben, is de geselecteerde winnaar niet-deterministisch. Alle regio's wordt echter geconvergeerd toe aan een enkele winnaar.

* **Conflict verwijderen:** als er conflicten verwijderen die betrokken zijn, het verwijderen altijd wins boven andere conflicten vervangen, ongeacht de waarde van het conflict resolutie-pad.

### <a name="custom--user-defined-procedure"></a>Aangepaste: gebruiker gedefinieerde Procedure

In deze modus conflict de gebruikersbesturingselementen oplossen door het registreren van een gebruiker gedefinieerde Procedure (UDP) aan de verzameling. Deze UDP heeft een specifieke handtekening. Als u deze optie selecteert, maar niet registreren van een UDP, of als het UDP-protocol een uitzondering genereert tijdens runtime, worden de conflicten geschreven naar de conflicten feed waar ze afzonderlijk kunnen worden opgelost.

Voor het registreren van een aangepaste – de gebruiker gedefinieerde procedure-modus voor het oplossen van conflict, richt u de verzameling met de ConflictResolutionPolicy zoals hieronder wordt weergegeven.

```csharp
DocumentCollection udpCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
  Id = "myCollection",
  ConflictResolutionPolicy = new ConflictResolutionPolicy
  {
     Mode = ConflictResolutionMode.Custom,
     ConflictResolutionProcedure = UriFactory.CreateStoredProcedureUri("myDatabase","myCollection","myUdpStoredProcedure").ToString()
  }
});
```

Voeg de gebruiker gedefinieerde procedure vervolgens toe aan de verzameling, zoals hieronder weergegeven.

```csharp
StoredProcedure myUdpStoredProc = new StoredProcedure
{
   Id = "myUdpStoredProcedure",
   Body = myUdpStoredProcText
};
await myClient.UpsertStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("myDatabase", "myCollection"), myUdpStoredProc);
```

De opgeslagen procedure die is geregistreerd bij de verzameling heeft een speciale handtekening. In dit voorbeeld hieronder het UDP-protocol maakt gebruik van een eigenschap `UserDefinedId` conflicten op te lossen. Het document met de grootste waarde van het conflict wins.

```javascript
function myUdpStoredProcedure(incomingDocument, existingDocument, isDeleteConflict, conflictingDocuments){
    var collection = getContext().getCollection();

    if (!incomingDocument) {
        if (existingDocument) {

            collection.deleteDocument(existingDocument._self, {}, function(err, responseOptions) {
                if (err) throw err;
            });
        }
    } else if (isDeleteConflict) {
        // delete always wins.
    } else {
        var documentToUse = incomingDocument;

        if (existingDocument) {
            if (documentToUse.userDefinedId < existingDocument.userDefinedId) {
                documentToUse = existingDocument;
            }
        }

        var i;
        for (i = 0; i < conflictingDocuments.length; i++) {
            if (documentToUse.userDefinedId < conflictingDocuments[i].userDefinedId) {
                documentToUse = conflictingDocuments[i];
            }
        }

        tryDelete(conflictingDocuments, incomingDocument, existingDocument, documentToUse);
    }

    function tryDelete(documents, incoming, existing, documentToInsert) {
        if (documents.length > 0) {
            collection.deleteDocument(documents[0]._self, {}, function(err, responseOptions) {
                if (err) throw err;

                documents.shift();
                tryDelete(documents, incoming, existing, documentToInsert);
            });
        } else if (existing) {
            collection.replaceDocument(existing._self, documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        } else {
            collection.createDocument(collection.getSelfLink(), documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        }
    }
}

```

De procedure heeft vier parameters:

* **incomingDocument:** geeft u de conflicterende versie van het document. Het conflict is een insert-, vervangen of een delete-conflict. Voor een delete-conflict is dit een delete-installatiekopie (tombstone) met geen inhoud.

* **existingDocument:** Hiermee geeft u de doorgevoerde versie van het document, dat dezelfde "verwijderen" waarde als de incomingDocument. Deze parameter is ingesteld op null voor een invoeg- en conflict verwijderen.

* **isDeleteConflict:** Booleaanse vlag waarmee wordt aangegeven als het binnenkomende document is in conflict met een eerder verwijderde document. Als deze parameter is ingesteld op true, existingDocument wordt ook niet null zijn.

* **conflictingDocuments:** Hiermee geeft u een verzameling van de toegezegde versie van alle documenten in de database, die conflicteren met incomingDocument op kolom-ID of een andere unieke index-velden. Deze documenten worden verschillende 'verwijderen'-waarde in vergelijking met de incomingDocument hebben.

De gebruiker gedefinieerde procedure heeft volledige toegang tot de partitiesleutel Cosmos DB en store bewerkingen voor het oplossen van conflicten kan uitvoeren. Als de gebruiker gedefinieerde procedure biedt de versie van het conflict niet worden doorgevoerd, het systeem wordt het conflict verwijderen en de existingDocument blijven doorgevoerd. Als de gebruiker gedefinieerde procedure mislukt of niet bestaat, Azure Cosmos DB wordt toe te voegen het conflict in de alleen-lezen conflicten feed waar de gegevens kan worden verwerkt asynchroon zoals wordt weergegeven in de [modus voor het oplossen van asynchrone conflict](#custom--asynchronous). 

### <a name="custom--asynchronous"></a>Aangepast – asynchrone  

In deze modus, Azure Cosmos DB uitgesloten voor alle conflicten (invoegen, vervangen en verwijderen) wordt doorgevoerd en registreert ze in de alleen-lezen conflicten feed voor het omzetten van de uitgestelde door toepassing van de gebruiker. De toepassing kunt conflictoplossing asynchroon uitvoeren en gebruik een logica of verwijzen naar een externe bron, toepassing of service het conflict op te lossen.

Voor het registreren van een aangepaste-modus voor het oplossen van asynchrone conflict, richt u de verzameling met de ConflictResolutionPolicy zoals hieronder wordt weergegeven.

```csharp
DocumentCollection manualCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
    Id = "myCollection",
    ConflictResolutionPolicy = new ConflictResolutionPolicy
    {
        Mode = ConflictResolutionMode.Custom
    }
});
```

Om te lezen en verwerken van eventuele conflicten in de feed conflicten, implementeert u de code hieronder wordt weergegeven. Gegevens die zijn opgeslagen in de feed conflicten voegt enige opslagkosten toe. Dus is het raadzaam om de gegevens die zijn opgeslagen in de feed nadat ze zijn verwerkt veroorzaakt een conflict te verwijderen.

```csharp
FeedResponse<Conflict> response = await myClient.ReadConflictFeedAsync(myCollectionUri);
  foreach (Conflict conflict in response)
  {
      switch(conflict.OperationKind)
      {
         case OperationKind.Create:
         //Process Insert Conflicts.
         …
         case OperationKind.Replace:
         //Process Replace Conflicts
         …
         case OperationKind.Delete:
         //Process Delete Conflicts
         …
      }
  //Optionally delete the conflict after processed
  await myClient.DeleteConflictAsync(conflict.SelfLink);
  }
```

> [!NOTE]
> De feed conflicten bevat geen een listener voor het verzenden van meldingen voor de downstreamverwerking, zoals het in Cosmos DB-wijzigingenfeed. U hebt voor het implementeren van de logica voor het pollen van de feed conflicten en te bepalen of conflicten aanwezig zijn.

## <a name="code-samples"></a>Codevoorbeelden

Hieronder vindt u voorbeeldtoepassingen die laten zien van conflictoplossing voor de API's die worden vermeld. Elk voorbeeld conflicten in een container en vervolgens ziet u hoe beleidsconflicten worden opgelost voor elke ondersteunde conflict resolutie-modus.

|API-model  | SDK |Voorbeeld |
|---------|---------|---------|
|SQL-API    | .NET    |[Azure-cosmos-DB-SQL-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-dotnet-multi-master)  |
|SQL-API    | Knooppunt    |[Azure-cosmos-js/samples/MultiRegionWrite /](https://github.com/Azure/azure-cosmos-js/tree/master/samples/MultiRegionWrite)  |
|SQL-API    | Java    |[Azure-cosmos-DB-SQL-Java-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-java-multi-master)  |
|MongoDB  | .NET    |[Azure-cosmos-DB-mongodb-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-multi-master)   |
|Tabel-API  | .NET    |[Azure-cosmos-DB-Table-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-multi-master)       |
|Gremlin-API | .NET | [Azure-cosmos-DB-gremlin-dontnet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-gremlin-dontnet-multi-master)|

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over conflict categorieën en conflict resolutiemodi voor Azure Cosmos DB. Haal vervolgens een overzicht van de volgende bronnen:

* [MongoDB-clients gebruiken met meerdere masters](multi-master-oss-nosql.md)
