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
ms.openlocfilehash: b09f5323f0378721412baade9be9926ebd0c171e
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Hoe biedt Azure Cosmos DB indexgegevens?

Standaard worden alle gegevens van Azure DB die Cosmos wordt geïndexeerd. Hoewel veel klanten graag laten Azure Cosmos DB automatisch verwerkt alle aspecten van het indexeren, kunt u een aangepaste *beleid indexeren* voor verzamelingen tijdens het maken in Azure Cosmos DB. De Indexing-beleid in Azure Cosmos DB zijn flexibelere en krachtigere dan secundaire indexen die worden aangeboden in een andere databaseplatforms. U kunt in Azure Cosmos DB ontwerpen en aanpassen van de vorm van de index zonder verlies van schemaflexibiliteit. 

Als u wilt weten hoe indexering werkt in Azure Cosmos DB, is het belangrijk te begrijpen wanneer u indexeringsbeleid beheert, kunt u fijnmazig verschillen tussen opslagoverhead voor indexering-, schrijf- en doorvoer van de query- en consistentie van de query maken.  

In dit artikel Bekijk we sluiten Azure Cosmos DB beleidsregels, bij het aanpassen van indexeringsbeleid en bijbehorende-en nadelen te indexeren. 

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden:

* Hoe kan ik de eigenschappen wilt opnemen of uitsluiten van het indexeren overschrijven?
* Hoe kan ik de index voor eventuele updates configureren?
* Hoe kan ik configureren indexering ORDER BY of bereik query's uitvoeren?
* Hoe kan ik wijzigingen aanbrengen aan een verzameling indexeringsbeleid?
* Hoe vergelijk ik opslag en prestaties van verschillende beleidsregels voor indexering

## Het indexeringsbeleid van een verzameling aanpassen<a id="CustomizingIndexingPolicy"></a>  
U kunt de verschillen tussen de opslag, schrijf- en prestaties van query's en consistentie van de query overschrijft de standaard-beleid op een verzameling Azure Cosmos DB te indexeren. U kunt de volgende aspecten configureren:

* **Opnemen of uitsluiten van documenten en paden naar en van de index**. U kunt uitsluiten of opnemen specifieke documenten in de index wanneer u invoegen of vervangen van de documenten in de verzameling. U kunt ook opnemen of uitsluiten van specifieke JSON-eigenschappen, ook wel *paden*, meerdere documenten die zijn opgenomen in een index worden geïndexeerd. Paden bevatten jokertekenpatronen.
* **Configureren van verschillende typen van de index**. U kunt het type van het pad is vereist voor een verzameling index opgeven voor elke opgenomen pad. U kunt opgeven welk type van de index op basis van gegevens van het pad, de query verwacht werkbelasting en de numerieke/tekenreeks "precisie."
* **Index update modi configureren**. Azure Cosmos DB ondersteunt drie indexering modi: consistente, vertraagde, en er is geen. U kunt de indexering modi via het indexeringsbeleid configureren op een verzameling Azure Cosmos DB. 

Het volgende codefragment van Microsoft .NET laat zien hoe een aangepast indexeringsbeleid ingesteld wanneer u een verzameling maken. In dit voorbeeld wordt het beleid met een bereikindex voor tekenreeksen en getallen op maximumprecisie instellen. U kunt dit beleid gebruiken om uit te voeren ORDER BY query uitgevoerd naar tekenreeksen.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> De JSON-schema voor het indexeringsbeleid gewijzigd met de release van de REST API-versie 2015-06-03. Met deze release ondersteunt de JSON-schema voor indexeren beleid bereik indexen op tekenreeksen. .NET SDK 1.2.0 en Java, Python en Node.js SDK's 1.1.0 ondersteuning voor het nieuwe beleid schema. Eerdere versies van de SDK gebruikt de REST-API-versie 2015-04-08. Ze ondersteunen het eerdere schema voor het beleid te indexeren.
> 
> Standaard indexeert Azure Cosmos DB alle eigenschappen van een verbindingsreeks in documenten consistent met een hashindex. Het indexeren van alle numerieke eigenschappen in documenten consistent met de bereikindex van een.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Het indexeringsbeleid in de portal aanpassen

