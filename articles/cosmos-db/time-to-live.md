---
title: Verlopen van gegevens in Azure Cosmos DB met levensduur | Microsoft Docs
description: Microsoft Azure Cosmos DB biedt TTL, de mogelijkheid om documenten automatisch verwijderd uit het systeem na een bepaalde periode.
services: cosmos-db
documentationcenter: 
keywords: TTL
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: arramac
ms.openlocfilehash: 3737a240d92d9420bac7d42475622182fb425a2b
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a>Gegevens in Azure Cosmos DB verzamelingen automatisch met TTL verloopt
Toepassingen kunnen maken en opslaan van de enorme hoeveelheden gegevens. Sommige van deze gegevens, zoals machine gegenereerde gegevens, logboeken en gebruiker gebeurtenissessie informatie is alleen nuttig voor een beperkte periode. Zodra de gegevens wordt overtollige aan de behoeften van de toepassing het is veilig om deze gegevens verwijderen en de opslagbehoeften van een toepassing te verminderen.

Microsoft Azure Cosmos DB biedt 'time to live' of de TTL, de mogelijkheid om documenten automatisch verwijderd uit de database na een bepaalde periode. Standaard time to live worden ingesteld op het niveau verzameling, en op basis van per document wordt genegeerd. Zodra TTL is ingesteld, de standaardwaarde voor een verzameling of op het documentniveau van een, wordt Cosmos DB documenten die bestaan na die periode, in seconden, omdat ze het laatst zijn gewijzigd automatisch verwijderd.

TTL in Cosmos-database maakt gebruik van een offset tegen wanneer het document voor het laatst is gewijzigd. Hiervoor wordt de `_ts` veld dat op elk document bestaat. Het veld _ts is een tijdstempel voor een unix-stijl-epoche die de datum en tijd. De `_ts` veld wordt telkens bijgewerkt wanneer een document wordt gewijzigd. 

## <a name="ttl-behavior"></a>TTL gedrag
De TTL-functie wordt bepaald door de eigenschappen op twee niveaus - het niveau verzameling en het niveau van de TTL-waarde. De waarden worden ingesteld in seconden, en worden behandeld als een delta van de `_ts` op dat het document voor het laatst is gewijzigd.

1. DefaultTTL voor de verzameling
   
   * Indien deze ontbreken (of op null ingesteld), documenten worden niet automatisch verwijderd.
   * Als aanwezig zijn en de waarde '-' 1 = oneindige – documenten verloopt niet standaard
   * Als het aanwezig zijn en de waarde is een nummer ("n") – documenten laten verlopen seconden "n" na de laatste wijziging
2. De TTL voor de documenten: 
   
   * Eigenschap is alleen van toepassing als DefaultTTL aanwezig voor de bovenliggende verzameling is.
   * Onderdrukt de DefaultTTL waarde voor de bovenliggende verzameling.

Als het document is verlopen (`ttl`  +  `_ts` < = huidige servertijd), het document is gemarkeerd als 'verlopen'. Er is geen bewerking is toegestaan op deze documenten na deze tijd en ze zullen worden uitgesloten van de resultaten van alle query's uitgevoerd. De documenten die fysiek zijn verwijderd uit het systeem en zijn verwijderd op de achtergrond optioneel op een later tijdstip. Dit verbruikt geen een [Aanvraageenheden (RUs)](request-units.md) uit de verzameling budget.

De bovenstaande logica kan worden weergegeven in de volgende matrix:

|  | DefaultTTL ontbreekt of niet is ingesteld op de verzameling | DefaultTTL = -1 op verzameling | DefaultTTL = "n" voor verzameling |
| --- |:--- |:--- |:--- |
| TTL ontbreekt op document |Er is niets om op te heffen op documentniveau omdat het document en de verzameling hebt geen concept van TTL. |Er zijn geen documenten in deze verzameling verloopt. |De documenten in deze verzameling verlopen wanneer n interval is verstreken. |
| TTL = -1 op document |Er zijn geen overschrijven op het documentniveau van het, omdat de verzameling bevat geen definitie van de eigenschap DefaultTTL die een document kan overschrijven. TTL voor een document is niet geïnterpreteerd door het systeem. |Er zijn geen documenten in deze verzameling verloopt. |Het document met TTL =-1 in deze verzameling verloopt nooit. Alle andere documenten verloopt na "n" interval. |
| TTL = n op document |Er zijn geen overschrijven op het documentniveau van het. TTL voor een document in niet geïnterpreteerd door het systeem. |Het document met TTL = n verloopt na interval n, in seconden. Andere documenten wordt overgenomen van het interval van -1 en nooit verlopen. |Het document met TTL = n verloopt na interval n, in seconden. Andere documenten overneemt "n" interval van de verzameling. |

## <a name="configuring-ttl"></a>TTL configureren
Standaard is de TTL standaard in alle Cosmos DB verzamelingen en op alle documenten uitgeschakeld. TTL kan worden ingesteld via programmacode of via de Azure-portal in de **instellingen** sectie voor de verzameling. 

