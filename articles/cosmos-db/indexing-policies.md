---
title: Azure Cosmos DB indexeren beleid | Microsoft Docs
description: Begrijpen hoe indexeren werkt in Azure Cosmos DB. Informatie over het configureren en wijzigen van het indexeringsbeleid voor automatisch indexeren en betere prestaties.
keywords: hoe indexeren werkt, automatische indexeren, database indexeren
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 791446fbd7eb025441f051e2d8f8f2b1e6c47ebe
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Hoe biedt Azure Cosmos DB indexgegevens?

Standaard worden alle gegevens van Azure DB die Cosmos wordt geïndexeerd. En hoewel veel klanten graag laten Azure Cosmos DB automatisch verwerkt alle aspecten van het indexeren, Azure Cosmos DB biedt ook ondersteuning voor het opgeven van een aangepaste **beleid indexeren** voor verzamelingen tijdens het maken van. Indexing-beleid in Azure Cosmos DB zijn flexibelere en krachtigere dan secundaire indexen aangeboden op andere databaseplatforms,, omdat ze u ontwerpen en aanpassen van de vorm van de index kunnen zonder verlies van schemaflexibiliteit. Als u wilt weten hoe indexering werkt in Azure Cosmos DB, moet u begrijpen dat door het indexeringsbeleid beheer, kunt u fijnmazig balans vinden tussen opslagoverhead voor indexering-, schrijf- en doorvoer van de query- en consistentie van de query.  

In dit artikel we Bekijk sluiten Azure Cosmos DB indexeren van beleid, hoe u indexeringsbeleid en de bijbehorende verschillen kunt aanpassen. 

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:

* Hoe kan ik de eigenschappen wilt opnemen of uitsluiten van het indexeren overschrijven?
* Hoe kan ik de index voor eventuele updates configureren?
* Hoe kan ik configureren indexering Order By of bereik query's uitvoeren?
* Hoe kan ik wijzigingen aanbrengen aan een verzameling indexeringsbeleid?
* Hoe vergelijk ik opslag en prestaties van verschillende beleidsregels voor indexering

## <a id="CustomizingIndexingPolicy"></a>Het indexeringsbeleid van een verzameling aanpassen
Ontwikkelaars kunnen de verschillen tussen de opslag, schrijven/queryprestaties en consistentie van de query, aanpassen door overschrijven het standaardbeleid voor indexering op een verzameling Azure Cosmos DB en het configureren van de volgende aspecten.

* **Inclusief/exclusief paden naar/van de index en documenten**. Ontwikkelaars bepaalde documenten die moeten worden uitgesloten of opgenomen in de index op het moment van invoegen of vervangen ze aan de verzameling zelf kunnen kiezen. Ontwikkelaars kunnen ook wilt opnemen of uitsluiten van bepaalde JSON-eigenschappen ook voor kiezen paden (inclusief jokertekenpatronen) over documenten die zijn opgenomen in een index worden geïndexeerd.
* **Configureren van verschillende typen Index**. Voor elk van de opgenomen paden opgeven ontwikkelaars ook het type van de index die ze nodig over een verzameling hebben op basis van hun gegevens en verwacht workload van query's en de numerieke/tekenreeks 'precisie' voor elk pad.
* **Configureren van Index Update modi**. Azure Cosmos DB ondersteunt drie indexering modi die kunnen worden geconfigureerd via het indexeringsbeleid op een verzameling Azure Cosmos DB: Consistent Lazy en er is geen. 

Het volgende fragment van de .NET-code laat zien hoe een aangepast indexeringsbeleid ingesteld tijdens het maken van een verzameling. Hier wordt het beleid met een bereikindex voor tekenreeksen en getallen instellen op de maximale precisie. Dit beleid kunt ons Order By-query's uitvoeren op tekenreeksen.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> De JSON-schema voor het indexeringsbeleid is gewijzigd met de release van REST-API versie 2015-06-03 ter ondersteuning van bereik indexen op tekenreeksen. .NET SDK 1.2.0 en Java, Python en Node.js SDK's 1.1.0 ondersteuning voor het nieuwe beleid schema. Oudere SDK's de REST-API-versie 2015-04-08 gebruiken en ondersteunen het oudere schema van het beleid te indexeren.
> 
> Standaard indexeert Azure Cosmos DB numerieke eigenschappen en alle eigenschappen van een verbindingsreeks in documenten consistent met een hashindex met de bereikindex van een.  
> 
> 