U kunt het indexeringsbeleid van een verzameling in de Azure portal wijzigen: 

1. Ga naar uw Azure DB die Cosmos-account in de portal en selecteer vervolgens uw verzameling. 
2. Selecteer in het navigatiemenu links **instellingen**, en selecteer vervolgens **indexeren beleid**. 
3. Onder **indexeren beleid**, wijzig de gewenste indexeringsbeleid en selecteer vervolgens **OK**. 

### Database indexing-modi<a id="indexing-modes"></a>  
Azure Cosmos DB ondersteunt drie indexering modi die u via het indexeringsbeleid op een verzameling Azure Cosmos DB configureren kunt: consistente, vertraagde, en er is geen.

**Consistente**: als een Azure DB die Cosmos-verzameling beleid Consistent is, de query's op een specifieke verzameling van Azure DB die Cosmos hetzelfde consistentieniveau als opgegeven voor de punt-leesbewerkingen volgen (sterk, gebonden-verouderd, sessie of uiteindelijke). De index is synchroon als onderdeel van de update van het document (invoegen, vervangen, bijwerken en verwijderen van een document in een verzameling Azure Cosmos DB) bijgewerkt.

Consistente indexeren ondersteunt consistente query's koste mogelijk verminderde doorvoer van schrijfbewerkingen. Deze verlaging is een functie van de unieke paden die moeten worden geïndexeerd en de 'consistentieniveau." Consistente indexering modus is ontworpen voor werkbelastingen 'snel query schrijven onmiddellijk'.

**Vertraagde**: de index asynchroon wordt bijgewerkt wanneer een verzameling Azure Cosmos DB quiescent, dat wil zeggen, is wanneer de doorvoercapaciteit van de verzameling is niet volledig worden gebruikt voor gebruikersaanvragen. De vertraagde indexering modus mogelijk geschikt is voor 'opnemen nu query later' werkbelastingen waarvoor document opneemt. Houd er rekening mee dat u inconsistente resultaten krijgt mogelijk omdat gegevens geconsumeerd en langzaam geïndexeerd. Dit betekent dat uw aantal query's of een specifieke query resultaten mogelijk geen consistente of repeatable op elk moment. 

De index wordt doorgaans in de modus bijwerken met opgenomen gegevens. Tijd tot live (TTL) wijzigt met Lazy indexeren, resultaat in de index wordt verwijderd en opnieuw gemaakt. Hierdoor is het aantal en de query-resultaten inconsistent voor een bepaalde periode. Als gevolg hiervan moeten de meeste Azure DB die Cosmos-accounts gebruiken de consistente indexing-modus.

**Geen**: een verzameling met een geen Indexmodus geen index die is gekoppeld heeft. Dit wordt meestal gebruikt als Azure Cosmos DB wordt gebruikt als een sleutel / waarde-opslag en documenten zijn geopend alleen door de ID-eigenschap. 

> [!NOTE]
> Configureren van het indexeringsbeleid met omdat geen heeft een neveneffect van het verwijderen van een bestaande index. Gebruik deze optie als de toegangspatronen alleen-ID is vereist of self link-element.
> 
> 

De volgende tabel ziet de consistentie voor query's op basis van de indexing-modus (consistente en Lazy) geconfigureerd voor de verzameling en de consistentieniveau opgegeven voor de queryaanvraag. Dit geldt voor query's die gemaakt met behulp van een interface: REST-API, SDK's, of vanuit opgeslagen procedures en triggers. 

|Consistentie|Modus voor indexering: Consistent|Modus voor indexering: vertraagde|
|---|---|---|
|Sterk|Sterk|Mogelijk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|
|Sessie|Sessie|Mogelijk|
|Mogelijk|Mogelijk|Mogelijk|

Azure Cosmos DB foutmelding een voor query's die zijn aangebracht op de verzamelingen die geen een modus te indexeren. Query's kunnen nog steeds worden uitgevoerd als scans via de expliciete **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie aanvragen met behulp van de .NET SDK. Sommige functies van de query, zoals ORDER BY worden niet ondersteund als scans met **EnableScanInQuery**.

