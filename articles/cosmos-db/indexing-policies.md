---
title: Azure Cosmos DB indexeringsbeleid | Microsoft Docs
description: Begrijp hoe indexering werkt in Azure Cosmos DB. Informatie over het configureren en wijzigen van het indexeringsbeleid voor automatische indexering en betere prestaties.
keywords: hoe indexering werkt, automatische indexering, database indexeren
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: ae2c6b6a53c6a195bbc79a5776161aab07e42f3d
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215261"
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Hoe gaat de gegevens van Azure Cosmos DB indexeren?

Standaard zijn alle gegevens van Azure Cosmos DB is geïndexeerd. Hoewel veel klanten graag al uw Azure Cosmos DB automatisch verwerkt alle aspecten van het indexeren, kunnen zijn, kunt u een aangepaste *indexeringsbeleid* voor verzamelingen tijdens het maken in Azure Cosmos DB. Indexering beleidsregels in Azure Cosmos DB zijn flexibeler en krachtiger zijn dan secundaire indexen die worden aangeboden in een andere database-platforms. U kunt in Azure Cosmos DB, ontwerpen en aanpassen van de vorm van de index zonder dat dit ten koste gaat van flexibiliteit van schema's. 

Als u wilt weten hoe indexering werkt in Azure Cosmos DB, is het belangrijk om te weten dat wanneer u indexeringsbeleid beheert, kunt u fijnmazig wisselwerking tussen opslagoverhead, schrijf- en querydoorvoer en queryconsistentie.  

In de volgende video toont de Azure Cosmos DB Program Manager Andrew Liu de automatische indexering mogelijkheden en af te stemmen en het indexeringsbeleid configureren voor uw Azure Cosmos DB-container Azure Cosmos DB. 

