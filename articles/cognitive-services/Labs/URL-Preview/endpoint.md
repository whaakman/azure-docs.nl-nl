---
title: URL-Preview-eindpunt - cognitieve Services van Microsoft Project | Microsoft Docs
description: Samenvatting van het URL-Preview-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: ddd53aa49db01d7a6db397eb285d0854edc59388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345279"
---
# <a name="project-url-preview-endpoint"></a>Project URL Preview-eindpunt

De URL Preview-API bevat één eindpunt.

## <a name="endpoint"></a>Eindpunt
Als u een voorbeeld-URL, een aanvraag te verzenden naar het volgende eindpunt. De kop- en URL-parameters gebruiken voor andere specificaties.

GET:
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

````

### <a name="query-parameters"></a>Queryparameters
|Naam|Waarde|Type|Vereist|  
|----------|-----------|----------|--------------|  
|q|URL voor de preview|Reeks |Ja|
|veilig zoeken|Ongeldige inhoud voor volwassenen of illegale inhoud is geblokkeerd met foutcode 400, en de *isFamilyFriendly* vlag wordt niet geretourneerd. <p>Voor juridische inhoud voor volwassenen ziet hieronder u het gedrag. Statuscode 200, retourneert en de *isFamilyFriendly* vlag is ingesteld op false.<ul><li>veilig zoeken = strict: titel, beschrijving, URL en de installatiekopie, worden niet geretourneerd.</li><li>veilig zoeken = gemiddeld; Titel, URL en beschrijving, maar niet de beschrijvende afbeelding ophalen.</li><li>veilig zoeken = off; Haal alle antwoord objecten/elementen – titel, de URL, de beschrijving en de installatiekopie.</li></ul> |Reeks|Niet vereist. </br> Standaard veilig zoeken = strict.| 

## <a name="response-object"></a>Response-object

Het antwoord bevat HTTP-headers en webpagina-object gevonden met kenmerken, zoals wordt weergegeven in het volgende voorbeeld: `name`, `url`, `description`, `isFamilyFriendly`, en `primaryImageOfPage`.

````
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

````

## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Knooppunt Quick Start](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)
