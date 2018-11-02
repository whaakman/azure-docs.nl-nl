---
title: Wat is de Bing lokale bedrijven zoeken-API? | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: De Bing lokale bedrijven zoeken-API is een RESTful-service waarmee u uw toepassingen om informatie over lokale locaties en bedrijven op basis van zoekquery's te vinden.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: f6299a8241b4ce43dc9276070f06ae4cc6566d43
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748673"
---
# <a name="what-is-bing-local-business-search"></a>Wat is er lokale bedrijven zoeken in Bing?
De Bing lokale bedrijven zoeken-API is een RESTful-service waarmee u uw toepassingen om informatie over lokale bedrijven op basis van zoekquery's te vinden. Bijvoorbeeld, `q=<business-name> in Redmond, Washington`, of `q=Italian restaurants near me`. 

## <a name="features"></a>Functies
| Functie | Beschrijving |  
| -- | -- | 
| [Lokale bedrijven en locaties zoeken](quickstarts/local-quickstart.md) | De Bing lokale bedrijven zoeken-API krijgt gelokaliseerde resultaten van een query. Resultaten opnemen van een URL voor de website van het bedrijf en weer te geven tekst, telefoonnummer en geografische locatie, met inbegrip van: GPS-coördinaten, plaats, adres |  
| [Lokale resultaten met geografische grenzen filteren](specify-geographic-search.md) | Coördinaten als zoekparameters om te beperken tot een specifiek geografisch gebied, opgegeven door een circulaire gebied of een vierkant selectiekader toevoegen. | 
| [Lokale zakelijke resultaten filteren op categorie](local-categories.md) | Zoeken naar lokale bedrijfsresultaten te verbeteren door de categorie. Deze optie maakt gebruik van reverse IP-locatie of GPS-coördinaten van de oproepende functie om terug te keren gelokaliseerde resultaten in verschillende categorieën van bedrijf.|

## <a name="workflow"></a>Werkstroom
De Bing lokale bedrijven zoeken-API aanroepen vanuit elke programmeertaal die HTTP-aanvragen te parseren van JSON-antwoorden. Deze service is toegankelijk via de REST-API.
 
1. Maak een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met toegang tot de Bing zoeken-API's. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) maken.   
2. URL-codering uw zoektermen voor de `q=""` queryparameter. Bijvoorbeeld, `q=nearby+restaurant` of `q=nearby%20restaurant`. Stel paginering, indien nodig. 
3. Verzendt een [aanvraag naar de Bing API voor zoeken van lokale bedrijven](quickstarts/local-quickstart.md) 
4. Parseren van het JSON-antwoord 

> [!NOTE]
> Lokale bedrijven zoeken ondersteunt momenteel alleen de `en-US` markt. 
> [!NOTE]
> Lokale bedrijven zoeken biedt op dit moment geen ondersteuning voor automatische suggesties. 

## <a name="next-steps"></a>Volgende stappen
- [Query's en antwoorden](local-search-query-response.md)
- [Snelstartgids voor lokale bedrijven zoeken](quickstarts/local-quickstart.md)
- [Lokale bedrijven zoeken-API-verwijzing](local-search-reference.md)
- [Vereisten voor gebruik en weergave](use-display-requirements.md)