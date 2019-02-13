---
title: Wat is de Bing Entiteiten zoeken-API?
titlesuffix: Azure Cognitive Services
description: Gebruik de Bing Entiteiten zoeken-API om te zoeken naar entiteiten en locaties en om deze te extraheren uit zoekquery's.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: overview
ms.date: 02/01/2019
ms.author: scottwhi
ms.openlocfilehash: 9190c87b7afff66162e25fb3cd08bfeac76aff74
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757773"
---
# <a name="what-is-bing-entity-search-api"></a>Wat is de Bing Entiteiten zoeken-API?

De Bing Entiteiten zoeken-API verzendt een zoekquery naar Bing en haalt resultaten op die zowel entiteiten als plaatsen bevatten. Plaatsresultaten kunnen restaurants, hotels of andere lokale bedrijven zijn. Bing retourneert plaatsen als in de query de naam van een lokaal bedrijf is opgegeven, of als er om een type bedrijf wordt gevraagd (bijvoorbeeld restaurants in de buurt). Bing retourneert entiteiten als in de query wordt verwezen naar bekende personen, plaatsen (toeristische attracties, provincies, landen, etc.) of dingen.

|Functie  |Beschrijving  |
|---------|---------|
|[Zoeksuggesties in realtime](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Geef zoeksuggesties op die kunnen worden weergegeven als een vervolgkeuzelijst wanneer gebruikers beginnen te typen.       | 
| [Ondubbelzinnig maken van entiteiten](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Meerdere entiteiten opvragen voor query's met meerdere mogelijke betekenissen. |
| [Locaties zoeken](concepts/search-for-entities.md#find-places) | Zoeken naar lokale bedrijven en entiteiten en de bijbehorende gegevens retourneren.  |

## <a name="workflow"></a>Werkstroom

De Bing Entiteiten zoeken-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de service gebruiken met de REST API of de SDK.

1. Maak een [Account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Verzend een aanvraag naar de API met een geldige zoekquery.
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

* Probeer de [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) van de Bing Entiteiten zoeken-API. 
* Volg een [quickstart](quickstarts/csharp.md) om snel aan de slag te gaan met uw eerste aanvraag.
* De [naslaghandleiding over de Bing Entiteiten zoeken-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference).
* In de [Bing-vereisten voor gebruik en weergave](./use-display-requirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.
