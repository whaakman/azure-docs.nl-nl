---
title: Voorbeelden van Lucene-query voor Azure Search | Microsoft Docs
description: Lucene-querysyntaxis voor fuzzy zoeken, zoeken bij benadering, term versterking, zoeken op reguliere expressies en zoeken met jokertekens.
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: Lucene query analyzer syntax
ms.assetid: 147f360d-a5ce-4d7b-a909-c8b65bfb748c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: liamca
ms.openlocfilehash: 1faed621039ecd04064cb074e6b9011418e6ec47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene query syntaxisvoorbeelden voor het bouwen van query's in Azure Search
Bij het maken van query's voor Azure Search, kunt u ofwel de standaard [vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) of de alternatieve [Lucene Queryparser in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search). De Queryparser Lucene ondersteunt een complexere query-constructs, zoals veldgerelateerde query's, fuzzy zoeken, zoeken bij benadering, term versterking en zoeken op reguliere expressies.

In dit artikel kunt u stapsgewijs door voorbeelden demonstreren querybewerkingen beschikbaar wanneer u de volledige syntaxis.

## <a name="viewing-the-examples-in-jsfiddle"></a>De voorbeelden weergeven in JSFiddle

Alle voorbeelden in dit artikel zijn uitvoerbare query's uitvoeren op een vooraf geladen Search-index in [JSFiddle](https://jsfiddle.net), een online code-editor voor het testen van scripts en HTML. 

Als u wilt uitvoeren, met de rechtermuisknop op de query voorbeeld-URL's openen JSFiddle in een apart browservenster.

