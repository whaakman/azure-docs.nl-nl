---
title: Lucene-querysyntaxis - Azure Search
description: Referentie voor de volledige Lucene-syntaxis als gebruikt met Azure Search.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/31/2019
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 59362b28390556f12cce8813635894c9f06b9a20
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56008473"
---
# <a name="lucene-query-syntax-in-azure-search"></a>Lucene-querysyntaxis in Azure Search
U kunt ook query's voor Azure Search op basis van de uitgebreide schrijven [Lucene-Queryparser](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) syntaxis voor het vormen van de speciale query: jokertekens, fuzzy zoeken, zoeken op nabijheid, reguliere expressies zijn een aantal voorbeelden. Veel van de syntaxis van Lucene-Queryparser [intact geïmplementeerd in Azure Search](search-lucene-query-architecture.md), met uitzondering van *bereik zoekopdrachten* die zijn gebouwd in Azure Search via `$filter` expressies. 

## <a name="how-to-invoke-full-parsing"></a>Het aanroepen van de volledige parseren

Stel de `queryType` zoeken parameter om op te geven welke parser te gebruiken. Geldige waarden zijn `simple|full`, met `simple` als de standaard, en `full` voor Lucene. 

<a name="bkmk_example"></a> 

## <a name="example-showing-full-syntax"></a>Voorbeeld van de volledige syntaxis van de weergeven

Het volgende voorbeeld worden documenten gevonden in de index met behulp van de Lucene-querysyntaxis, duidelijk in de `queryType=full` parameter. Deze query retourneert hotels waar het categorieveld bevat de term 'budget' en alle doorzoekbare velden die de zin 'onlangs renovated'. Documenten die de zin 'onlangs renovated' worden hoger gerangschikt als gevolg van de termijn boost waarde (3).  

De `searchMode=all` parameter is van belang voor dit voorbeeld. Wanneer operators op de query zijn, moet u in het algemeen instellen `searchMode=all` om ervoor te zorgen dat *alle* van de criteria overeenkomt.

```  
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28&querytype=full  
```  

 U kunt ook gebruik POST:  

```  
POST /indexes/hotels/docs/search?api-version=2015-02-28  
{  
  "search": "category:budget AND \"recently renovated\"^3",  
  "queryType": "full",  
  "searchMode": "all"  
}  
```  