>[!VIDEO https://www.youtube.com/embed/uFu2D-GscG0]

In dit artikel nemen we bekijken aan Azure Cosmos DB indexeringsbeleid, hoe u indexeringsbeleid en de bijbehorende wisselwerking aanpassen. 

Na het lezen van dit artikel, zal het mogelijk om de volgende vragen te beantwoorden:

* Hoe kan ik de eigenschappen wilt opnemen of uitsluiten van het indexeren overschrijven?
* Hoe kan ik de index voor uiteindelijke-updates configureren?
* Hoe kan ik configureren om uit te voeren van de ORDER BY of bereik-query's te indexeren?
* Hoe kan ik wijzigingen aanbrengen aan het indexeringsbeleid van een verzameling?
* Hoe ik opslag en prestaties van verschillende beleidsregels voor indexering vergelijken?

## Het indexeringsbeleid van een verzameling aanpassen <a id="CustomizingIndexingPolicy"></a>  
U kunt de wisselwerking tussen opslag, schrijf- en prestaties van query's en queryconsistentie aanpassen door de standaardbeleidsregels voor indexering van beleid op een Azure Cosmos DB-verzameling te overschrijven. U kunt de volgende aspecten:

* **Opnemen of uitsluiten van paden naar en van de index voor documenten en**. U kunt uitsluiten of opnemen van specifieke documenten in de index bij het invoegen of vervangen door de documenten in de verzameling. U kunt ook opnemen of uitsluiten van specifieke JSON-eigenschappen, ook wel genoemd *paden*, documenten die zijn opgenomen in een index worden geïndexeerd. Paden bevatten jokertekenpatronen.
* **Configureren van verschillende typen van de index**. Voor elk pad opgenomen, kunt u het type index dat het pad is vereist voor een verzameling opgeven. U kunt opgeven dat het type van de index op basis van gegevens van het pad, de verwachte queryworkload en een numerieke waarde/tekenreeks "precisie."
* **Configureren van index update modi**. Azure Cosmos DB ondersteunt drie indexering modi: consistente, vertraagde, en niets in. U kunt de indexering modi via het indexeringsbeleid kunt configureren op een Azure Cosmos DB-verzameling. 

De volgende Microsoft .NET-codefragment laat zien hoe in te stellen van een aangepast indexeringsbeleid wanneer u een verzameling maken. In dit voorbeeld wordt het beleid met een bereik-index voor tekenreeksen en getallen instellen op de maximale precisie. U kunt dit beleid gebruiken voor het uitvoeren van de ORDER BY-query's op tekenreeksen.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> De JSON-schema voor het indexeringsbeleid gewijzigd met de release van REST API-versie 2015-06-03. Met deze release ondersteunt het JSON-schema voor het indexeringsbeleid bereik indexen op tekenreeksen. .NET SDK 1.2.0 en Java, Python en Node.js-SDK's 1.1.0 ondersteuning voor het nieuwe beleidsschema. Eerdere versies van de SDK gebruikt de REST-API-versie 2015-04-08. Ze ondersteunen het eerdere schema voor het indexeringsbeleid.
> 
> Standaard indexeert Azure Cosmos DB alle eigenschappen van een verbindingsreeks in documenten consistent met een hash-index. Het indexeren van alle numerieke eigenschappen in documenten consistent met een bereik-index.  
> 
> 

### <a name="customize-the-indexing-policy-in-the-portal"></a>Het indexeringsbeleid in de portal aanpassen

U kunt het indexeringsbeleid van een verzameling in de Azure-portal wijzigen: 

1. In de portal, gaat u naar uw Azure Cosmos DB-account en selecteer vervolgens uw verzameling. 
2. Selecteer in het navigatiemenu links **instellingen**, en selecteer vervolgens **Indexeringsbeleid**. 
3. Onder **Indexeringsbeleid**, gewenste indexeringsbeleid wijzigen en selecteer vervolgens **OK**. 

### Database indexering modi <a id="indexing-modes"></a>  
Azure Cosmos DB ondersteunt drie indexering modi die u via het indexeringsbeleid op een Azure Cosmos DB-verzameling kunt configureren: consistente, vertraagde, en niets in.

**Consistente**: als een Azure Cosmos DB-verzameling-beleid Consistent is, de query's op een specifieke Azure Cosmos DB-verzameling volgt u de dezelfde consistentieniveau als is opgegeven voor de punt-leesbewerkingen (sterk, gebonden veroudering, sessie of uiteindelijke). De index wordt synchroon als onderdeel van de update van het document (invoegen, vervangen, bijwerken en verwijderen van een document in een Azure Cosmos DB-verzameling) bijgewerkt.

Consistente indexeren biedt ondersteuning voor consistente-query's leiden mogelijk vermindering in de doorvoer van schrijfbewerkingen. Deze verlaging is een functie van de unieke paden die moeten worden geïndexeerd en het 'consistentieniveau'. Consistente indexering modus is ontworpen voor "snel query schrijven onmiddellijk" werklasten.

**Vertraagde**: de index wordt asynchroon bijgewerkt wanneer een Azure Cosmos DB-verzameling quiescent, dat wil zeggen, is wanneer de doorvoercapaciteit van de verzameling niet volledig is verbruikt voor het bieden van aanvragen van gebruikers.  Houd er rekening mee dat u inconsistente resultaten krijgt mogelijk omdat gegevens die zijn opgenomen en langzaam worden geïndexeerd. Dit betekent dat uw aantal query's of de resultaten van specifieke query mogelijk niet consistent of herhaalbare op de opgegeven tijd. 

De index is meestal in de modus bijwerken met opgenomen gegevens. Tijd naar live (TTL) wordt gewijzigd met Lazy indexeren, resultaat in de index wordt verwijderd en opnieuw wordt gemaakt. Hierdoor wordt het aantal en de query-resultaten inconsistent voor een bepaalde periode. De meeste Azure Cosmos DB-accounts moeten de consistente indexering modus gebruiken.

**Geen**: een verzameling met een geen Indexmodus geen index die is gekoppeld heeft. Dit wordt vaak gebruikt als Azure Cosmos DB wordt gebruikt als een sleutel / waarde-opslag en documenten worden gebruikt alleen door de ID-eigenschap. 

> [!NOTE]
> Configureren van het indexeringsbeleid met geen heeft een neveneffect van elke bestaande index verwijderen. Gebruik deze optie als uw patronen voor databasetoegang alleen-ID is vereist of self link.
> 
> 

De volgende tabel ziet u de consistentie voor query's op basis van de indexing-modus (consistente en Lazy) geconfigureerd voor de verzameling en het consistentieniveau die is opgegeven voor de queryaanvraag. Dit geldt voor query's die gemaakt met behulp van een interface: REST-API, SDK's, of vanuit opgeslagen procedures en triggers. 

|Consistentie|Indexering modus: Consistent|Indexering modus: vertraagde|
|---|---|---|
|Sterk|Sterk|Mogelijk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|
|Sessie|Sessie|Mogelijk|
|Mogelijk|Mogelijk|Mogelijk|

Azure Cosmos DB foutmelding een voor query's op verzamelingen die nog een indexeren modus niet hebt gemaakt. Query's kunnen nog steeds worden uitgevoerd als scans via de expliciete **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie vragen via de .NET SDK. Sommige queryfuncties, zoals ORDER BY worden niet ondersteund als scans met **EnableScanInQuery**.

De volgende tabel ziet u de consistentie voor query's op basis van de indexing-modus (consistente, Lazy en geen) als **EnableScanInQuery** is opgegeven.

|Consistentie|Indexeren van modus: Consistent|Indexeren van modus: vertraagde|Indexeren van modus: geen|
|---|---|---|---|
|Sterk|Sterk|Mogelijk|Sterk|
|Gebonden veroudering|Gebonden veroudering|Mogelijk|Gebonden veroudering|
|Sessie|Sessie|Mogelijk|Sessie|
|Mogelijk|Mogelijk|Mogelijk|Mogelijk|

Een Azure Cosmos DB-verzameling de volgende code voorbeeld ziet hoe maken met behulp van de .NET-SDK met consistente indexering voor alle invoegingen van het document.

     // Default collection creates a Hash index for all string fields and a Range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index paden
Azure Cosmos DB-JSON-documenten en de index als structuren modellen. U kunt afstemmen voor beleid voor paden in de structuur. In documenten, kunt u de paden die u wilt opnemen of uitsluiten van het indexeren. Dit kan bieden verbeterde schrijven prestaties en lagere indexopslag voor scenario's waarin de querypatronen vooraf bekend.

Index paden beginnen met de hoofdpartitie (/) en meestal eindigen met de? Wildcard-operator. Dit geeft aan dat er meerdere mogelijke waarden voor het voorvoegsel zijn. Bijvoorbeeld voor het bieden van SELECT * FROM Families F waar F.familyName = 'Andersen', moet u een pad van de index voor /familyName/ opnemen? in het beleid van de index van de verzameling.

Index paden kunt ook de \* jokertekens operator op die het gedrag voor paden recursief onder het voorvoegsel opgeven. Bijvoorbeeld: / nettolading / * uitsluiten alles onder de eigenschap van de nettolading van het indexeren kan worden gebruikt.

Hier volgen de algemene patronen voor het opgeven van index paden:

| Pad                | Beschrijving/use-case                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Het standaardpad voor de verzameling. Recursieve en is van toepassing op de structuur van de hele document.                                                                                                                                                                                                                                   |
| / prop /?             | Index-pad voor het uitvoeren van query's zoals de volgende (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop > 5<br><br>Selecteer uit verzameling c ORDER BY c.prop                                                                       |
| / prop / *             | Pad van de index voor alle paden onder het opgegeven label. Werkt met de volgende query 's<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop.subprop > 5<br><br>Selecteer uit verzameling c waar c.prop.subprop.nextprop = "waarde"<br><br>Selecteer uit verzameling c ORDER BY c.prop         |
| / Eigenschappen / [] /?         | Index-pad is vereist voor het beheer iteratie en JOIN-query's op matrices met hoeken, zoals ["a", "b", "c"]:<br><br>Selecteer taggen van code IN collection.props waar tag = "waarde"<br><br>Selecteer tag van verzameling c JOIN-tag IN c.props waar code > 5                                                                         |
| /props/ [] /subprop/? | Pad van de index is vereist om herhaling van dienst en JOIN-query's op matrices met objecten, zoals [{subprop: "a"}, {subprop: "b"}]:<br><br>Selecteer taggen van code IN collection.props waar tag.subprop = "waarde"<br><br>Selecteer taggen van verzameling c JOIN-tag IN c.props waar tag.subprop = "waarde"                                  |
| / prop/subprop /?     | Index-pad voor het uitvoeren van query's (met de typen hash- of -bereik, respectievelijk):<br><br>Selecteer uit verzameling c waar c.prop.subprop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Als u aangepaste index paden instelt, u bent verplicht om op te geven van de standaardregel voor indexering voor de hele document-structuur die wordt aangeduid met de speciale pad ' / * '. 
> 
> 

Het volgende voorbeeld wordt een specifiek pad met bereik index en de precisiewaarde van een aangepaste van 20-bytes:

```
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
```

Wanneer een pad voor het indexeren wordt toegevoegd, zijn de cijfers en tekenreeksen binnen deze paden worden geïndexeerd. Dus zelfs als u de indexering voor alleen tekenreeksen hebt gedefinieerd, Azure Cosmos DB standaarddefinitie voor getallen ook voegt. Met andere woorden, Azure Cosmos DB biedt de mogelijkheid voor pad uitsluiten voor indexeringsbeleid, maar niet uitsluiten voor een specifiek pad te typen. Hieronder volgt een voorbeeld, houd er rekening mee dat alleen dat is één index is opgegeven voor beide paden (pad = "/ * ' en het pad =" /\"attr1\"/? "), maar het numeriek gegevenstype wordt ook toegevoegd aan het resultaat.

```
var indices = new[]{
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 }// <- note: only 1 index specified
                    },
                    Path =  "/*"
                },
                new IncludedPath  {
                    Indexes = new Collection<Index>
                    {
                        new RangeIndex(DataType.String) { Precision = 3 } // <- note: only 1 index specified
                    },
                    Path =  "/\"attr1\"/?"
                }
            };...

            foreach (var index in indices)
            {
                documentCollection.IndexingPolicy.IncludedPaths.Add(index);
            }
```

Resultaat van het maken van een index:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        },
        {
            "path": "/\"attr\"/?",
            "indexes": [
                {
                    "kind": "Range",
                    "dataType": "String",
                    "precision": 3
                },
                {
                    "kind": "Range",
                    "dataType": "Number",
                    "precision": -1
                }
            ]
        }
    ],
}
```

### <a name="index-data-types-kinds-and-precisions"></a>Index-gegevenstypen, typen en Precision-systemen
U hebt meerdere opties bij het configureren van het indexeringsbeleid voor een pad. U kunt een of meer indexering definities voor elk pad opgeven:

* **Gegevens van het type**: tekenreeks, getal, punt, Polygon of LineString (kan slechts één vermelding per gegevenstype per pad bevatten).
* **Type index**: Hash (gelijkheid query's), bereik (gelijkheid, bereik of ORDER BY-query's) of Spatial (ruimtelijke query's).
* **Precisie**: voor een hashindex dit varieert van 1 tot 8 voor tekenreeksen en cijfers. De standaardwaarde is 3. Voor de index van een bereik, kan deze waarde -1 (maximumprecisie) zijn. Dit kan verschillen tussen 1 en 100 (maximumprecisie) voor de tekenreeks of numerieke waarden.

#### <a name="index-kind"></a>Index-type
Azure Cosmos DB biedt ondersteuning voor hash-index en bereik index typen voor elk pad die kan worden geconfigureerd voor tekenreeks of getal gegevenstypen, of beide.

* **Hash** biedt ondersteuning voor efficiënte gelijkheid en JOIN-query's. Voor de meeste gevallen nodig geen Hash-indexen een grotere precisie dan de standaardwaarde van 3 bytes. Het gegevenstype mag tekenreeks of getal.
* **Bereik** biedt ondersteuning voor efficiënte gelijkheid query's, bereik-query's (met behulp van >, <>, =, < =,! =), en ORDER BY-query's. ORDER By-query's standaard vereist ook maximale index precisie (-1). Het gegevenstype mag tekenreeks of getal.

Azure Cosmos DB ondersteunt ook het type van de ruimtelijke index voor elk pad dat kan worden opgegeven voor de gegevenstypen punt, Polygon of LineString. De waarde in het opgegeven pad moet een geldige GeoJSON-fragment zoals `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Ruimtelijke** ondersteunt efficiënte ruimtelijke (binnen en afstand) query's. Het gegevenstype mag punt, Polygon of LineString.

