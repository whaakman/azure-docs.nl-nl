---
title: Full-text search engine (Lucene) architectuur in Azure Search | Microsoft Docs
description: Uitleg van Lucene query verwerken en het document ophalen van concepten voor zoeken in volledige tekst, met betrekking tot Azure Search.
manager: jlembicz
author: yahnoosh
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: jlembicz
ms.openlocfilehash: 55d361e90dbc5fe48bc118088a6f859d096048ff
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036867"
---
# <a name="how-full-text-search-works-in-azure-search"></a>Hoe vol tekstzoekopdrachten werkt in Azure Search

In dit artikel is voor ontwikkelaars die een beter begrip van de werking van Lucene zoeken in volledige tekst in Azure Search nodig hebben. Voor tekst-query's, Azure Search naadloos verwachte resultaten in de meeste gevallen levert, maar soms krijgt u mogelijk een resultaat met een hoog 'M 'uit'. In deze situaties, dat u een achtergrond in de vier fasen van de uitvoering van Lucene-query (query's uitvoeren bij het parseren, lexicale analyse, document-koppeling, score) kunt u specifieke wijzigingen in query-parameters of indexconfiguratie die de gewenste verzorgt identificeren resultaat. 

> [!Note] 
> Azure Search Lucene gebruikt voor zoeken in volledige tekst, maar Lucene-integratie is niet volledig. We selectief beschikbaar maken en uitbreiden van Lucene-functionaliteit waarmee de scenario's die belangrijk dat u Azure Search. 

## <a name="architecture-overview-and-diagram"></a>Overzicht van de architectuur en -diagram

Verwerking van een zoekquery voor volledige tekst begint met het parseren van de tekst van de query om uit te pakken zoektermen. De zoekmachine maakt gebruik van een index om documenten met een overeenkomende te halen. Afzonderlijke querytermen zijn soms opgedeeld en geregenereerd naar nieuwe formulieren te casten van een bredere net over wat kan worden beschouwd als een mogelijke overeenkomst. Een resultatenset wordt vervolgens gesorteerd op een relevantiescore toegewezen aan elke afzonderlijke overeenkomende document. Die aan de bovenkant van de ranglijst staat keert terug naar de aanroepende toepassing.

Aangepast, bestaat uitvoeren van query's uit vier fasen: 

1. Parseren van de query 
2. Lexicale analyse 
3. Voor het document ophalen 
4. Scoren 

Het volgende diagram illustreert de onderdelen die zijn gebruikt voor het verwerken van een zoekaanvraag. 

 ![Architectuurdiagram van Lucene-query in Azure Search][1]


| Belangrijkste onderdelen | Functionele beschrijving | 
|----------------|------------------------|
|**Query-parsers** | Zoektermen te scheiden van standaardoperators voor query's en de querystructuur (een boomstructuur van de query) worden verzonden naar de zoekmachine maken. |
|**Analyzers** | Lexicale analyse uitvoeren op zoektermen. Dit proces kan betrekking hebben op transformeren, te verwijderen of uitbreiden van zoektermen. |
|**Index** | Een efficiënte gegevensstructuur voor het opslaan en organiseren doorzoekbare voorwaarden die zijn geëxtraheerd uit geïndexeerde documenten. |
|**Zoekmachine** | Opgehaald en scores overeenkomende documenten op basis van de inhoud van de omgekeerde index. |

## <a name="anatomy-of-a-search-request"></a>De anatomie van een zoekaanvraag

Een zoekaanvraag is een complete specificatie van wat moet worden geretourneerd in een resultatenset. Het is een lege query zonder criteria van welke aard dan ook in de meest eenvoudige vorm. Een voorbeeld van een realistischer bevat parameters, verschillende querytermen, misschien binnen het bereik van bepaalde velden, met mogelijk een filterexpressie en de volgorde van regels.  

Het volgende voorbeeld wordt een zoekaanvraag die u met behulp van Azure Search verzenden kunt de [REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2017-11-11 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

Voor deze aanvraag zijn de zoekmachine doet het volgende:

1. Documenten gefilterd waarbij de prijs is ten minste $60 en is minder dan $300.
2. De query wordt uitgevoerd. In dit voorbeeld wordt de query bestaat van zinnen en voorwaarden: `"Spacious, air-condition* +\"Ocean view\""` (gebruikers normaal gesproken geen leestekens in te voeren, maar opneemt in het voorbeeld, kunnen we waarin wordt uitgelegd hoe analyzers verwerkt). Voor deze query is de zoekmachine scant de beschrijving en titelvelden opgegeven in `searchFields` voor documenten met 'In de Indische Oceaan weergeven', en daarnaast de term 'groot', of op termen die met het voorvoegsel beginnen 'air-condition'. De `searchMode` parameter wordt gebruikt om op een term (standaard) of al deze gevallen waarbij een term niet expliciet vereist is (`+`).
3. De resulterende set hotels door nauwe binding met een bepaalde geo-locatie en vervolgens wordt geretourneerd naar de aanroepende toepassing de orders. 

Het merendeel van dit artikel is over de verwerking van de *zoekquery*: `"Spacious, air-condition* +\"Ocean view\""`. Filteren en bestellen vallen buiten het bereik. Zie voor meer informatie de [zoeken-API-referentiedocumentatie](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>Fase 1: Parseren de Query 

Zoals is vermeld, is de query-tekenreeks in de eerste regel van de aanvraag: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

De queryparser scheidt operators (zoals `*` en `+` in het voorbeeld) uit zoekopdracht voorwaarden en de zoekopdracht in deconstructs *subquery's* van een ondersteund type: 

+ *term query* voor zelfstandige voorwaarden (zoals groot)
+ *woordgroep query* voor tussen aanhalingstekens voorwaarden (zoals in de Indische Oceaan weergeven)
+ *voorvoegsel query* voor voorwaarden, gevolgd door een operator voorvoegsel `*` (zoals air-condition)

Zie voor een volledige lijst van ondersteunde querytypen [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Operators die zijn gekoppeld aan een subquery bepalen of de query 'moet' of 'moet' voldaan opdat een document om te worden beschouwd als een overeenkomst. Bijvoorbeeld, `+"Ocean view"` 'moet' is vanwege de `+` operator. 

De queryparser is hergestructureerd de subquery's in een *querystructuur* (een interne structuur voor de query) waar deze naar de zoekmachine doorstuurt op. In de eerste fase van het parseren van de query, uitziet de querystructuur van de.  

 ![Booleaanse waarde searchmode elke query][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Ondersteund parsers: eenvoudig en volledige Lucene 

 Azure Search wordt aangegeven dat twee verschillende querytalen `simple` (standaard) en `full`. Door in te stellen de `queryType` parameter met uw zoekopdracht u instellen dat de queryparser welke querytaal u kiest zodat deze over het interpreteren van de operators en syntaxis. De [eenvoudige query-taal](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuïtief en robuuste, vaak geschikt is voor de invoer van de gebruiker als interpreteren-is zonder client-side-verwerking. Het ondersteunt standaardoperators voor query's van webzoekmachines bekend. De [volledige Lucene-querytaal](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), die wordt geleverd door in te stellen `queryType=full`, wordt de standaardtaal van eenvoudige query uitgebreid met ondersteuning voor meer operators en typen query's, zoals jokerteken, fuzzy, regex en veldgerelateerde query's. Bijvoorbeeld, wordt een reguliere expressie verzonden eenvoudige query-syntaxis geïnterpreteerd als een queryreeks en niet een expressie. De voorbeeldaanvraag in dit artikel maakt gebruik van de volledige Lucene-querytaal.

### <a name="impact-of-searchmode-on-the-parser"></a>Gevolgen van het searchMode in de parser 

Een andere zoekopdracht aanvraagparameter die gevolgen heeft voor het parseren is de `searchMode` parameter. Hiermee kunt u de standaardoperator voor Boole-query's: een (standaard) of alle.  

Wanneer `searchMode=any`, is de standaardoptie, de spatie als scheidingsteken tussen groot en air-condition is of (`||`), gelijk aan het maken van de tekst van de voorbeeld-query: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Expliciete operators, zoals `+` in `+"Ocean view"`, ondubbelzinnige Booleaanse query constructie zijn (de term *moet* overeen). Minder duidelijk wordt beschreven hoe u met het interpreteren van de overige voorwaarden: groot en air-condition. Moet de zoekmachine overeenkomsten worden gevonden voor de weergave in de Indische Oceaan *en* groot *en* air-condition? Of moet deze vinden in de Indische Oceaan weergave plus *uit, één* van de overige voorwaarden? 

Standaard (`searchMode=any`), de zoekmachine wordt ervan uitgegaan dat de bredere interpretatie. Een veld *moet* overeen, zetten op basis van 'of'-semantiek. De structuur van de oorspronkelijke query eerder weergegeven, met de twee 'moet' operations, ziet u de.  

Stel dat we nu `searchMode=all`. In dit geval wordt de ruimte geïnterpreteerd als een 'en'-bewerking. Elk van de overige voorwaarden moet beide aanwezig zijn in het document dat moet worden gekwalificeerd als een overeenkomst. De resulterende voorbeeldquery wordt geïnterpreteerd als volgt: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Een gewijzigde query-structuur voor deze query zou zijn als volgt, waarbij een overeenkomende document is het snijpunt van alle drie subquery's: 

 ![Booleaanse query searchmode alle][3]

> [!Note] 
> Kiezen `searchMode=any` via `searchMode=all` is een aanbevolen beslissing is aangekomen door uit te voeren representatieve query's. Gebruikers die waarschijnlijk operators (algemeen zijn bij het zoeken naar documenten worden opgeslagen) bevat mogelijk intuïtiever resultaten als `searchMode=all` informeert Booleaanse query constructies. Voor meer informatie over de wisselwerking tussen `searchMode` en operators, Zie [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>Fase 2: Lexicale analyse 

Lexicale analyse proces *termijn query's* en *formuleren van query's* nadat de querystructuur van de is opgebouwd. Een analyzer accepteert de invoer van de tekst krijgen tot het door de parser, verwerkt de tekst en vervolgens verzendt terug tokenized voorwaarden kunnen worden opgenomen in de querystructuur van de. 

Is de meestgebruikte vorm van lexicale analyse *linguïstische* welke transformaties query voorwaarden op basis van regels die specifiek zijn voor een bepaalde taal: 

* Een termijn van de query voor de vorm van de hoofdmap van een woord te beperken 
* Verwijderen van niet-essentiële woorden (stopwoorden, zoals 'de' of 'en' in het Engels) 
* Een samengestelde woord verdelen in onderdelen 
* Kleine hoofdlettergebruik van een woord hoofdletters 

Al deze bewerkingen meestal om te wissen van de verschillen tussen de tekstinvoer die is opgegeven door de gebruiker en de voorwaarden die zijn opgeslagen in de index. Dergelijke bewerkingen plaatsvindt verder gaan dan de verwerking van tekst en diepgaande kennis van de taal zelf vereist. Als u wilt toevoegen van deze laag linguïstische bewust te zijn, Azure Search biedt ondersteuning voor een lange lijst [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) van Lucene en Microsoft.

> [!Note]
> Vereisten voor analyse kunnen variëren van minimale voor uitgebreide afhankelijk van uw scenario. U kunt de complexiteit van lexicale analyse beheren door het selecteren van de vooraf gedefinieerde analyzers of door het maken van uw eigen [aangepaste analysefunctie](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Analysefuncties zijn gericht op doorzoekbare velden en zijn opgegeven als onderdeel van de velddefinitie van een. Hiermee kunt u verschillende lexicale analyse op basis van per veld. Niets opgeeft, de *standard* Lucene analyzer wordt gebruikt.

In ons voorbeeld voor analyse, heeft de structuur van de oorspronkelijke query de term 'Spacious', met een hoofdletter "S" en een door komma's die de queryparser wordt geïnterpreteerd als onderdeel van de queryterm die (een door komma's wordt niet beschouwd als een query language-operator).  

Wanneer de analyzer standaard de term verwerkt, wordt "in de Indische Oceaan view" en "groot", kleine letter en verwijder de komma. De structuur van de gewijzigde query ziet er als volgt uit: 

 ![Booleaanse query's uitvoeren met geanalyseerde voorwaarden][4]

### <a name="testing-analyzer-behaviors"></a>Analyzer gedrag testen 

Het gedrag van een analyzer kan worden getest met de [analyseren API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Geef de tekst die u wilt analyseren om te zien wat vermeldingen analyzer wordt gegenereerd. Bijvoorbeeld, als u wilt zien hoe de standaard analyzer de tekst "air-condition" wilt verwerken, kunt u de volgende aanvraag uitgeven:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

De standaard analyzer verdeelt de invoertekst in de volgende twee tokens, ze aantekeningen te maken met kenmerken, zoals het begin en einde offsets (gebruikt voor treffers markeren), evenals hun positie (gebruikt voor het overeenkomende woordgroepen):

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

<a name="exceptions"></a>

### <a name="exceptions-to-lexical-analysis"></a>Uitzonderingen aan lexicale analyse 

Lexicale analyse geldt alleen voor de typen query's waarvoor volledige voorwaarden – een term query of een woordgroep-query. Het is niet van toepassing op de typen query's met een onvolledige – voorvoegsel query, jokertekens query, regex query – of op een fuzzy query. De typen, met inbegrip van de query voorvoegsel met de term query `air-condition*` in ons voorbeeld rechtstreeks aan de querystructuur, overslaan van de fase van de analyse worden toegevoegd. De enige transformatie uitgevoerd op de voorwaarden van deze typen query is locatieargument.

<a name="stage3"></a>

## <a name="stage-3-document-retrieval"></a>Fase 3: Document ophalen 

Het ophalen van document verwijst naar het zoeken van documenten met de bijbehorende voorwaarden in de index. Deze fase is best begrepen door een voorbeeld. Laten we beginnen met een index hotels het volgende eenvoudige schema: 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

Verder wordt ervan uitgegaan dat deze index de volgende vier documenten bevat: 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance to the beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

**Hoe voorwaarden worden geïndexeerd**

Voor meer informatie over het ophalen van, helpt het om te weten van enkele basisbeginselen indexeren. De eenheid voor opslag is een omgekeerde index, één voor elke doorzoekbaar veld. Is een gesorteerde lijst met alle termen uit alle documenten binnen een omgekeerde index. Elke term wordt toegewezen aan de lijst met documenten waarin deze zich, als duidelijk in het onderstaande voorbeeld voordoet.

Als u wilt maken van de voorwaarden van een omgekeerde index, voert de zoekmachine lexicale analyse over de inhoud van documenten, vergelijkbaar met wat er tijdens het verwerken van query's gebeurt:

1. *Teksten* worden doorgegeven aan een analyzer, kleine-indeling, interpunctie, enzovoort, afhankelijk van de configuratie analyzer verwijderd. 
2. *Tokens* zijn de uitvoer van de analyse van tekst.
3. *Voorwaarden* worden toegevoegd aan de index.

Het is algemene, maar niet vereist, voor het gebruik van de dezelfde analyzers voor zoeken en indexeren van bewerkingen, zodat de querytermen zoek meer, zoals de voorwaarden in de index.

> [!Note]
> Azure Search kunt u opgeven van verschillende analyzers voor indexeren en zoeken via extra `indexAnalyzer` en `searchAnalyzer` veld parameters. Als u niets opgeeft, wordt de analyzer instellen met de `analyzer` eigenschap wordt gebruikt voor zowel indexeren en zoeken.  

**Omgekeerde index voor de voorbeelddocumenten**

Teruggaan naar ons voorbeeld voor de **titel** veld, de omgekeerde index er als volgt uitzien:

| Termijn | Lijst met standaarddocumenten |
|------|---------------|
| atman | 1 |
| strand | 2 |
| hotel | 1, 3 |
| in de Indische Oceaan | 4  |
| playa | 3 |
| instantie | 3 |
| Terugtrekken | 4 |

In het titelveld alleen *hotel* wordt weergegeven in de twee documenten: 1, 3.

Voor de **beschrijving** veld, de index is als volgt:

| Termijn | Lijst met standaarddocumenten |
|------|---------------|
| AIR | 3
| en | 4
| strand | 1
| voorwaarde | 3
| vertrouwd | 3
| afstand | 1
| eiland | 2
| kauaʻi | 2
| zich bevindt | 2
| Noord | 2
| in de Indische Oceaan | 1, 2, 3
| van | 2
| op |2
| stille | 4
| ruimten  | 1, 3
| secluded | 4
| wal | 2
| groot | 1
| de | 1, 2
| tot | 1
| weergeven | 1, 2, 3
| lopen | 1
| met | 3


**Overeenkomende querytermen tegen geïndexeerde voorwaarden**

De bovenstaande omgekeerde indexen worden gegeven, gaan we terug naar de voorbeeldquery en Zie hoe overeenkomende documenten gevonden voor de voorbeeldquery. Let erop dat de querystructuur van de uiteindelijke er als uitzien volgt: 

 ![Booleaanse query's uitvoeren met geanalyseerde voorwaarden][4]

Tijdens de uitvoering van de query, afzonderlijke query's worden uitgevoerd op basis van de doorzoekbare velden onafhankelijk van elkaar. 

+ Het komt overeen met 'groot', TermQuery, documenteer 1 (Hotel Atman). 

+ De PrefixQuery "air-condition * ', komt niet overeen met alle documenten. 

  Dit is een probleem dat soms confuses ontwikkelaars. Hoewel de term air-conditioned in het document bestaat, wordt dit verdeeld in twee termen door de analyzer standaard. Intrekken voorvoegsel query's die voor gedeeltelijke termen bevatten, worden niet geanalyseerd. Daarom worden voorwaarden met het voorvoegsel "air-condition" opgezocht in de omgekeerde index en niet gevonden.

+ De PhraseQuery, 'in de Indische Oceaan weergeven', zoekt de termen "in de Indische Oceaan" en "weergeven" en de nabijheid van voorwaarden in het originele document wordt gecontroleerd. Documenten 1, 2 en 3 overeenkomen met deze query in het omschrijvingsveld. U ziet dat document 4 heeft de term in de Indische Oceaan in de titel, maar wordt niet beschouwd als een overeenkomst, omdat we op zoek bent voor de zinsnede 'in de Indische Oceaan weergeven' in plaats van afzonderlijke woorden. 

> [!Note]
> Een zoekopdracht, tenzij u de velden die zijn ingesteld met beperken onafhankelijk tegen alle doorzoekbare velden in de Azure Search-index wordt uitgevoerd de `searchFields` parameter, zoals wordt geïllustreerd in het voorbeeld van de search-aanvraag. Documenten die overeenkomen met in een van de geselecteerde velden worden geretourneerd. 

Op de gehele voor de query is betrokken zijn de documenten die overeenkomen met 1, 2, 3. 

## <a name="stage-4-scoring"></a>Stap 4: scoren  

Elk document in een resultatenset search is een relevantiescore toegewezen. De functie van de relevantiescore is positie hoger documenten die het beste een gebruiker vragen beantwoorden die door de zoekopdracht wordt uitgedrukt als. De score wordt berekend op basis van statistische eigenschappen van voorwaarden die overeenkomen met. De spil van de scoring formule is [TF/IDF (frequentie term frequentie inverse document)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). In de query's met zeldzame en algemene voorwaarden, bevordert TF/IDF resultaten met de term zeldzaam. Bijvoorbeeld in een hypothetische index met alle Wikipedia-artikelen van documenten die overeenkomen met de query *de president*, documenten die overeenkomen op *president* worden beschouwd als relevanter dan documenten overeenkomende op *de*.


### <a name="scoring-example"></a>Scoring-voorbeeld

De drie documenten die overeenkomen met onze voorbeeldquery intrekken:
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance to the beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

Document 1 overeenkomen met de query beste omdat zowel de term *groot* en de vereiste woordgroep *weergeven in de Indische Oceaan* optreden in het omschrijvingsveld. De volgende twee documenten overeenkomen met alleen de woordgroep *weergeven in de Indische Oceaan*. Het is mogelijk verrassende dat de relevantiescore van document 2 en 3 anders is, zelfs als ze overeenkomen met de query op dezelfde manier. Dit is omdat de scoring formule meer onderdelen dan alleen TF/IDF bevat. In dit geval document 3 die een enigszins hogere score is toegewezen, omdat de beschrijving korter is. Meer informatie over [van Lucene praktische scoren formule](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) om te begrijpen hoe het veldlengte en andere factoren van invloed is op de relevantiescore.

Sommige querytypen (jokerteken, voorvoegsel, reguliere expressie) hun bijdragen altijd een constante score is de totale score van document. Dit kan overeenkomsten gevonden via de uitbreiding van de query moet worden opgenomen in de resultaten, maar zonder gevolgen voor de volgorde. 

Een voorbeeld laat zien waarom dit belangrijk is. Zoeken met jokertekens, met inbegrip van voorvoegsel zoekopdrachten, zijn niet-eenduidige per definitie omdat de invoer een gedeeltelijke tekenreeks met mogelijke overeenkomsten op een zeer groot aantal verschillende voorwaarden is (Houd rekening met de invoer van 'rondleiding *', met overeenkomsten gevonden op "rondleidingen", 'tourettes', en " tourmaline'). Gezien de aard van deze resultaten, wordt er geen manier om te redelijkerwijs afleiden welke voorwaarden vindt u meer dan andere waardevolle is. Om deze reden negeren we term frequenties wanneer resultaten scoring in query's van de typen jokerteken, voorvoegsel en reguliere expressie. In de zoekaanvraag van een meerdelige met gedeeltelijke en volledige voorwaarden, worden de resultaten van de gedeeltelijke invoer opgenomen met een constante score om te voorkomen dat de afwijking op mogelijk onverwachte resultaten.

### <a name="score-tuning"></a>Score afstemmen

Er zijn twee manieren om af te stemmen op relevantie scores in Azure Search:

1. **Scoreprofielen** promoveren van documenten in de gerangschikte lijst met resultaten op basis van een set regels. In ons voorbeeld, kunnen wij beschouwen als documenten die overeenkomen met in het titelveld relevanter dan documenten die overeenkomen met in het omschrijvingsveld. Als onze index een prijsveld voor elke hotel heeft, kunnen we ook documenten met een lagere prijs promoten. Meer informatie meer over het [scoren profielen toevoegen aan een search-index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Versterking** (alleen beschikbaar in de volledige Lucene-querysyntaxis) biedt een Gradient boosting-operator `^` die kunnen worden toegepast op een deel van de querystructuur van de. In ons voorbeeld, in plaats van te zoeken op het voorvoegsel *air-condition*\*, een zoekterm voor de exacte termen *air-condition* of het voorvoegsel, maar de documenten die overeenkomen met de exacte zoekterm hoger gerangschikt op boost toe te passen op de term-query: * lucht-voorwaarde ^ 2 || AIR-condition **. Meer informatie over [versterking](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Scoring in een gedistribueerde index

Alle indexen in Azure Search worden automatisch onderverdeeld in meerdere shards, zodat we de index tussen meerdere knooppunten snel kan worden gedistribueerd tijdens service schalen omhoog of omlaag schalen. Wanneer u een zoekaanvraag verzendt, worden deze onafhankelijk van elkaar uitgegeven voor elke shard. De resultaten van elke shard vervolgens worden samengevoegd en geordend op score (als er geen andere volgorde is gedefinieerd). Het is belangrijk te weten dat de score functie gewichten query's term frequentie op basis van de frequentie inverse document in alle documenten in de shard niet voor alle shards uitvoeren.

Dit betekent dat een relevantiescore *kan* zijn verschillend voor identieke documenten als ze zich in verschillende shards bevinden. Deze verschillen vaak gelukkig verdwijnt wanneer het aantal documenten in de index vanwege meer zelfs term distributie groeit. Het is niet mogelijk om aan te nemen op welke shard een document wordt geplaatst. Echter, ervan uitgaande dat de documentsleutel van een niet wijzigen, deze wordt altijd worden toegewezen aan de dezelfde shard.

In het algemeen is document score niet de beste kenmerk voor het bestellen van documenten als de stabiliteit van de volgorde belangrijk is. Bijvoorbeeld is twee documenten met een identieke score gegeven, er geen garantie waarvoor een eerst in de volgende uitvoeringen van dezelfde query verschijnt. Document score geeft alleen een algemeen beeld van document relevantie ten opzichte van andere documenten in de resultatenset.

## <a name="conclusion"></a>Conclusie

Het succes van zoekmachines internet heeft deze gebeurtenis treedt verwachtingen voor zoeken in volledige tekst van persoonlijke gegevens. Voor vrijwel elke soort zoekervaring verwachten we nu de engine voor onze bedoeling kan begrijpen, zelfs wanneer de voorwaarden zijn onjuist gespeld of zijn onvolledig. We kunnen zelfs overeenkomsten in de buurt equivalente zoektermen of synoniemen die we hebben nooit daadwerkelijk opgegeven op basis van verwachten.

Zoeken in volledige tekst is van een technisch oogpunt zeer complex zijn, die geavanceerde linguïstische analyse- en een systematische benadering voor verwerking op een manier die distilleer, uitbreiden en transformeren querytermen leveren een relevante resultaat nodig hebben. Gezien de inherente complexiteit, zijn er veel van de factoren die invloed kunnen zijn op het resultaat van een query. Om deze reden biedt de tijd om te begrijpen van het mechanisme van zoeken in volledige tekst duidelijke voordelen bij te werken via onverwachte resultaten.  

In dit artikel verkend zoeken in volledige tekst in de context van Azure Search. We hopen dat dit biedt u voldoende achtergrond voor het herkennen van mogelijke oorzaken en oplossingen voor veelvoorkomende queryproblemen-adressering. 

## <a name="next-steps"></a>Volgende stappen

+ De voorbeeldindex bouwen, probeer verschillende query's en bekijk de resultaten. Zie voor instructies [maken en query uitvoeren in een index in de portal](search-get-started-portal.md#query-index).

+ Probeer van aanvullende query-syntaxis van de [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) voorbeeld in deze sectie of [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in de portal.

+ Beoordeling [scoreprofielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) als u wilt om af te stemmen classificatie in uw zoektoepassing.

+ Meer informatie over het toepassen van [taalspecifieke lexicale analyse](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Configureren van aangepaste analyse](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) voor minimale verwerking of gespecialiseerde verwerking op specifieke velden.

+ [Standaard- en Engelse analyzers vergelijken](http://alice.unearth.ai/)) naast elkaar op deze demo-website. 

## <a name="see-also"></a>Zie ook

[REST-API voor Search-documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

[Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

[Volledige Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 

[Zoekresultaten verwerken](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
