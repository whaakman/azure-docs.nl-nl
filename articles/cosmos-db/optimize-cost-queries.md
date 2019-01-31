---
title: Optimaliseren aanvraageenheden en de kosten voor het uitvoeren van query's in Azure Cosmos DB
description: Informatie over het evalueren van de kosten van de aanvraag unit voor een query en de query in termen van prestaties en kosten te optimaliseren.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: cb85d09a1d5dee6cb54254baac4698cdad093785
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457663"
---
# <a name="optimize-query-cost-in-azure-cosmos-db"></a>Kosten van de query in Azure Cosmos DB optimaliseren

Azure Cosmos DB biedt een uitgebreide set databasebewerkingen, waaronder relationele en hiërarchische query's die worden uitgevoerd op de items in een container. De kosten die gepaard gaan met elk van deze bewerkingen varieert op basis van de CPU, IO en geheugen die nodig is om de bewerking te voltooien. In plaats van nadenken over en beheren van hardware, kunt u een aanvraageenheid (RU) als één maateenheid voor de resources die nodig zijn om uit te voeren van de verschillende databasebewerkingen om van dienst van een aanvraag te zien. In dit artikel wordt beschreven hoe u aanvraag eenheid kosten voor een query evalueren en de query in termen van prestaties en kosten te optimaliseren. 

Query's in Azure Cosmos DB zijn doorgaans gerangschikt op de snelste/meest efficiënt te trager/minder efficiënte in termen van doorvoer als volgt:  

* GET-bewerking op een enkele partitiesleutel en Itemsleutel.

* Query's uitvoeren met een filterclausule op één partitiesleutel.

* Een query uitvoeren zonder een filtercomponent gelijkheid of het bereik op een eigenschap.

* Een query zonder filters.

Query's die gegevens uit een of meer partities lezen in rekening worden gebracht hogere latentie en hoe hoger de waarde van aanvraageenheden gebruiken. Aangezien elke partitie automatische indexering voor alle eigenschappen heeft, kan de query efficiënt worden uitgevoerd in de index. U kunt query's die gebruikmaken van meerdere partities sneller met behulp van de opties voor parallelle uitvoering. Zie voor meer informatie over partitionering en partitiesleutels [partitionering in Azure Cosmos DB](partitioning-overview.md).

## <a name="evaluate-request-unit-charge-for-a-query"></a>Kosten voor aanvraag-eenheden voor een query evalueren

Zodra u sommige gegevens in uw Azure Cosmos-containers hebt opgeslagen, kunt u de Data Explorer in Azure portal te maken en uw query's uitvoeren. U kunt ook de kosten van de query's ophalen met behulp van data explorer. Deze methode geeft u een idee van de werkelijke kosten in rekening gebracht bij normale query's en bewerkingen die ondersteuning biedt voor uw systeem.

U kunt ook de kosten van query's via een programma ophalen met behulp van de SDK's. Voor het meten van de overhead van elke bewerking zoals zoals het maken, bijwerken of verwijderen controleren de `x-ms-request-charge` header bij het gebruik van REST-API. Als u de .net of de Java SDK, de `RequestCharge` eigenschap is de eigenschap gelijk aan de aanvraag kosten in rekening gebracht en deze eigenschap is aanwezig in de ResourceResponse of FeedResponse.

```csharp
// Measure the performance (request units) of writes 
ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument); 

Console.WriteLine("Insert of an item consumed {0} request units", response.RequestCharge); 

// Measure the performance (request units) of queries 
IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery(); 

while (queryable.HasMoreResults) 
     { 
          FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
          Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge); 
     }
```

## <a name="factors-influencing-request-unit-charge-for-a-query"></a>Factoren die invloed op verzoek-eenheid in rekening gebracht voor een query

Aanvraageenheden voor query's zijn afhankelijk van een aantal factoren. Bijvoorbeeld, het aantal items van Azure Cosmos geladen/geretourneerd, het aantal lookups op basis van de index, de Querytijd compilatie details enzovoort. Azure Cosmos DB garandeert dat dezelfde query bij het op dezelfde gegevens altijd hetzelfde aantal aanvraageenheden zelfs met herhalingen uitvoeringen wordt verbruikt. De query-profiel met behulp van de query uitvoering van metrische gegevens biedt u een goed inzicht in hoe de basis van aanvraageenheden worden gebruikt.  

