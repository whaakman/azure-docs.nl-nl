---
title: Nieuws zoeken met de Bing Nieuws zoeken-API
titlesuffix: Azure Cognitive Services
description: Ontdek hoe u zoekquery's verzendt voor algemeen nieuws, populaire onderwerpen en koppen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 06/19/2019
ms.author: scottwhi
ms.openlocfilehash: da1dd68b8e155e121f26f5599207691877fbf0ca
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274166"
---
# <a name="search-for-news-with-the-bing-news-search-api"></a>Nieuws zoeken met de Bing Nieuws zoeken-API

Met de Bing Afbeeldingen zoeken-API kunt u de cognitieve zoekmogelijkheden makkelijk in uw toepassingen integreren.

Terwijl de Bing Nieuws zoeken-API hoofdzakelijk relevante nieuwsartikelen zoekt en retourneert, biedt het een aantal functies voor het intelligent en gericht ophalen van nieuws op internet.

## <a name="suggest-and-use-search-terms"></a>Zoektermen voorstellen en gebruiken

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker de zoekterm heeft ingevoerd, past u URL-codering toe op de term voordat u de queryparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#query) instelt. Als de gebruiker bijvoorbeeld *sailing dinghies* invoert, stelt u `q` in op `sailing+dinghies` of `sailing%20dinghies`.

## <a name="get-general-news"></a>Algemeen nieuws ophalen

Verstuur de volgende GET-aanvraag om algemene nieuwsartikelen op te halen van internet die gerelateerd zijn aan de zoekterm van de gebruiker:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat.

