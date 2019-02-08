---
title: API aanroepen vanuit een app - Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Hoe de Custom Decision Service API's aanroepen vanuit een smartphone-app.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: slivkins
ms.openlocfilehash: 0e5c99aae61fb927ea7f101bab74d661a747f88b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870007"
---
# <a name="call-api-from-an-app"></a>API aanroepen vanuit een app

Aanroepen naar de Azure Custom Decision Service API's vanuit een smartphone-app. In dit artikel wordt uitgelegd hoe u aan de slag met enkele eenvoudige opties.

Zorg ervoor dat u [uw toepassing registreren](custom-decision-service-get-started-register.md), eerste.

Er zijn twee API-aanroepen die u in uw smartphone-app in Custom Decision Service aanbrengt: een aanroep naar de Trefwoordenrangschikking-API om een gerangschikte lijst uw inhoud en een aanroep naar de API van derden voor het rapporteren van een beloning te verkrijgen. Hier volgen de voorbeeld-aanroepen in [cURL](https://en.wikipedia.org/wiki/CURL).

Voor meer informatie, Zie de verwijzing [API](custom-decision-service-api-reference.md).

Beginnen met de aanroep naar de API positie. Maak het bestand `<request.json>`, die is de actie ingesteld ID. Deze ID is de naam van de bijbehorende RSS of Atom-feed die u hebt ingevoerd in de portal:

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

Deze JSON-bestand wordt verzonden als onderdeel van de rangschikking van aanvraag:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Hier `<appId>` is de naam van uw toepassing geregistreerd op de portal. U ontvangt een geordende reeks inhoud items, die u in uw toepassing kan weergeven. Een voorbeeld van een afzender ziet eruit zoals:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

Het eerste deel van het rendement heeft een lijst met geordende acties, met de actie-id's opgegeven. Voor een artikel is de actie-ID van een URL. De totale aanvraag heeft een unieke `<eventId>`, gemaakt door het systeem.

Later, u kunt opgeven als u een klik op het eerste inhoudsitem dat deze gebeurtenis, die is waargenomen `<actionId3>`. U kunt vervolgens een beloning rapporteren over dit `<eventId>` op Custom Decision Service via de API van derden, met een andere vragen zoals:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Tot slot moet u de actie ingesteld API, die resulteert in de lijst met artikelen (acties) worden beschouwd door Custom Decision Service opgeven. Implementeer deze API als een RSS-feed, zoals hier wordt weergegeven:

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

Hier, elk op het hoogste niveau `<item>` element een artikel wordt beschreven. De `<link>` is verplicht en als een actie-ID wordt gebruikt door Custom Decision Service. Geef `<date>` (in een standaardindeling van RSS) als u meer dan 15 artikelen hebt. De meest recente 15 artikelen worden gebruikt. De `<title>` is optioneel en wordt gebruikt voor het maken van tekst-gerelateerde functies voor het artikel.

## <a name="next-steps"></a>Volgende stappen

* Doorloop een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een meer diepgaande.
* Raadpleeg de [API](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit van de opgegeven.
