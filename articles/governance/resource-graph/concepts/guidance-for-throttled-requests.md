---
title: Richtlijnen voor beperkte aanvragen
description: Informatie over het maken van betere query's om te voorkomen dat aanvragen tot Azure Resourcegrafiek van wordt beperkt.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: c644d230846d9c644c3845348431eef36c8279c8
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276897"
---
# <a name="guidance-for-throttled-requests-in-azure-resource-graph"></a>Richtlijnen voor beperkte aanvragen in Azure Resource Graph

Bij het maken van programmatische en vaak gebruik van Azure-Resource grafiekgegevens, moet rekening worden gemaakt voor hoe beperking gevolgen de resultaten van de query's. Wijzigen van de manier waarop gegevens wordt aangevraagd, kunt u en uw organisatie te voorkomen dat wordt beperkt en onderhouden van de stroom van actuele gegevens over uw Azure-resources.

Dit artikel vindt u vier gebieden en patronen met betrekking tot het maken van query's in Azure Resource Graph:

- Bandbreedtebeperking headers begrijpen
- Batchverwerking van query 's
- Spreiding query 's
- De impact van paginering

## <a name="understand-throttling-headers"></a>Bandbreedtebeperking headers begrijpen

Azure Resource Graph toegewezen quota-aantal voor elke gebruiker op basis van een bepaalde periode. Een gebruiker kan maximaal 15 query's, bijvoorbeeld verzenden in het venster van elke 5 seconden zonder wordt beperkt. De quotawaarde wordt bepaald door vele factoren en kan worden gewijzigd.

Azure Resource Graph Hiermee worden twee bandbreedtebeperking kopteksten toegevoegd in elke query-antwoord:

- `x-ms-user-quota-remaining` (int): Het resterende resourcequotum voor de gebruiker. Deze waarde wordt toegewezen aan het aantal query's.
- `x-ms-user-quota-resets-after` het uu: mm (:): De tijdsduur totdat het quotum-verbruik van een gebruiker opnieuw wordt ingesteld.

Ter illustratie van de werking van de headers, we kijken naar een queryantwoord dat de koptekst en de waarden van is `x-ms-user-quota-remaining: 10` en `x-ms-user-quota-resets-after: 00:00:03`.

- In de volgende 3 seconden mag maximaal 10 query's worden verzonden zonder wordt beperkt.
- In 3 seconden, de waarden van `x-ms-user-quota-remaining` en `x-ms-user-quota-resets-after` worden opnieuw ingesteld op `15` en `00:00:05` respectievelijk.