### <a name="customizing-the-indexing-policy-using-the-portal"></a>Het indexeringsbeleid dat via de portal aanpassen

U kunt het indexeringsbeleid van een verzameling met de Azure portal kunt wijzigen. Open uw Azure DB die Cosmos-account in de Azure portal, selecteer uw verzameling in het linkernavigatievenster menu klikt u op **instellingen**, en klik vervolgens op **indexeren beleid**. In de **indexeren beleid** blade gewenste indexeringsbeleid wijzigen en klik vervolgens op **OK** uw wijzigingen op te slaan. 

### <a id="indexing-modes"></a>Database indexing-modi
Azure Cosmos DB ondersteunt drie indexering modi die kunnen worden geconfigureerd via het indexeringsbeleid op een verzameling Azure Cosmos DB – consistente, Lazy en geen.

**Consistente**: als beleid voor een Azure DB die Cosmos-gegevensverzameling is aangewezen als 'consistent', de query's op een bepaalde verzameling van Azure DB die Cosmos hetzelfde consistentieniveau als opgegeven voor de punt-leesbewerkingen volgen (dat wil zeggen sterk, gebonden-verouderd, sessie of uiteindelijke). De index is synchroon als onderdeel van de update van het document (dat wil zeggen invoegen, vervangen, bijwerken en verwijderen van een document in een verzameling Azure Cosmos DB) bijgewerkt.  Consistente indexeren biedt ondersteuning voor consistente query's koste mogelijke vermindering in doorvoer van schrijfbewerkingen. Deze verlaging is een functie van de unieke paden die moeten worden geïndexeerd en het consistentieniveau' '. Consistente indexering modus is ontworpen voor werkbelastingen 'snel query schrijven onmiddellijk'.

**Vertraagde**: In dit geval de index wordt bijgewerkt asynchroon wanneer een verzameling Azure Cosmos DB quiescent, dat wil zeggen, wanneer de doorvoercapaciteit van de verzameling is niet volledig worden gebruikt voor gebruikersaanvragen. Voor 'opnemen nu query later' werkbelastingen document opname vereisen, zijn de 'vertraagde' indexering modus afgestemd. Houd er rekening mee dat u inconsistente resultaten krijgt mogelijk als gegevens opgehaald geconsumeerd en langzaam geïndexeerd. Dit betekent dat uw aantal query's of een specifieke query resultaten mogelijk geen consistente of repeatable op elk moment. De index wordt doorgaans in catch opstartmodus met opgenomen gegevens. Tijd voor live (TTL) wijzigingen ten opzichte van vertraagde indexeren resultaat in de index ophalen verwijderd en opnieuw gemaakt, waardoor het aantal en de query-resultaten inconsistent is voor een bepaalde periode. Daarom moet een meerderheid van Azure DB die Cosmos-accounts gebruiken consistent te indexeren.

**Geen**: een verzameling die is gemarkeerd met Indexmodus van 'None' heeft geen index die is gekoppeld. Dit wordt vaak gebruikt als Azure Cosmos DB die wordt gebruikt als een sleutel / waarde-opslag en documenten zijn alleen toegankelijk met de ID-eigenschap. 

> [!NOTE]
> Configureren van het indexeringsbeleid met 'Geen' heeft een neveneffect van het verwijderen van een bestaande index. Gebruik deze optie als de toegangspatronen zijn alleen nodig 'id' en/of 'Self link-element'.
> 
> 

De volgende tabel ziet de consistentie voor query's op basis van de indexing-modus (consistente en Lazy) geconfigureerd voor de verzameling en de consistentieniveau opgegeven voor de queryaanvraag. Dit geldt voor query's die gemaakt met behulp van een interface - REST-API SDK's of vanuit opgeslagen procedures en triggers. 

|Consistentie|Indexeren van modus: Consistent|Indexeren van modus: vertraagde|
|---|---|---|
|Sterk|Sterk|Mogelijk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|
|Sessie|Sessie|Mogelijk|
|Mogelijk|Mogelijk|Mogelijk|