De volgende tabel ziet u de consistentie voor query's op basis van de indexing-modus (consistente, Lazy en geen) wanneer **EnableScanInQuery** is opgegeven.

|Consistentie|Indexeren van modus: Consistent|Indexeren van modus: vertraagde|Indexeren van modus: geen|
|---|---|---|---|
|Sterk|Sterk|Mogelijk|Sterk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|Gebonden veroudering|
|Sessie|Sessie|Mogelijk|Sessie|
|Mogelijk|Mogelijk|Mogelijk|Mogelijk|

Een verzameling Azure Cosmos DB de volgende code voorbeeld weergeven hoe maken met behulp van de .NET SDK met Consistent op alle document invoegingen te indexeren.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index paden
Azure Cosmos DB modellen JSON-documenten en de index als structuren. U kunt beleid voor paden binnen de structuur afstemmen. In documenten, kunt u de paden wilt opnemen of uitsluiten van het indexeren. Dit kan bieden verbeterde schrijfprestaties en lagere opslagkosten voor index voor scenario's waarin de querypatronen vooraf bekend.

Index paden beginnen met de hoofdmap (/) en meestal eindigen met het? Wildcard-operator. Dit geeft aan dat er meerdere mogelijke waarden voor het voorvoegsel zijn. Bijvoorbeeld, selecteer bedienen * van Families F waar F.familyName = 'Andersen', moet u een pad van de index voor /familyName/ opnemen? in de verzameling index beleid.

Index paden kunt ook de \* jokertekens operator het gedrag voor recursief onder het voorvoegsel paden opgeven. Bijvoorbeeld: / nettolading / * kan worden gebruikt voor het uitsluiten van alles onder de eigenschap nettolading te indexeren.

Hier volgen de algemene patronen voor index paden opgeven:

| Pad                | Beschrijving/gebruiksvoorbeeld                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Het standaardpad voor de verzameling. Recursieve en geldt voor de hele document-structuur.                                                                                                                                                                                                                                   |
| / prop /?             | Index-pad voor het uitvoeren van query's als volgt (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop > 5<br><br>Selecteer in de c verzameling ORDER BY c.prop                                                                       |
| / prop / *             | Pad van de index voor alle paden die onder het opgegeven label. Werkt met de volgende query 's<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop.subprop > 5<br><br>Selecteer uit verzameling c WHERE c.prop.subprop.nextprop = '' waarde ''<br><br>Selecteer in de c verzameling ORDER BY c.prop         |
| Eigenschappen / [] /?         | Het pad van de index moeten dienst herhaling en JOIN-query's op matrices met scalaire waarden zoals ["a", "b", "c"]:<br><br>Selecteer label van de tag IN collection.props waar tag = '' waarde ''<br><br>Selecteer tag uit verzameling c JOIN-tag IN c.props waar tag > 5                                                                         |
| /props/ [] /subprop/? | Pad van de index moet fungeren herhaling en JOIN-query's op matrices van objecten, zoals [{subprop: "a"}, {subprop: "b"}]:<br><br>Selecteer label van de tag IN collection.props waar tag.subprop '' waarde '' =<br><br>Selecteer labelen van verzameling c JOIN-tag IN c.props waar tag.subprop '' waarde '' =                                  |
| / prop/subprop /?     | Index-pad voor het uitvoeren van query's (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c WHERE c.prop.subprop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Als u aangepaste index paden instelt, u zijn vereist om op te geven van de standaardregel voor indexering voor de hele document-structuur die wordt aangeduid met de speciale pad ' / * '. 
> 
> 

Het volgende voorbeeld configureert een specifiek pad met bereikindex en een aangepaste precisiewaarde van 20 bytes:

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
U hebt meerdere mogelijkheden bij het configureren van het indexeringsbeleid voor een pad. U kunt een of meer indexering definities voor elke pad opgeven:

