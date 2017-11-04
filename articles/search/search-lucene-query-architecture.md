---
title: Volledige tekst zoeken (Lucene)-engine-architectuur in Azure Search | Microsoft Docs
description: Uitleg van de Lucene query verwerking en document ophalen concepten voor zoeken in volledige tekst, met betrekking tot de Azure Search.
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: 0b2e66cd40c1b49832b865e5bf59edcf78996eb8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a>Hoe vol tekst zoeken werkt in Azure Search

Dit artikel is voor ontwikkelaars die u een beter begrip moeten van de werking van Lucene zoeken in volledige tekst in Azure Search. Voor tekst-query's, Azure Search naadloos verwachte resultaten in de meeste gevallen levert, maar soms mogelijk krijgt u een resultaat enigszins lijkt "off". In deze situaties als zich een achtergrond op de vier fasen van de queryuitvoering Lucene (query parseren, lexicale analyse, document-koppeling, score berekenen) kunt u specifieke wijzigingen in queryparameters of de indexconfiguratie van de die het gewenste resultaat levert te identificeren. 

> [!Note] 
> Azure Search Lucene gebruikt voor zoekopdrachten in volledige tekst, maar Lucene-integratie is niet volledig. We selectief weergeven en Lucene uitbreiden zodat de naar Azure Search belangrijke scenario's. 

## <a name="architecture-overview-and-diagram"></a>Overzicht van de architectuur en diagram

Verwerking van een zoekopdracht voor volledige tekst begint met het parseren van de querytekst om uit te pakken zoektermen. De zoekmachine maakt gebruik van een index ophalen van documenten met overeenkomende voorwaarden. Afzonderlijke querytermen zijn soms uitgesplitst en in nieuwe formulieren voor het casten van een bredere net over wat kan worden beschouwd als een mogelijke overeenkomst geregenereerd. Geen resultatenset is vervolgens gesorteerd op een toegewezen aan elk document dat overeenkomende afzonderlijke relevantie-score. Die aan de bovenkant van de ranglijst worden teruggestuurd naar de aanroepende toepassing.

Aangepast, bestaat uitvoering van de query uit vier fasen: 

1. Query bij het parseren van 
2. Lexicale analyse 
3. Document ophalen 
4. Score berekenen 

Het volgende diagram illustreert de onderdelen die wordt gebruikt voor het verwerken van een zoekaanvraag. 

 ![Architectuurdiagram van de Lucene-query in Azure Search][1]


| Belangrijkste onderdelen | Functionele beschrijving | 
|----------------|------------------------|
|**Query parsers** | Afzonderlijke querytermen van query's en de querystructuur (een querystructuur) worden verzonden naar de zoekmachine maken. |
|**Analyzers** | Lexicale analyse uitvoeren op querytermen. Dit proces kan betrekking hebben op transformeren, te verwijderen of uitbreiden van de voorwaarden van de query. |
|**Index** | Een efficiënte gegevensstructuur op te slaan en te organiseren doorzoekbare voorwaarden opgehaald uit de geïndexeerde documenten. |
|**Zoekmachine** | Haalt en scores overeenkomende documenten op basis van de inhoud van de omgekeerde index. |

## <a name="anatomy-of-a-search-request"></a>Anatomie van een zoekaanvraag

Een zoekaanvraag is een complete specificatie van wat moet worden geretourneerd in een resultatenset. In de meest eenvoudige vorm is een lege query zonder criteria alle soorten. Een voorbeeld van een meer realistische bevat parameters, verschillende querytermen, mogelijk binnen het bereik van bepaalde velden, met mogelijk een filterexpressie en ordening van regels.  