Zie voor meer voorbeelden van [Lucene-queryvoorbeelden syntaxis voor het bouwen van query's in Azure Search](search-query-lucene-examples.md). Zie voor meer informatie over het opgeven van de volledige afhankelijk is van queryparameters [documenten zoeken &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Azure Search biedt ook ondersteuning voor [eenvoudige querysyntaxis](query-simple-syntax.md), een eenvoudige en krachtige querytaal die kunnen worden gebruikt voor eenvoudige trefwoorden zoeken.  


##  <a name="bkmk_fields"></a> Veldgerelateerde query 's  
 Kunt u een `fieldname:searchterm` bouwen voor het definiëren van een querybewerking fielded het veld is één woord, waarbij de zoekterm is ook één woord of een woordgroep, eventueel met Booleaanse operators. Enkele voorbeelden omvatten het volgende:  

-   GEEN geschiedenis genre: jazz  

-   Artiesten: ("Miles Davis" "John Coltrane")

 Zorg ervoor dat u meerdere tekenreeksen tussen aanhalingstekens plaatsen als u wilt dat beide tekenreeksen die moeten worden geëvalueerd als één entiteit in dit geval zoekt naar twee verschillende artiesten in de `artists` veld.  

 Het veld dat is opgegeven `fieldname:searchterm` moet een `searchable` veld.  Zie [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over het gebruik van indexkenmerken in velddefinities.  

##  <a name="bkmk_fuzzy"></a> fuzzy zoeken  
 Bij een fuzzy zoekopdracht overeenkomsten zoekt in termen die een dergelijke constructie hebben. Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), fuzzy zoekopdrachten zijn gebaseerd op [Damerau Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance).  

 Bij een fuzzy zoekopdracht, gebruikt u de tilde ' ~ ' symbool aan het einde van één woord met een optionele parameter, een getal tussen 0 en 2 (standaard), dat de afstand bewerken geeft. Bijvoorbeeld: "blauw ~ ' of ' blue ~ 1" retourneert "blue", 'blues' en 'lijmen'.

 Fuzzy zoeken kan alleen worden toegepast met voorwaarden, niet zinnen. Fuzzy zoekopdrachten kunnen een term die tot het maximum van 50 termen die voldoen aan de criteria afstand uitbreiden.

##  <a name="bkmk_proximity"></a> zoeken op nabijheid  
 Nabijheid zoekopdrachten worden gebruikt om te vinden van voorwaarden die zich dicht bij elkaar in een document. Invoegen van een tilde ' ~ ' symbool aan het einde van een zin gevolgd door het aantal woorden die de grens nabijheid maken. Bijvoorbeeld, `"hotel airport"~5` vindt de termen "Hotels" en "luchthaven" binnen 5 woorden van elkaar in een document.  


##  <a name="bkmk_termboost"></a> versterking  
 Termenverbetering verwijst naar de positie van een document hoger als deze bevat de boosted term, ten opzichte van documenten die de term niet bevatten. Dit wijkt af van het scoreprofielen in die scoreprofielen bepaalde velden, in plaats van specifieke voorwaarden verhogen.  

In het volgende voorbeeld kunt u de verschillen laten zien. Stel dat er is een scoring-profiel die de loyaliteit komt overeen met in een bepaald veld bijvoorbeeld *genre* in de [musicstoreindex voorbeeld](index-add-scoring-profiles.md#bkmk_ex). Termenverbetering kan worden gebruikt om bepaalde zoeken verder hoger dan de andere voorwaarden te verbeteren. Bijvoorbeeld, `rock^2 electronic` documenten met de zoektermen in het veld genre hoger is dan andere doorzoekbare velden in de index wordt verhogen. Verder, documenten die de zoekterm bevatten *rock* hoger dan de andere zoekterm wordt gerangschikt *elektronische* als gevolg van de termijn boost waarde (2).  

 Om te verbeteren gebruiken een termijn van het caret-teken, "^", symbool met een boost factor (een getal) aan het einde van de term die u zoekt. U kunt ook zinnen verhogen. Hoe hoger de boost-factor, meer relevante wordt de term is ten opzichte van andere zoektermen. Standaard is de factor boost 1. Hoewel de factor boost moet positief zijn, kan het zijn dat kleiner is dan 1 (bijvoorbeeld 0.20).  

##  <a name="bkmk_regex"></a> zoeken op reguliere expressies  
 Een zoekopdracht reguliere expressie zoekt een overeenkomst op basis van de inhoud tussen forward slashes bevatten '/', zoals beschreven in de [klasse RegExp](https://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).  

 Bijvoorbeeld, om te vinden met "motel" of "Hotels" documenten, geef `/[mh]otel/`.  Reguliere expressie zoekopdrachten worden vergeleken met enkele woorden.   

##  <a name="bkmk_wildcard"></a> zoeken met jokertekens  
 U kunt gebruiken in het algemeen erkende syntaxis voor meerdere (*) of één (?) teken zoeken met jokertekens. Houd er rekening mee dat de Lucene-queryparser ondersteunt het gebruik van deze symbolen met een termijn van één en niet een zin.  

 Bijvoorbeeld, als u wilt zoeken naar documenten met de woorden met het voorvoegsel "Opmerking", zoals "notebook" of "Kladblok" opgeven "Opmerking *".  

> [!NOTE]  
>  U kunt geen gebruiken een * of? symbool als het eerste teken van een zoekopdracht.  
>  Er is geen tekstanalyse wordt uitgevoerd op query's zoeken met jokertekens. Jokertekens querytermen zijn op het moment dat de query, vergeleken met de geanalyseerde voorwaarden in de search-index en uitgebreid.

##  <a name="bkmk_syntax"></a> Syntaxisgrondbeginselen  
 De volgende van de syntaxisgrondbeginselen van toepassing op alle query's die gebruikmaken van de syntaxis van Lucene.  

### <a name="operator-evaluation-in-context"></a>Evaluatie van de operator in context

Voor de plaatsing bepaald of een symbool wordt geïnterpreteerd als een operator of gewoon een teken in een tekenreeks.

In de volledige Lucene-syntaxis, bijvoorbeeld de tilde (~) gebruikt voor zowel fuzzy zoekopdrachten en zoeken op nabijheid. Als na een zin in tussen aanhalingstekens geplaatst ~ zoeken op nabijheid aanroept. Wanneer geplaatst aan het einde van een term ~ fuzzy zoeken wordt aangeroepen.

Binnen een term, zoals ' zakelijke ~ analist ", het teken wordt niet beoordeeld als operator. In dit geval een zoekterm of woordgroep query, ervan uitgaande dat de query is [zoeken in volledige tekst](search-lucene-query-architecture.md) met [lexicale analyse](search-lucene-query-architecture.md#stage-2-lexical-analysis) verwijdert de ~ en wordt de term ' zakelijke ~ analist ' in twee: zakelijke of analist.

Het bovenstaande voorbeeld is de tilde (~), maar de hetzelfde geldt voor elke operator.

### <a name="escaping-special-characters"></a>Aanhalingstekens speciale tekens

 Speciale tekens moeten worden weergegeven om te worden gebruikt als onderdeel van de zoektekst. U kunt deze escape door het voorvoegsel wordt deze gemaakt met een backslash (\\). Speciale tekens die moeten worden weergegeven, omvatten het volgende:  
`+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ /`  

 Bijvoorbeeld, om te escapen een jokerteken, gebruik \\*.

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Codering onveilige en gereserveerde tekens in URL 's

 Zorg ervoor dat alle onveilige en gereserveerde tekens in een URL zijn gecodeerd. Bijvoorbeeld, is '#' een onveilige teken omdat dit een fragement/anker-id in een URL is. Het teken moet worden gecodeerd naar `%23` als in een URL gebruikt. ' &' en '=' zijn voorbeelden van gereserveerde tekens zoals ook het scheiden van de parameters en geef waarden op in Azure Search. Raadpleeg [RFC1738: Uniform Resource Locator (URL)](https://www.ietf.org/rfc/rfc1738.txt) voor meer informatie.

 Onveilige tekens zijn ``" ` < > # % { } | \ ^ ~ [ ] ``. Gereserveerde tekens zijn `; / ? : @ = + &`.

### <a name="precedence-operators-grouping-and-field-grouping"></a>Bewerkingsvolgorde: groeperen en groepering van veld  
 Subquery's, met inbegrip van operators binnen de tussen haakjes-instructie maken kunt u tussen haakjes. Bijvoorbeeld, `motel+(wifi||luxury)` wordt zoeken naar documenten met de term "motel" en "Wi-Fi" of 'luxe' (of beide).

Veld groepering is vergelijkbaar, maar het bereik van de groepering één veld. Bijvoorbeeld, `hotelAmenities:(gym+(wifi||pool))` zoekt naar het veld "hotelAmenities" voor "sportschool" en "Wi-Fi," of '-sportschool' en 'groep'.  

### <a name="searchmode-parameter-considerations"></a>Overwegingen met betrekking tot SearchMode-parameter  
 De impact van `searchMode` op query's, zoals beschreven in [vereenvoudigde querysyntaxis in Azure Search](query-simple-syntax.md), geldt ook voor de Lucene-querysyntaxis. Dat wil zeggen `searchMode` operators kunnen in combinatie met niet resulteren in query-resultaten die ongebruikelijke lijken mogelijk als u niet op de gevolgen van het stelt u de parameter. Als u de standaard, behoudt `searchMode=any`, en gebruik een niet-operator, de bewerking wordt berekend als een actie of zodat "New York" niet "Seattle" retourneert alle plaatsen die geen Seattle.  

##  <a name="bkmk_boolean"></a> Booleaanse operators  
 Geef tekst Booleaanse operators (AND, OR, NOT) altijd in hoofdletters.  

#### <a name="or-operator-or-or-"></a>OR-operator `OR` of `||`

De OR-operator is een verticale balk of een pipe-teken. Bijvoorbeeld: `wifi || luxury` wordt zoeken naar documenten "Wi-Fi" of 'luxe' of beide. Omdat of is de standaardoperator combinatie kan u dit ook laten zodat `wifi luxury` is het equivalent van `wifi || luxuery`.

#### <a name="and-operator-and--or-"></a>EN de operator `AND`, `&&` of `+`

De operator is een en-teken of een plusteken (+). Bijvoorbeeld: `wifi && luxury` wordt zoeken naar documenten met zowel "Wi-Fi" en 'luxe'. Het plus-teken (+) wordt gebruikt voor vereiste voorwaarden. Bijvoorbeeld, `+wifi +luxury` bepaalt dat beide termen ergens moeten worden weergegeven in het veld van een document.


#### <a name="not-operator-not--or--"></a>GEEN operator `NOT`, `!` of `-`

De niet-operator is een uitroepteken of het minteken. Bijvoorbeeld: `wifi !luxury` wordt zoeken naar documenten waarvoor de "Wi-Fi" termijn en/of geen 'luxe'. De `searchMode` besturingselementen de optie of een term met de operator NOT and of samengevoegd met de andere voorwaarden in de query in de afwezigheid van is een + of || operator. Intrekken die `searchMode` kan worden ingesteld op `any`(standaard) of `all`.

Met behulp van `searchMode=any` verhoogt het intrekken van query's door op te nemen meer resultaten, en de standaard - wordt geïnterpreteerd als 'Of niet'. Bijvoorbeeld, `wifi -luxury` komt overeen met documenten dat beide de term bevatten *Wi-Fi* of die niet de term hebben *luxe.*

Met behulp van `searchMode=all` verhoogt de precisie van de query's door minder resultaten, en de standaard - wordt geïnterpreteerd als 'En niet'. Bijvoorbeeld, `wifi -luxury` komt overeen met de documenten die de term bevatten `wifi` en geen bevatten die de term `luxury`. Dit is weliswaar een meer intuïtieve benadering dat voor de - operator. Daarom kunt u overwegen kiezen `searchMode=all` via `searchMode=any` als u wilt optimaliseren zoekt precisie in plaats van terugroeping *en* uw gebruikers gebruiken vaak de `-` operator in zoekopdrachten.

##  <a name="bkmk_querysizelimits"></a> Beperkingen van de bestandsgrootte query  
 Er is een limiet voor de grootte van query's die u naar Azure Search verzenden kunt. U kunt hebt maximaal 1024 van de EU (expressies gescheiden door AND, OR, enzovoort). Er is ook een limiet van ongeveer 32 KB voor de grootte van elke afzonderlijke term in een query. Als uw toepassing zoekquery's via een programma genereert, wordt u aangeraden het ontwerpen van deze zodanig dat deze query's van niet-gebonden grootte wordt gegenereerd.  

##  <a name="bkmk_searchscoreforwildcardandregexqueries"></a> Scoring jokerteken en regex-query 's
 Azure Search wordt gebruikt op basis van een frequentie scoren ([TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)) voor query's van tekst. Echter wordt voor jokertekens en regex query's waarbij bereik van voorwaarden mogelijk breed zijn kan, de frequentie van meerdere factoren om te voorkomen dat de volgorde van voor de overeenkomsten van sommige voorwaarden bijstelling genegeerd. Alle overeenkomsten worden behandeld voor jokertekens en regex zoekopdrachten.

## <a name="see-also"></a>Zie ook  

+ [Documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Syntaxis voor OData-expressie voor filters en sorteren](query-odata-filter-orderby-syntax.md)   
+ [Vereenvoudigde querysyntaxis in Azure Search](query-simple-syntax.md)   