Azure Cosmos DB foutmelding een voor query's die zijn aangebracht op verzamelingen geen modus te indexeren. Query's kunnen nog steeds worden uitgevoerd als scans via de expliciete `x-ms-documentdb-enable-scan` -header in de REST-API of de `EnableScanInQuery` aanvragen optie met de .NET SDK. Sommige query-functies zoals ORDER BY worden niet ondersteund als scans met `EnableScanInQuery`.

De volgende tabel ziet u de consistentie voor query's op basis van de indexing-modus (consistente, Lazy en geen) als EnableScanInQuery is opgegeven.

|Consistentie|Indexeren van modus: Consistent|Indexeren van modus: vertraagde|Indexeren van modus: geen|
|---|---|---|---|
|Sterk|Sterk|Mogelijk|Sterk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|Gebonden veroudering|
|Sessie|Sessie|Mogelijk|Sessie|
|Mogelijk|Mogelijk|Mogelijk|Mogelijk|

De volgende code voorbeeld tonen hoe een Azure DB die Cosmos-verzameling met de .NET SDK met consistente indexeren op alle document invoegingen maken.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index paden
Azure Cosmos DB modellen JSON-documenten en de index als structuren en kunt u af te stemmen op beleidsregels voor paden binnen de structuur. Binnen documenten, kunt u kiezen welke paden moeten worden opgenomen of uitgesloten van het indexeren. Dit kunt verbeterde schrijfprestaties en lagere opslagkosten voor index voor scenario's bieden de querypatronen vooraf bepaald.

Index paden beginnen met de hoofdmap (/) en meestal eindigen met het? jokertekens-operator, die aangeeft dat er meerdere mogelijke waarden voor het voorvoegsel zijn. Bijvoorbeeld, selecteer bedienen * van Families F waar F.familyName = 'Andersen', moet u een pad van de index voor /familyName/ opnemen? in de verzameling index beleid.

Index paden kunt ook de * jokerteken operator het gedrag voor recursief onder het voorvoegsel paden opgeven. Bijvoorbeeld: / nettolading / * kan worden gebruikt voor het uitsluiten van alles onder de eigenschap nettolading te indexeren.

Hier volgen de algemene patronen voor index paden opgeven:

| Pad                | Beschrijving/gebruiksvoorbeeld                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Het standaardpad voor de verzameling. Recursieve en toegepast op het hele Documentstructuur.                                                                                                                                                                                                                                   |
| / prop /?             | Index-pad voor het uitvoeren van query's als volgt (met hash- of -bereik van het type respectievelijk):<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop > 5<br><br>Selecteer in de c verzameling ORDER BY c.prop                                                                       |
| / prop / *             | Pad van de index voor alle paden die onder het opgegeven label. Werkt met de volgende query 's<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop.subprop > 5<br><br>Selecteer uit verzameling c WHERE c.prop.subprop.nextprop = '' waarde ''<br><br>Selecteer in de c verzameling ORDER BY c.prop         |
| Eigenschappen / [] /?         | Het pad van de index moeten dienst herhaling en JOIN-query's op matrices met scalaire waarden zoals ["a", "b", "c"]:<br><br>Selecteer label van de tag IN collection.props waar tag = '' waarde ''<br><br>Selecteer tag uit verzameling c JOIN-tag IN c.props waar tag > 5                                                                         |
| /props/ [] /subprop/? | Pad van de index moet fungeren herhaling en JOIN-query's op matrices van objecten, zoals [{subprop: "a"}, {subprop: "b"}]:<br><br>Selecteer label van de tag IN collection.props waar tag.subprop '' waarde '' =<br><br>Selecteer labelen van verzameling c JOIN-tag IN c.props waar tag.subprop '' waarde '' =                                  |
| / prop/subprop /?     | Index-pad voor het uitvoeren van query's (met hash- of -bereik van het type respectievelijk):<br><br>Selecteer uit verzameling c WHERE c.prop.subprop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Tijdens het instellen van aangepaste index paden u zijn vereist om op te geven van de standaardregel voor indexering voor de hele documentstructuur aangeduid met de speciale pad ' / * '. 
> 
> 