> [!NOTE]
> Azure Cosmos DB biedt ondersteuning voor automatische indexering van punt veelhoek en LineString gegevenstypen.
> 
> 

Hier worden de soorten ondersteunde index en voorbeelden van query's die ze kunnen worden gebruikt om:

| Index-type | Beschrijving/use-case                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hash       | Hash-via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Hash voor/eigenschappen / [] /? (of / of/eigenschappen /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer taggen van verzameling c JOIN-tag IN c.props waar tag = 5                                                                                                                       |
| Bereik      | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop > 5<br><br>Selecteer uit verzameling c ORDER BY c.prop                                                                                                                                                                                                              |
| Ruimtelijk     | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit de verzameling-c<br><br>WAAR ST_DISTANCE (c.prop, {"type": "Wijst", "coördinaten": [0.0, 10.0]}) < 40<br><br>Selecteer uit verzameling c waar ST_WITHIN(c.prop, {"type": "Polygon",...})--met het indexeren van punten ingeschakeld<br><br>Selecteer uit verzameling c waar ST_WITHIN({"type": "Point",...}, c.prop)--met indexering voor veelhoeken ingeschakeld              |

Standaard wordt een fout geretourneerd voor query's met bereik operators zoals > = als er geen index bereik (van een precisie is) om aan te geven dat een scan die nodig zijn om de query van dienst kan zijn. Bereik-query's kunnen worden uitgevoerd zonder een bereik-index met behulp van de **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie vragen via de .NET SDK. Als er geen andere filters in de query die Azure Cosmos DB de index gebruiken kunt om te filteren op basis van, wordt geen fout geretourneerd.

Dezelfde regels gelden voor ruimtelijke query's. Standaard wordt een fout geretourneerd voor ruimtelijke query's als er geen ruimtelijke index, en er zijn geen andere filters die kunnen worden weergegeven in de index. Ze kunnen worden uitgevoerd als een scan met behulp van **x-ms-documentdb-enable-scan** of **EnableScanInQuery**.

#### <a name="index-precision"></a>Index precisie
U kunt index precisie wisselwerking tussen de overhead van indexopslag en prestaties van query's maken. Voor getallen, wordt u aangeraden de standaardconfiguratie van de precisie van-1 (maximum). Omdat de getallen 8 bytes in JSON, is dit gelijk aan een configuratie van 8 bytes. Het kiezen van een lagere waarde voor de precisie, zoals 1 tot en met 7, index betekent dat de waarden binnen bepaalde bereiken worden toegewezen aan dezelfde-vermelding. Daarom u opslagruimte in de index verminderen, maar het uitvoeren van query's mogelijk om meer documenten te verwerken. Als gevolg daarvan kunnen verbruikt deze meer doorvoer op basis van aanvraageenheden.

Index precisie-configuratie bevat meer praktische toepassingen met bereiken van de tekenreeks. Omdat tekenreeksen elke willekeurige lengte zijn kunnen, kan de prestaties van tekenreeks bereik-query's door de keuze van de precisie van de index beïnvloeden. Het ook mogelijk van invloed op de hoeveelheid opslagruimte index die is vereist. Tekenreeks bereik indexen kunnen worden geconfigureerd met 1 tot en met 100 of -1 (maximum). Als u uitvoeren ORDER BY query's op de eigenschappen van een verbindingsreeks wilt, moet u een precisie van-1 voor de bijbehorende paden opgeven.

Ruimtelijke indexen gebruik altijd de precisie van de index standaard voor alle typen (Point, LineString en Veelhoek). De precisie van de index standaard voor ruimtelijke indexen kan niet worden overschreven. 

Het volgende voorbeeld ziet hoe u kunt de precisie voor bereik indexen in een verzameling verhogen met behulp van de .NET SDK. 

**Een verzameling met een nauwkeurigheid van de aangepaste index maken**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for strings to Range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB, wordt er een fout geretourneerd wanneer een query maakt gebruik van ORDER BY, maar beschikt niet over een bereik-index op basis van het aangevraagde pad met de maximale precisie. 
> 
> 

Op deze manier kunt u volledig paden uitsluiten van het indexeren. In het volgende voorbeeld laat zien hoe u kunt uitsluiten van een hele sectie van de documenten (een *substructuur*) niet kan indexeren met behulp van de \* wildcard-operator.

    var excluded = new DocumentCollection { Id = "excludedPathCollection" };
    excluded.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    excluded.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opt-in-and-opt-out-of-indexing"></a>Aanmelden en afmelden voor indexeren
U kunt kiezen of u wilt dat de verzameling moeten worden alle documenten automatisch te indexeren. Standaard alle documenten automatisch worden geïndexeerd, maar u kunt automatische indexering uitschakelen. Wanneer indexeren is uitgeschakeld, documenten zijn toegankelijk via alleen hun Self link-elementen of door query's met behulp van het document-id.

Met automatische indexering uitgeschakeld, kunt u nog steeds selectief alleen specifieke documenten toevoegen aan de index. Daarentegen, kunt u automatische indexering voor laten en selectief wilt uitsluiten van specifieke documenten. Indexeren in-of uitschakelen configuraties zijn nuttig wanneer u hebt alleen een subset van documenten die moeten worden opgevraagd.

Het volgende voorbeeld toont hoe u een document expliciet opnemen met behulp van de [SQL-API .NET SDK](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet) en de [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) eigenschap.

    // If you want to override the default collection behavior to either
    // exclude (or include) a document in indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modify-the-indexing-policy-of-a-collection"></a>Het indexeringsbeleid van een verzameling wijzigen
In Azure Cosmos DB, kunt u wijzigingen aanbrengen aan het indexeringsbeleid van een verzameling op elk gewenst moment. Een wijziging in het indexeren van beleid op een Azure Cosmos DB-verzameling kan leiden tot een wijziging in de vorm van de index. De wijziging is van invloed op de paden die kunnen worden geïndexeerd, hun precisie en het consistentiemodel van de index zelf. Een wijziging in het indexeringsbeleid effectief vereist een transformatie van de oude index in een nieuwe index. 

**Online-index transformaties**

![Hoe indexering werkt: Azure Cosmos DB online index transformaties](./media/indexing-policies/index-transformations.png)

Index transformaties worden online gemaakt. Dit betekent dat de basis van de oude beleid geïndexeerde documenten efficiënt worden getransformeerd per het nieuwe beleid *zonder gevolgen voor de beschikbaarheid voor schrijven of de ingerichte doorvoer* van de verzameling. De consistentie van lezen en schrijven van bewerkingen die zijn aangebracht met behulp van de REST-API, SDK's, of vanuit de opgeslagen procedures en triggers niet beïnvloed tijdens de transformatie van de index. Is er geen vertragingen of downtime voor uw apps wanneer u een indexeringsbeleid wijzigen.

Tijdens de tijd die de transformatie van de index uitgevoerd wordt, zijn query's echter uiteindelijk consistent, ongeacht de indexering modus-configuratie (consistente of Lazy). Dit geldt ook voor query's uit alle interfaces: de REST-API, SDK's, en vanuit opgeslagen procedures en triggers. Net als met Lazy indexeren, wordt index-transformatie asynchroon uitgevoerd op de achtergrond op de replica's met behulp van de ongebruikte bronnen die beschikbaar voor een specifieke replica zijn. 

Index transformaties ook worden aangebracht in de plaats. Azure Cosmos DB biedt geen twee kopieën van de index en het wisselen van de oude index met de nieuwe onderhouden. Dit betekent dat er geen extra schijfruimte is vereist of die in uw verzamelingen worden gebruikt tijdens de index transformaties.

Wanneer u indexeringsbeleid wijzigt, worden wijzigingen worden toegepast om te verplaatsen van de oude index naar de nieuwe voornamelijk gebaseerd op de indexering modus-configuraties. Indexering configuraties spelen een grotere rol dan andere waarden, zoals opgenomen/uitgesloten paden, index-typen en Precision-systemen. 

Als uw oude en nieuwe beleidsregels voor beide consistente indexeren, voert Azure Cosmos DB een transformatie online-index. U kunt een andere indexering beleidswijziging met consistente indexering modus terwijl de transformatie uitgevoerd wordt niet toepassen. U kunt echter verplaatsen naar Lazy of geen indexering modus tijdens een transformatie uitgevoerd wordt: 

* Wanneer u naar Lazy verplaatst, wordt de beleidswijziging index onmiddellijk van kracht. Azure Cosmos DB begint de index opnieuw asynchroon te maken. 
* Wanneer u op geen verplaatst, wordt de index onmiddellijk verwijderd. Verplaatsen naar geen is handig als u wilt een transformatie in uitvoering annuleren en opnieuw beginnen met een ander beleid voor indexering. 

Het volgende codefragment laat zien hoe het indexeringsbeleid van een verzameling van consistente indexering modus naar de vertraagde indexering modus wijzigen. Als u de .NET SDK, u kunt een vliegende start een indexering beleid wijzigen met behulp van de nieuwe **ReplaceDocumentCollectionAsync** methode.

**Indexeringsbeleid van consistente naar Lazy wijzigen**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);

