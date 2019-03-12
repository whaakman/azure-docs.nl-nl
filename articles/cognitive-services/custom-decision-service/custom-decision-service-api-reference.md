---
title: API-verwijzing - Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Een volledige API-handleiding voor Custom Decision Service.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/11/2018
ms.author: slivkins
ms.openlocfilehash: be9966f5d8e8d94aa3f49aac91b35b105195b108
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552058"
---
# <a name="api"></a>API

Azure Custom Decision Service biedt twee API's die worden aangeroepen voor elk besluit: de [Trefwoordenrangschikking API](#ranking-api) voor het invoeren van de volgorde van de acties en de [beloning API](#reward-api) om uit te voeren van de prijs. Daarnaast bieden u een [actie ingesteld API](#action-set-api-customer-provided) om op te geven welke acties moeten Azure Custom Decision Service. In dit artikel bevat informatie over deze drie API's. Een typisch scenario is hieronder gebruikt om weer te geven wanneer Custom Decision Service de rangorde van artikelen optimaliseert.

## <a name="ranking-api"></a>Ranking API

De classificatie-API maakt gebruikt van een standaard [JSONP](https://en.wikipedia.org/wiki/JSONP)-stijl communicatiepatroon om de latentie te optimaliseren en overslaan van de [beleid voor zelfde oorsprong](https://en.wikipedia.org/wiki/Same-origin_policy). De laatste verboden JavaScript uit het ophalen van gegevens die zich buiten de oorsprong van de pagina.

Dit fragment invoegen in de HTML-kop van de front-pagina (indien een aangepaste lijst met artikelen worden weergegeven):

```html
// define the "callback function" to render UI
<script> function callback(data) { … } </script>
// "data" provides the ranking of URLs, see example below.

// call to Ranking API
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>?<parameters>" async></script>
// action set id is the name of the corresponding RSS/Atom feed.

// same call with multiple action sets:
// <script src="https://ds.microsoft.com/api/v2/<appId>/rank/<A1>/<A2>/.../<An>?<parameters>" async></script>
```

> [!IMPORTANT]
> De callback-functie moet worden gedefinieerd voordat de aanroep naar de API positie.

> [!TIP]
> Voor een betere latentie, de Trefwoordenrangschikking-API is beschikbaar via HTTP in plaats van HTTPS, zoals in `https://ds.microsoft.com/api/v2/<appId>/rank/*`.
> Een HTTPS-eindpunt moet echter worden gebruikt als de front-pagina wordt geleverd via HTTPS.

Als parameters niet gebruikt worden, is het HTTP-antwoord van de API Trefwoordenrangschikking een tekenreeks in-en JSONP-indeling:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

Deze tekenreeks wordt vervolgens in de browser uitgevoerd als een aanroep naar de functie `callback()`.

De parameter voor de callbackfunctie in het voorgaande voorbeeld is het volgende schema:

- `ranking` biedt de rangorde van URL's moet worden weergegeven.
- `eventId` wordt intern gebruikt door Custom Decision Service zodat deze overeenkomt met deze classificatie met de bijbehorende klikken.
- `appId` Hiermee staat de callbackfunctie onderscheid maken tussen meerdere toepassingen van Custom Decision Service die wordt uitgevoerd op de webpagina van de dezelfde.
- `actionSets` Geeft een lijst van elke actie die moet worden gebruikt in de volgorde API-aanroep samen met de UTC-timestamp van de laatste geslaagde vernieuwing. Custom Decision Service wordt regelmatig vernieuwd voor de actie set-feeds. Bijvoorbeeld, als deel van de sets actie niet actueel zijn, de callback-functie mogelijk terugvallen op hun positie standaard.

> [!IMPORTANT]
> De opgegeven actie-sets zijn verwerkt, en mogelijk verwijderd, om de standaard-volgorde van artikelen. De rangorde van de standaard opgehaald vervolgens opnieuw geordend en in het HTTP-antwoord geretourneerd. Hier wordt de standaard-volgorde gedefinieerd:
>
> - De artikelen zijn binnen elke set actie de 15 meest recente artikelen verwijderd (als er meer dan 15 worden geretourneerd).
> - Wanneer u meerdere sets van actie zijn opgegeven, moeten ze worden samengevoegd in dezelfde volgorde als in de API-aanroep. De oorspronkelijke positie van de artikelen bewaard binnen elke set actie. Dubbele waarden zijn verwijderd en vervangen door de eerdere exemplaren.
> - De eerste `n` artikelen worden bewaard in de samengevoegde lijst met artikelen, waarbij `n=20` standaard.

### <a name="ranking-api-with-parameters"></a>Volgorde-API met parameters

De rangorde-API kunt deze parameters:

- `details=1` en `details=2` Hiermee voegt u aanvullende informatie over elk die worden vermeld in artikel `ranking`.
- `limit=<n>` Hiermee geeft u het maximale aantal van de artikelen in de standaard-classificatie. `n` moet liggen tussen `2` en `30` (of anders wordt deze afgekapt tot `2` of `30`respectievelijk).
- `dnt=1` Hiermee schakelt u gebruikerscookies.

Parameters kunnen worden gecombineerd in standaard, syntaxis in de tekenreeks, bijvoorbeeld `details=2&dnt=1`.

> [!IMPORTANT]
> De standaardinstelling in Europa moet `dnt=1` totdat de klant stemt ermee in op de banner van de cookie. Ook moet de standaardinstelling voor websites die zijn gericht op minderjarigen. Zie voor meer informatie de [gebruiksvoorwaarden](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

De `details=1` element voegt van elk artikel `guid`, als deze wordt geleverd door de API voor het instellen van actie. Het HTTP-antwoord:

```json
callback({
   "ranking":[{"id":"url1","details":[{"guid":"123"}]},
              {"id":"url2","details":[{"guid":"456"}]},
              {"id":"url3","details":[{"guid":"789"}]}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

De `details=2` element voegt meer details die Custom Decision Service aan artikelen Zoekmachineoptimalisatie metatags onttrekken kunnen [parametrisatie code](https://github.com/Microsoft/mwt-ds/tree/master/Crawl):

- `title` van `<meta property="og:title" content="..." />` of `<meta property="twitter:title" content="..." />` of `<title>...</title>`
- `description` van `<meta property="og:description" ... />` of `<meta property="twitter:description" content="..." />` of `<meta property="description" content="..." />`
- `image` Van `<meta property="og:image" content="..." />`
- `ds_id` Van `<meta name=”microsoft:ds_id” content="..." />`

Het HTTP-antwoord:

```json
callback({
   "ranking":[{"id":"url1","details":<details>},
              {"id":"url2","details":<details>},
              {"id":"url3","details":<details>}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"timeStamp1"},
                 {"id":"<A2>","lastRefresh":"timeStamp2"}]});
```

De `<details>` element:

```json
[{"guid":"123"}, {"description":"some text", "ds_id":"234", "image":"ImageUrl1", "title":"some text"}]
```

## <a name="reward-api"></a>Beloning API

Aangepaste Decision Service maakt gebruik van klikken op de bovenste site. Elke muisklik wordt geïnterpreteerd als een beloning van 1. Het ontbreken van een klik wordt geïnterpreteerd als een beloning van 0. Klikken met de bijbehorende classificatie wordt voldaan met behulp van de gebeurtenis-id's die worden gegenereerd door de [Trefwoordenrangschikking API](#ranking-api) aanroepen. Indien nodig, event id's kunnen worden doorgegeven via sessiecookies.

Voor het afhandelen van een klik op de bovenste site, zet u deze code op de front-pagina:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Hier `data` is het argument voor de `callback()` functioneren, zoals eerder beschreven. Met behulp van `data` in de op zorgvuldig te werk gaan verwerken code vereist. Een voorbeeld ziet u in deze [zelfstudie](custom-decision-service-tutorial-news.md#use-the-apis).

Alleen voor testdoeleinden, de API van derden kan worden aangeroepen [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

Het verwachte resultaat is een HTTP-respons van 200 (OK). U ziet de prijs van 1 voor deze gebeurtenis in het logboek (als de sleutel van een Azure storage-account is opgegeven in de portal).

## <a name="action-set-api-customer-provided"></a>Actie ingesteld API (klantgeleverde)

De API voor het instellen van actie retourneert op hoog niveau, een lijst met artikelen (acties). Elk artikel wordt opgegeven door de URL en (optioneel) de titel van het artikel en de publicatiedatum. U kunt dat verschillende actie wordt ingesteld op de portal. Een andere actie instellen-API moet worden gebruikt voor elke actie, als een afzonderlijke URL.

Elke API voor het instellen van actie kan worden geïmplementeerd op twee manieren: als een RSS-feed of een Atom-feed. Ofwel een moet voldoen aan de standaard en een juiste XML geretourneerd. RSS volgt hier een voorbeeld:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <guid>guid (could be a your internal database id)</guid>
      <pubDate>date</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Elke op het hoogste niveau `<item>` -element een actie worden beschreven:

- `<link>` is verplicht en wordt gebruikt als een actie-ID.
- `<date>` wordt genegeerd als deze kleiner dan of gelijk aan 15 items is; anders is het verplicht.
  - Als er meer dan 15 items, worden 15 meest recente die gebruikt.
  - Het moet de standaardindeling voor RSS- of Atom, respectievelijk:
    - [RFC 822](https://tools.ietf.org/html/rfc822) voor RSS: bijvoorbeeld `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) voor Atom: bijvoorbeeld `"2016-12-19T16:39:57-08:00"`
- `<title>` is optioneel en wordt gebruikt voor het genereren van functies die het artikel wordt beschreven.
- `<guid>` is optioneel en is doorgegeven via het systeem de callback-functie (als de `?details` parameter is opgegeven in de volgorde API-aanroep).

Andere elementen binnen een `<item>` worden genegeerd.

De Atom-feed versie gebruikt dezelfde XML-syntaxis en conventies.

> [!TIP]
> Als uw systeem een eigen artikel-id's gebruikt, ze kunnen worden doorgegeven in de callback-functie door met behulp van `<guid>`.
