---
title: API aanroepen vanuit een browser - cognitieve Azure-Services | Microsoft Docs
description: Aan de slag met Azure aangepaste besluit Service naar een webpagina optimaliseren door het maken van API-aanroepen rechtstreeks vanuit een browser.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/09/2018
ms.author: slivkins,marcozo,alekh
ms.openlocfilehash: 10236c9d8f70d9b90a896464b4f86a847ee904c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345529"
---
# <a name="call-api-from-a-browser"></a>API aanroepen vanuit een browser

In dit artikel helpt u aanroepen naar de Azure aangepaste besluit Service API's direct vanuit een browser.

Zorg ervoor dat [uw toepassing registreren](custom-decision-service-get-started-register.md), eerste.

Aan de slag. Uw toepassing wordt gemodelleerd als een voorpagina naar diverse artikel's koppelingen hebben. De voorpagina gebruikmaakt van aangepaste besluit Service om op te geven de volgorde van de artikel-pagina's. Voeg de volgende code toe aan de HTML-kop van de voorpagina:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

De `data` argument bevat de volgorde van URL's moet worden gerenderd. Zie voor meer informatie de verwijzing [API](custom-decision-service-api-reference.md).

Roep de volgende code op de front-pagina voor het afhandelen van een gebruiker Klik op het bovenste artikel:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Hier `data` is het argument voor de `callback()` functie. Een voorbeeld van de implementatie vindt u in dit [zelfstudie](custom-decision-service-tutorial-news.md#use-the-apis).

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

Hier kunt elke site op het hoogste `<item>` element een artikel wordt beschreven. De `<link>` is verplicht en als een actie-ID wordt gebruikt door aangepaste besluit Service. Geef `<date>` (in een standaard RSS-indeling) als er meer dan 15 artikelen. De meest recente 15 artikelen worden gebruikt. De `<title>` is optioneel en wordt gebruikt voor het maken van tekst-gerelateerde functies voor het artikel.

## <a name="next-steps"></a>Volgende stappen

* Werken via een [zelfstudie](custom-decision-service-tutorial-news.md) voor een voorbeeld van een uitgebreidere.
* Raadpleeg de [API](custom-decision-service-api-reference.md) voor meer informatie over de functionaliteit voor opgegeven.