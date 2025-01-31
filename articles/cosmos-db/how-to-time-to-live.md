---
title: Meer informatie over het configureren en beheren van Time to Live in Azure Cosmos DB
description: Meer informatie over het configureren en beheren van Time to Live in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 618e7e19b20f361aa0a8c668e9621a29db43772d
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797742"
---
# <a name="configure-time-to-live-in-azure-cosmos-db"></a>Time to Live configureren in Azure Cosmos DB

In Azure Cosmos DB kunt u Time to Live (TTL) configureren op containerniveau, maar u kunt het ook overschrijven op een itemniveau nadat u het voor de container hebt ingesteld. U kunt TTL voor een container configureren via de Azure-portal of de taalspecifieke SDK's. Overschrijvingen van TTL op itemniveau kunnen worden geconfigureerd met behulp van de SDK's.

## <a name="enable-time-to-live-on-a-container-using-azure-portal"></a>Time to Live inschakelen op een container via de Azure-portal

Gebruik de volgende stappen om Time to Live in te schakelen op een container zonder vervaldatum. Schakel deze optie in om TTL te overschrijven op itemniveau. U kunt de TTL ook instellen door een waarde (in seconden) ongelijk aan nul in te voeren.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Maak een nieuw Azure Cosmos DB-account of selecteer een bestaand account.

3. Open het deelvenster **Data Explorer**.

4. Selecteer een bestaande container, vouw deze uit en wijzig de volgende waarden:

   * Open het venster **Schaal en instellingen**.
   * Onder **Instelling** zoekt u **Time to Live**.
   * Selecteer **On (no default)** of **Aan** en stel een TTL-waarde in
   * Klik op **Opslaan** om de wijzigingen op te slaan.

   ![Time to Live configureren in de Azure-portal](./media/how-to-time-to-live/how-to-time-to-live-portal.png)


- Als DefaultTimeToLive null is vervolgens is uw Time to Live uitgeschakeld
- Wanneer DefaultTimeToLive wordt -1 en vervolgens de Time-to Live-instelling is ingeschakeld (geen standaard)
- Wanneer DefaultTimeToLive een andere Int-waarde (met uitzondering van 0 heeft) is de Time-to Live-instelling ingeschakeld

## <a name="enable-time-to-live-on-a-container-using-sdk"></a>Time to Live inschakelen op een container met behulp van de SDK

### <a id="dotnet-enable-noexpiry"></a>.NET SDK

```csharp
// Create a new collection with TTL enabled and without any expiration value
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = -1; //(never expire by default)

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

## <a name="set-time-to-live-on-a-container-using-sdk"></a>Time to Live instellen op een container met behulp van de SDK

### <a id="dotnet-enable-withexpiry"></a>.NET SDK

Als u de Time to Live voor een container wilt instellen, dient u een positief getal (geen nul) op te geven die de tijd in seconden aangeeft. Op basis van de geconfigureerde TTL-waarde worden alle items in de container verwijderd na het laatste gewijzigde tijdstempel `_ts` van het item.

```csharp
// Create a new collection with TTL enabled and a 90 day expiration
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

### <a id="nodejs-enable-withexpiry"></a>NodeJS-SDK

```javascript
const containerDefinition = {
          id: "sample container1",
        };

async function createcontainerWithTTL(db: Database, containerDefinition: ContainerDefinition, collId: any, defaultTtl: number) {
      containerDefinition.id = collId;
      containerDefinition.defaultTtl = defaultTtl;
      await db.containers.create(containerDefinition);
}
```

## <a name="set-time-to-live-on-an-item"></a>Geldigheidsduur voor een item instellen

U kunt niet alleen de standaardwaarde voor Time to Live voor een container instellen, maar ook voor een item. Als u Time to Live op itemniveau instelt, wordt de standaard-TL van het item in die container overschreven.

* Als u de TTL voor een item instelt, dient u een dient u een positief getal (geen nul) op te geven die de tijd in seconden aangeeft. Hiermee wordt aangegeven dat het item vervalt na het laatste gewijzigde tijdstempel `_ts` van het item.

* Als het item geen TTL-veld heeft, wordt standaard de TTL-waarde die voor de container is ingesteld, op het item toegepast.