Het volgende voorbeeld wordt een zoekaanvraag u verzenden naar Azure Search met behulp van mogelijk de [REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
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

1. Documenten gefilterd waar de prijs is ten minste $60 en minder dan 300 $.
2. De query worden uitgevoerd. In dit voorbeeld wordt de zoekopdracht bestaat uit zinnen en voorwaarden: `"Spacious, air-condition* +\"Ocean view\""` (gebruikers doorgaans niet leestekens in te voeren, maar dit op te nemen in het voorbeeld, kunnen wij waarin wordt uitgelegd hoe analyzers verwerkt; het). Voor deze query scant het zoekprogramma de beschrijving en titelvelden opgegeven in `searchFields` voor documenten die 'Oceaan weergave' bevatten en ook op de term 'groot', of op voorwaarden die met het voorvoegsel beginnen 'air-condition'. De `searchMode` parameter wordt gebruikt op een term (standaard) of al deze gevallen waarbij een term niet expliciet hoeft (`+`).
3. Hiermee worden de resulterende set hotels door nabijheid tot een opgegeven geografische locatie en vervolgens wordt geretourneerd naar de aanroepende toepassing gerangschikt. 

Het merendeel van dit artikel gaat over de verwerking van de *zoekquery*: `"Spacious, air-condition* +\"Ocean view\""`. Filteren en ordening vallen buiten het bereik. Zie voor meer informatie de [zoeken-API-naslagdocumentatie](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>Stap 1: Een Query bij het parseren van 

Zoals vermeld, is de queryreeks in de eerste regel van de aanvraag: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

De queryparser scheidt operators (zoals `*` en `+` in het voorbeeld) in een zoekvak voorwaarden en de query in deconstructs *subquery's* met een ondersteund type: 

+ *term query* voor zelfstandige voorwaarden (zoals groot)
+ *woordgroep query* voor tussen aanhalingstekens voorwaarden (zoals Oceaan weergave)
+ *voorvoegsel query* voor voorwaarden gevolgd door een operator voorvoegsel `*` (zoals air-condition)

Zie voor een volledige lijst met ondersteunde querytypen [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Operators die zijn gekoppeld aan een subquery bepalen of de query 'moet' of 'moet' voldaan voordat een document om te worden beschouwd als een overeenkomst. Bijvoorbeeld: `+"Ocean view"` 'moet' is vanwege de `+` operator. 

De queryparser worden opnieuw gestructureerd de subquery's in een *querystructuur* (een interne structuur voor de query) dit wordt doorgegeven op naar de zoekmachine. In de eerste fase van het parseren van de query uitziet de querystructuur van de.  

 ![Booleaanse waarde searchmode ieder query][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Ondersteund parsers: eenvoudig en volledige Lucene 

 Azure Search beschrijft de twee andere querytalen `simple` (standaard) en `full`. Door in te stellen de `queryType` parameter bij uw zoekaanvraag, geeft u aan de queryparser welke querytaal gewenst zodat deze het interpreteren van de operators en syntaxis. De [eenvoudige querytaal](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuïtieve en robuuste, vaak geschikte gebruikersinvoer als interpreteren-is zonder dat de verwerking van de clientzijde. Biedt ondersteuning voor query's van webzoekmachines bekend. De [volledige Lucene-querytaal](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), die wordt geleverd door in te stellen `queryType=full`, wordt de standaardtaal voor eenvoudige query uitgebreid met ondersteuning voor meer operators en querytypen zoals jokerteken, bij benadering, regex en veldgerelateerde query's. Bijvoorbeeld, wordt een reguliere expressie in vereenvoudigde querysyntaxis verzonden geïnterpreteerd als een queryreeks bevat en niet een expressie. De voorbeeldaanvraag in dit artikel maakt gebruik van de volledige Lucene-querytaal.

### <a name="impact-of-searchmode-on-the-parser"></a>Gevolgen van het searchMode in de parser 

Een andere aanvraag zoekparameter die gevolgen heeft voor het parseren is de `searchMode` parameter. Hiermee kunt u de standaardoperator voor Boole-query's: een (standaard) of alle.  

Wanneer `searchMode=any`, die is de standaardinstelling en de spatie als scheidingsteken tussen groot en air-condition is of (`||`), zodat de tekst van de query voorbeeld gelijk aan: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Expliciete operators, zoals `+` in `+"Ocean view"`, zijn niet-ambigue Booleaanse query constructie (de term *moet* overeen). Minder duidelijk is het interpreteren van de overige voorwaarden: groot en air-condition. Moet de zoekmachine overeenkomsten vinden voor de weergave Oceaan *en* groot *en* air-condition? Of moet Oceaan weergave vinden plus *een* van de overige voorwaarden? 

Standaard (`searchMode=any`), de zoekmachine wordt ervan uitgegaan dat de breder interpretatie. Een veld *moet* overeenkomen, opgetreden bij het weergeven 'of' semantiek. De structuur van de eerste query eerder geïllustreerd, met de twee 'moet' bewerkingen, ziet u de standaardwaarde.  

Stel dat we nu `searchMode=all`. In dit geval wordt wordt de ruimte geïnterpreteerd als een 'en'-bewerking. Elk van de overige voorwaarden zijn beide aanwezig in het document in aanmerking wilt komen als een overeenkomst. De resulterende voorbeeldquery wordt geïnterpreteerd als volgt: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Een boomstructuur gewijzigde query voor deze query zou zijn als volgt, waarbij een overeenkomende document is het snijpunt van alle drie subquery's: 

 ![Booleaanse query searchmode alle][3]

> [!Note] 
> Kiezen `searchMode=any` via `searchMode=all` is een beste beslissing ontvangen op door het uitvoeren van representatieve query's. Gebruikers die waarschijnlijk ook operators (algemeen zijn wanneer zoeken document opslaat) wellicht resultaten intuïtievere als `searchMode=all` informeert Booleaanse query constructies. Voor meer informatie over de wisselwerking tussen `searchMode` en operators, Zie [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>Stap 2: Lexicale analyse 

Lexicale analyzers proces *benaming query's* en *woorden van query's* nadat de structuur van de querystructuur van de. Een analyzer tekst invoerwaarden krijgen tot het door de parser accepteert, verwerkt de tekst en klikt u vervolgens verzendt terug tokenized voorwaarden om te worden opgenomen in de querystructuur van de. 

De meest voorkomende vorm van lexicale analyse is *taalkundige analysis* welke transformaties query voorwaarden op basis van regels die specifiek zijn voor een bepaalde taal: 

* Een zoekterm voor het root-formulier van een woord te beperken 
* Verwijderen van niet-essentiële woorden (stopwoorden, zoals 'het' of 'en' in het Engels) 
* Een samengestelde woord in onderdelen op te splitsen 
* Kleine hoofdlettergebruik van een woord hoofdletters 

Al deze bewerkingen vaak verschillen tussen de ingevoerde tekst door de gebruiker en de voorwaarden die zijn opgeslagen in de index wilt wissen. Dergelijke bewerkingen voorbij de verwerking van tekst en vereisen diepgaande kennis van de taal zelf. Als u wilt toevoegen in deze laag van de taalkundige awareness, Azure Search biedt ondersteuning voor een lange lijst met [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) van Lucene en van Microsoft.

> [!Note]
> Vereisten voor de analyse kunnen variëren van minimale tot uitgebreide afhankelijk van uw scenario. U kunt de complexiteit van lexicale analyse beheren door het selecteren van de vooraf gedefinieerde analyzers of door het maken van uw eigen [aangepaste analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Analyzers zijn gericht op de doorzoekbare velden en zijn opgegeven als onderdeel van de velddefinitie van een. Hiermee kunt u lexicale analyse op basis van veld per variëren. Niet is opgegeven, de *standaard* Lucene analyzer wordt gebruikt.

In ons voorbeeld vóór analyse, heeft de structuur van de eerste query de term 'Spacious' met een hoofdletter "S" en een komma, die de queryparser wordt geïnterpreteerd als een deel van de zoekterm (een door komma's wordt niet beschouwd als een query language-operator).  

Wanneer de standaard analyzer de term verwerkt, wordt deze kleine letters 'Oceaan weergeven' en 'groot' en verwijder het teken met door komma's. De gewijzigde query-structuur ziet er als volgt uit: 

 ![Booleaanse query geanalyseerde voorwaarden][4]

### <a name="testing-analyzer-behaviors"></a>Analyzer gedrag testen 

Het gedrag van een analyzer kan worden getest met de [analyseren API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Geef de tekst die u wilt analyseren om te zien wat vermeldingen analyzer wordt gegenereerd. Als u wilt zien hoe de standaard analyzer zou de tekst 'air-condition' verwerken, kunt u bijvoorbeeld de volgende aanvraag uitgeven:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

De standaard analyzer verdeelt de ingevoerde tekst in de volgende twee tokens aantekeningen te maken met kenmerken zoals begin en einde offsets (gebruikt voor het markeren van treffers), evenals hun positie (gebruikt voor de zinsnede overeenkomende):

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

### <a name="exceptions-to-lexical-analysis"></a>Uitzonderingen op lexicale analyse 

Lexicale analysis geldt alleen voor querytypen waarvoor volledige voorwaarden – een term query of een woordgroep-query. Het is niet van toepassing op querytypen met onvolledige voorwaarden – voorvoegsel query, jokertekens query, regex query – of op een fuzzy query. De querytypen, met inbegrip van de query voorvoegsel met de term *air-condition\**  in ons voorbeeld rechtstreeks aan de querystructuur, het omzeilen van de analyse-fase worden toegevoegd. De enige transformatie die wordt uitgevoerd op de voorwaarden van deze typen query is lowercasing.

<a name="stage3"></a>

## <a name="stage-3-document-retrieval"></a>Stap 3: Document ophalen 

Ophalen van het document verwijst naar het zoeken van documenten met die overeenkomt met de voorwaarden in de index. Deze fase is beste begrepen door een voorbeeld. Laten we beginnen met een index hotels met de volgende eenvoudige schema: 

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

Het helpt om te begrijpen ophalen, te weten van enkele basisbeginselen van het indexeren. De eenheid voor opslag is een omgekeerde index, één voor elk doorzoekbaar veld. Is een gesorteerde lijst van alle voorwaarden van alle documenten in een omgekeerde index. Elke term wordt toegewezen aan de lijst met documenten waarin zich, duidelijk naar voren, in het onderstaande voorbeeld voordoet.

De voorwaarden in een omgekeerde index produceren, voert de zoekmachine lexicale analyse over de inhoud van documenten, vergelijkbaar met wat er tijdens de verwerking van query's gebeurt uit:

1. *Tekst invoer* worden doorgegeven aan een analyzer, geïntegreerd in een kleine, enz., afhankelijk van de configuratie analyzer en leestekens verwijderd. 
2. *Tokens* zijn van de uitvoer van de analyse van tekst.
3. *Voorwaarden* worden toegevoegd aan de index.

Het is algemene, maar niet vereist, met de dezelfde analyzers voor zoeken en indexeren operations zodat querytermen meer gelijke termen in de index uitzien.

> [!Note]
> Azure Search kunt u verschillende analyzers voor indexeren opgeven en zoek via aanvullende `indexAnalyzer` en `searchAnalyzer` veld parameters. Als u niets opgeeft, wordt de analyzer ingesteld met de `analyzer` eigenschap wordt gebruikt voor het indexeren en het zoeken.  

**Omgekeerde index voor voorbeelddocumenten**

Terugkeren naar het voorbeeld voor het **titel** veld omgekeerde index ziet er als volgt:

| Termijn | Lijst met standaarddocumenten |
|------|---------------|
| atman | 1 |
| strand | 2 |
| hotel | 1, 3 |
| Oceaan | 4  |
| playa | 3 |
| toevlucht | 3 |
| Terugtrekken | 4 |

In het titelveld alleen *hotel* wordt weergegeven in twee documenten: 1, 3.

Voor de **beschrijving** veld de index is als volgt:

| Termijn | Lijst met standaarddocumenten |
|------|---------------|
| lucht | 3
| en | 4
| strand | 1
| voorwaarde | 3
| vertrouwd | 3
| afstand | 1
| eiland | 2
| kauaʻi | 2
| zich bevindt | 2
| Noord | 2
| Oceaan | 1, 2, 3
| van | 2
| op |2
| Stille | 4
| ruimten  | 1, 3
| secluded | 4
| wal | 2
| groot | 1
| de | 1, 2
| tot | 1
| weergeven | 1, 2, 3
| roulatie van | 1
| met | 3


**Overeenkomende querytermen tegen geïndexeerde voorwaarden**

Gezien de bovenstaande omgekeerde indexen, gaan we u terug naar de voorbeeldquery en Zie hoe overeenkomende documenten zijn gevonden voor de voorbeeldquery. Let erop dat de laatste querystructuur ziet er als volgt: 

 ![Booleaanse query geanalyseerde voorwaarden][4]

Tijdens het uitvoeren van query worden afzonderlijke query's uitgevoerd met de doorzoekbare velden onafhankelijk. 

+ Het komt overeen met 'groot' TermQuery, documenteer 1 (Hotel Atman). 

+ De PrefixQuery ' air-condition * ', komt niet overeen met alle documenten. 

  Dit is een probleem dat soms confuses ontwikkelaars. Hoewel de term air-conditioned in het document bestaat, is deze opgesplitst in twee termen door de standaard analyzer. Intrekken voorvoegsel query's die voor gedeeltelijke termen bevatten, worden niet geanalyseerd. Daarom worden voorwaarden met het voorvoegsel 'air-condition' opgezocht in de omgekeerde index en niet gevonden.

+ De PhraseQuery, 'Oceaan weergave', zoekt de termen 'Oceaan' en 'weergeven' en de buurt van de voorwaarden in het originele document wordt gecontroleerd. Documenten 1, 2 en 3 overeen met deze query in het omschrijvingsveld. U ziet document 4 heeft de term Oceaan in de titel, maar wordt niet beschouwd als een overeenkomst als er op zoek bent naar de zinsnede 'Oceaan weergave' in plaats van afzonderlijke woorden. 

> [!Note]
> Een zoekquery tenzij u de velden in te stellen beperken onafhankelijk tegen de doorzoekbare velden in de Azure Search-index wordt uitgevoerd de `searchFields` parameter, zoals geïllustreerd in de zoekaanvraag voorbeeld. Documenten die overeenkomen met een van de geselecteerde velden worden geretourneerd. 

Op de gehele voor de desbetreffende query worden de documenten die overeenkomen met 1, 2, 3. 

## <a name="stage-4-scoring"></a>Stap 4: score berekenen  

Elk document dat in een resultatenset search is een score relevantie toegewezen. De functie van de score relevantie is positie hoger documenten die het beste een gebruiker-vraag te beantwoorden zoals deze wordt weergegeven door de query. De score wordt berekend op basis van statistische eigenschappen van de voorwaarden die overeenkomen met. Is de kern van de formule van de score [TF/IDF (frequentie term frequentie inverse document)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). In de query's met zeldzame en algemene voorwaarden, bevordert TF/IDF resultaten met de zeldzame term. Bijvoorbeeld in een hypothetische index met alle Wikipedia artikelen van documenten die overeenkomen met de query *de directeur*, documenten die overeenkomen met op *directeur* worden beschouwd als meer dan documenten overeen op de relevante *de*.


### <a name="scoring-example"></a>Score berekenen voor voorbeeld

Intrekken van de drie documenten die overeenkomen met de voorbeeldquery:
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

Document 1 komt overeen met de query beste omdat zowel de term *groot* en de vereiste woordgroep *Oceaan weergave* optreden in het omschrijvingsveld. De volgende twee documenten die overeenkomen met alleen de woordgroep *Oceaan weergave*. Mogelijk verrassend dat de score relevantie voor document 2 en 3 anders is, ondanks dat ze overeenkomen met de query op dezelfde manier. Omdat de formule van de score meer onderdelen dan alleen TF/IDF bevat wordt. In dit geval document 3 die een enigszins hogere score is toegewezen, omdat de beschrijving korter is. Meer informatie over [Lucene van praktische score berekenen voor de formule](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) om te begrijpen hoe het veldlengte en andere factoren van invloed is op de score relevantie.

Sommige querytypen (jokerteken, voorvoegsel, regex) bijdragen altijd een constante score voor de algehele document score. Hierdoor komt overeen met gevonden via de uitbreiding van de query moet worden opgenomen in de resultaten, maar zonder de volgorde. 

Een voorbeeld illustreert waarom dit is van belang. Zoekopdrachten met jokertekens, met inbegrip van zoekopdrachten voor voorvoegsel, zijn niet-eenduidige per definitie omdat de invoer is gedeeltelijke tekenreeks met mogelijke overeenkomsten op een zeer groot aantal verschillende voorwaarden (overweeg invoer van het "rondleiding *" met overeenkomsten gevonden op 'rondleidingen', 'tourettes' en 'tourmaline'). Gezien de aard van deze resultaten, kan er niet worden redelijkerwijs afleiden welke voorwaarden vindt u meer dan andere waardevolle. Om deze reden negeren we term frequenties wanneer resultaten scoren van typen jokerteken, voorvoegsel en regex-query's. In een meerdelig zoekaanvraag die gedeeltelijke en volledige voorwaarden bevat, worden de resultaten van de gedeeltelijk invoer opgenomen met een constante score om te voorkomen dat de afwijking naar mogelijk onverwacht komt overeen met.

### <a name="score-tuning"></a>Score afstemmen

Er zijn twee manieren om af te stemmen relevantie scores in Azure Search:

1. **Score berekenen voor profielen** promoveren van documenten in de gerangschikte lijst met resultaten op basis van een set regels. In ons voorbeeld kan we kunt u documenten die overeenkomen met in het titelveld meer relevante dan documenten die overeenkomen met in het omschrijvingsveld. Bovendien kan er als onze index had een prijsveld voor elke hotel, documenten met lagere prijs promoveren. Meer informatie over het meer [score berekenen voor profielen toevoegen aan een search-index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Versterking** (alleen beschikbaar in de volledige Lucene-querysyntaxis) biedt een prestatieverbetering operator `^` die kunnen worden toegepast op een deel van de querystructuur van de. In ons voorbeeld, in plaats van het zoeken op het voorvoegsel *air-condition*\*, een kan zoekterm voor de exacte *air-condition* of het voorvoegsel, maar de documenten die overeenkomen met de exacte term hoger door toe te passen versterking aan de query term zijn gerangschikt: *lucht voorwaarde ^ 2 || AIR-condition**. Meer informatie over [versterking](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Score berekenen voor in een gedistribueerde index

Alle indexen in Azure Search automatisch opgesplitst in meerdere shards ons toestemming te distribueren snel de index tussen meerdere knooppunten tijdens service schaal omhoog of omlaag schalen. Wanneer een zoekaanvraag is uitgegeven, wordt deze onafhankelijk tegen elke shard verstrekt. De resultaten van elke shard worden vervolgens samengevoegd en geordend op score (als er geen andere volgorde gedefinieerd). Het is belangrijk te weten dat de scoreprofiel functie gewichten term frequentie tegen de inverse document frequentie in alle documenten in de shard query niet over de breedte van alle shards.

Dit betekent dat een score relevantie *kan* zijn verschillend voor identieke documenten als ze zich op andere shards bevinden. Deze verschillen vaak gelukkig verdwijnen wanneer het aantal documenten in de index vanwege meer zelfs term distributie groeit. Het is niet mogelijk om aan te nemen op welke shard een document wordt geplaatst. Echter, ervan uitgaande dat de documentsleutel van een niet wijzigen, deze wordt altijd worden toegewezen aan de dezelfde shard.

In het algemeen is document score niet het aanbevolen kenmerk voor het ordenen van documenten als stabiliteit van de volgorde belangrijk is. Bijvoorbeeld, is gezien twee documenten met een identieke score, er geen garantie waarvoor een eerste wordt weergegeven in latere uitvoeringen van dezelfde query. Document score geeft alleen een algemeen beeld van het document relevantie ten opzichte van andere documenten in de resultatenset.

## <a name="conclusion"></a>Conclusie

Het succes van zoekmachines internet heeft verwachtingen voor zoeken in volledige tekst gegenereerd via persoonlijke gegevens. Voor vrijwel elke soort zoekervaring verwachten we nu de engine om te begrijpen onze bedoeling, zelfs wanneer de voorwaarden zijn onjuist gespeld of zijn onvolledig. We verwachten mogelijk zelfs op basis van in de buurt vergelijkbare termen of synoniemen die we hebben nooit echt opgegeven.

Zoekopdracht in volledige tekst is van een technisch oogpunt zeer complex is of waarvoor geavanceerde analyses van de taalkundige en een systematische aanpak op de verwerking van de manieren waarop distill, vouw en transformeren querytermen leveren een relevante resultaat. Gezien de intrinsieke complexiteit, zijn er veel factoren die van invloed kunnen zijn op het resultaat van een query. Om deze reden biedt voldoende tijd om te begrijpen van het mechanisme van zoeken in volledige tekst concrete voordelen bij een poging om te werken via onverwachte resultaten.  

In dit artikel verkend zoeken in volledige tekst in de context van Azure Search. We hopen dat dit biedt u voldoende achtergrond voor het herkennen van mogelijke oorzaken en oplossingen voor het adresseren van veelvoorkomende problemen van de query. 

## <a name="next-steps"></a>Volgende stappen

+ De voorbeeld-index bouwen, uitproberen verschillende query's en resultaten bekijken. Zie voor instructies [bouwen en query uitvoeren op een index in de portal](search-get-started-portal.md#query-index).

+ Probeer extra querysyntaxis van de [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) voorbeeld in deze sectie of van [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in de portal.

+ Bekijk [score berekenen voor profielen](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) als u wilt afstemmen ranking in uw zoektoepassing.

+ Meer informatie over het toepassen van [taalspecifieke lexicale analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Configureren van aangepaste analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) voor minimale verwerking of speciale verwerking voor specifieke velden.

+ [Standard- en Engelse analyzers vergelijken](http://alice.unearth.ai/)) naast elkaar op deze demo-website. 

## <a name="see-also"></a>Zie ook

[REST-API documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

[Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

[Volledige Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 

[Zoekresultaten verwerken](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