Een voorbeeld van het gebruik van de headers _uitstel_ op queryaanvragen, ziet u het voorbeeld in [query's parallel](#query-in-parallel).

## <a name="batching-queries"></a>Batchverwerking van query 's

Query's op het abonnement, resourcegroep of afzonderlijke resource batchverwerking is efficiënter dan het gebruik van query's. De kosten van het quotum van een grotere query is vaak minder is dan de kosten van het quotum van veel kleine en gerichte query's. De batchgrootte wordt aanbevolen moet minder dan _300_.

- Voorbeeld van een onvoldoende geoptimaliseerde benadering

  ```csharp
  // NOT RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  foreach (var subscriptionId in subscriptionIds)
  {
      var userQueryRequest = new QueryRequest(
          subscriptions: new[] { subscriptionId },
          query: "project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

  // ...
  }
  ```

- Voorbeeld #1 van een geoptimaliseerde batchverwerkingsindeling benadering

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  const int batchSize = 100;
  for (var i = 0; i <= subscriptionIds.Count / batchSize; ++i)
  {
      var currSubscriptionBatch = subscriptionIds.Skip(i * batchSize).Take(batchSize).ToList();
      var userQueryRequest = new QueryRequest(
          subscriptions: currSubscriptionBatch,
          query: "project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

- Voorbeeld #2 van een geoptimaliseerde batchverwerkingsindeling benadering

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var resourceIds = /* A big list of resourceIds */

  const int batchSize = 100;
  for (var i = 0; i <= resourceIds.Count / batchSize; ++i)
  {
      var resourceIdBatch = string.Join(",",
          resourceIds.Skip(i * batchSize).Take(batchSize).Select(id => string.Format("'{0}'", id)));
      var userQueryRequest = new QueryRequest(
          subscriptions: subscriptionList,
          query: $"where id in~ ({resourceIds}) | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

## <a name="staggering-queries"></a>Spreiding query 's

Vanwege de manier waarop beperking wordt afgedwongen, is het raadzaam query's worden gespreid. Dat wil zeggen, in plaats van 60 query's op hetzelfde moment verzendt, spreiding van de query's in vier 5 seconden windows:

- Query niet is gespreid plannen

  | Aantal query 's         | 60  | 0    | 0     | 0     |
  |---------------------|-----|------|-------|-------|
  | Tijdsinterval (sec) | 0-5 | 5-10 | 10-15 | 15-20 |

- Gespreid query plannen

  | Aantal query 's         | 15  | 15   | 15    | 15    |
  |---------------------|-----|------|-------|-------|
  | Tijdsinterval (sec) | 0-5 | 5-10 | 10-15 | 15-20 |

Hieronder volgt een voorbeeld van beperking headers respecteren bij het opvragen van Azure Resource Graph:

```csharp
while (/* Need to query more? */)
{
    var userQueryRequest = /* ... */
    // Send post request to Azure Resource Graph
    var azureOperationResponse = await this.resourceGraphClient
        .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
        .ConfigureAwait(false);

    var responseHeaders = azureOperationResponse.response.Headers;
    int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
    TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
    if (remainingQuota == 0)
    {
        // Need to wait until new quota is allocated
        await Task.Delay(resetAfter).ConfigureAwait(false);
    }
}
```

### <a name="query-in-parallel"></a>Query's parallel uitvoeren

Hoewel batchverwerking geniet de voorkeur boven parallellisering, zijn er tijden wanneer query's gemakkelijk kunnen niet worden verzameld. In dergelijke gevallen kunt u query's uitvoeren grafiek van de Azure-resources met meerdere query's verzenden in een parallelle manier. Hieronder volgt een voorbeeld van hoe u _uitstel_ op basis van de beperking van headers in dergelijke scenario's:

```csharp
IEnumerable<IEnumerable<string>> queryBatches = /* Batches of queries  */
// Run batches in parallel.
await Task.WhenAll(queryBatches.Select(ExecuteQueries)).ConfigureAwait(false);

async Task ExecuteQueries(IEnumerable<string> queries)
{
    foreach (var query in queries)
    {
        var userQueryRequest = new QueryRequest(
            subscriptions: subscriptionList,
            query: query);
        // Send post request to Azure Resource Graph.
        var azureOperationResponse = await this.resourceGraphClient
            .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
            .ConfigureAwait(false);
        
        var responseHeaders = azureOperationResponse.response.Headers;
        int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
        TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
        if (remainingQuota == 0)
        {
            // Delay by a random period to avoid bursting when the quota is reset.
            var delay = (new Random()).Next(1, 5) * resetAfter;
            await Task.Delay(delay).ConfigureAwait(false);
        }
    }
}
```

## <a name="pagination"></a>Paginering

Aangezien Azure Resource Graph maximaal 1000 items in een enkele query-antwoord retourneert, moet u mogelijk [pagineren](./work-with-data.md#paging-results) uw query's waarmee de volledige gegevensset die u zoekt. Echter, sommige Azure-Resource Graph-clients verwerken paginering anders dan andere.

- C# SDK

  Wanneer u ResourceGraph SDK gebruikt, moet u paginering door door te geven de skip-token wordt geretourneerd door de vorige queryantwoord op de volgende gepagineerde query verwerken. Dit ontwerp betekent dat u nodig hebt voor het verzamelen van resultaten van alle aanroepen van gepagineerde en combineer ze samen aan het einde. In dit geval wordt elke gepagineerde query die u verzendt een quotum van de query:

  ```csharp
  var results = new List<object>();
  var queryRequest = new QueryRequest(
      subscriptions: new[] { mySubscriptionId },
      query: "project id, name, type | top 5000");
  var azureOperationResponse = await this.resourceGraphClient
      .ResourcesWithHttpMessagesAsync(queryRequest, header)
      .ConfigureAwait(false);
  while (!string.Empty(azureOperationResponse.Body.SkipToken))
  {
      queryRequest.SkipToken = azureOperationResponse.Body.SkipToken;
      // Each post call to ResourceGraph consumes one query quota
      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(queryRequest, header)
          .ConfigureAwait(false);
      results.Add(azureOperationResponse.Body.Data.Rows);

      // Inspect throttling headers in query response and delay the next call if needed.
  }
  ```

- Azure CLI / Azure PowerShell

  Wanneer u Azure CLI of Azure PowerShell, worden query's naar Azure Resource Graph automatisch gepagineerde om op te halen van maximaal 5000 vermeldingen. Resultaten van de query retourneert een gecombineerde lijst met vermeldingen van alle gepagineerde aanroepen. In dit geval, afhankelijk van het aantal vermeldingen in het queryresultaat, één gepagineerde query mag worden gebruikt voor meer dan één query quotum. Bijvoorbeeld, in het volgende voorbeeld wordt een enkele uitvoeren van de query mag worden gebruikt voor maximaal vijf query quotum:

  ```azurecli-interactive
  az graph query -q 'project id, name, type' -top 5000
  ```

  ```azurepowershell-interactive
  Search-AzGraph -Query 'project id, name, type' -Top 5000
  ```

## <a name="still-get-throttled"></a>Nog steeds te maken met beperkingen?

Als u na te oefenen met de bovenstaande aanbevelingen ophalen beperkt bent, neem dan contact op met het team via [ resourcegraphsupport@microsoft.com ](mailto:resourcegraphsupport@microsoft.com).

Geef de volgende gegevens:

- Stuurprogramma voor de specifieke use case en zakelijke nodig zijn voor een hogere limiet voor bandbreedteregeling.
- Hoeveel bronnen hebt u toegang tot? Hoeveel van de worden geretourneerd door een eenvoudige query uitvoeren?
- Welke typen resources bent u geïnteresseerd?
- Wat is het patroon van uw query? X-query's per Y seconden enzovoort.

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md).
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md).
- Meer informatie over het [resources verkennen](explore-resources.md).