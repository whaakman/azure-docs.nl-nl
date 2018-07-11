---
title: Voorbeelden van Lucene voor Azure Search | Microsoft Docs
description: Lucene-querysyntaxis voor fuzzy zoeken, zoeken op nabijheid, termenverbetering, zoeken op reguliere expressies en zoeken met jokertekens.
author: LiamCa
manager: pablocas
tags: Lucene query analyzer syntax
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: liamca
ms.openlocfilehash: a3baa17906e3bfede8a7fc5f8a0bfbde9d2a57ce
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951019"
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene-queryvoorbeelden syntaxis voor het bouwen van query's in Azure Search
Bij het maken van query's voor Azure Search, kunt u ofwel de standaard [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) of de alternatieve [Lucene-Queryparser in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). De Lucene-Queryparser biedt ondersteuning voor complexere query-constructs, zoals veldgerelateerde query's, fuzzy zoeken, zoeken op nabijheid, termenverbetering en zoeken op reguliere expressies.

In dit artikel kunt u stap voor stap door voorbeelden demonstreren querybewerkingen beschikbaar wanneer u de volledige syntaxis.

## <a name="viewing-the-examples-in-jsfiddle"></a>De voorbeelden weergeven in JSFiddle

Alle van de voorbeelden in dit artikel zijn uitvoerbare query's worden uitgevoerd voor een vooraf geladen Search-index in [JSFiddle](https://jsfiddle.net), een online code-editor voor het testen van script en HTML-code. 

Als u wilt uitvoeren, met de rechtermuisknop op de query voorbeeld-URL's JSFiddle openen in een nieuw browservenster.