> [!NOTE]
> De volgende voorbeelden gebruikmaken van een zoekindex die bestaat uit functies die beschikbaar zijn op basis van een gegevensset die is geleverd door de [stad New York OpenData](https://nycopendata.socrata.com/) initiatief. Deze gegevens moet niet worden beschouwd als huidige of voltooid. De index is van een sandboxservice geleverd door Microsoft. U hoeft niet een Azure-abonnement of een Azure Search om te proberen deze query's.
>


## <a name="how-to-invoke-full-lucene-parsing"></a>Hoe u aan te roepen bij het parseren van volledige Lucene

Alle voorbeelden in dit artikel geeft de **queryType = full** parameter die aangeeft dat de volledige syntaxis wordt verwerkt door de Queryparser Lucene zoeken. 

**Voorbeeld 1** --met de rechtermuisknop op het volgende fragment van de query om deze te openen in een nieuwe browserpagina die wordt geladen JSFiddle en de query wordt uitgevoerd:

* [& queryType volledige = & zoeken = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

In het nieuwe browservenster wordt de JavaScript-bron- als HTML-uitvoer weergegeven naast elkaar. Het script verwijst naar een volledige query (niet alleen het fragment, zoals wordt weergegeven in de koppeling). De volledige query wordt weergegeven in de URL's voor elk voorbeeld. 

Deze query retourneert documenten uit onze index New York City-taken (nycjobs, op een sandboxservice geladen). Als beknopt alternatief bevat geeft de query alleen zakelijke titels worden geretourneerd. De volledige onderliggende query is als volgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

De **searchFields** parameter de zoekopdracht beperkt tot alleen de titel op werk. De **queryType** is ingesteld op **volledige**, die Hiermee geeft u Azure Search met de Queryparser Lucene voor deze query.

> [!NOTE]
> Zie voor achtergrondinformatie over de verwerking van query's, [hoe volledige tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md). Zie voor meer informatie over zoekparameters [documenten zoeken (Azure Search Service REST-API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).
>

### <a name="fielded-query-operation"></a>Fielded querybewerking
U kunt de voorbeelden in dit artikel wijzigen door op te geven een **fieldname:searchterm** bouw fielded querybewerking, het veld is één woord waarbij de zoekterm is ook een woord of een woordgroep, eventueel met Booleaanse operators definiëren. Voorbeelden zijn de volgende:

* business_title:(senior NOT junior)
* status: ("Den Haag ' en"Nieuwe Jersey")

Zorg ervoor dat meerdere tekenreeksen tussen aanhalingstekens zetten als u wilt dat beide tekenreeksen die moeten worden geëvalueerd als één entiteit, zoals in dit geval zoekt naar twee verschillende plaatsen in het locatieveld. Zorg er ook voor de operator een hoofdletter is als u niet ziet en AND.

Het veld dat is opgegeven in **fieldname:searchterm** moet een doorzoekbaar veld. Zie [Create Index (Azure Search Service REST-API)](https://docs.microsoft.com/rest/api/searchservice/create-index) voor meer informatie over hoe indexkenmerken in velddefinities worden gebruikt.

**Voorbeeld 2** --met de rechtermuisknop op de volgende query fragment deze query zakelijke titels met de term senior in ze, maar niet beginnend zoekt:

* [& queryType volledige = & zoeken = business_title:senior niet beginnend](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="fuzzy-search-example"></a>Voorbeeld van fuzzy zoeken
Een fuzzy zoeken overeenkomsten worden gevonden in de termen die vergelijkbaar gebouwd zijn. Per [Lucene documentatie](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), fuzzy zoekopdrachten zijn gebaseerd op [Damerau Levenshtein afstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance).

Als u wilt fuzzy zoeken, toevoegen de tilde ' ~ ' symbool aan het einde van één woord met een optionele parameter, een waarde tussen 0 en 2, waarmee de afstand bewerken. Bijvoorbeeld ' blue ~ ' of ' blue ~ 1" zou retourneren blauw, blauwe en lijm.

**Voorbeeld 3** --met de rechtermuisknop op het volgende fragment van de query. Deze query zoekt naar taken met de term koppelen (waar het is verkeerd gespeld):

* [& queryType volledige = & zoeken = business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

> [!Note]
> Fuzzy query's zijn niet [geanalyseerd](https://docs.microsoft.com/azure/search/search-lucene-query-architecture#stage-2-lexical-analysis), wat verrassend als u verwacht als gevolg of Lemmata dat kan zijn. Lexicale analyse wordt alleen uitgevoerd op de volledige (een term query of woordgroep query). Querytypen met onvolledige voorwaarden (voorvoegsel query, jokertekens query, regex query, fuzzy query) worden toegevoegd rechtstreeks aan de querystructuur van de voor het omzeilen van de analyse-fase. De enige transformatie die wordt uitgevoerd op onvolledige querytermen is lowercasing.
>

## <a name="proximity-search-example"></a>Voorbeeld van nabijheid zoeken
Nabijheid zoekopdrachten worden gebruikt voor het vinden van de voorwaarden die zich dicht bij elkaar in een document. Invoegen van een tilde ' ~ ' symbool aan het einde van een wachtwoordzin gevolgd door het aantal woorden die de nabijheid grens maken. Bijvoorbeeld 'hotel luchthaven' ~ 5 vindt u de voorwaarden hotel en luchthaven binnen 5 woorden van elkaar in een document.

**Voorbeeld 4** --met de rechtermuisknop op de query. Zoeken naar taken met de term 'senior analist' waarin deze worden gescheiden door niet meer dan één woord:

* [& queryType volledige = & zoeken = business_title: 'senior analist' ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Voorbeeld 5** --verwijderen van de woorden tussen de term "senior analist" opnieuw te proberen.

* [& queryType volledige = & zoeken = business_title: 'senior analist' ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting-examples"></a>Term versterking van voorbeelden
Term versterking verwijst naar de positie van een document hoger als de term gestimuleerd ten opzichte van documenten die niet de term hebben bevat. Dit verschilt van de score berekenen voor profielen in de betreffende scoreprofiel profielen bepaalde velden in plaats van door specifieke termen verbeteren. Kunt u het volgende voorbeeld ziet u de verschillen.

U kunt een scoreprofiel die verhoogt, zoals in een bepaalde veld overeenkomt met **genre** in het voorbeeld musicstoreindex. Term versterking kan worden gebruikt voor het verder verbeteren bepaalde zoeken hoger dan de andere voorwaarden. Bijvoorbeeld ' steen ^ 2 elektronische ' verhoogd documenten met de zoektermen op in de **genre** veld hoger is dan andere doorzoekbare velden in de index. Bovendien wordt documenten met de zoekterm 'uit' gerangschikt hoger dan de andere 'elektronische' als gevolg van de termijn versterking waarde (2) zoekterm.

Als u wilt een term verhogen, gebruikt u de caret ' ^ ', symbool met een factor versterking (een getal) aan het einde van de voorwaarden die u zoekt. Hoe hoger de versterking factor, de relevante wordt de term ten opzichte van andere zoektermen zijn. Standaard wordt de versterking factor 1. Hoewel de factor versterking moet positief zijn, kan kleiner dan 1 zijn (bijvoorbeeld 0,2) zijn.

**Voorbeeld 6** --met de rechtermuisknop op de query. Zoeken naar taken met de term 'computer analist"waar we zien er zijn geen resultaten met woorden computer en analist nog analist taken worden aan de bovenkant van de resultaten.

* [& queryType volledige = & zoeken = business_title:computer analist](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Voorbeeld 7** --opnieuw probeert, versterking van deze tijd resulteert met de term computer via de analist term als beide woorden bestaat niet.

* [& queryType volledige = & zoeken = business_title:computer ^ 2 analist](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression-example"></a>Voorbeeld van een reguliere expressie
Een zoekopdracht reguliere expressie gevonden op basis van de inhoud tussen voorwaartse slashes '/', zoals beschreven in de [klasse RegExp](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html).

**Voorbeeld 8** --met de rechtermuisknop op de query. Zoeken naar taken met ofwel de term Senior of Junior.

* `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

De URL voor dit voorbeeld wordt niet correct weergegeven op de pagina. Kopieer de onderstaande URL en plak deze in de browser-URL-adres als tijdelijke oplossing:`http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`

## <a name="wildcard-search-example"></a>Voorbeeld van jokertekens zoeken
U kunt een algemeen erkende syntaxis gebruiken voor meerdere (\*) of van afzonderlijke zoekopdrachten met jokertekens teken (?). Noteer dat de queryparser Lucene ondersteunt het gebruik van deze symbolen met één begrip en niet een wachtwoordzin.

**Voorbeeld 9** --met de rechtermuisknop op de query. Zoeken naar taken die het voorvoegsel prog, waaronder zakelijke titels met de voorwaarden programmeren en programmeurs erin zou bevatten.

* [& queryType = volledige & $select = business_title & search business_title:prog* =](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2016-09-01%26queryType=full%26$select=business_title%26search=business_title:prog*)

U kunt geen gebruiken een * of? symbool als het eerste teken van een zoekopdracht.

## <a name="next-steps"></a>Volgende stappen
Geef de Queryparser Lucene in uw code. De volgende koppelingen wordt uitgelegd hoe zoekquery's instellen voor .NET- en de REST-API. De koppelingen gebruiken de eenvoudige standaardsyntaxis dus moet u wat u hebt geleerd uit dit artikel om op te geven van de toepassing de **queryType**.

* [Query uitvoeren op uw Azure Search-Index met de .NET SDK](search-query-dotnet.md)
* [Query uitvoeren op uw Azure Search-Index met de REST API](search-query-rest-api.md)

## <a name="see-also"></a>Zie ook

 [Hoe vol tekst zoeken werkt in Azure Search](search-lucene-query-architecture.md)