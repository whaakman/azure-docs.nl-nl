---
title: Gegevens in Azure Cosmos DB met time to live van verlopen | Microsoft Docs
description: TTL-waarde biedt Microsoft Azure Cosmos DB de mogelijkheid om documenten automatisch verwijderd uit het systeem na een bepaalde periode.
services: cosmos-db
keywords: Time to live van
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: sngun
ms.openlocfilehash: 2cae74224a9d59939175ac7e43d4d6b183ca3933
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050735"
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a>Gegevens in Azure Cosmos DB-verzamelingen met time to live van automatisch laten verlopen
Toepassingen kunnen maken en opslaan van grote hoeveelheden gegevens. Sommige van deze gegevens, zoals machine gegenereerde gegevens, logboeken en gebruiker gebeurtenissessie informatie is alleen nuttig voor een beperkte periode. Zodra de gegevens van de 1e overschot aan de behoeften van de toepassing, het is veilig om deze gegevens opschonen en de opslagbehoeften van een toepassing te verminderen.

Microsoft Azure Cosmos DB biedt 'time to live' of TTL-waarde, de mogelijkheid om documenten automatisch verwijderd uit de database na een bepaalde periode. Standaard time to live kan worden ingesteld op het niveau verzameling en op basis van per document wordt overschreven. Zodra TTL-waarde is ingesteld, de standaardwaarde voor een verzameling of op het documentniveau van een, wordt Cosmos DB-documenten die bestaan na die periode van de tijd in seconden, omdat ze het laatst zijn gewijzigd automatisch verwijderen.

Time to live in Azure Cosmos DB van maakt gebruik van een offset op basis van wanneer het document voor het laatst is gewijzigd. Hiervoor gebruikt de `_ts` veld, die op elk document bestaat. Het veld _ts is een unix-stijl-epoche tijdstempel voor de datum en tijd. De `_ts` veld wordt bijgewerkt telkens wanneer een document is gewijzigd. 

## <a name="ttl-behavior"></a>TTL-gedrag
De TTL-functie wordt bepaald door de eigenschappen op twee niveaus - het niveau van de verzameling en het niveau van de TTL-waarde. De waarden in een paar seconden worden ingesteld en worden behandeld als een verschillen van de `_ts` op dat het document voor het laatst is gewijzigd.

1. DefaultTTL voor de verzameling
   
   * Indien ontbrekend (of is ingesteld op null), documenten worden niet automatisch verwijderd.
   * Als aanwezig zijn en de waarde is ingesteld op "-"1 = oneindige – documenten verloopt niet standaard
   * Als het aanwezig zijn en de waarde is ingesteld op een nummer ("n"): documenten laten verlopen seconden "n" na de laatste wijziging
2. TTL-waarde voor de documenten: 
   
   * De eigenschap is alleen van toepassing als DefaultTTL aanwezig zijn voor de bovenliggende verzameling.
   * Onderdrukt de DefaultTTL-waarde voor de bovenliggende verzameling.

Als het document is verlopen (`ttl`  +  `_ts` < = huidige servertijd), het document is gemarkeerd als 'verlopen'. Er is geen bewerking is toegestaan op deze documenten na deze tijd en ze worden uitgesloten van de resultaten van alle query's uitgevoerd. De documenten fysiek in het systeem worden verwijderd en worden verwijderd op de achtergrond alleen op een later tijdstip. Dit verbruikt geen een [Aanvraageenheden (ru's)](request-units.md) van het budget van de verzameling.

De bovenstaande logica kan worden weergegeven in de volgende matrix:

