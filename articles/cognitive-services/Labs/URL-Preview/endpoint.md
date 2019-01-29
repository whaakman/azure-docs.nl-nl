---
title: Project-URL-voorbeeld-eindpunt
titlesuffix: Azure Cognitive Services
description: Samenvatting van het eindpunt van de URL-voorbeeld.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 8ed28ef356a4e884fca61b950c59506762fb8bdc
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099659"
---
# <a name="project-url-preview-endpoint"></a>Project-URL-voorbeeld-eindpunt

De API van de voorbeeld-URL bevat één eindpunt.

## <a name="endpoint"></a>Eindpunt
Als u een URL-voorbeeld, een aanvraag te verzenden naar het volgende eindpunt. De URL-parameters en headers gebruiken voor andere specificaties.

GET:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

```

### <a name="query-parameters"></a>Queryparameters
|Name|Value|Type|Vereist|  
|----------|-----------|----------|--------------|  
|q|URL om een voorbeeld van|Reeks |Ja|
|veilig zoeken|Ongeldige inhoud voor volwassenen, of illegale inhoud, is geblokkeerd met foutcode 400, en de *isFamilyFriendly* vlag wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen, ziet hieronder u het gedrag. Statuscode 200 wordt geretourneerd en de *isFamilyFriendly* vlag is ingesteld op false.<ul><li>safeSearch=strict: Titel, beschrijving, URL en afbeelding wordt niet geretourneerd.</li><li>veilig zoeken = gemiddeld; Titel, URL en beschrijving, maar niet de beschrijvende afbeelding ophalen</li><li>veilig zoeken = uit; Krijg alle antwoord objecten/elementen: titel, URL, beschrijving en afbeelding.</li></ul> |Reeks|Niet vereist. </br> Standaard ingesteld op safeSearch = strikte.| 

## <a name="response-object"></a>Responsobject

Het antwoord bevat HTTP-headers en webpagina-object met de kenmerken, zoals wordt weergegeven in het volgende voorbeeld: `name`, `url`, `description`, `isFamilyFriendly`, en `primaryImageOfPage`.

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