## <a name="enabling-ttl"></a>TTL inschakelen
Om de TTL voor een verzameling of de documenten binnen een verzameling inschakelt, moet u de DefaultTTL-eigenschap van een verzameling instelt op 1 of een positief getal zijn dan nul. Als de DefaultTTL op 1 betekent dat die door alle documenten in de verzameling wordt permanent live standaard, maar de Cosmos-DB-service, moet deze verzameling voor documenten waarvoor deze standaardinstelling genegeerd controleren.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Standaard-TTL op een verzameling configureren
U zijn kunt een levensduur standaard op het niveau van een verzameling configureren. Als u wilt de TTL-waarde ingesteld op een verzameling, moet u een niet-nul positief getal dat aangeeft van de periode, in seconden, verloopt alle documenten in de verzameling nadat het laatst is gewijzigd tijdstempel van het document (`_ts`). Of u kunt de standaardwaarde instellen op -1, wat betekent dat alle documenten in de verzameling ingevoegd standaard voor onbepaalde tijd blijft bestaan.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>De instelling van TTL op een document
Naast het instellen van een standaard TTL-waarde op een verzameling kunt u specifieke TTL instellen op het documentniveau van een. Hierdoor wordt de standaardwaarde van de verzameling overschreven.

* Als u wilt de TTL-waarde ingesteld op een document, moet u een niet-nul positief getal waarmee wordt aangegeven van de periode, in seconden, verloopt het document nadat het laatst is gewijzigd tijdstempel van het document (`_ts`).
* Als een document geen veld TTL heeft, wordt de standaardwaarde van de verzameling van toepassing zijn.
* Als de TTL is uitgeschakeld op het niveau verzameling, wordt de TTL-veld op het document wordt genegeerd totdat de TTL opnieuw is ingeschakeld op de verzameling.

Hier volgt een codefragment waarin wordt getoond hoe de TTL-vervaldatum instellen voor een document:

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


## <a name="extending-ttl-on-an-existing-document"></a>TTL op een bestaand document uitbreiden
U kunt de TTL voor een document opnieuw instellen als volgt een schrijfbewerking voor het document. Doen dit stelt u de `_ts` op de huidige tijd en de aftelling tot de vervaldatum document, zoals ingesteld door de `ttl`, opnieuw wordt gestart. Als u wilt wijzigen de `ttl` van een document, kunt u het veld bijwerken zoals u met een ander veld worden ingesteld doen kunt.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>TTL verwijderen uit een document
Als een TTL-waarde is ingesteld op een document en niet langer dat document wilt verloopt, kunt vervolgens u ophalen van het document, verwijder de TTL-veld en vervang het document op de server. Als het veld TTL wordt verwijderd uit het document, wordt de standaardwaarde van de verzameling worden toegepast. Om te stoppen van een document verlopen en niet overgenomen van de verzameling moet u de TTL-waarde ingesteld op-1.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>TTL uitschakelen
Als u wilt uitschakelen TTL volledig op een verzameling en stoppen van het achtergrondproces van verlopen documenten zoekt u de eigenschap DefaultTTL in de verzameling moeten worden verwijderd. Als u deze eigenschap verschilt van het instellen op -1. Instellen op 1 betekent dat nieuwe documenten die worden toegevoegd aan de verzameling wordt permanent live, maar u kunt deze waarschuwing negeren op specifieke documenten in de verzameling. Verwijderen van deze eigenschap volledig uit de verzameling betekent dat er geen documenten verloopt, zelfs als er documenten waarvoor een eerdere standaard expliciet worden genegeerd.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);

<a id="ttl-and-index-interaction"></a> 
## <a name="ttl-and-index-interaction"></a>Interactie van de TTL en index
De onderliggende index toevoegen of wijzigen van de TTL-instelling op een verzameling wordt gewijzigd. Wanneer de TTL-waarde wordt gewijzigd van uitgeschakeld in ingeschakeld, wordt de verzameling opnieuw geïndexeerd. Wanneer u wijzigingen in het indexeringsbeleid wanneer de modus voor indexering consistent is, zullen gebruikers een wijziging in de index niet zien. Wanneer de indexing-modus is is ingesteld op vertraagde, worden de index is altijd vastgelegd en als de TTL-waarde is gewijzigd, de index is gemaakt vanaf het begin. Wanneer de TTL-waarde wordt gewijzigd en de Indexmodus is ingesteld op vertraagde, retourneren query's uitgevoerd tijdens het opnieuw opbouwen van de index niet volledig of juist resultaten.

Als u moet de exacte gegevens die zijn geretourneerd, wijzig niet de TTL-waarde als de indexing-modus is ingesteld op vertraagde. In het ideale geval moet consistent index worden gekozen om consistente resultaten te behalen. 

## <a name="faq"></a>Veelgestelde vragen
**Wat TTL kost mij?**

Er is geen extra kosten voor het instellen van een TTL-waarde voor een document.

**Hoe lang duurt het mijn document verwijderd zodra de TTL is?**

De documenten zijn verlopen onmiddellijk nadat de TTL opgestart is, en niet meer toegankelijk via CRUD of de query API's. 

**Wordt de TTL voor een document invloed hebben op RU kosten**

Nee, er zijn geen gevolgen op RU kosten voor verwijderingen van verlopen documenten via TTL in Cosmos-database.

**De functie TTL alleen geldt voor volledige documenten of kan ik afzonderlijke document eigenschapswaarden verlopen?**

TTL geldt voor het hele document. Als u laten verlopen slechts een deel van een document wilt, wordt aanbevolen dat u het gedeelte van het document in naar een afzonderlijke 'gekoppelde' document uitpakken en gebruik vervolgens de TTL op het uitgepakte document.

**Heeft de functie TTL specifieke vereisten voor indexering?**

Ja. De verzameling moet hebben [indexeren groepsbeleid](indexing-policies.md) consistente of Lazy. Bij DefaultTTL instellen op een verzameling met indexeren ingesteld op None leidt tot een fout op als wilt uitschakelen indexeren op een verzameling met een DefaultTTL al ingesteld.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg voor meer informatie over Azure Cosmos DB, de service [ *documentatie* ](https://azure.microsoft.com/documentation/services/cosmos-db/) pagina.

