---
title: Voor beeld-eind punt project-URL
titlesuffix: Azure Cognitive Services
description: Samen vatting van het URL-preview-eind punt.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: 3ef5ebd4ec88deac8c49430f36956d3711c8c535
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706939"
---
# <a name="project-url-preview-endpoint"></a>Voor beeld-eind punt project-URL

De URL-preview-API bevat één eind punt.

## <a name="endpoint"></a>Eindpunt
Als u een URL-voor beeld wilt ophalen, verzendt u een aanvraag naar het volgende eind punt. Gebruik de para meters headers en URL voor andere specificaties.

GET:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```

### <a name="query-parameters"></a>Queryparameters
|Name|Waarde|type|Vereist|  
|----------|-----------|----------|--------------|  
|q|URL voor preview|Reeks |Ja|
|safeSearch|Illegale inhoud voor volwassenen, of illegale inhoud, is geblokkeerd met fout code 400 en de vlag *isFamilyFriendly* wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen is hieronder het gedrag. De status code retourneert 200 en de vlag *isFamilyFriendly* is ingesteld op false.<ul><li>safeSearch = strict: Titel, beschrijving, URL en afbeelding worden niet geretourneerd.</li><li>safeSearch = gemiddeld; Titel, URL en beschrijving ophalen, maar niet de beschrijvende afbeelding.</li><li>safeSearch = uit; Alle antwoord objecten/elementen – titel, URL, beschrijving en afbeelding ophalen.</li></ul> |Tekenreeks|Niet vereist. </br> De standaard waarde is safeSearch = strict.| 

## <a name="response-object"></a>Responsobject

Het antwoord bevat HTTP-headers en een webpagina-object met kenmerken, zoals wordt weer `name`gegeven in `description`het `isFamilyFriendly`volgende voor `primaryImageOfPage`beeld:, `url`,, en.

```
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

```

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