* Als TTL wordt uitgeschakeld op containerniveau, wordt het TTL-veld van het item genegeerd totdat TTL opnieuw voor de container wordt ingeschakeld.

### <a id="portal-set-ttl-item"></a>Azure-portal

Gebruik de volgende stappen uit om in te schakelen time to live op een item van:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Maak een nieuw Azure Cosmos DB-account of selecteer een bestaand account.

3. Open het deelvenster **Data Explorer**.

4. Selecteer een bestaande container, vouw deze uit en wijzig de volgende waarden:

   * Open het venster **Schaal en instellingen**.
   * Onder **Instelling** zoekt u **Time to Live**.
   * Selecteer **op (niet standaard)** of selecteer **op** en stel een TTL-waarde. 
   * Klik op **Opslaan** om de wijzigingen op te slaan.

5. Vervolgens gaat u naar het item waarvoor u wenst te insteltijd voor de live, voegt u de `ttl` eigenschap en selecteer **Update**. 

   ```json
   {
    "id": "1",
    "_rid": "Jic9ANWdO-EFAAAAAAAAAA==",
    "_self": "dbs/Jic9AA==/colls/Jic9ANWdO-E=/docs/Jic9ANWdO-EFAAAAAAAAAA==/",
    "_etag": "\"0d00b23f-0000-0000-0000-5c7712e80000\"",
    "_attachments": "attachments/",
    "ttl": 10,
    "_ts": 1551307496
   }
   ```

### <a id="dotnet-set-ttl-item"></a>.NET SDK

```csharp
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName="cid")]
    public string CustomerId { get; set; }
    // used to set expiration policy
    [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
    public int? ttl { get; set; }

    //...
}
// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder
{
    Id = "SO05",
    CustomerId = "CO18009186470",
    ttl = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days
};
```

### <a id="nodejs-set-ttl-item"></a>NodeJS-SDK

```javascript
const itemDefinition = {
          id: "doc",
          name: "sample Item",
          key: "value", 
          ttl: 2
        };
```


## <a name="reset-time-to-live"></a>Time to Live opnieuw instellen

U kunt Time to Live opnieuw voor een item instellen door een schrijf- of bijwerkbewerking voor het item uit te voeren. Met de schrijf- of bijwerkbewerking wordt `_ts` ingesteld op de huidige tijd en de TTL voor dat item begint van voren af aan. Als u de TTL van een item wilt wijzigen, kunt u het veld op dezelfde manier bijwerken als elk ander veld.

### <a id="dotnet-extend-ttl-item"></a>.NET SDK

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = 60 * 30 * 30; // update time to live
response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="turn-off-time-to-live"></a>Time to Live uitzetten

Als Time to Live voor een item is ingesteld en u wilt dat item niet langer laten verlopen, dan haalt u dat item op, verwijdert u het TTL-veld en vervangt u het item op de server. Als het TTL-veld van het item wordt verwijderd, wordt de standaardwaarde van de TTL die aan de container is toegewezen, op het item toegepast. Stel de TTL-waarde in op -1 om te voorkomen dat een item verloopt en dat de TTL-waarde van de container wordt overgenomen.

### <a id="dotnet-turn-off-ttl-item"></a>.NET SDK

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.ttl = null; // inherit the default TTL of the collection

response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="disable-time-to-live"></a>Time to Live uitschakelen

Als u Time to Live voor een container wilt uitschakelen en het achtergrondproces dat op verlopen items controleert, wilt stoppen, moet eigenschap `DefaultTimeToLive` op de container worden verwijderd. Het verwijderen van deze eigenschap is iets anders dan het instellen ervan op -1. Als u de eigenschap op -1 instelt, zullen nieuwe items die aan de container worden toegevoegd, nooit verlopen. U kunt deze waarde echter voor bepaalde items in de container overschrijven. Wanneer u de TTL-eigenschap verwijderd uit de container verloopt de items nooit, zelfs als er zijn dat ze expliciet de vorige standaard TTL-waarde zijn overschreven.

### <a id="dotnet-disable-ttl"></a>.NET SDK

```csharp
// Get the collection, update DefaultTimeToLive to null
DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
// Disable TTL
collection.DefaultTimeToLive = null;
await client.ReplaceDocumentCollectionAsync(collection);
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Time to Live vindt u in het volgende artikel:

* [Time to live](time-to-live.md)