|  | DefaultTTL ontbreekt of niet is ingesteld op de verzameling | DefaultTTL =-1 op verzameling | DefaultTTL = n' op verzameling |
| --- |:--- |:--- |:--- |
| TTL voor het document ontbreekt |Niets om op te heffen op het documentniveau van, omdat het document en de verzameling hebt geen concept van TTL-waarde. |Er zijn geen documenten in deze verzameling verloopt. |De documenten in deze verzameling zal verlopen wanneer interval n' is verstreken. |
| TTL =-1 op document |Er is niets om op te heffen op het documentniveau van het, omdat de verzameling bevat geen definitie van de eigenschap DefaultTTL die een document kunt onderdrukken. TTL voor een document is niet-geïnterpreteerde door het systeem. |Er zijn geen documenten in deze verzameling verloopt. |Het document met TTL =-1 in deze verzameling verloopt nooit. Alle andere documenten verloopt na n' interval. |
| TTL = n op document |Niets om op te heffen op het documentniveau van het. TTL voor een document is niet-geïnterpreteerde door het systeem. |Het document met TTL = n verloopt na n interval, in seconden. Andere documenten worden overgenomen van het interval van -1 en nooit verlopen. |Het document met TTL = n verloopt na n interval, in seconden. Andere documenten worden overgenomen door n' interval van de verzameling. |

## <a name="configuring-ttl"></a>TTL configureren
Standaard wordt de time to live van standaard in alle Cosmos DB-verzamelingen en op alle documenten uitgeschakeld. TTL-waarde kan worden ingesteld via programmacode of via de Azure-portal. Gebruik de volgende stappen uit om te configureren van TTL vanuit Azure-portal:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en navigeer naar uw Azure Cosmos DB-account.  

2. Ga aan de verzameling die u wilt de TTL-waarde instellen, opent u de **schaal en instellingen** deelvenster. U kunt zien dat de Time to Live standaard ingesteld is op **uit**. U kunt deze wijzigen **op (niet standaard)** of **op**.

   **uit** -documenten worden niet automatisch verwijderd.  
   **aan (niet standaard)** -deze optie stelt u de TTL-waarde op-"1" (oneindig) wat betekent dat documenten standaard verloopt niet.  
   **op** -documenten laten verlopen "n" seconden nadat het laatst is gewijzigd.  

   ![Time to live van instellen](./media/time-to-live/set-ttl-in-portal.png)

## <a name="enabling-ttl"></a>TTL inschakelen
Als u wilt TTL voor een verzameling of de documenten in een verzameling inschakelt, moet u de DefaultTTL-eigenschap van een verzameling instellen op 1 of een positief getal dan nul. Als de DefaultTTL op-1 betekent dat die door alle documenten in de verzameling zich altijd bevinden standaard, maar de Cosmos DB-service, moet deze verzameling documenten waarvoor deze standaardinstelling worden genegeerd controleren.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Standaard-TTL op een verzameling configureren
U zijn kunt de time to live op het niveau van een verzameling van een standaard configureren. Als u wilt de TTL-waarde instellen op een verzameling, moet u een niet-nul positief getal dat aangeeft van de periode, in seconden, alle documenten in de verzameling verlopen na de laatste wijziging timestamp van het document opgeven (`_ts`). Of u kunt de standaard ingesteld op -1, wat betekent dat alle documenten die zijn ingevoegd in de verzameling voor onbepaalde tijd standaard wordt live.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>TTL-instelling op een document
Naast het instellen van een standaard-TTL op een verzameling, kunt u specifieke TTL instellen op het documentniveau van een. Dit doet, wordt de standaardwaarde van de verzameling overschreven.

* Om in te stellen de TTL-waarde voor een document, moet u een positief getal dan nul, waarmee wordt aangegeven van de periode, in seconden voor het verlopen van het document na de laatste wijziging timestamp van het document opgeven (`_ts`).
* Als een document geen TTL-veld is, wordt de standaardwaarde van de verzameling van toepassing zijn.
* Als de TTL is uitgeschakeld op het niveau verzameling, wordt de TTL-veld in het document wordt genegeerd totdat de TTL opnieuw is ingeschakeld op de verzameling.

