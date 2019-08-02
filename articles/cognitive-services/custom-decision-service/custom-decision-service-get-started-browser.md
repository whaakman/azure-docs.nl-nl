---
title: API aanroepen vanuit een browser-Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Een webpagina optimaliseren door API-aanroepen rechtstreeks vanuit een browser naar het Custom Decision Service te maken.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 28ad4681242765bf2da9b1f13dc828e23cce1794
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707271"
---
# <a name="call-api-from-a-browser"></a>API aanroepen vanuit een browser

Dit artikel helpt u bij het rechtstreeks aanroepen van de Azure Custom Decision Service Api's vanuit een browser.

Zorg ervoor dat u [uw toepassing](custom-decision-service-get-started-register.md)eerst registreert.

Aan de slag. Uw toepassing is gemodelleerd als een front page die aan verschillende artikel pagina's is gekoppeld. Op de front page wordt Custom Decision Service gebruikt om de volg orde van de artikel pagina's op te geven. Voeg de volgende code toe aan de HTML-kop van de front page:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

Het `data` argument bevat de rang schikking van url's die moeten worden weer gegeven. Zie de referentie- [API](custom-decision-service-api-reference.md)voor meer informatie.

Als u een gebruiker wilt afhandelen, klikt u op het bovenste artikel en roept u de volgende code aan op de front page:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Hier is het argument voor de `callback()` functie. `data` In deze [zelf studie](custom-decision-service-tutorial-news.md#use-the-apis)vindt u een voor beeld van de implementatie.

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