In sommige gevallen ziet u een reeks van 200 en 429 antwoorden en variabele aanvraageenheden in een resultatenpagina's uitvoering van query's, omdat de query's zo snel mogelijk op basis van de beschikbare ru's worden uitgevoerd. Mogelijk ziet u de uitvoering van een query op te splitsen in meerdere pagina's / retouren tussen server en client. Bijvoorbeeld kunnen 10.000 items worden geretourneerd als meerdere pagina's, elk in rekening gebracht op basis van de berekening uitgevoerd voor die pagina. Als u op deze pagina's telt, krijgt u moet hetzelfde aantal ru's, u voor de hele query krijgt.  

## <a name="metrics-for-troubleshooting"></a>Metrische gegevens voor het oplossen van problemen

De prestaties en de doorvoer die voornamelijk worden gebruikt door query's, gebruiker gedefinieerde functies (UDF's), is afhankelijk van de hoofdtekst van de functie. De eenvoudigste manier om erachter te komen hoeveel tijd van de uitvoering van de query in de UDF en het aantal ru's die worden gebruikt, is door in te schakelen van de Query metrische gegevens. Als u de .net SDK gebruikt, moet u dit voorbeeld query metrische gegevens die wordt geretourneerd door de SDK zijn:

```bash
Retrieved Document Count                 :               1              
Retrieved Document Size                  :           9,963 bytes        
Output Document Count                    :               1              
Output Document Size                     :          10,012 bytes        
Index Utilization                        :          100.00 %            
Total Query Execution Time               :            0.48 milliseconds 
  Query Preparation Times 
    Query Compilation Time               :            0.07 milliseconds 
    Logical Plan Build Time              :            0.03 milliseconds 
    Physical Plan Build Time             :            0.05 milliseconds 
    Query Optimization Time              :            0.00 milliseconds 
  Index Lookup Time                      :            0.06 milliseconds 
  Document Load Time                     :            0.03 milliseconds 
  Runtime Execution Times 
    Query Engine Execution Time          :            0.03 milliseconds 
    System Function Execution Time       :            0.00 milliseconds 
    User-defined Function Execution Time :            0.00 milliseconds 
  Document Write Time                    :            0.00 milliseconds 
  Client Side Metrics 
    Retry Count                          :               1              
    Request Charge                       :            3.19 RUs  
```

## <a name="best-practices-to-cost-optimize-queries"></a>Aanbevolen procedures om de kosten te optimaliseren van query 's 

Houd rekening met de volgende aanbevolen procedures bij het optimaliseren van query's voor kosten:

* **Meerdere Entiteitstypen plaatsen**

   Probeer meerdere Entiteitstypen binnen een enkele of kleinere aantal containers plaatsen. Deze methode voordelen niet alleen vanuit het oogpunt van de prijzen, maar ook voor het uitvoeren van query's en transacties. Query's zijn gericht op een enkele container; en atomische transacties via meerdere records via opgeslagen procedures/triggers worden afgestemd op een partitiesleutel binnen een enkele container. Entiteiten in dezelfde container plaatsen kunt verminderen het aantal netwerk retouren om op te lossen relaties voor records. Dus dit verhoogt de end-to-end-prestaties, kunt atomische transacties via meerdere records voor een grotere gegevensset en verlaagt de kosten als gevolg hiervan. Als meerdere Entiteitstypen binnen een enkele of kleinere aantal containers plaatsen moeilijk voor uw scenario, meestal omdat u een bestaande toepassing migreert en u niet wilt aanbrengen van codewijzigingen - u moet vervolgens kunt u inrichting doorvoer op het databaseniveau van de.  

* **Meet en af te stemmen voor lagere aanvraag aanvraageenheden/seconde gebruik**

   De complexiteit van een query heeft gevolgen voor het aantal aanvraageenheden (ru's) worden gebruikt voor een bewerking. Het aantal predikaten, aard van de predikaten, aantal UDF's en de grootte van de bron-gegevensset. Al deze factoren van invloed zijn op de kosten van querybewerkingen. 

   Aanvraag kosten in rekening gebracht die worden geretourneerd in de aanvraagheader geeft de kosten van een bepaalde query. Bijvoorbeeld, als een query 1000 1 KB-artikelen retourneert, is de kosten van de bewerking 1000. Binnen één seconde houdt de server daarom slechts twee dergelijke aanvragen voordat de volgende aanvragen beperken. Zie voor meer informatie, [aanvraageenheden](request-units.md) artikel en de aanvraag eenheid calculator. 

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [hoe Azure Cosmos prijzen](how-pricing-works.md)
* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [optimaliseren van de kosten van Azure Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)
* Meer informatie over [Azure Cosmos DB gereserveerde capaciteit](cosmos-db-reserved-capacity.md)