**Voortgang van de index transformatie volgen**

U kunt de voortgang van het percentage van de index-transformatie naar een consistente index volgen met behulp van de **IndexTransformationProgress** antwoord-eigenschap van een **ReadDocumentCollectionAsync** aanroepen. Andere SDK's en de REST-API, ondersteuning voor equivalente eigenschappen en methoden voor het aanbrengen van wijzigingen in de indexing beleid. U kunt de voortgang van een index transformatie naar een consistente index controleren door het aanroepen van **ReadDocumentCollectionAsync**: 

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
> * De **IndexTransformationProgress** eigenschap is van toepassing alleen tijdens het omzetten van een consistente index. Gebruik de **ResourceResponse.LazyIndexingProgress** eigenschap voor het bijhouden van transformaties naar een vertraagde index.
> * De **IndexTransformationProgress** en de **LazyIndexingProgress** eigenschappen alleen voor een niet-gepartitioneerde verzameling, dat wil zeggen, een verzameling die is gemaakt zonder een partitiesleutel worden ingevuld.
>

U kunt de index voor een verzameling verwijderen door naar de geen indexeren modus te verplaatsen. Dit kan een handig hulpmiddel voor operationele zijn als u wilt annuleren van een transformatie wordt uitgevoerd en vervolgens meteen een nieuwe.