Hier volgt een codefragment waarin wordt getoond hoe de TTL is verlopen op een document instellen:

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a>TTL-waarde op een bestaand document uitbreiden
U kunt de TTL voor een document opnieuw aan de hand van een schrijfbewerking voor het document. Hiermee wordt ingesteld als dit de `_ts` aan de huidige tijd en de aftelling tot de vervaldatum document, zoals ingesteld door de `ttl`, opnieuw gestart met. Als u wilt wijzigen de `ttl` van een document, kunt u het veld bijwerken zoals u met elk ander instelbare veld doen kunt.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="removing-ttl-from-a-document"></a>TTL-waarde verwijderen van een document
Als een TTL-waarde is ingesteld op een document en u niet langer dat document laten verlopen, kunt klikt u vervolgens u het document ophalen, verwijderen van de TTL-veld en vervangt het document op de server. Wanneer de TTL-veld van het document wordt verwijderd, wordt de standaardwaarde van de verzameling worden toegepast. Als u wilt stoppen van een document verlopen en niet overgenomen van de verzameling moet u de TTL-waarde ingesteld op-1.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="disabling-ttl"></a>TTL uitschakelen
Als u wilt uitschakelen TTL volledig op een verzameling en stop het achtergrondproces van verlopen documenten zoeken naar de eigenschap DefaultTTL op de verzameling moeten worden verwijderd. Als u deze eigenschap wijkt af van het instelt op-1. Instelling van 1 betekent dat nieuwe documenten die worden toegevoegd aan de verzameling bevinden zich altijd, maar u kunt dit opheffen in specifieke documenten in de verzameling. Deze eigenschap volledig uit de verzameling verwijderen betekent dat er geen documenten verloopt, zelfs als er documenten waarvoor een eerdere standaard expliciet worden genegeerd.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);

<a id="ttl-and-index-interaction"></a> 
## <a name="ttl-and-index-interaction"></a>Tussenkomst van de TTL-waarde en index
De onderliggende index toevoegen of wijzigen van de TTL-instelling op een verzameling wordt gewijzigd. Als de TTL-waarde wordt gewijzigd van uitgeschakeld bij, wordt de verzameling opnieuw geïndexeerd. Als u wijzigingen aanbrengt aan het indexeringsbeleid wanneer de modus voor indexering consistent is, wordt gebruikers niet ziet u een wijziging in de index. Wanneer de indexing-modus is ingesteld op vertraagde, is altijd afvangen van van de index en als de TTL-waarde is gewijzigd, de index helemaal wordt opnieuw gemaakt. Wanneer de TTL-waarde wordt gewijzigd en de Indexmodus is ingesteld op vertraagde, retourneren query's uitgevoerd tijdens de indexen niet voltooid of de juiste resultaten.

Als u moet de exacte gegevens die worden geretourneerd, veranderen niet de TTL-waarde wanneer de indexing-modus is ingesteld op vertraagde. Consistente index moet in het ideale geval worden gekozen om consistente resultaten te behalen. 

## <a name="faq"></a>Veelgestelde vragen
**Wat TTL kost mij?**

Er is geen extra kosten aan het instellen van een TTL-waarde op een document.

**Hoe lang duurt het voordat mijn document verwijderen wanneer de TTL is?**

De documenten zijn verlopen onmiddellijk nadat de TTL opgestart is, en zijn niet meer toegankelijk zijn via CRUD- of -query's. 

**TTL voor een document, heeft invloed op RU kosten in rekening gebracht?**

Nee, er zijn geen invloed op de RU-kosten voor verwijderingen van verlopen documenten via TTL in Cosmos DB.

**Is de TTL-functie alleen van toepassing op hele documenten of kan ik afzonderlijke document eigenschapswaarden verlopen?**

TTL-waarde is van toepassing op het hele document. Als u wilt laten verlopen slechts een deel van een document, klikt u vervolgens het verdient aanbeveling dat u het gedeelte uit het document in een afzonderlijke "gekoppelde" document extraheren en TTL-waarde vervolgens voor het desbetreffende uitgepakte document gebruiken.

**Beschikt over de functie TTL specifieke indexering vereisten?**

Ja. De verzameling moet hebben [indexeren beleidsset](indexing-policies.md) consistente of Lazy. Het instellen van DefaultTTL op een verzameling met indexeren is ingesteld op None leidt tot een fout op als wilt uitschakelen indexering voor een verzameling met een DefaultTTL al ingesteld.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Azure Cosmos DB verwijzen naar de service [ *documentatie* ](https://azure.microsoft.com/documentation/services/cosmos-db/) pagina.