Als u nieuws uit een bepaald domein wilt opvragen, gebruikt u de query-operator [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

De volgende JSON-voorbeeld toont het antwoord op de vorige query. Als onderdeel van de [vereisten voor gebruik en weergave](../useanddisplayrequirements.md) van de Bing-API's voor zoeken moet u nieuwsartikelen weergeven in de volgorde waarin deze in het antwoord zijn opgenomen. Als het artikel geclusterde artikelen bevat, moet u aangeven dat er gerelateerde artikelen bestaan en dat deze op aanvraag moeten worden weergegeven.

```json
{
    "_type" : "News",
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v5\/news\/search?q=sailing+dinghies",
    "totalEstimatedMatches" : 88400,
    "value" : [{
        "name" : "Sailing Vies for Four Trophies",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891FE99A72...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.9C23AA5...",
                "width" : 650,
                "height" : 341
            }
        },
        "description" : "College Rankings, presented by Zim...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-14T15:28:00"
    },

    ...

    {
        "name" : "Fabrikam Sailing Club to host Mirror Dinghy...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.36...",
                "width" : 448,
                "height" : 300
            }
        },
        "description" : "The sailing club that trained Olympian Ben...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-04T11:02:00",
        "category" : "Sports"
    }]
}
```

De respons [News](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v5-reference#news) bevat de nieuwsartikelen die volgens Bing relevant zijn voor de query. Het veld `totalEstimatedMatches` bevat een schatting van het aantal artikelen dat beschikbaar is om weer te geven. Zie [Nieuwsartikelen per pagina weergeven](../paging-news.md) voor meer informatie over het weergeven van artikelen per pagina.

Elk [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v5-reference#newsarticle) in de lijst bevat de naam en beschrijving van het artikel, plus de URL naar het artikel op de website van de host. Als in het artikel een afbeelding staat, bevat het object een miniatuur van de afbeelding. Gebruik `name` en `url` om een hyperlink te maken waarmee de gebruiker naar het nieuwsartikel op de site van de host gaat. Als het artikel een afbeelding bevat, moet u de afbeelding bovendien klikbaar maken met `url`. Vergeet niet om met `provider` bronvermelding toe te passen voor het artikel.

Als Bing de categorie van het nieuwsartikel kan bepalen, bevat het artikel het veld `category`.

## <a name="get-todays-top-news"></a>Het belangrijkste nieuws van vandaag ophalen

Als u de bovenste nieuwsartikelen van vandaag, kunt u de dezelfde algemene nieuws aanvraag sturen als voorheen, terwijl de `q` parameter uitschakelen.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Het antwoord voor het ophalen van belangrijk nieuws is bijna hetzelfde als het account dat voor het ophalen van algemene nieuws. met als verschil dat de respons `news` geen veld `totalEstimatedMatches` bevat omdat het om een bepaald aantal resultaten gaat. Het aantal belangrijke nieuwsartikelen kan variëren, afhankelijk van de nieuwscyclus. Zorg ervoor dat u de `provider` veld naar het artikel van het kenmerk.

## <a name="get-news-by-category"></a>Nieuws ophalen per categorie

Als u nieuwsartikelen op categorie wilt opvragen, zoals de belangrijkste artikelen over sport of entertainment, verstuurt u de volgende GET-aanvraag naar Bing:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news?category=sports&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Gebruik de queryparameter [category](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category) om op te geven welke categorie artikelen u wilt ophalen. Zie [Nieuwscategorieën per markt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news-categories-by-market) voor een lijst van nieuwscategorieën die u kunt opgeven.

De respons voor het ophalen van belangrijk nieuws op categorie is bijna hetzelfde als die voor het ophalen van algemeen nieuws, Alleen bevat de respons alleen artikelen uit de opgegeven categorie.

## <a name="get-headline-news"></a>Nieuwskoppen ophalen

Als u nieuwskoppen wilt opvragen voor hoofdartikelen uit alle nieuwscategorieën, verstuurt u de volgende GET-aanvraag naar Bing:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Laat de queryparameter [category](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category) weg.

De respons voor het ophalen van nieuwskoppen is hetzelfde als die voor het ophalen van het belangrijkste nieuws. Als het artikel een hoofdartikel is, heeft het veld `headline` de waarde **true**.

De respons bevat standaard maximaal 12 hoofdartikelen. Als u dit aantal wilt wijzigen, geeft u de queryparameter [headlineCount](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#headlinecount) op. De respons bevat ook maximaal vier niet-hoofdartikelen per nieuwscategorie.

Clusters worden als één artikel geteld. Omdat een cluster verschillende artikelen kan bevatten, kan de respons meer dan 12 hoofdartikelen en meer dan vier niet-hoofdartikelen per categorie bevatten.

## <a name="get-trending-news"></a>Trending nieuws ophalen

Als u nieuwsonderwerpen wilt ophalen die trending zijn op sociale netwerken, verstuurt u de volgende GET-aanvraag naar Bing:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
X-MSAPI-UserState: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!NOTE]
> Deze functie is alleen beschikbaar voor de markten en-US en zh-CN.

De volgende JSON is de respons op de vorige aanvraag. Elk trending nieuwsartikel bevat een gerelateerde afbeelding, een vlag voor belangrijk nieuws en een URL naar de Bing-zoekresultaten voor het artikel. Gebruik de URL in het veld `webSearchUrl` om de gebruiker naar de pagina met zoekresultaten van Bing te brengen. U kunt ook de querytekst gebruiken om de [Webzoekopdrachten-API](../../bing-web-search/search-the-web.md) aan te roepen om de resultaten zelf weer te geven.

```json
{
    "_type" : "TrendingTopics",
    "value" : [{
        "name" : "Canada pot measure",
        "image" : {
            "url" : "https:\/\/www.bing.com\/th?id=OPN.RTNews_hHD...",
            "provider" : [{
                "_type" : "Organization",
                "name" : "Contoso Images"
            }]
        },
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=070292D8CEDD...",
        "isBreakingNews" : false,
        "query" : {
            "text" : "Canada marijuana"
        }
    },
    {
        "name" : "Down on Vegas move",
        "image" : {
            "url" : "https:\/\/www.bing.com\/th?id=OPN.RTNews_Bfbmg8h...",
            "provider" : [{
                "_type" : "Organization",
                "name" : "Contoso"
            }]
        },
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=070292D8CEDD...",
        "isBreakingNews" : false,
        "query" : {
            "text" : "Marcus Appel Las Vegas"
        }
    },

    ...

    ]
}
```

## <a name="getting-related-news"></a>Gerelateerd nieuws ophalen

Als er andere artikelen zijn die gerelateerd zijn aan een nieuwsartikel, kan het nieuwsartikel het veld [clusteredArticles](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle-clusteredarticles) bevatten. Hieronder ziet u een artikel met geclusterde artikelen.

```json
    {
        "name" : "Playoffs 2017: Betting lines, point spreads...",
        "url" : "http:\/\/www.bing.com\/cr?IG=4B7056CEC271408997D115...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.D7B1...",
                "width" : 700,
                "height" : 393
            }
        },
        "description" : "April 14, 2017 3:37pm EDT April 14, 2017 3:34pm...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-14T19:43:00",
        "category" : "Sports",
        "clusteredArticles" : [{
            "name" : "Playoffs 2017: Betting odds, favorites to win...",
            "url" : "http:\/\/www.bing.com\/cr?IG=4B7056CEC271408997D1159E...",
            "description" : "April 14, 2017 3:30pm EDT April 14, 2017 3:27pm...",
            "provider" : [{
                "_type" : "Organization",
                "name" : "Contoso"
            }],
            "datePublished" : "2017-04-14T19:37:00",
            "category" : "Sports"
        }]
    },
```

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bladeren door de pagina's resultaten met Bing Nieuws zoeken-resultaten](../paging-news.md)