**De index voor een verzameling verwijderen**

    // Switch to Lazy indexing mode.
    Console.WriteLine("Dropping index by changing to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Als zou u indexering beleidswijzigingen aanbrengen in uw Azure Cosmos DB-verzamelingen Hier volgen de meest voorkomende gebruiksvoorbeelden:

* Consistente resultaten dienen tijdens normale werking, maar terugvallen op vertraagde indexering modus tijdens bulkimport voor gegevens.
* Start met behulp van nieuwe indexering functies op uw huidige Azure Cosmos DB-verzamelingen. Bijvoorbeeld, kunt u gebruiken georuimtelijke uitvoeren van query's, die de aard van de ruimtelijke index, vereist of ORDER BY / bereik-query's, waarvoor de tekenreeks bereik index type string.
* De eigenschappen die moeten worden geïndexeerd handmatig selecteren en ze na verloop van tijd wijzigen.
* Stem indexering precisie om queryprestaties te verbeteren of om te beperken van de opslag die wordt gebruikt.

> [!NOTE]
> Indexeringsbeleid wijzigen met behulp van **ReplaceDocumentCollectionAsync**, moet u versie 1.3.0 of een latere versie van de .NET SDK gebruiken.
> 
> Voor transformatie van de index is voltooid, zorg ervoor dat er voldoende vrije ruimte beschikbaar op de verzameling. Als de verzameling de opslaglimiet bereikt, wordt de index-transformatie is onderbroken. Index transformatie hervat automatisch wanneer opslagruimte beschikbaar, bijvoorbeeld is, als u een aantal documenten verwijdert.
> 
> 

## <a name="performance-tuning"></a>Prestaties afstemmen
De SQL-API's bevatten informatie over metrische gegevens voor prestaties, zoals de indexopslag die wordt gebruikt en de doorvoer-kosten (request units) voor elke bewerking. U kunt deze informatie gebruiken om te vergelijken van verschillende indexering beleidsregels, en voor het afstemmen van prestaties.

Uitvoeren om te controleren of de opslaglimiet en het gebruik van een verzameling, een **HEAD** of **ophalen** aanvraag indient voor de verzameling-resource. Vervolgens, controleert de **x-ms-request-quotum** en de **x-ms-request-gebruik** headers. In de .NET SDK, de [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) en [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) eigenschappen in [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) deze overeenkomende waarden bevatten.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


Voor het meten van de overhead van indexering voor elke schrijfbewerking (maken, bijwerken of verwijderen), Inspecteer de **x-ms-request-kosten in rekening gebracht** header (of een vergelijkbare [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) eigenschap in [ ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) in de .NET SDK) voor het meten van het aantal aanvraageenheden die worden gebruikt door deze bewerkingen.

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

## <a name="changes-to-the-indexing-policy-specification"></a>Wijzigingen in de indexing beleidsspecificatie
Een wijziging in het schema voor het indexeringsbeleid is geïntroduceerd voor 7 juli 2015 met de REST API-versie 2015-06-03. De bijbehorende klassen in de SDK-versies hebben nieuwe implementaties zodat deze overeenkomt met het schema. 

De volgende wijzigingen zijn geïmplementeerd in de JSON-specificatie:

* Indexeringsbeleid ondersteunt bereik indexen voor tekenreeksen.
* Elk pad hebben meerdere definities van de index. Het kan een voor elk gegevenstype hebben.
* Indexeren van precisie ondersteunt 1 t/m 8 voor getallen, 1 tot en met 100 voor tekenreeksen en -1 (maximumprecisie).
* Padsegmenten vereisen een dubbele aanhalingstekens als escape voor elk pad. U kunt bijvoorbeeld een pad voor toevoegen   **/titel /?** in plaats van **/ "title" /?**.
* Het hoofdpad die staat voor 'alle paden' kan worden weergegeven als **/ \*** (in aanvulling op **/**).

Als u code die verzamelingen bepalingen met een aangepast indexeringsbeleid die zijn geschreven met versie 1.1.0 van de .NET SDK of een eerdere versie hebt, als u wilt verplaatsen naar de SDK-versie 1.2.0, moet u de code van uw toepassing voor het afhandelen van deze wijzigingen. Als u geen code hebt die indexeringsbeleid configureert of als u van plan bent om door te gaan met behulp van een eerdere versie van de SDK, er zijn geen wijzigingen vereist zijn.

Hier volgt een voorbeeld van een aangepast indexeringsbeleid ontwikkeld met behulp van REST API-versie 2015-06-03, gevolgd door de dezelfde indexeringsbeleid ontwikkeld met behulp van de eerdere REST-API-versie 2015-04-08 voor een praktische vergelijking.

**Huidige indexeringsbeleid JSON (REST API-versie 2015-06-03)**

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


**Indexeringsbeleid eerder JSON (REST API-versie 2015-04-08)**

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
Zie voor voorbeelden van index beleid en voor meer informatie over de querytaal van Azure Cosmos DB de volgende koppelingen:

* [SQL-API .NET index management-codevoorbeelden](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
* [SQL-API-REST-verzameling-bewerkingen](https://msdn.microsoft.com/library/azure/dn782195.aspx)
* [Query's uitvoeren met SQL](sql-api-sql-query.md)

