---
title: Wat is de Bing Afbeeldingen zoeken-API?
titleSuffix: Azure Cognitive Services
description: "Met de Bing Afbeeldingen zoeken-API kunt u de cognitieve zoekfunctionaliteit voor afbeeldingen van Bing in uw toepassing gebruiken. Door gebruikerszoekquery's te verzenden met de API kunt u relevante en hoogwaardige afbeeldingen ophalen en weergeven die vergelijkbaar zijn met Bing: afbeeldingen."
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 4ae9c0c1fb6cd341e812f9e8b35eaff789637561
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55765643"
---
# <a name="what-is-the-bing-image-search-api"></a>Wat is de Bing Afbeeldingen zoeken-API?

Met de Bing Afbeeldingen zoeken-API kunt u de cognitieve zoekfunctionaliteit voor afbeeldingen van Bing in uw toepassing gebruiken. Door gebruikerszoekquery's te verzenden met de API kunt u relevante en hoogwaardige afbeeldingen ophalen en weergeven die vergelijkbaar zijn met [Bing - Afbeeldingen](https://www.bing.com/images).

Let erop dat met de Bing Afbeeldingen zoeken-API de resultaten alleen afbeeldingen bevatten. Gebruik de [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md), [Video's zoeken-API](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search) en [Nieuws zoeken-API](https://review.docs.microsoft.com/azure/cognitive-services/bing-news-search) voor andere typen webinhoud.

## <a name="bing-image-search-features"></a>Functies van Bing Afbeeldingen zoeken

Hoewel met Bing Afbeeldingen zoeken vooral relevante afbeeldingen worden gezocht en geretourneerd op basis van een zoekquery, bevat de service ook diverse aanvullende functies waarmee intelligent en gericht afbeeldingen worden opgehaald op het web.


| Functie                                                                                                                                                                                 | Beschrijving                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Zoektermen in realtime voorstellen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Verbeter uw app-ervaring met de [Bing Automatische suggestie-API](../bing-autosuggest/get-suggested-search-terms.md) om voorgestelde zoektermen weer te geven wanneer deze worden getypt. |
| [Afbeeldingsresultaten filteren en beperken](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images)                       | Filter de afbeeldingen die via Bing worden geretourneerd door queryparameters te bewerken.                                                                                                       |
| [Miniaturen bijsnijden en weergeven en het formaat hiervan wijzigen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/resize-and-crop-thumbnails)                                                | Bewerk miniatuurweergaven voor de afbeeldingen die zijn geretourneerd door Bing Afbeeldingen zoeken en geef deze weer.                                                                                      |
| [Gebruikerszoekquery's draaien en uitvouwen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries)               | Breid uw zoekmogelijkheden uit door voorgestelde zoektermen door Bing op te nemen en weer te geven in query's.                                                                    |
| [Trending afbeeldingen ophalen](https://review.docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)                                                                     | Pas een zoekopdracht voor trending afbeeldingen van over de hele wereld aan.                                                                                                          |

## <a name="workflow"></a>Werkstroom

De Bing Afbeeldingen zoeken-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de service gebruiken met de [REST API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) of de [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

1. Maak een [Account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Verzend een aanvraag naar de API met een geldige [zoekquery](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries).
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst de [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) van de Bing Afbeeldingen zoeken-API uit.
In deze demo ziet u hoe u snel een zoekquery kunt aanpassen en internet kunt doorzoeken op afbeeldingen.

Wanneer u klaar bent om de API aan te roepen, maakt u een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Als u snel aan de slag wilt met uw eerste API-aanvraag, kunt u het volgende leren:

* [Zoekquery's naar Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) verzenden met de REST API of
* De afbeeldingen [aanvragen en filteren](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) die door Bing worden geretourneerd met behulp van de SDK.

## <a name="see-also"></a>Zie ook

* De referentiesectie [Bing Afbeeldingen zoeken-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) bevat definities en informatie over de eindpunten, headers, API-antwoorden en queryparameters waarmee u afbeeldingszoekresultaten kunt aanvragen.

* In de [Bing-vereisten voor gebruik en weergave](./useanddisplayrequirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.

* In het onderwerp [Afbeeldingen ophalen van internet met de Bing Afbeeldingen zoeken-API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) wordt beschreven hoe u afbeeldingen kunt zoeken en ophalen van internet.

* In het onderwerp [Zoekquery's verzenden en ermee werken](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) wordt beschreven hoe u zoekquery's kunt maken, aanpassen en draaien.
