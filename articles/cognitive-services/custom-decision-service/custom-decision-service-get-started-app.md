---
title: API aanroepen vanuit een app-Custom Decision Service
titlesuffix: Azure Cognitive Services
description: De Custom Decision Service-Api's aanroepen vanuit een smartphone-app.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 08fbc1716d402c83bc2c33be82cba143c1737a55
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707258"
---
# <a name="call-api-from-an-app"></a>API aanroepen vanuit een app

Aanroepen naar de Azure Custom Decision Service-Api's vanuit een smartphone-app. In dit artikel wordt uitgelegd hoe u aan de slag kunt gaan met enkele basis opties.

Zorg ervoor dat u [uw toepassing](custom-decision-service-get-started-register.md)eerst registreert.

Er zijn twee API-aanroepen van uw smartphone-app naar Custom Decision Service: een aanroep van de classificatie-API voor het verkrijgen van een geclassificeerde lijst met inhoud en een aanroep naar de belonings-API om een beloning te rapporteren. Hier volgen de voorbeeld gesprekken in [krul](https://en.wikipedia.org/wiki/CURL).

Zie de referentie- [API](custom-decision-service-api-reference.md)voor meer informatie.

Begin met het aanroepen van de classificatie-API. Maak het bestand `<request.json>`, dat de actie set-id heeft. Deze ID is de naam van de bijbehorende RSS-of Atom-feed die u hebt ingevoerd in de portal:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

U kunt een groot aantal actie sets als volgt opgeven:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Dit JSON-bestand wordt vervolgens verzonden als onderdeel van de aanvraag voor de rang schikking:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Hier volgt de naam van uw toepassing die isgeregistreerdopdeportal.`<appId>` U ontvangt een geordende set inhouds items, die u in uw toepassing kunt weer geven. Een voor beeld van een resultaat ziet er als volgt uit:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

Het eerste deel van de retour bevat een lijst met geordende acties, opgegeven door hun actie-Id's. Voor een artikel is de actie-ID een URL. De algemene aanvraag heeft ook een uniek `<eventId>`, gemaakt door het systeem.

Later kunt u opgeven of u een klik op het eerste inhouds item van deze gebeurtenis `<actionId3>`hebt gevolgd. U kunt vervolgens met behulp van de `<eventId>` belonings-API een vergoeding op dit rapport geven om te Custom decision service, met een andere aanvraag, zoals:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Ten slotte moet u de Actions set API opgeven, waarmee de lijst met artikelen (acties) wordt geretourneerd die door Custom Decision Service worden overwogen. Implementeer deze API als een RSS-feed, zoals hier wordt weer gegeven:

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

Hier wordt voor elk element op `<item>` het hoogste niveau een artikel beschreven. De `<link>` is verplicht en wordt gebruikt als actie-id door Custom decision service. Geef `<date>` (in een standaard-RSS-indeling) op als u meer dan 15 artikelen hebt. De 15 meest recente artikelen worden gebruikt. De `<title>` is optioneel en wordt gebruikt voor het maken van tekst-gerelateerde functies voor het artikel.

## <a name="next-steps"></a>Volgende stappen

* Gebruik een [zelf studie](custom-decision-service-tutorial-news.md) voor een uitgebreidere voor beeld.
* Raadpleeg de verwijzings- [API](custom-decision-service-api-reference.md) voor meer informatie over de beschik bare functionaliteit.
