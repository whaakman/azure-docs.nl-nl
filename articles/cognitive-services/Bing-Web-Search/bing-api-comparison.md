---
title: Wat zijn de Bing zoeken-API's?
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel voor meer informatie over de Bing zoeken-API's, en hoe u cognitive zoekacties in uw apps en services op internet kunt inschakelen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 5a883fcb3533374afbbf946281b6a4a1e9a2912e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57903110"
---
# <a name="what-are-the-bing-search-apis"></a>Wat zijn de Bing zoeken-API's?

De Bing zoeken-API's kunt u bouwen van verbonden web apps en services die webpagina's, afbeeldingen, nieuws, locaties en meer zonder advertenties. Zoeken door verzoeken te sturen met behulp van de Bing zoeken-REST API's of de SDK's, kunt u relevante informatie en inhoud krijgen voor zoeken op het web. Gebruik dit artikel voor meer informatie over de verschillende Bing zoeken-API's en hoe u cognitieve zoekopdrachten kunt integreren in uw toepassingen en services. Limieten voor prijzen en snelheid kunnen variëren tussen API's.

## <a name="the-bing-web-search-api"></a>De Bing webzoekopdrachten-API

De [Bing webzoekopdrachten-API](../Bing-Web-Search/index.yml) retourneert webpagina's, afbeeldingen, video, nieuws en meer. U kunt de zoekquery's verzonden naar deze API wilt opnemen of uitsluiten van bepaalde typen inhoud filteren.

Overweeg het gebruik van de Bing webzoekopdrachten-API in toepassingen die mogelijk nodig hebt om te zoeken naar alle typen van de relevante webinhoud. Als uw toepassing zoekt naar een specifiek type online inhoud, kunt u overwegen een van de search API's die hieronder:

## <a name="content-specific-bing-search-apis"></a>Specifieke inhoud Bing zoeken-API 's

De volgende Bing zoeken-API's retourneren van specifieke inhoud op het web, zoals afbeeldingen, nieuws, lokale bedrijven en video's.

| API voor Bing | Description |
| -- | -- |
| [Entiteiten zoeken](../Bing-Entities-Search/index.yml) | Bing Entity Search API retourneert entiteiten, personen, plaatsen of dingen met zoekresultaten. Afhankelijk van de query retourneert de API een of meer entiteiten die voldoen aan de zoekquery. De zoekopdracht kunt opnemen opmerkelijk personen, lokale bedrijven, monumenten, doelen en meer. |
| [Afbeeldingen zoeken](../Bing-Image-Search/index.yml) | De Bing afbeeldingen zoeken-API kunt u zoeken naar en zoeken naar statische- en animatiefilms afbeeldingen van hoge kwaliteit die vergelijkbaar is met [Bing.com/images](https://www.Bing.com/images). U kunt zoekopdrachten als u wilt opnemen of uitsluiten van installatiekopieën door het kenmerk, met inbegrip van de grootte, kleur, licentie en webdocumenten verfijnen. U kunt ook zoeken naar trending afbeeldingen, uploaden van afbeeldingen om inzicht te verkrijgen over deze en weergeven van miniatuurweergaven. |
| [Nieuws zoeken](../Bing-News-Search/index.yml) | De Bing nieuws zoeken-API kunt u vinden nieuws die vergelijkbaar is met [Bing.com/news](https://www.Bing.com/news). De API retourneert nieuwsartikelen uit meerdere bronnen of specifieke domeinen. U kunt zoeken in verschillende categorieën naar artikelen, topverhalen en koppen ophalen trends. |
| [Video's zoeken](../Bing-Video-Search/index.yml) | De Bing video's zoeken-API kunt u video's zoeken op Internet. Trending video's en gerelateerde inhoud miniatuurweergaven ophalen. |
| [Visuele zoekopdrachten](../Bing-visual-search/index.yml) | Upload een afbeelding of gebruik een URL voor inzichtelijke informatie over het, zoals visueel vergelijkbare producten, afbeeldingen en verwante zoekopdrachten. |
 [Lokale bedrijven zoeken](../bing-local-business-search/index.yml) | De Bing lokale bedrijven zoeken-API kunt uw toepassingen en neem contact op met de locatie-informatie over lokale bedrijven op basis van zoekquery's. |

## <a name="the-bing-custom-search-api"></a>De Bing Custom Search-API

Het maken van een exemplaar voor aangepast zoeken met de [Bing Custom Search](../Bing-Custom-Search/index.yml) API kunt u maken van een zoekervaring altijd alleen gericht op inhoud en -onderwerpen u geïnteresseerd bent. Nadat u de domeinen, websites en specifieke webpagina's met Bing zoeken-wordt opgegeven, wordt Bing Custom Search de resultaten op die specifieke inhoud aanpassen. U kunt opnemen de Bing aangepaste automatische suggesties, afbeelding, en ervaring van de Video zoeken-API's om aan te passen uw zoekopdracht.

## <a name="additional-bing-search-apis"></a>Aanvullende Bing zoeken-API 's

De volgende Bing zoeken-API's kunt u uw zoekervaring te verbeteren door ze te combineren met andere Bing zoeken-API's.

| API | Description |
| -- | -- |
| [Bing Automatische suggesties](../Bing-Autosuggest/index.yml) | Verbeter de zoekfunctie van uw toepassing met de Automatische suggestie-API door de voorgestelde zoekopdrachten in realtime worden geretourneerd.  |
| [Bing statistieken](bing-web-stats.md) | Bing statistieken biedt analytics voor de Bing zoeken-API maakt gebruik van uw toepassing. Enkele van de beschikbare analytics zijn aanroepvolume, top queryreeksen en geografische verdeling. |

## <a name="next-steps"></a>Volgende stappen

* Bing zoeken-API [prijsinformatie](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* In de [Bing-vereisten voor gebruik en weergave](./use-display-requirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.
