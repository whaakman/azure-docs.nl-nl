---
title: Voorbeelden van Lucene voor Azure Search | Microsoft Docs
description: Lucene-querysyntaxis voor fuzzy zoeken, zoeken op nabijheid, termenverbetering, zoeken op reguliere expressies en zoeken met jokertekens in Azure Search-service.
author: LiamCa
manager: pablocas
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: liamca
ms.openlocfilehash: 7da1f5d54a9dd5b6119b81ef801b674263a98bae
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716413"
---
# <a name="lucene-syntax-query-examples-for-building-advanced-queries-in-azure-search"></a>Voorbeelden van Lucene-querysyntaxis voor het bouwen van geavanceerde query's in Azure Search
Bij het maken van query's voor Azure Search, kunt u de standaard vervangen [de eenvoudige queryparser](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) met de alternatieve [Lucene-Queryparser in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) naar gespecialiseerde en geavanceerde query formuleren definities. 

De Lucene-Queryparser biedt ondersteuning voor complexere query-constructs, zoals veldgerelateerde query's, zoeken bij benadering en zoeken met jokertekens, zoeken op nabijheid, termenverbetering en zoeken op reguliere expressies. De extra capaciteit wordt geleverd met extra verwerkingsvereisten. In dit artikel kunt u stap voor stap door voorbeelden demonstreren querybewerkingen beschikbaar wanneer u de volledige syntaxis.

> [!Note]
> Veel van de speciale query-constructies ingeschakeld via de volledige Lucene-querysyntaxis zijn niet [tekst geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), wat verrassende als u verwacht als gevolg of lemmatisering dat kan zijn. Lexicale analyse wordt alleen uitgevoerd op de volledige (een term query of woordgroep query). Typen query's met onvolledige voorwaarden (voorvoegsel query, jokertekens query, regex-query, fuzzy query) worden toegevoegd rechtstreeks naar de querystructuur van de voor het overslaan van de analyse-fase. De enige transformatie uitgevoerd op onvolledig querytermen is locatieargument. 
>

## <a name="formulate-requests-in-postman"></a>Aanvragen in Postman formuleren

