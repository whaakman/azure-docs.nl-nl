---
title: API aanroepen vanuit een app - cognitieve Azure-Services | Microsoft Docs
description: Klik hier voor meer informatie over het aan de slag met Azure aangepaste besluit Service als u de API vanuit een smartphone-app aanroept.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: slivkins
ms.reviewer: marcozo, alekh
ms.openlocfilehash: 2d02b0deaaa701dd0b4818638827c04e2c946558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345844"
---
# <a name="call-api-from-an-app"></a>API aanroepen vanuit een app

Controleer de API's van Azure aangepaste besluit Service-aanroepen vanuit een smartphone-app. In dit artikel wordt uitgelegd hoe u aan de slag met enkele eenvoudige opties.

Zorg ervoor dat [uw toepassing registreren](custom-decision-service-get-started-register.md), eerste.

Er zijn twee API-aanroepen die u van uw smartphone-app met aangepaste besluit Service maken: een aanroep van de API positie ophalen van een gerangschikte lijst met uw inhoud en een aanroep naar de API van derden voor het rapporteren van een compensatie. Hier worden de voorbeeld-aanroepen in [cURL](https://en.wikipedia.org/wiki/CURL).

Zie voor meer informatie de verwijzing [API](custom-decision-service-api-reference.md).

Beginnen met het aanroepen van de API positie. Maak het bestand `<request.json>`, welke heeft de actie set ID. Deze ID is de naam van de bijbehorende RSS of Atom-feed dat u hebt ingevoerd in de portal:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

Veel actie sets kunnen als volgt worden opgegeven:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Deze JSON-bestand wordt verzonden als onderdeel van de ranking-aanvraag:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Hier `<appId>` is de naam van uw toepassing geregistreerd op de portal. U ontvangt een geordende reeks inhoud items, die u in uw toepassing kan weergeven. Een voorbeeld van een retourwaarde ziet eruit als:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

Het eerste deel van het retourtype bevat een lijst met geordende acties, die door de actie-id's opgegeven. Voor een artikel wordt het actie-ID van een URL. De algehele aanvraag heeft een unieke `<eventId>`, gemaakt door het systeem.

Later kunt u opgeven als u een klik op het eerste inhoudsitem deze gebeurtenis, die is waargenomen `<actionId3>`. U kunt een compensatie vervolgens rapporteren over dit `<eventId>` met aangepaste besluit Service via de API van derden, met een andere aanvragen, zoals:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Tot slot moet u de actie ingesteld API, die resulteert in de lijst met artikelen (acties) moet worden beschouwd door aangepaste besluit Service opgeven. Deze API als de RSS-feed implementeren als volgt te werk:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Hier kunt elke site op het hoogste `<item>` element een artikel wordt beschreven. De `<link>` is verplicht en als een actie-ID wordt gebruikt door aangepaste besluit Service. Geef `<date>` (in een standaard RSS-indeling) als u meer dan 15 artikelen hebt. De meest recente 15 artikelen worden gebruikt. De `<title>` is optioneel en wordt gebruikt voor het maken van tekst-gerelateerde functies voor het artikel.

## <a name="next-steps"></a>Volgende stappen

* Werken via een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een uitgebreidere.
* Raadpleeg de [API](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit voor opgegeven.