Het volgende voorbeeld wordt geconfigureerd voor een specifiek pad met bereik indexeren en de precisiewaarde van een aangepaste van 20 bytes:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Gegevenstypen van de index, typen en Precision-systemen
Nu dat we kijken hoe paden opgeven, bekijken we de opties die we gebruiken kunnen voor het configureren van het indexeringsbeleid voor een pad hebt genomen. U kunt een of meer indexering definities voor elke pad opgeven:

* Gegevenstype: **tekenreeks**, **getal**, **punt**, **veelhoek**, of **LineString** (kan slechts één vermelding per gegevenstype per pad bevatten)
* Indexeren van type: **Hash** (gelijkheid query's), **bereik** (gelijkheid, bereik of Order By-query's) of **Spatial** (ruimtelijke query's) 
* Precisie: Voor hash-index dit varieert van 1 tot en met 8 voor zowel tekenreeksen en getallen met standaard als 3. Voor bereikindex deze waarde 1 (maximale precisie) en verschillen tussen de 1-100 (maximale precisie) voor de tekenreeks of numerieke waarden.

#### <a name="index-kind"></a>Index-type
Azure Cosmos DB ondersteunt Hash en bereik index soorten voor elk pad (die kunnen worden geconfigureerd voor tekenreeksen, cijfers of beide).

* **Hash** biedt ondersteuning voor efficiënte gelijkheid en JOIN-query's. Voor de meeste gevallen nodig hash-indexen niet een hogere precisie dan de standaardwaarde van 3 bytes. Gegevenstype is tekenreeks of het getal.
* **Bereik** ondersteunt gelijkheid efficiënt query's, bereik query's (met behulp van >, <>, =, < =,! =), en Order By-query's. Order By-query's standaard moeten echter ook maximumindex precision (-1). Gegevenstype is tekenreeks of het getal.

Azure Cosmos DB ondersteunt ook het type van de ruimtelijke index voor elk pad dat kan worden opgegeven voor de gegevenstypen punt, Polygon of LineString. De waarde in het opgegeven pad moet een geldige GeoJSON-fragment zoals `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Ruimtelijke** ondersteunt efficiënt ruimtelijke (binnen en afstand) query's. Gegevenstype mag punt, Polygon of LineString.

> [!NOTE]
> Azure Cosmos DB biedt ondersteuning voor automatisch indexeren van punten, Multilinestring en multipoint.
> 
> 

Hier worden de soorten ondersteunde index en voorbeelden van query's die ze kunnen worden gebruikt om aan te leveren:

| Index-type | Beschrijving/gebruiksvoorbeeld                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash-via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Hash via Eigenschappen / [] /? (of / of eigenschappen /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer labelen van verzameling c JOIN-tag IN c.props waar tag = 5                                                                                                                       |
| Bereik      | Bereik via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop > 5<br><br>Selecteer in de c verzameling ORDER BY c.prop                                                                                                                                                                                                              |
| Ruimtelijk     | Bereik via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c<br><br>WAAR ST_DISTANCE (c.prop, {'type': 'Point', 'coördinaten': [0,0, 10.0]}) < 40<br><br>Selecteer uit verzameling c waar ST_WITHIN(c.prop, {"type": "Polygon",...})--met indexeren op punten ingeschakeld<br><br>Selecteer uit verzameling c waar ST_WITHIN({"type": "Point",...}, c.prop)--met indexeren op veelhoek ingeschakeld              |

Standaard wordt een fout geretourneerd voor query's met bereik operators zoals > = als er geen bereikindex (van eventuele precisie is) om aan te geven dat een scan mogelijk nodig voor het uitvoeren van de query. Bereik query's kunnen worden uitgevoerd zonder een bereikindex met behulp van de header x-ms-documentdb-enable-scan in de REST-API of de EnableScanInQuery aanvraagoptie met de .NET SDK. Als er geen andere filters in de query die Cosmos Azure DB die wordt de index kunt gebruiken om te filteren op basis van dan geen fout wordt geretourneerd.

Er gelden dezelfde regels voor ruimtelijke query's. Standaard wordt een fout geretourneerd voor ruimtelijke query's als er geen ruimtelijke index is en er zijn geen filters die kunnen worden weergegeven in de index. Ze kunnen worden uitgevoerd als een scan met behulp van de x-ms-documentdb-enable-scan/EnableScanInQuery.

#### <a name="index-precision"></a>Index precisie
Index precisie kunt u een afweging tussen index opslagoverhead en prestaties van query's. Voor getallen, wordt u aangeraden de standaardconfiguratie van de precisie van-1 ('maximum'). Aangezien getallen 8 bytes in JSON, is dit gelijk aan een configuratie van 8 bytes. Verzamelen van een lagere waarde voor de precisie, zoals 1-7, betekent dat binnen een bepaalde adresbereiken waarden aan de dezelfde indexvermelding toewijzen. U wordt daarom verlaagd index opslagruimte, maar queryuitvoering wellicht meer documenten worden verwerkt en als gevolg daarvan dat wil zeggen, verbruikt meer doorvoer aanvraageenheden.

Index precisie configuratie heeft praktischer toepassing met de tekenreeks bereiken. Aangezien tekenreeksen elke willekeurige lengte zijn kunnen, kunt de keuze van de index precisie invloed op de prestaties van query's string-bereik en invloed van de hoeveelheid opslagruimte index vereist. Tekenreeks bereik indexen kunnen worden geconfigureerd met 1-100 of -1 ('maximum'). Als u uitvoeren van de Order By-query's op basis van eigenschappen van een verbindingsreeks wilt, moet u een precisie van -1 voor de bijbehorende paden opgeven.

Ruimtelijke indexen worden altijd de precisie van de index standaard gebruiken voor alle typen (punten multipoint en Veelhoek) en kunnen niet worden overschreven. 

Het volgende voorbeeld ziet hoe u verhoogt de precisie voor bereik indexen in een verzameling met de .NET SDK. 

**Een verzameling met een aangepaste index precisie maken**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for Strings to range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB, wordt er een fout geretourneerd als een query Order By gebruikt, maar geen bereikindex op basis van het pad van de query met de maximale precisie heeft. 
> 
> 

Paden kunnen op dezelfde manier volledig uitgesloten van het indexeren. Het volgende voorbeeld ziet een hele sectie van de documenten (ook uitsluiten een substructuur) vanuit indexering met de ' * ' jokerteken.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Inschrijving en buiten het indexeren uitschakelen
U kunt kiezen of u wilt dat de verzameling automatisch alle documenten te indexeren. Standaard worden alle documenten automatisch worden geïndexeerd, maar u kunt kiezen uit te schakelen. Wanneer het indexeren is uitgeschakeld, documenten toegankelijk zijn alleen via hun Self link-elementen of door query's met-id.

Met automatische indexeren uitgeschakeld, kunt u nog steeds selectief alleen bepaalde documenten toevoegen aan de index. U kunt daarentegen laat automatische indexering op en selectief wilt uitsluiten van alleen specifieke documenten. Indexeren aan/uit-configuraties zijn nuttig wanneer u hebt alleen een subset van documenten die moeten worden opgevraagd.

Het volgende voorbeeld ziet u bijvoorbeeld het opnemen van een document expliciet met behulp van de [DocumentDB API .NET SDK](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet) en de [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) eigenschap.

    // If you want to override the default collection behavior to either
    // exclude (or include) a Document from indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-the-indexing-policy-of-a-collection"></a>Wijzigen van het indexeringsbeleid van een verzameling
Azure Cosmos-database kunt u wijzigingen aanbrengen in het indexeringsbeleid van een verzameling op elk gewenst moment. Een wijziging in het beleid op een verzameling Azure Cosmos DB indexeren kan leiden tot een wijziging in de vorm van de index, waaronder die de paden kunnen worden geïndexeerd, de precisie, evenals het model van de consistentie van de index zelf. Effectief vereist een wijziging in indexeringsbeleid, dus een transformatie van de oude index in een nieuwe. 

**Online-Index transformaties**

![Hoe indexeren werkt – Azure Cosmos DB online index transformaties](./media/indexing-policies/index-transformations.png)

Index transformaties zijn aangebracht online, wat betekent dat de basis van de oude beleid geïndexeerde documenten efficiënt worden getransformeerd per het nieuwe beleid **zonder de beschikbaarheid schrijven of de ingerichte doorvoer** van de verzameling. De consistentie van lees- en schrijfbewerkingen die zijn gemaakt met behulp van de REST-API SDK's of vanuit de opgeslagen procedures en triggers niet beïnvloed tijdens index transformatie. Dit betekent dat er is geen verslechtering van de prestaties of om uw apps uit te schakelen wanneer u een indexeringsbeleid wijzigen.

Tijdens de tijd die index transformatie uitgevoerd is, zijn query's echter uiteindelijk consistent, ongeacht de indexering modus-configuratie (consistente of Lazy). Dit ook van toepassing op query's uit alle interfaces – REST-API SDK's, en vanuit opgeslagen procedures en triggers. Net als met Lazy indexeren, is index-transformatie asynchroon op de achtergrond uitgevoerd op de replica's met behulp van de beschikbare vrije bronnen voor een bepaalde replica. 

Index transformaties worden ook gemaakt **in situ** (in plaats), dat wil zeggen Azure Cosmos DB niet twee kopieën van de index en de oude index uit door de nieuwe wisselen. Dit betekent dat er geen extra schijfruimte vereist of in uw verzamelingen gebruikt tijdens het uitvoeren van de index transformaties.

Wanneer u indexeringsbeleid wijzigt, hoe de wijzigingen worden toegepast om te verplaatsen uit de oude index naar de nieuwe een zijn hoofdzakelijk afhankelijk van de indexering modus configuraties meer dus dan de andere waarden, zoals opgenomen/uitgesloten paden, index-typen en Precision-systemen. Als uw oude en nieuwe beleidsregels consistent indexeren, voert Azure Cosmos DB een transformatie online-index. U kunt een andere indexering beleidswijziging met consistente indexing-modus niet toepassen, terwijl de transformatie uitgevoerd wordt.

U kunt echter verplaatsen naar Lazy of None indexering modus tijdens een transformatie uitgevoerd wordt. 

* Wanneer u naar Lazy verplaatst, wordt de beleidswijziging index effectieve onmiddellijk en Azure Cosmos DB begint asynchroon opnieuw maken van de index. 
* Wanneer u op geen verplaatst, is wordt de index weggehaald effectieve onmiddellijk. Op None verplaatsen is handig als u wilt annuleren van een onderhanden transformatie en begin met een ander beleid voor indexering. 

Als u de .NET SDK, kunt u ere van een indexering beleidswijziging met behulp van de nieuwe **ReplaceDocumentCollectionAsync** methode en bijhouden van de voortgang percentage van de index transformatie met behulp van de **IndexTransformationProgress** antwoord-eigenschap van een **ReadDocumentCollectionAsync** aanroepen. Andere SDK en de REST-API ondersteuning voor equivalente eigenschappen en methoden voor het maken van wijzigingen in het indexeren.

Hier volgt een codefragment die laat zien hoe het indexeringsbeleid een verzameling van consistente indexering modus naar Lazy wijzigen.

**Indexeringsbeleid van consistente naar vertraagde wijzigen**

    // Switch to lazy indexing.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


U kunt de voortgang van een index transformatie controleren door het aanroepen van ReadDocumentCollectionAsync, bijvoorbeeld zoals hieronder wordt weergegeven.

**Voortgang van de Index transformatie volgen**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

U kunt de index voor een verzameling verwijderen door te verplaatsen naar de geen modus te indexeren. Dit wordt mogelijk een handig hulpmiddel operationele als u wilt een transformatie in voortgang annuleren en een nieuwe onmiddellijk starten.

**Verwijderen van de index voor een verzameling**

    // Switch to lazy indexing.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Wanneer zou u indexering beleidswijzigingen aanbrengen in uw Azure DB die Cosmos-verzamelingen Hier volgen de meest voorkomende gebruiksvoorbeelden:

* Consistente resultaten tijdens normale werking, maar vallen terug naar de vertraagde indexeren tijdens bulkimport gegevens leveren
* Bijvoorbeeld Start met nieuwe functies op uw huidige Azure DB die Cosmos-verzamelingen indexeren georuimtelijke uitvoeren van query's die het type van de ruimtelijke index vereisen of Order By, zoals / bereik-query's waarvoor de tekenreeks bereik index type string
* Hand selecteert u de eigenschappen die moeten worden geïndexeerd en wijzig ze na verloop van tijd
* Indexering precisie voor queryprestaties verbeteren of te verkleinen opslag verbruikt afstemmen

> [!NOTE]
> Als u wilt wijzigen met behulp van ReplaceDocumentCollectionAsync indexeringsbeleid, moet u versie > = 1.3.0 van de .NET SDK
> 
> Voor de transformatie van de index te voltooien, moet u ervoor zorgen dat er voldoende vrije ruimte beschikbaar voor de verzameling is. Als de verzameling de opslaglimiet bereikt, wordt de index transformatie worden onderbroken. Transformatie van de index wordt automatisch voortgezet zodra opslagruimte beschikbaar is, bijvoorbeeld als u bepaalde documenten verwijdert.
> 
> 

## <a name="performance-tuning"></a>Prestaties afstemmen
De DocumentDB APIs bieden informatie over de maatstaven voor prestaties zoals de index opslag gebruikt en de kosten van doorvoer (aanvraageenheden) voor elke bewerking. Deze informatie kan worden gebruikt om te vergelijken van verschillende beleidsregels voor indexering en voor prestaties afstemmen.

Controleer de opslaglimiet en informatie over het gebruik van een verzameling, uitvoeren van een HEAD- of GET-aanvraag op de verzamelingsbron en controleren van de x-ms-aanvraag-quota en -headers van de x-ms-aanvraag-gebruik. In de .NET SDK, de [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) en [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) eigenschappen in [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) deze bijbehorende waarden bevatten.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Voor het meten van de overhead van het indexeren op elke schrijfbewerking (maken, bijwerken of verwijderen) inspecteren van de header x-ms-aanvraag-kosten (of een vergelijkbare [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) eigenschap in [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) in de .NET SDK) voor het meten van het aantal aanvraageenheden verbruikt door deze bewerkingen.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Wijzigingen in de beleidsspecificatie voor indexering
Een wijziging in het schema voor het indexeringsbeleid is op 7 juli 2015 met de REST API-versie 2015-06-03 geïntroduceerd. De bijbehorende klassen in de SDK-versies hebben nieuwe implementaties van overeen met het schema. 

De volgende wijzigingen zijn geïmplementeerd in de JSON-specificatie:

* Indexeren van beleid ondersteunt bereik indexen voor tekenreeksen
* Elk pad kan meerdere definities voor index, één voor elk gegevenstype hebben
* Indexeren van precisie ondersteunt 1-8 voor cijfers, 1-100 naar tekenreeksen en -1 (maximumprecisie)
* Paden segmenten vereisen een dubbele aanhalingstekens als escapeteken voor elk pad niet. U kunt bijvoorbeeld een pad voor de titel/toevoegen? in plaats van / 'title' /?
* Het hoofdpad 'alle paden' die kan worden weergegeven als / * (naast /)

Als u code die voorziet in verzamelingen met een aangepast indexeringsbeleid geschreven met versie 1.1.0 van de .NET SDK of ouder hebt, moet u uw toepassingscode voor het afhandelen van deze wijzigingen kunnen worden verplaatst naar SDK-versie 1.2.0 wijzigen. Als u geen code die u indexeringsbeleid configureert of van plan bent om met behulp van een oudere versie van de SDK, zijn geen wijzigingen vereist.

Voor een praktische vergelijking volgt hier een voorbeeld van aangepast indexeringsbeleid geschreven met behulp van de REST-API-versie 2015-06-03, evenals de vorige versie 2015-04-08.

**Vorige Indexeringsbeleid JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Huidige Indexeringsbeleid JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Volgende stappen
Volg de onderstaande koppelingen voor index beleid management voorbeelden en voor meer informatie over Azure Cosmos DB querytaal.

1. [DocumentDB-API .NET indexbeheer-codevoorbeelden](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [DocumentDB API REST-verzameling bewerkingen](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [De query met behulp van SQL](documentdb-sql-query.md)