> [!NOTE]
> De volgende voorbeelden gebruikmaken van een zoekindex die bestaat uit functies die beschikbaar zijn op basis van een gegevensset die is geleverd door de [stad New York OpenData](https://nycopendata.socrata.com/) initiatief. Deze gegevens moet niet worden beschouwd als huidige of voltooid. De index is een sandboxservice van Microsoft. U hoeft niet een Azure-abonnement of een Azure Search om te proberen deze query's.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Het aanroepen van de volledige Lucene-parseren

Alle voorbeelden in dit artikel geeft de **queryType = full** parameter, die aangeeft dat de volledige syntaxis wordt verwerkt door de Lucene-Queryparser zoeken. 

**Voorbeeld 1** --met de rechtermuisknop op het volgende codefragment in de query te openen in een nieuwe browserpagina die wordt geladen JSFiddle en de query wordt uitgevoerd:

* [& queryType = full & zoeken = *](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

In het nieuwe browservenster worden de JavaScript-bron- en HTML-uitvoer weergegeven naast elkaar. Het script verwijst naar een volledige query (niet alleen het codefragment, zoals wordt weergegeven in de koppeling). De volledige query wordt weergegeven in de URL's voor elk voorbeeld. 

Deze query retourneert documenten uit onze index New York City-taken (nycjobs, geladen in een sandboxservice). Beknopt alternatief bevat de query alleen zakelijke titels worden geretourneerd. De volledige onderliggende query is als volgt:

    https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

De **searchFields** parameter beperkt de zoekopdracht tot alleen het titelveld business. De **queryType** is ingesteld op **volledige**, die Azure Search met de Lucene-Queryparser voor deze query geeft.

> [!NOTE]
> Zie voor achtergrondinformatie over de verwerking van query's, [hoe volledige tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md). Zie voor meer informatie over zoekparameters [documenten zoeken (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded querybewerking
U kunt de voorbeelden in dit artikel wijzigen door op te geven een **fieldname:searchterm** bouwen voor het definiëren van een querybewerking fielded het veld is één woord, waarbij de zoekterm is ook één woord of een woordgroep, eventueel met Booleaanse operators. Enkele voorbeelden omvatten het volgende:

* business_title:(senior NOT junior)
* status: (' New York ' en "Nieuwe Jersey")

Zorg ervoor dat meerdere tekenreeksen tussen aanhalingstekens plaatsen als u wilt dat beide tekenreeksen die moeten worden geëvalueerd als één entiteit, zoals in dit geval zoekt naar twee verschillende steden in het locatieveld. Zorg er ook voor de operator is een hoofdletter als u niet ziet en en.

Het veld dat is opgegeven **fieldname:searchterm** moet een doorzoekbaar veld. Zie [Index maken (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over het gebruik van indexkenmerken in velddefinities.

**Voorbeeld 2** --met de rechtermuisknop op het volgende queryfragment deze query titels met het senior term in deze, maar niet jonge bedrijven zoekt:

* [& queryType = full & zoeken = business_title:senior niet jonge](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Een voorbeeld van fuzzy zoekopdracht
Bij een fuzzy zoekopdracht overeenkomsten zoekt in termen die een dergelijke constructie hebben. Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), fuzzy zoekopdrachten zijn gebaseerd op [Damerau Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Om te doen bij een fuzzy zoekopdracht, toevoegen de tilde ' ~ ' symbool aan het einde van één woord met een optionele parameter, een waarde tussen 0 en 2, die de afstand bewerken geeft. Bijvoorbeeld: "blauw ~ ' of ' blue ~ 1" retourneerde blauw, blauwe en lijm.

**Voorbeeld 3** --met de rechtermuisknop op het volgende codefragment in de query. Deze query zoekt voor taken met de term koppelen (indien deze is onjuist gespeld):

* [& queryType = full & zoeken business_title:asosiate = ~](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Fuzzy query's zijn niet [geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), wat verrassende als u verwacht als gevolg of lemmatisering dat kan zijn. Lexicale analyse wordt alleen uitgevoerd op de volledige (een term query of woordgroep query). Typen query's met onvolledige voorwaarden (voorvoegsel query, jokertekens query, regex-query, fuzzy query) worden toegevoegd rechtstreeks naar de querystructuur van de voor het overslaan van de analyse-fase. De enige transformatie uitgevoerd op onvolledig querytermen is locatieargument.
>

## <a name="proximity-search-example"></a>Voorbeeld van de zoekopdracht nabijheid
Nabijheid zoekopdrachten worden gebruikt om te vinden van voorwaarden die zich dicht bij elkaar in een document. Invoegen van een tilde ' ~ ' symbool aan het einde van een zin gevolgd door het aantal woorden die de grens nabijheid maken. Bijvoorbeeld, "hotel luchthaven" ongeveer 5 vindt de voorwaarden hotel en luchthaven binnen 5 woorden van elkaar in een document.

**Voorbeeld 4** --met de rechtermuisknop op de query. Zoeken naar taken met de term "senior analist" waarin deze worden gescheiden door het niet meer dan één woord:

* [& queryType = full & zoeken = business_title: 'senior analist' ~ 1](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Voorbeeld 5** --probeer het opnieuw verwijderen, de woorden tussen de term 'senior analist'.

* [& queryType = full & zoeken = business_title: 'senior analist' ~ 0](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Term versterking van voorbeelden
Termenverbetering verwijst naar de positie van een document hoger als deze bevat de boosted term, ten opzichte van documenten die de term niet bevatten. Dit wijkt af van het scoreprofielen in die scoreprofielen bepaalde velden, in plaats van specifieke voorwaarden verhogen. In het volgende voorbeeld kunt u de verschillen laten zien.

Houd rekening met een scoringprofiel die de loyaliteit zoals in een bepaald veld overeenkomt met **genre** in het voorbeeld musicstoreindex. Termenverbetering kan worden gebruikt om bepaalde zoeken verder hoger dan de andere voorwaarden te verbeteren. Bijvoorbeeld, "rock ^ elektronische 2" wordt verhogen documenten met de zoektermen op in de **genre** veld hoger is dan andere doorzoekbare velden in de index. Bovendien wordt documenten die de zoekterm "rock" bevatten gerangschikt hoger dan de andere 'elektronische' als gevolg van de termijn boost waarde (2) zoekterm.

Een term te stimuleren, gebruik het caret-teken, "^", symbool met een boost factor (een getal) aan het einde van de term die u zoekt. Hoe hoger de boost-factor, meer relevante wordt de term is ten opzichte van andere zoektermen. Standaard is de factor boost 1. Hoewel de factor boost moet positief zijn, kan deze minder dan 1 (bijvoorbeeld 0.2) worden.

**Voorbeeld 6** --met de rechtermuisknop op de query. Zoeken naar taken met de term 'computer analist"waar we er zijn geen resultaten met zowel de woorden computer en de analist, maar analist taken worden aan de bovenkant van de resultaten zien.

* [& queryType = full & zoeken = business_title:computer analist](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Voorbeeld 7** --probeer het opnieuw, deze tijd versterking resultaten met de term computer via de analist term als beide woorden bestaan niet.

* [& queryType = full & zoeken = business_title:computer ^ 2 analist](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Voorbeeld van een reguliere expressie
Een zoekopdracht reguliere expressie zoekt een overeenkomst op basis van de inhoud tussen forward slashes bevatten '/', zoals beschreven in de [klasse RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Voorbeeld 8** --met de rechtermuisknop op de query. Zoeken naar taken met ofwel de term Senior of Junior.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

De URL voor dit voorbeeld worden niet goed weergegeven op de pagina. Kopieer de onderstaande URL en plak deze in de browser-URL-adres als tijdelijke oplossing: `https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Een voorbeeld van jokertekens zoekopdracht
U kunt in het algemeen erkende syntaxis gebruiken voor meerdere (\*) of van afzonderlijke zoeken met jokertekens teken (?). Houd er rekening mee dat de Lucene-queryparser ondersteunt het gebruik van deze symbolen met een termijn van één en niet een zin.

**Voorbeeld 9** --met de rechtermuisknop op de query. Zoeken naar taken bevatten met het voorvoegsel 'programma', die zakelijke titels met de voorwaarden programmeren en de programmeur erin zou bevatten.

* [& queryType = full & $select = business_title & zoeken business_title:prog* =](https://jsfiddle.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2017-11-11%26queryType=full%26$select=business_title%26search=business_title:prog*)

U kunt geen gebruiken een * of? symbool als het eerste teken van een zoekopdracht.

## <a name="next-steps"></a>Volgende stappen
Probeer de Lucene-Queryparser in uw code op te geven. De volgende koppelingen wordt uitgelegd hoe het instellen van zoekquery's voor .NET en de REST-API. De koppelingen gebruikt u de standaard eenvoudige syntaxis, daarom u moet om toe te passen wat u hebt geleerd in dit artikel om op te geven de **queryType**.

* [Query uitvoeren op uw Azure Search-Index met de .NET SDK](search-query-dotnet.md)
* [Query uitvoeren op uw Azure Search-Index met behulp van de REST-API](search-query-rest-api.md)

## <a name="see-also"></a>Zie ook

 [Hoe vol tekstzoekopdrachten werkt in Azure Search](search-lucene-query-architecture.md)