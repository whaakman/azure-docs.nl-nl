---
title: Wat is de Bing Video's zoeken-API?
titlesuffix: Azure Cognitive Services
description: Hier vindt u informatie over het zoeken naar video's op internet, met behulp van de Bing Video's zoeken-API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 06/26/2019
ms.author: scottwhi
ms.openlocfilehash: 64d6613a5bfb9b13500d2263d2f826d342972639
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67867849"
---
# <a name="what-is-the-bing-video-search-api"></a>Wat is de Bing Video's zoeken-API?

Met de Bing Video's zoeken-API kunt u eenvoudig mogelijkheden voor het zoeken van video's toevoegen aan uw services en toepassingen. Door zoekquery's van gebruikers te verzenden met de API kunt u relevante en hoogwaardige video's ophalen en weergeven die vergelijkbaar zijn met [Bing Video's](https://www.bing.com/video). Gebruik deze API voor zoekresultaten die alleen video's bevatten. De [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md) kan andere typen webinhoud retourneren, waaronder webpagina's, video's, nieuws en afbeeldingen.

## <a name="bing-video-search-api-features"></a>Functies van Bing Video's zoeken-API

| Functie                                                                                                                                                                                 | Description                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Zoektermen in realtime voorstellen](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Verbeter uw app-ervaring met de [Bing Automatische suggestie-API](../bing-autosuggest/get-suggested-search-terms.md) om voorgestelde zoektermen weer te geven wanneer deze worden getypt. |
| [Videoresultaten filteren en beperken](concepts/get-videos.md#filtering-videos)                      | Filter de video's die worden geretourneerd door queryparameters te bewerken.                                                                                                       |
| [Miniaturen bijsnijden en weergeven en het formaat hiervan wijzigen](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Bewerk miniatuurweergaven voor de video's die zijn geretourneerd door de Bing Video's zoeken-API en geef deze weer.                                                                                      |
| [Trending video's ophalen](trending-videos.md) | Zoek naar trending video's van over de hele wereld.                                                                                                          |
| [Inzicht verkrijgen over video](video-insights.md) | Pas een zoekopdracht voor trending video's van over de hele wereld aan.                                                                                                          |

## <a name="workflow"></a>Werkstroom

De Bing Video's zoeken-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de service gebruiken met de [REST API](csharp.md) of de [SDK](video-search-sdk-quickstart.md).

1. Maak een [Account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Verzend een aanvraag naar de API met een geldige zoekquery.
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.


## <a name="next-steps"></a>Volgende stappen

De [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) over de Bing Video's zoeken-API laat zien hoe u een zoekquery kunt aanpassen en op internet kunt zoeken naar video's.

Wanneer u klaar bent om de API aan te roepen, maakt u een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Gebruik de [quickstart](csharp.md) om snel uw eerste API-aanvraag te maken.

## <a name="see-also"></a>Zie ook

* De naslaghandleiding over de [Bing Video's zoeken-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen.

* In de [Bing-vereisten voor gebruik en weergave](./useanddisplayrequirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.