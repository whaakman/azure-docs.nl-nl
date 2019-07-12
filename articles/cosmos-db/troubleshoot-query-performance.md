---
title: Problemen vaststellen en oplossen van problemen met query bij het gebruik van Azure Cosmos DB
description: Informatie over het identificeren, onderzoeken en oplossen van problemen met Azure Cosmos DB SQL-query.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 079e8677febfe6683d4f0e60a0e7ba6b06ea549d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835839"
---
# <a name="troubleshoot-query-performance-for-azure-cosmos-db"></a>Prestaties van query's voor Azure Cosmos DB oplossen
In dit artikel wordt uitgelegd hoe u identificeren, onderzoeken en oplossen van problemen met Azure Cosmos DB SQL-query. Als u wilt bereiken van optimale prestaties voor Azure Cosmos DB-query's, volgt u de onderstaande stappen. 

## <a name="collocate-clients-in-same-azure-region"></a>Clients in hetzelfde Azure-regio te plaatsen 
De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure Cosmos DB-eindpunt. Zie voor een lijst van beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/#services) artikel.

## <a name="check-consistency-level"></a>Consistentieniveau van de controleren
[Consistentieniveau](consistency-levels.md) kan invloed hebben op prestaties en kosten in rekening gebracht. Zorg ervoor dat uw consistentieniveau is geschikt voor het gegeven scenario. Zie voor meer informatie [Consistentieniveau kiezen](consistency-levels-choosing.md).

## <a name="log-query-metrics"></a>Meld u query metrische gegevens
Gebruik `QueryMetrics` om op te lossen trage of dure query's. 

  * Stel `FeedOptions.PopulateQueryMetrics = true` hebben `QueryMetrics` in het antwoord.
  * `QueryMetrics` de klasse heeft een overbelaste `.ToString()` -functie die kan worden aangeroepen om op te halen van de tekenreeksvoorstelling van `QueryMetrics`. 
  * De metrische gegevens kan worden gebruikt voor het afleiden van de volgende inzichten, onder andere: 
  
      * Een specifiek onderdeel van de querypijplijn duurde of abnormaal lang (in volgorde van honderden milliseconden of meer). 

          * Bekijk `TotalExecutionTime`.
          * Als de `TotalExecutionTime` van de query lager is dan de end-to-uitvoeringstijd en vervolgens de tijd wordt besteed in clientzijde of netwerk. Controleer dat de client en de Azure-regio worden samengevoegd.
      
      * (Als uitvoer documentaantal veel minder dan opgehaald documentaantal is) of er fout-positieven beschikbaar in de documenten zijn worden geanalyseerd.  

          * Bekijk `Index Utilization`.
          * `Index Utilization` = (Aantal geretourneerde documenten / aantal geladen van documenten)
          * Als het aantal geretourneerde documenten veel minder dan het aantal geladen is, worden klikt u vervolgens fout-positieven geanalyseerd.
          * Beperk het aantal documenten worden opgehaald met smaller filters.  

      * Hoe afzonderlijke retouren fared (Zie de `Partition Execution Timeline` van de tekenreeksvoorstelling van `QueryMetrics`). 
      * De query gebruikt of hoge aanvraag kosten in rekening gebracht. 

Zie voor meer informatie [over het verkrijgen van metrische gegevens voor uitvoering van SQL-query](profile-sql-api-query.md) artikel.
      
## <a name="tune-query-feed-options-parameters"></a>Feed opties queryparameters afstemmen 
Prestaties van query's kan worden afgestemd via van de aanvraag [Feed opties](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.feedoptions?view=azure-dotnet) Parameters. Probeer instelling van de onderstaande opties:

  * Stel `MaxDegreeOfParallelism` op-1 en vervolgens vergelijken prestaties via verschillende waarden. 
  * Stel `MaxBufferedItemCount` op-1 en vervolgens vergelijken prestaties via verschillende waarden. 
  * Stel `MaxItemCount` op-1.

Als prestaties van verschillende waarden te vergelijken, kunt u waarden zoals 2, 4, 8, 16 en anderen.
 
## <a name="read-all-results-from-continuations"></a>Alle resultaten van doorloopt lezen
Als u denkt dat u niet alle resultaten ontvangt dat, zorg ervoor dat de voortzetting volledig leegmaken. Met andere woorden, houd resultaten lezen terwijl het vervolgtoken heeft meer documenten te dragen aan.

Volledig verwerkingsstop kan worden bereikt met een van de volgende patronen:

  * Verwerking resultaten blijven terwijl voortzetting niet leeg zijn is.
  * Doorgaan met het verwerken terwijl query meer resultaten heeft. 

    ```csharp
    // using AsDocumentQuery you get access to whether or not the query HasMoreResults
    // If it does, just call ExecuteNextAsync until there are no more results
    // No need to supply a continuation token here as the server keeps track of progress
    var query = client.CreateDocumentQuery<Family>(collectionLink, options).AsDocumentQuery();
    while (query.HasMoreResults)
    {
        foreach (Family family in await query.ExecuteNextAsync())
        {
            families.Add(family);
        }
    }
    ```

## <a name="choose-system-functions-that-utilize-index"></a>Kies systeemfuncties die gebruikmaken van de index
Als de expressie kan worden vertaald naar een reeks tekenreekswaarden, kan klikt u vervolgens het gebruikmaken van de index; anders wordt deze niet. 