De volgende voorbeelden gebruikmaken van een NYC-Jobs search-index die bestaat uit functies die beschikbaar zijn op basis van een gegevensset die is geleverd door de [stad New York OpenData](https://opendata.cityofnewyork.us/) initiatief. Deze gegevens moet niet worden beschouwd als huidige of voltooid. De index is een sandboxservice van Microsoft, wat betekent dat u hoeft niet een Azure-abonnement of een Azure Search om te proberen deze query's.

Wat u moet is Postman of een gelijkwaardig hulpprogramma voor het uitgeven van HTTP-aanvraag voor ontvangen. Zie voor meer informatie, [testen met REST-clients](search-fiddler.md).

### <a name="set-the-request-header"></a>De aanvraagheader instellen

1. Stel in de aanvraagheader **Content-Type** naar `application/json`.

2. Voeg een **api-sleutel**, en stel deze in op deze tekenreeks: `252044BE3886FE4A8E3BAA4F595114BB`. Dit is de querysleutel van een voor de sandbox-search-service die als host fungeert voor de index NYC-Jobs.

Nadat u de aanvraagheader opgeeft, u deze opnieuw kunt gebruiken voor alle van de query's in dit artikel wordt alleen simpelweg de **search =** tekenreeks. 

  ![De aanvraagheader voor Postman](media/search-query-lucene-examples/postman-header.png)

### <a name="set-the-request-url"></a>De aanvraag-URL instellen

De aanvraag is een GET-opdracht die is gekoppeld aan een URL met de tekenreeks die Azure Search-eindpunt en zoeken.

  ![De aanvraagheader voor Postman](media/search-query-lucene-examples/postman-basic-url-request-elements.png)

Samenstelling van de URL heeft de volgende elementen:

+ **`https://azs-playground.search.windows.net/`** een sandbox-zoekservice is onderhouden door het ontwikkelingsteam Azure Search. 
+ **`indexes/nycjobs/`** is de index NYC-Jobs in de verzameling indexen van de betreffende service. De servicenaam en de index zijn vereist voor de aanvraag.
+ **`docs`** de verzameling documenten is die alle doorzoekbare inhoud bevat. De query api-sleutel opgegeven in de aanvraagheader werkt alleen op leesbewerkingen die gericht is op de verzameling documenten.
+ **`api-version=2017-11-11`** Hiermee stelt u de api-versie, is een vereiste parameter bij elke aanvraag.
+ **`search=*`** de querytekenreeks, die in de eerste query null is is, de eerste 50 resultaten retourneren (standaard).

## <a name="send-your-first-query"></a>Uw eerste query verzenden

Als verificatiestap, plak de volgende aanvraag ophalen en klikt u op **verzenden**. Resultaten worden geretourneerd als uitgebreide JSON-documenten. U kunt kopiëren en plakken deze URL in het eerste voorbeeld hieronder.

  ```http
  https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&search=*
  ```

De queryreeks ** `search=*` **, is een niet-opgegeven zoekopdracht gelijk is aan null of leeg zijn zoeken. Het is niet met name nuttig, maar is de eenvoudigste zoeken die u kunt doen.

U kunt desgewenst toevoegen ** `$count=true` ** naar de URL om een telling van de documenten die voldoen aan de zoekcriteria te retourneren. Op een lege zoekopdracht tekenreeks is is dat alle documenten in de index (2802 in het geval van NYC-Jobs).

## <a name="how-to-invoke-full-lucene-parsing"></a>Het aanroepen van de volledige Lucene-parseren

Voeg **queryType = full** aanroepen van de volledige querysyntaxis, overschrijven de standaard vereenvoudigde querysyntaxis. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&queryType=full&search=*
```

Alle voorbeelden in dit artikel geeft de **queryType = full** parameter, die aangeeft dat de volledige syntaxis wordt verwerkt door de Lucene-Queryparser zoeken. 

## <a name="example-1-field-scoped-query"></a>Voorbeeld 1: Veldgerelateerde query

De eerste query is niet een demonstratie van volledige Lucene-syntaxis (dit werkt voor zowel eenvoudige als de volledige syntaxis), maar we leiden met het volgende voorbeeld om te introduceren een basislijn query concept dat een redelijk leesbare JSON-antwoord produceert. Beknopt alternatief, gericht op de query alleen de *business_title* veld en geeft u alleen zakelijke titels worden geretourneerd. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=*
```

De **searchFields** parameter beperkt de zoekopdracht tot alleen het titelveld business. De **Selecteer** parameter bepaalt welke velden zijn opgenomen in de resultatenset.

Antwoord voor deze query moet eruitzien als op de volgende schermafbeelding.

  ![Het voorbeeldantwoord postman](media/search-query-lucene-examples/postman-sample-results.png)

Je misschien opgevallen dat de zoekscore ook wordt geretourneerd voor elk document dat hoewel zoekscore is niet opgegeven. Dit komt omdat zoekscore metagegevens, met de waarde die aangeeft positie volgorde van de resultaten. Uniform scores van 1 optreden wanneer er is geen positie, ofwel omdat de zoekopdracht is niet zoeken in volledige tekst, of omdat er geen criteria om toe te passen. Voor null search, is er geen criteria en de rijen die in willekeurige volgorde. Als de zoekcriteria neemt meer definitie, ziet u scores in zinvolle waarden ontwikkelen zoeken.

## <a name="example-2-in-field-filtering"></a>Voorbeeld 2: In-veld filteren

Volledige Lucene-syntaxis ondersteunt expressies in een veld. Deze query zoekt naar titels met het senior term in deze, maar niet jonge bedrijven:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:senior+NOT+junior
```

Door op te geven een **fieldname:searchterm** bouw, kunt u een querybewerking fielded het veld is één woord, waarbij de zoekterm is ook één woord of een woordgroep, eventueel met Booleaanse operators definiëren. Enkele voorbeelden omvatten het volgende:

* business_title:(senior NOT junior)
* status: (' New York ' en "Nieuwe Jersey")

Zorg ervoor dat meerdere tekenreeksen tussen aanhalingstekens plaatsen als u wilt dat beide tekenreeksen die moeten worden geëvalueerd als één entiteit, zoals in dit geval zoekt naar twee verschillende steden in het locatieveld. Zorg er ook voor de operator is een hoofdletter als u niet ziet en en.

Het veld dat is opgegeven **fieldname:searchterm** moet een doorzoekbaar veld. Zie [Index maken (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over het gebruik van indexkenmerken in velddefinities.

## <a name="example-3-fuzzy-search"></a>Voorbeeld 3: Fuzzy zoeken

Fuzzy zoeken, die overeenkomt met van voorwaarden die u een dergelijke constructie hebt biedt ook ondersteuning voor volledige Lucene-syntaxis. Om te doen bij een fuzzy zoekopdracht, toevoegen de tilde `~` symbool aan het einde van één woord met een optionele parameter, een waarde tussen 0 en 2, die de afstand bewerken geeft. Bijvoorbeeld, `blue~` of `blue~1` retourneerde blauw, blauwe en lijm.

Deze query zoekt voor taken met de term 'koppelen' (opzettelijk onjuist gespeld):

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:asosiate~
```

Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), fuzzy zoekopdrachten zijn gebaseerd op [Damerau Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

> [!Note]
> Fuzzy query's zijn niet [geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). Typen query's met onvolledige voorwaarden (voorvoegsel query, jokertekens query, regex-query, fuzzy query) worden toegevoegd rechtstreeks naar de querystructuur van de voor het overslaan van de analyse-fase. De enige transformatie uitgevoerd op onvolledig querytermen is locatieargument.
>

## <a name="example-4-proximity-search"></a>Voorbeeld 4: Zoeken op nabijheid
Nabijheid zoekopdrachten worden gebruikt om te vinden van voorwaarden die zich dicht bij elkaar in een document. Invoegen van een tilde ' ~ ' symbool aan het einde van een zin gevolgd door het aantal woorden die de grens nabijheid maken. Bijvoorbeeld, "hotel luchthaven" ongeveer 5 vindt de voorwaarden hotel en luchthaven binnen 5 woorden van elkaar in een document.

In deze query voor taken met de term 'senior analist"waarin deze worden gescheiden door het niet meer dan één woord:

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~1
```

Probeer het opnieuw verwijderen, de woorden tussen de term 'senior analist'. U ziet dat 8 documenten voor deze query in plaats van 10 voor de vorige query zijn geretourneerd.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:%22senior%20analyst%22~0
```

## <a name="example-5-term-boosting"></a>Voorbeeld 5: Termenverbetering
Termenverbetering verwijst naar de positie van een document hoger als deze bevat de boosted term, ten opzichte van documenten die de term niet bevatten. Een term te stimuleren, gebruik het caret-teken, "^", symbool met een boost factor (een getal) aan het einde van de term die u zoekt. 

Zoeken in deze 'voor' query voor taken met de term *computer analist* en u ziet er zijn geen resultaten met beide woorden *computer* en *analist*, nog * computer* taken worden aan de bovenkant van de resultaten.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst
```

Herhaal de zoekactie, ditmaal versterking van resultaten met de term in de query 'na' *analist* gedurende de termijn *computer* als beide woorden bestaan niet. 

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:computer%20analyst%5e2
```
Er is een nog mensachtigere leesbare versie van de bovenstaande query `search=business_title:computer analyst^2`. Voor een werkbare query `^2` is gecodeerd als `%5E2`, die het moeilijker is om te zien.

Termenverbetering wijkt af van het scoreprofielen in die scoreprofielen bepaalde velden, in plaats van specifieke voorwaarden verhogen. In het volgende voorbeeld kunt u de verschillen laten zien.

Houd rekening met een scoringprofiel die de loyaliteit zoals in een bepaald veld overeenkomt met **genre** in het voorbeeld musicstoreindex. Termenverbetering kan worden gebruikt om bepaalde zoeken verder hoger dan de andere voorwaarden te verbeteren. Bijvoorbeeld, "rock ^ elektronische 2" wordt verhogen documenten met de zoektermen op in de **genre** veld hoger is dan andere doorzoekbare velden in de index. Bovendien wordt documenten die de zoekterm "rock" bevatten gerangschikt hoger dan de andere 'elektronische' als gevolg van de termijn boost waarde (2) zoekterm.

Bij het instellen van het niveau van de multi-factor Authentication, hoe hoger de boost-factor, meer relevante de term is ten opzichte van andere zoektermen. Standaard is de factor boost 1. Hoewel de factor boost moet positief zijn, kan deze minder dan 1 (bijvoorbeeld 0.2) worden.


## <a name="example-6-regex"></a>Voorbeeld 6: reguliere expressie

Een zoekopdracht reguliere expressie zoekt een overeenkomst op basis van de inhoud tussen forward slashes bevatten '/', zoals beschreven in de [klasse RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

In deze query zoekt voor taken met de term Senior of Junior: ' search business_title:/(Sen| = Juni) ior /''.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:/(Sen|Jun)ior/
```
> [!Note]
> Regex-query's zijn niet [geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). De enige transformatie uitgevoerd op onvolledig querytermen is locatieargument.
>

## <a name="example-7-wildcard-search"></a>Voorbeeld 7: Zoeken met jokertekens
U kunt in het algemeen erkende syntaxis gebruiken voor meerdere (\*) of van afzonderlijke zoeken met jokertekens teken (?). Houd er rekening mee dat de Lucene-queryparser ondersteunt het gebruik van deze symbolen met een termijn van één en niet een zin.

In deze query zoeken naar taken bevatten met het voorvoegsel 'programma', die zakelijke titels met de voorwaarden programmeren en de programmeur erin zou bevatten.

```GET
https://azs-playground.search.windows.net/indexes/nycjobs/docs?api-version=2017-11-11&$count=true&searchFields=business_title&$select=business_title&queryType=full&search=business_title:prog*
```
U kunt geen gebruiken een * of? symbool als het eerste teken van een zoekopdracht.

> [!Note]
> Query's met jokertekens zijn niet [geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis). De enige transformatie uitgevoerd op onvolledig querytermen is locatieargument.
>

## <a name="next-steps"></a>Volgende stappen
Probeer de Lucene-Queryparser in uw code op te geven. De volgende koppelingen wordt uitgelegd hoe het instellen van zoekquery's voor .NET en de REST-API. De koppelingen gebruikt u de standaard eenvoudige syntaxis, daarom u moet om toe te passen wat u hebt geleerd in dit artikel om op te geven de **queryType**.

* [Query uitvoeren op uw Azure Search-Index met de .NET SDK](search-query-dotnet.md)
* [Query uitvoeren op uw Azure Search-Index met behulp van de REST-API](search-query-rest-api.md)

Naslaginformatie over aanvullende syntaxis voor query-architectuur en voorbeelden vindt u in de volgende koppelingen:

+ [Voorbeelden van eenvoudige querysyntaxis](search-query-simple-examples.md)
+ [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)
+ [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Volledige Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)