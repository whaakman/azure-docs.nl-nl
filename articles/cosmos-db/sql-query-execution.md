---
title: SQL-query kan worden uitgevoerd in Azure Cosmos DB
description: Meer informatie over de uitvoering van de SQL-query in Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: e4e26b658bd29e4589be40e4d29935059836c909
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342606"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>Uitvoeren van een Azure Cosmos DB SQL-query 's

Elke taal waarmee HTTP/HTTPS-aanvragen kan het Cosmos DB REST-API aanroepen. Cosmos DB biedt ook programmeringsbibliotheken geboden voor .NET, Node.js, JavaScript en Python programmeertalen. De REST-API en bibliotheken ondersteuning voor het uitvoeren van query's via SQL en de .NET SDK biedt ook ondersteuning voor [LINQ uitvoeren van query's](sql-query-linq-to-sql.md).

De volgende voorbeelden laten zien hoe u een query maakt en verzendt met een Cosmos DB-databaseaccount.

## <a id="REST-API"></a>REST API

Cosmos DB biedt een open RESTful-programmeermodel via HTTP. Het resourcemodel dat bestaat uit een set met resources onder een databaseaccount,, die een bepaalde Azure-abonnement. Account van de database bestaat uit een reeks *databases*, die elk meerdere mag *containers*, die op zijn beurt bevatten *items*, UDF's en andere resourcetypen. Elke Cosmos DB-resource is opgevraagd met de URI van een logische en stabiel. Een set met resources heet een *feed*. 

Het model basic interactie met deze resources is via het HTTP-termen `GET`, `PUT`, `POST`, en `DELETE`, met hun standaard interpretaties. Gebruik `POST` voor het maken van een nieuwe resource, een opgeslagen procedure uitvoeren of een Cosmos DB-query. Query's zijn altijd alleen-lezen bewerkingen zonder neveneffecten.

De volgende voorbeelden ziet u een `POST` voor een SQL-API-query op de voorbeelditems. De query is een eenvoudige filter op de JSON- `name` eigenschap. De `x-ms-documentdb-isquery` en Content-Type: `application/query+json` headers duiden dat de bewerking een query wordt. Vervang `mysqlapicosmosdb.documents.azure.com:443` met de URI voor uw Cosmos DB-account.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

De resultaten zijn:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

De volgende complexere query retourneert verschillende resultaten van een join:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": []
    }
```

De resultaten zijn: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Als de resultaten van een query niet op één pagina passen, de REST-API retourneert een vervolgtoken via de `x-ms-continuation-token` response-header. Clients kunnen resultaten pagineren door de header op te nemen in de volgende resultaten. U kunt ook bepalen het aantal resultaten per pagina via de `x-ms-max-item-count` getal-header.

Als een query een statistische functie, zoals het aantal bevat, kan de querypagina een gedeeltelijk geaggregeerde waarde via slechts één pagina met resultaten geretourneerd. Clients moeten een aggregatie op het tweede niveau uitvoeren via deze resultaten voor het produceren van de laatste resultaten. Bijvoorbeeld, som via de aantallen die worden geretourneerd in de afzonderlijke pagina's om de totale telling te retourneren.

Voor het beheren van het beleid van de consistentie van gegevens voor query's, gebruikt u de `x-ms-consistency-level` header in alle REST API-aanvragen. Sessieconsistentie is ook vereist de meest recente echo `x-ms-session-token` cookie-header in de query-aanvragen. Het indexeringsbeleid van de container waarop de query wordt uitgevoerd kan ook van invloed zijn op de consistentie van queryresultaten. Met de standaardbeleidsregels voor indexering beleidsinstellingen voor containers, de index is altijd actueel zijn met de inhoud van de items en queryresultaten overeenkomen met de consistentie gekozen voor gegevens. Voor meer informatie, Zie de [Azure Cosmos DB-consistentieniveaus] [consistentieniveaus].

Als de opgegeven query kan niet worden ondersteund door het geconfigureerde indexeringsbeleid voor de container, retourneert de Azure Cosmos DB-server 400 'Ongeldige aanvraag". Dit foutbericht wordt geretourneerd voor query's met paden expliciet is uitgesloten van het indexeren. U kunt opgeven de `x-ms-documentdb-query-enable-scan` header om toe te staan van de query voor het uitvoeren van een scan wanneer een index is niet beschikbaar.

U kunt gedetailleerde metrische gegevens krijgen bij uitvoeren van query's door in te stellen de `x-ms-documentdb-populatequerymetrics` koptekst `true`. Zie [Metrische gegevens van SQL-query's voor Azure Cosmos DB](sql-api-query-metrics.md) voor meer informatie.

## <a name="c-net-sdk"></a>C# (.NET SDK)

De .NET SDK biedt ondersteuning voor het uitvoeren van zowel SQL-query's als LINQ-query's. Het volgende voorbeeld ziet hoe u de voorgaande filterquery met .NET uitvoert:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Het volgende voorbeeld worden twee eigenschappen voor gelijkheid binnen elk item vergeleken en anonieme projecties gebruikt.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Het volgende voorbeeld toont joins, uitgedrukt via LINQ `SelectMany`.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

Alle pagina's van de resultaten van de query in de .NET-client automatisch doorloopt de `foreach` wordt geblokkeerd, zoals wordt weergegeven in het voorgaande voorbeeld. De opties voor query's die zijn geïntroduceerd in de [REST-API](#REST-API) sectie zijn ook beschikbaar in de .NET SDK, met behulp van de `FeedOptions` en `FeedResponse` klassen in de `CreateDocumentQuery` methode. U kunt het aantal pagina's beheren met behulp van de `MaxItemCount` instelling.

U kunt ook expliciet paginering beheren met het maken van `IDocumentQueryable` met behulp van de `IQueryable` object, klikt u vervolgens met het lezen van de `ResponseContinuationToken` waarden en geven ze weer als `RequestContinuationToken` in `FeedOptions`. U kunt instellen `EnableScanInQuery` scans inschakelen wanneer de query wordt niet ondersteund door het geconfigureerde beleid voor indexering. Voor gepartitioneerde containers, kunt u `PartitionKey` aan de query uitvoeren op een enkele partitie, hoewel Azure Cosmos DB dit automatisch uit de tekst van de query ophalen kunt. U kunt `EnableCrossPartitionQuery` query's uitvoeren op basis van meerdere partities.

Zie voor meer voorbeelden van .NET met query's, de [Azure Cosmos DB .NET-voorbeelden](https://github.com/Azure/azure-cosmosdb-dotnet) in GitHub.

## <a id="JavaScript-server-side-API"></a>JavaScript-API op de server

Azure Cosmos DB biedt een programmeermodel voor [JavaScript uitvoeren op basis van de toepassing](stored-procedures-triggers-udfs.md) logica rechtstreeks op containers, met behulp van opgeslagen procedures en triggers. De JavaScript-logica die is geregistreerd op het niveau van de container kan vervolgens databasebewerkingen uitvoeren op de items van de opgegeven container, verpakt in ambient ACID-transactions.

Het volgende voorbeeld ziet u hoe u `queryDocuments` op de server in JavaScript API voor het maken van query's uit binnen opgeslagen procedures en triggers:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [.NET-voorbeelden voor Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Azure Cosmos DB-consistentieniveaus](consistency-levels.md)