Hier volgt de lijst van de tekenreeksfuncties die de index kunnen gebruiken: 
    
  * STARTSWITH(str_expr, str_expr) 
  * LEFT(str_expr, num_expr) = str_expr 
  * SUBSTRING(str_expr, num_expr, num_expr) = str_expr, but only if first num_expr is 0 
    
    Hier volgen enkele voorbeelden: 
    
    ```sql

    -- If there is a range index on r.name, STARTSWITH will utilize the index while ENDSWITH won't 
    SELECT * 
    FROM c 
    WHERE STARTSWITH(c.name, 'J') AND ENDSWITH(c.name, 'n')

    ```
    
    ```sql

    -- LEFT will utilize the index while RIGHT won't 
    SELECT * 
    FROM c 
    WHERE LEFT(c.name, 2) = 'Jo' AND RIGHT(c.name, 2) = 'hn'

    ```

  * Voorkomen dat de systeemfuncties in het filter (of de component WHERE) die niet worden verwerkt door de index. Enkele voorbeelden van dergelijke systeemfuncties zijn bevat, hoofdletters, kleine.
  * Neem in query's indien mogelijk een filter op de partitiesleutel op.
  * Voor het bereiken van goed presterende query's te voorkomen dat het aanroepen van hoofdletters en kleine in het filter. In plaats daarvan normaliseren hoofdlettergebruik van de waarden bij het invoegen. Voeg de waarde met de gewenste hoofdlettergebruik voor elk van de waarden of zowel de oorspronkelijke waarde als de waarde met het gewenste hoofdlettergebruik invoegen. 

    Bijvoorbeeld:
    
    ```sql

    SELECT * FROM c WHERE UPPER(c.name) = "JOE"

    ```
    
    In dit geval, store "JAAP" in hoofdletters zijn of opslaan van zowel "JAAP" als "Jaap" de oorspronkelijke waarde. 
    
    Als het hoofdlettergebruik JSON-gegevens is genormaliseerd, de query wordt:
    
    ```sql

    SELECT * FROM c WHERE c.name = "JOE"

    ```

    De tweede query worden beter als er geen transformaties uitvoeren op elk van de waarden hoeven om de waarden vergelijken met "Jan".

Zie voor meer systeemfunctie details [systeemfuncties](sql-query-system-functions.md) artikel.

## <a name="check-indexing-policy"></a>Beleid voor indexering controleren
Om te controleren of de huidige [Indexeringsbeleid](index-policy.md) optimaal is:

  * Zorg ervoor dat alle JSON-paden die worden gebruikt in query's zijn opgenomen in het indexeringsbeleid voor snellere leesbewerkingen.
  * Paden niet gebruikt in query's voor meer goed werkende schrijfbewerkingen uitsluiten.

Zie voor meer informatie [hoe om te Indexeringsbeleid beheren](how-to-manage-indexing-policy.md) artikel.

## <a name="spatial-data-check-ordering-of-points"></a>Ruimtelijke gegevens: Controleren op volgorde van de punten
Punten in een Polygoon moeten worden opgegeven in volgorde van linksom draaien. Een veelhoek in rechtsom volgorde opgegeven vertegenwoordigt de omgekeerde waarde van de regio binnen het.

## <a name="optimize-join-expressions"></a>JOIN-expressies optimaliseren
`JOIN` expressies kunnen uitbreiden tot grote cross-producten. Wanneer mogelijk, query op een kleinere zoekopdracht ruimte via een meer gedetailleerde filter.

Subquery's met meerdere waarden kunnen optimaliseren `JOIN` expressies door te pushen predicaten na elke select-veel-expressie in plaats van nadat alle cross-joins in de `WHERE` component. Zie voor een gedetailleerd voorbeeld [Join expressies optimaliseren](https://docs.microsoft.com/azure/cosmos-db/sql-query-subquery#optimize-join-expressions) artikel.

## <a name="optimize-order-by-expressions"></a>ORDER BY-expressies optimaliseren 
`ORDER BY` prestaties van query's mogelijk te maken krijgen als de velden verspreide of niet opgenomen in de index-beleid zijn.

  * Voor sparse velden, zoals de tijd, verlaagt u de search-ruimte zo veel mogelijk met filters. 
  * Voor één eigenschap `ORDER BY`, eigenschap opnemen in de index-beleid. 
  * Voor meerdere eigenschap `ORDER BY` expressies definieert een [samengestelde index](https://docs.microsoft.com/azure/cosmos-db/index-policy#composite-indexes) op velden wordt gesorteerd.  

## <a name="many-large-documents-being-loaded-and-processed"></a>Veel grote documenten wordt geladen en verwerkt
De tijd en de RU's die door een query vereist zijn zijn niet alleen afhankelijk van de grootte van het antwoord, ze zijn ook afhankelijk van het werk dat wordt uitgevoerd door de query verwerkings-pipeline. Tijd en RUs verhogen evenredig met de hoeveelheid werk dat door de volledige query verwerkings-pipeline. Meer werk wordt uitgevoerd voor grote documenten, waardoor meer tijd en ru's zijn vereist om te laden en verwerken van grote documenten.

## <a name="low-provisioned-throughput"></a>Lage ingerichte doorvoer
Zorg ervoor dat de ingerichte doorvoer werkbelasting kan verwerken. RU-budget voor betrokken verzamelingen verhogen.

## <a name="try-upgrading-to-the-latest-sdk-version"></a>Voer een upgrade uit naar de nieuwste SDK-versie
Om te bepalen van de meest recente SDK-Zie [opmerkingen bij de SDK downloaden en vrijgeven](sql-api-sdk-dotnet.md) artikel.

## <a name="next-steps"></a>Volgende stappen
Verwijzen naar de volgende documenten voor het meten van ru's per query, Uitvoeringsstatistieken om af te stemmen uw query's en meer ophalen:

* [SQL-query tot uitvoering van metrische gegevens met behulp van .NET-SDK ophalen](profile-sql-api-query.md)
* [Queryprestaties afstemmen met Azure Cosmos DB](sql-api-sql-query-metrics.md)
* [Tips voor betere prestaties voor .NET SDK](performance-tips.md)