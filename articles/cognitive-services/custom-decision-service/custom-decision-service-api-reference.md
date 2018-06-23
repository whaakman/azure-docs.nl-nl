---
title: API - cognitieve Azure-Services | Microsoft Docs
description: Een volledig en gebruiksvriendelijk API handleiding voor Azure-Service voor het besluit van aangepaste, een cloud-gebaseerde API voor de contextuele besluitvorming die ervaring biedt.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/11/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 403b17e33394016a07a7b33ba1bcbfe6afdcc05b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345597"
---
# <a name="api"></a>API

Azure aangepaste besluit Service biedt twee API's die worden aangeroepen voor elke beslissing: de [rangschikking API](#ranking-api) voor het invoeren van de volgorde van acties en de [derden API](#reward-api) voor uitvoer van de prijs. Bovendien bieden u een [actie ingesteld API](#action-set-api-customer-provided) om op te geven welke acties moeten Azure aangepaste besluit Service. In dit artikel bevat informatie over deze drie API's. Een typisch scenario is hieronder gebruikt om weer te geven wanneer u aangepaste besluit Service optimaliseert de volgorde van artikelen.

## <a name="ranking-api"></a>Ranking API

De ranking-API maakt gebruikt van een standaard [JSONP](https://en.wikipedia.org/wiki/JSONP)-stijl communicatiepatronen te optimaliseren latentie en overslaan van de [dezelfde oorsprong beleid](https://en.wikipedia.org/wiki/Same-origin_policy). De laatste verhindert JavaScript ophalen van gegevens van buiten de oorsprong van de pagina.

In dit fragment invoegen in de HTML-kop van uw voorpagina (waarbij een aangepaste lijst met artikelen worden weergegeven):

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
> De callback-functie moet worden gedefinieerd vóór de aanroep naar de API positie.

> [!TIP]
> Ter verbetering van de latentie van de API rangschikking wordt blootgesteld via HTTP in plaats van HTTPS, zoals in `http://ds.microsoft.com/api/v2/<appId>/rank/*`.
> Een HTTPS-eindpunt moet echter worden gebruikt als de voorpagina worden geleverd via HTTPS.

Als parameters niet gebruikt worden, is het HTTP-antwoord van de API rangschikking een JSONP opgemaakte tekenreeks:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "actionSets":[{"id":"<A1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<A2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]});
```

De browser voert deze tekenreeks als een aanroep van de `callback()` functie.

De parameter voor de callback-functie in het voorgaande voorbeeld is het volgende schema:

- `ranking` Geeft de positie van URL's moet worden weergegeven.
- `eventId` wordt intern gebruikt door aangepaste besluit Service overeenkomen met deze ranking met de bijbehorende klikken.
- `appId` kan de functie voor retouraanroepen onderscheid maken tussen meerdere toepassingen van aangepaste besluit-Service wordt uitgevoerd op dezelfde webpagina.
- `actionSets` Geeft een lijst van elke actie die moet worden gebruikt in de volgorde API-aanroep samen met de UTC-timestamp van de laatste voltooide vernieuwing. De actie set-feeds van aangepaste besluit Service worden regelmatig vernieuwd. Bijvoorbeeld, als een deel van de actie-sets niet actueel zijn, de callback-functie mogelijk terugvallen op de rangschikking van hun standaard.

> [!IMPORTANT]
> De opgegeven actie sets worden verwerkt en wordt mogelijk verwijderd om de volgorde van de standaard van artikelen. De volgorde van de standaard haalt vervolgens opnieuw geordend en in het HTTP-antwoord geretourneerd. Hier wordt de standaard-volgorde gedefinieerd:
>
> - Binnen elke set actie worden de artikelen weggehaald naar de meest recente 15 artikelen (als meer dan 15 worden geretourneerd).
> - Wanneer meerdere actie sets zijn opgegeven, moeten ze worden samengevoegd in dezelfde volgorde als in de API-aanroep. De oorspronkelijke ordening van de artikelen bewaard binnen elke set actie. Duplicaten zijn verwijderd en vervangen door de eerdere exemplaren.
> - De eerste `n` artikelen worden bewaard in de samengevoegde lijst met artikelen, waarbij `n=20` standaard.

### <a name="ranking-api-with-parameters"></a>Ranking API met parameters

De positie API kunt deze parameters:

- `details=1` en `details=2` voegt aanvullende details over elk vermeld in artikel `ranking`.
- `limit=<n>` Hiermee geeft u het maximale aantal van de artikelen in de volgorde van de standaard. `n` moet liggen tussen `2` en `30` (of anders wordt deze afgekapt tot `2` of `30`respectievelijk).
- `dnt=1` Hiermee schakelt u gebruikerscookies.

Parameters kunnen worden gecombineerd in standaard, syntaxis in de tekenreeks, bijvoorbeeld `details=2&dnt=1`.

> [!IMPORTANT]
> De standaardinstelling in Europa moet `dnt=1` totdat de klant ermee akkoord naar de banner cookie gaat. Deze moet ook de standaardinstelling voor websites die zijn gericht op minderjarigen. Zie voor meer informatie de [gebruiksvoorwaarden](https://www.microsoft.com/cognitive-services/en-us/legal/CognitiveServicesTerms20160804).

De `details=1` element voegt elk artikel `guid`, als deze wordt geleverd door de API voor het instellen van actie. Het HTTP-antwoord:

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

De `details=2` element voegt meer details die aangepaste besluit Service uit de artikelen Zoekmachineoptimalisatie metatags extraheren mogelijk [featurization code](https://github.com/Microsoft/mwt-ds/tree/master/Crawl):

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

## <a name="reward-api"></a>Derden API

Maakt gebruik van aangepaste besluit Service alleen op de bovenste sleuf klikt. Bij elke muisklik wordt geïnterpreteerd als een compensatie van 1. Het ontbreken van een klik wordt geïnterpreteerd als een compensatie van 0. Muisklikken worden vergeleken met de bijbehorende classificatie met behulp van de gebeurtenis-id's die worden gegenereerd door de [rangschikking API](#ranking-api) aanroepen. Indien nodig, event id's kunnen worden doorgegeven via sessiecookies.

Plaats deze code op de front-pagina voor het afhandelen van een klik op de bovenste sleuf:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Hier `data` is het argument voor de `callback()` functioneren, zoals eerder beschreven. Met behulp van `data` in de op zorgvuldig te werk gaan afhandeling van code vereist. Een voorbeeld wordt weergegeven in deze [zelfstudie](custom-decision-service-tutorial-news.md#use-the-apis).

Alleen voor testdoeleinden, kan de API van derden worden aangeroepen [cURL](https://en.wikipedia.org/wiki/CURL):

```sh
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST -d 1 -H "Content-Type: application/json"
```

Het verwachte resultaat is een HTTP-antwoord van 200 (OK). U ziet de prijs van 1 voor deze gebeurtenis in het logboek (als de sleutel van een Azure storage-account is opgegeven in de portal).

## <a name="action-set-api-customer-provided"></a>Actie ingesteld API (de klant is opgegeven)

Op hoog niveau resulteert de API voor het instellen van actie een lijst met artikelen (acties). Elk artikel is opgegeven door de URL en (optioneel) titel van het artikel en de publicatiedatum. U kunt dat verschillende actie wordt ingesteld op de portal. Een andere actie ingesteld API moet worden gebruikt voor elke set actie als een unieke URL.

Elke actie ingesteld API kan worden geïmplementeerd op twee manieren: als een RSS-feed of als een Atom-feed. Ofwel een moet voldoen aan de standaard en retourneren een juiste XML. RSS volgt hier een voorbeeld:

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

Elke site op het hoogste `<item>` element een actie beschrijft:

- `<link>` is verplicht en wordt gebruikt als een actie-ID.
- `<date>` wordt genegeerd als deze kleiner dan of gelijk aan 15 items is; anders is het verplicht.
  - Als er meer dan 15 items, worden 15 meest recente die gebruikt.
  - Het moet de standaardindeling voor RSS- of Atom, respectievelijk:
    - [RFC 822](https://tools.ietf.org/html/rfc822) voor RSS: bijvoorbeeld: `"Fri, 28 Apr 2017 18:02:06 GMT"`
    - [RFC 3339](https://tools.ietf.org/html/rfc3339) voor Atom: bijvoorbeeld: `"2016-12-19T16:39:57-08:00"`
- `<title>` is optioneel en wordt gebruikt voor het genereren van functies die het artikel wordt beschreven.
- `<guid>` is optioneel en doorgegeven via het systeem voor de callbackfunctie (als de `?details` parameter is opgegeven in de volgorde API-aanroep).

Andere elementen in een `<item>` worden genegeerd.

De Atom-feed versie gebruikt dezelfde XML-syntaxis en conventies.

> [!TIP]
> Als uw systeem gebruikmaakt van een eigen artikel-id's, ze kunnen worden doorgegeven in de functie voor retouraanroepen door met `<guid>`.