* **Gegevenstype**: String, getal, punt, Polygon of LineString (kan slechts één vermelding per gegevenstype per pad bevatten).
* **Indexeren van type**: Hash (gelijkheid query's), bereik (gelijkheid, bereik of ORDER BY-query's) of Spatial (ruimtelijke query's).
* **Precisie**: voor een hashindex dit hangt af van 1 tot en met 8 voor zowel tekenreeksen en getallen. De standaardwaarde is 3. Voor de bereikindex van een, kan deze waarde -1 (maximumprecisie) zijn. Dit kan variëren tussen 1 en 100 (maximumprecisie) voor de tekenreeks of numerieke waarden.

#### <a name="index-kind"></a>Index-type
Azure Cosmos DB ondersteunt hash-index en bereik index typen voor elk pad die kan worden geconfigureerd voor de gegevenstypen String of een cijfer of beide.

* **Hash** biedt ondersteuning voor efficiënte gelijkheid en JOIN-query's. Voor de meeste gevallen hoeft Hash-indexen niet een hogere precisie dan de standaardwaarde van 3 bytes. Het type is tekenreeks of het getal.
* **Bereik** ondersteunt gelijkheid efficiënt query's, bereik query's (met behulp van >, <>, =, < =,! =), en ORDER BY-query's. ORDER By-query's standaard moeten echter ook maximumindex precision (-1). Het type is tekenreeks of het getal.

Azure Cosmos DB ondersteunt ook het type van de ruimtelijke index voor elk pad dat kan worden opgegeven voor de gegevenstypen punt, Polygon of LineString. De waarde in het opgegeven pad moet een geldige GeoJSON-fragment zoals `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Ruimtelijke** ondersteunt efficiënt ruimtelijke (binnen en afstand) query's. Het gegevenstype mag punt, Polygon of LineString.

> [!NOTE]
> Azure Cosmos DB biedt ondersteuning voor automatisch indexeren van punt veelhoek en LineString gegevenstypen.
> 
> 

Hier worden de soorten ondersteunde index en voorbeelden van query's die ze kunnen worden gebruikt om aan te leveren:

| Index-type | Beschrijving/gebruiksvoorbeeld                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash-via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Hash via Eigenschappen / [] /? (of / of eigenschappen /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer labelen van verzameling c JOIN-tag IN c.props waar tag = 5                                                                                                                       |
| Bereik      | Bereik via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c WHERE c.prop = '' waarde ''<br><br>Selecteer uit verzameling c WHERE c.prop > 5<br><br>Selecteer in de c verzameling ORDER BY c.prop                                                                                                                                                                                                              |
| Ruimtelijk     | Bereik via/prop /? (of /) efficiënt voor de volgende query's kunnen worden gebruikt:<br><br>Selecteer uit verzameling c<br><br>WAAR ST_DISTANCE (c.prop, {'type': 'Point', 'coördinaten': [0,0, 10.0]}) < 40<br><br>Selecteer uit verzameling c waar ST_WITHIN(c.prop, {"type": "Polygon",...})--met indexeren op punten ingeschakeld<br><br>Selecteer uit verzameling c waar ST_WITHIN({"type": "Point",...}, c.prop)--met indexeren op veelhoek ingeschakeld              |

Standaard wordt een fout geretourneerd voor query's met bereik operators zoals > = als er geen index bereik (van eventuele precisie is) om aan te geven dat een scan mogelijk nodig voor het uitvoeren van de query. Bereik query's kunnen worden uitgevoerd zonder de bereikindex van een met behulp van de **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie aanvragen met behulp van de .NET SDK. Als er in de query die Azure Cosmos DB de index gebruiken kunt om te filteren op basis van andere filters, wordt geen fout geretourneerd.

Er gelden dezelfde regels voor ruimtelijke query's. Standaard wordt een fout geretourneerd voor ruimtelijke query's als er geen ruimtelijke index is en er zijn geen filters die kunnen worden weergegeven in de index. Ze kunnen worden uitgevoerd als een scan met behulp van **x-ms-documentdb-enable-scan** of **EnableScanInQuery**.

#### <a name="index-precision"></a>Index precisie
U kunt index precisie afweging tussen index opslagoverhead en prestaties van query's. Voor getallen, wordt u aangeraden de standaardconfiguratie van de precisie van-1 (maximum). Omdat de getallen 8 bytes in JSON, is dit gelijk aan een configuratie van 8 bytes. Het kiezen van een lagere waarde voor de precisie, zoals 1 tot en met 7, index betekent dat de waarden in een aantal bereiken worden toegewezen aan dezelfde vermelding. Daarom verminderen van index opslagruimte, maar queryuitvoering wellicht meer documenten worden verwerkt. Als gevolg daarvan kan verbruikt meer doorvoer in aanvraageenheden.

Index precisie configuratie heeft praktischer toepassing met de tekenreeks bereiken. Omdat tekenreeksen elke willekeurige lengte zijn kunnen, mogelijk de keuze van de index precisie invloed hebben op de prestaties van query's string-bereik. Het ook mogelijk van invloed op de hoeveelheid opslagruimte index die is vereist. Tekenreeks bereik indexen kunnen worden geconfigureerd met 1 tot 100 of -1 (maximum). Als u uitvoeren van de ORDER BY-query's op basis van eigenschappen van een verbindingsreeks wilt, moet u een precisie van -1 voor de bijbehorende paden opgeven.

Ruimtelijke indexen gebruik altijd de precisie van de index standaard voor alle typen (Point, LineString en Veelhoek). De precisie van de index standaard voor ruimtelijke indexen kan niet worden overschreven. 

Het volgende voorbeeld laat zien hoe de precisie voor bereik indexen in een verzameling verhogen met behulp van de .NET SDK. 

**Een verzameling met een aangepaste index precisie maken**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB, wordt er een fout geretourneerd als een query ORDER BY gebruikt, maar beschikt niet over een bereikindex tegen het pad van de query met de maximale precisie. 
> 
> 

U kunt ook volledig paden uitsluiten van het indexeren. Het volgende voorbeeld toont het uitsluiten van een volledige gedeelte van de documenten (een *substructuur*) niet kan indexeren met behulp van de \* jokertekens operator.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Aanmelden en afmelden indexeren
U kunt kiezen of u wilt dat de verzameling automatisch alle documenten te indexeren. Alle documenten automatisch worden geïndexeerd, maar u kunt automatische indexering uitschakelen. Wanneer het indexeren is uitgeschakeld, documenten toegankelijk zijn alleen via hun Self link-elementen of door query's met behulp van het document-id.

Met automatische indexeren uitgeschakeld, kunt u nog steeds selectief alleen bepaalde documenten toevoegen aan de index. U kunt daarentegen laat automatische indexering op en selectief wilt uitsluiten van specifieke documenten. Indexeren aan/uit-configuraties zijn nuttig wanneer u hebt alleen een subset van documenten die moeten worden opgevraagd.

Het volgende voorbeeld laat zien hoe een document expliciet opnemen met behulp van de [SQL API .NET SDK](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) en de [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) eigenschap.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Het indexeringsbeleid van een verzameling wijzigen
In Azure Cosmos DB, kunt u wijzigingen aanbrengen aan het indexeringsbeleid van een verzameling op elk gewenst moment. Een wijziging in het beleid op een verzameling Azure Cosmos DB indexeren kan leiden tot een wijziging in de vorm van de index. De wijziging invloed op de paden die kunnen worden geïndexeerd, hun precision en het model van de consistentie van de index zelf. Een wijziging in het beleid effectief indexeren vereist een transformatie van de oude index in een nieuwe index. 

**Online-index transformaties**

![Hoe indexeren werkt – Azure Cosmos DB online index transformaties](./media/indexing-policies/index-transformations.png)

Index transformaties worden online gedaan. Dit betekent dat de basis van de oude beleid geïndexeerde documenten efficiënt worden getransformeerd per het nieuwe beleid *zonder de beschikbaarheid schrijven of de ingerichte doorvoer* van de verzameling. De consistentie van lees- en schrijfbewerkingen uitgevoerd met behulp van de REST-API SDK's, of vanuit de opgeslagen procedures en triggers niet beïnvloed tijdens index transformatie. Er is geen verslechtering van de prestaties of om uw apps uit te schakelen wanneer u een indexeringsbeleid wijzigen.

Tijdens de tijd die index transformatie uitgevoerd is, zijn query's echter uiteindelijk consistent, ongeacht de indexering modus-configuratie (consistente of Lazy). Dit geldt ook voor query's uit alle interfaces: REST-API, SDK's, en vanuit opgeslagen procedures en triggers. Net als met Lazy indexeren, is index-transformatie asynchroon op de achtergrond uitgevoerd op de replica's met behulp van de ongebruikte resources die beschikbaar voor een specifieke replica zijn. 

Index transformaties ook worden aangebracht in de plaats. Azure Cosmos-DB niet twee exemplaren van de index en verwijder de oude index met de nieuwe database te onderhouden. Dit betekent dat er geen extra schijfruimte vereist of in uw verzamelingen verbruikt terwijl index transformaties plaatsvinden.

Wanneer u indexeringsbeleid wijzigt, worden de wijzigingen toegepast van de oude index naar de nieuwe voornamelijk gebaseerd op de indexering modus configuraties te verplaatsen. Indexering configuraties spelen een grotere rol dan andere waarden zoals opgenomen/uitgesloten paden, index-typen en Precision-systemen. 

Als uw oude en nieuwe beleidsregels beide Consistent indexeren gebruiken, voert Azure Cosmos DB een transformatie online-index. U kunt een andere indexering beleidswijziging waarvoor Consistent indexering is terwijl de transformatie uitgevoerd wordt niet toepassen. U kunt echter verplaatsen naar Lazy of None indexering modus tijdens een transformatie uitgevoerd wordt: 

* Wanneer u naar Lazy verplaatst, wordt de beleidswijziging index onmiddellijk van kracht. Azure Cosmos DB begint de index asynchroon worden opnieuw gemaakt. 
* Wanneer u op geen verplaatst, wordt de index onmiddellijk verwijderd. Op None verplaatsen is handig als u wilt een transformatie in voortgang annuleren en opnieuw beginnen met een ander beleid voor indexering. 

Het volgende codefragment laat zien hoe het indexeringsbeleid een verzameling van consistente indexering modus vertraagde indexering modus wijzigen. Als u de .NET SDK, kunt u ere van een indexering beleid wijzigen met behulp van de nieuwe **ReplaceDocumentCollectionAsync** methode.

**Indexeringsbeleid van consistente naar Lazy wijzigen**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Voortgang van de index transformatie volgen**

U kunt de voortgang van het percentage van de index transformatie naar een consistente index volgen met behulp van de **IndexTransformationProgress** antwoord-eigenschap van een **ReadDocumentCollectionAsync** aanroepen. Andere SDK en de REST API, ondersteuning voor equivalente eigenschappen en methoden voor het maken van wijzigingen in het indexeren. U kunt de voortgang van een index transformatie naar een consistente index controleren door het aanroepen van **ReadDocumentCollectionAsync**: 

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

> [!NOTE]
> * De **IndexTransformationProgress** eigenschap is alleen toepasbaar wanneer transformeren naar een consistente index. Gebruik de **ResourceResponse.LazyIndexingProgress** eigenschap voor het bijhouden van transformaties naar een vertraagde index.
> * De **IndexTransformationProgress** en de **LazyIndexingProgress** eigenschappen alleen voor een niet-gepartitioneerde verzameling, dat wil zeggen, een verzameling die is gemaakt zonder een partitiesleutel worden ingevuld.
>

U kunt de index voor een verzameling verwijderen door te verplaatsen naar de geen modus te indexeren. Dit wordt mogelijk een handig hulpmiddel operationele als u wilt annuleren een transformatie wordt uitgevoerd en vervolgens een nieuwe onmiddellijk te starten.

**De index voor een verzameling verwijderen**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Wanneer zou u indexering beleidswijzigingen aanbrengen in uw Azure DB die Cosmos-verzamelingen Hier volgen de meest voorkomende gebruiksvoorbeelden:

* Consistente resultaten dienen tijdens normale werking, maar terugvallen op vertraagde indexering modus tijdens bulkimport gegevens.
* Start met behulp van de nieuwe indexering functies op uw huidige Azure DB die Cosmos-verzamelingen. Bijvoorbeeld, kunt u gebruiken georuimtelijke query wordt uitgevoerd, waarvoor het type ruimtelijke index of ORDER BY / bereik query's waarvoor de tekenreeks bereik index type string.
* Hand-selecteren welke eigenschappen u moet worden geïndexeerd, en wijzig ze gedurende een bepaalde periode.
* Afstemmen indexering precisie ter verbetering van prestaties van query's of opslagruimte is verbruikt verminderen.

> [!NOTE]
> Indexeringsbeleid wijzigen met behulp van **ReplaceDocumentCollectionAsync**, moet u versie 1.3.0 of een latere versie van de .NET SDK.
> 
> Voor de transformatie van de index te kunnen voltooien, zorg ervoor dat er voldoende vrije ruimte beschikbaar op de verzameling. Als de verzameling de opslaglimiet bereikt, wordt de index-transformatie is onderbroken. Transformatie van de index hervat automatisch als opslagruimte beschikbaar, bijvoorbeeld is als u bepaalde documenten verwijderen.
> 
> 

## <a name="performance-tuning"></a>Prestaties afstemmen
De SQL-API's bevatten informatie over de maatstaven voor prestaties, zoals de index opslag gebruikt en de kosten van doorvoer (aanvraageenheden) voor elke bewerking. U kunt deze informatie gebruiken om te vergelijken van verschillende beleidsregels voor indexering en voor prestaties afstemmen.

Om te controleren van de opslaglimiet en het gebruik van een verzameling, voer een **HEAD** of **ophalen** -aanvraag in voor de verzamelingsbron. Vervolgens, controleert de **x-ms-aanvraag-quotum** en de **x-ms-aanvraag-Gebruik** headers. In de .NET SDK, de [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) en [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) eigenschappen in [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) deze bijbehorende waarden bevatten.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Voor het meten van de overhead van het indexeren op elke schrijfbewerking (maken, bijwerken of verwijderen) Inspecteer de **x-ms-aanvraag-kosten** header (of een vergelijkbare [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) eigenschap in [ ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) in de .NET SDK) voor het meten van het aantal aanvraageenheden die worden verbruikt door deze bewerkingen.

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
Een wijziging in het schema voor indexering van beleid is 7 juli 2015 met de REST API-versie 2015-06-03 geïntroduceerd. De bijbehorende klassen in de SDK-versies hebben nieuwe implementaties van overeen met het schema. 

De volgende wijzigingen zijn geïmplementeerd in de JSON-specificatie:

* Indexeren van beleid ondersteunt bereik indexen voor tekenreeksen.
* Elk pad kan meerdere definities van de index hebben. Dit kan een voor elk gegevenstype hebben.
* Indexeren van precisie ondersteunt 1 tot en met 8 voor getallen, 1 tot 100 naar tekenreeksen en -1 (maximumprecisie).
* Padsegmenten geen een dubbele aanhalingstekens als escapeteken voor elk pad is vereist. U kunt bijvoorbeeld een pad voor toevoegen **titel /?** in plaats van **/ 'title' /?**.
* Het hoofdpad die staat voor 'alle paden' kan worden weergegeven als  **/ \***  (in aanvulling op  **/** ).

Hebt u code die voorziet in verzamelingen met een aangepast indexeringsbeleid geschreven met versie 1.1.0 van de .NET SDK of een eerdere versie, als u wilt verplaatsen naar SDK-versie 1.2.0, moet u uw toepassingscode voor het afhandelen van deze wijzigingen. Als u geen code hebt die indexeringsbeleid configureert of als u van plan bent om door te gaan met een eerdere versie van de SDK niet te worden gewijzigd.

Hier volgt een voorbeeld van een aangepast indexeringsbeleid geschreven met behulp van REST API-versie 2015-06-03, gevolgd door de dezelfde indexeringsbeleid geschreven met behulp van de eerdere versie 2015-04-08 van de REST-API voor een praktische vergelijking.

**Huidige indexeren JSON (REST-API versie 2015-06-03) van het beleid**

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


**Eerder indexeren JSON (REST-API versie 2015-04-08) van het beleid**

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


## <a name="next-steps"></a>Volgende stappen
Zie de volgende koppelingen voor index beleid management voorbeelden en voor meer informatie over de Azure DB die Cosmos-querytaal:

* [SQL-API .NET index management-codevoorbeelden](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [SQL-API REST verzameling bewerkingen](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [De query met behulp van SQL](sql-api-sql-query.md)

