---
title: API aanroepen vanuit een browser - Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Klik hier voor meer informatie over het optimaliseren van een webpagina rechtstreeks vanuit een browser door API-aanroepen naar de Custom Decision Service.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.openlocfilehash: 2b356e2f0fe9235d49dffa7417cd3894059f9caf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867559"
---
# <a name="call-api-from-a-browser"></a>API aanroepen vanuit een browser

Dit artikel helpt u aanroepen naar de Azure Custom Decision Service API's rechtstreeks vanuit een browser.

Zorg ervoor dat u [uw toepassing registreren](custom-decision-service-get-started-register.md), eerste.

Aan de slag. Uw toepassing is gemodelleerd als bestaande uit een front-pagina die is gekoppeld aan meerdere pagina's het artikel. De front-pagina wordt Custom Decision Service om op te geven de volgorde van de artikelpagina's die worden gebruikt. Voeg de volgende code in de HTML-kop van de front-pagina:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

De `data` argument bevat de volgorde van URL's moet worden gerenderd. Voor meer informatie, Zie de verwijzing [API](custom-decision-service-api-reference.md).

Bellen met de volgende code op de front-pagina voor het afhandelen van een gebruiker-Klik op de best gewaardeerd artikel:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Hier `data` is het argument voor de `callback()` functie. Een voorbeeld van de implementatie vindt u in deze [zelfstudie](custom-decision-service-tutorial-news.md#use-the-apis).

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

Hier, elk op het hoogste niveau `<item>` element een artikel wordt beschreven. De `<link>` is verplicht en als een actie-ID wordt gebruikt door Custom Decision Service. Geef `<date>` (in een standaard RSS-indeling) hebt u meer dan 15 artikelen. De meest recente 15 artikelen worden gebruikt. De `<title>` is optioneel en wordt gebruikt voor het maken van tekst-gerelateerde functies voor het artikel.

## <a name="next-steps"></a>Volgende stappen

* Doorloop een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een meer diepgaande.
* Raadpleeg de [API](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit van de opgegeven.
