---
title: Wat is Bing Visual Search-API?
titleSuffix: Azure Cognitive Services
description: Bing Visual Search biedt details of inzichten over een afbeelding, zoals vergelijkbare afbeeldingen of winkelbronnen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 03/27/2019
ms.author: scottwhi
ms.openlocfilehash: 8bcb0372ebb60ac3a46cf06bf85322b288e153ba
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630208"
---
# <a name="what-is-the-bing-visual-search-api"></a>Wat is Bing Visual Search-API?

De Bing visuele zoekopdrachten-API retourneert insights voor een afbeelding. U kunt een installatiekopie uploaden of geef een URL naar een. Inzichten zijn visueel vergelijkbare afbeeldingen, bronnen voor winkelen, webpagina's, zoals de installatiekopie en meer. Inzichten die zijn geretourneerd door de Bing visuele zoekopdrachten-API zijn vergelijkbaar met die op Bing.com/images weergegeven.

Als u de [Bing afbeeldingen zoeken-API](../bing-image-search/overview.md), kunt u inzicht tokens uit de zoekresultaten van die API gebruiken voor de Bing visuele zoekopdrachten in plaats van het uploaden van een afbeelding.

> [!IMPORTANT]
> Als u afbeeldingsinzichten ophaalt met behulp van Bing Afbeeldingen zoeken-API, kunt u overwegen om over te schakelen naar Bing Visual Search-API, omdat deze uitgebreidere inzichten biedt.

## <a name="insights"></a>Inzichten

U kunt de volgende inzichten detecteren met behulp van Bing visuele zoekopdrachten:

| Inzicht                              | Description |
|--------------------------------------|-------------|
| Visueel vergelijkbare afbeeldingen              | Een lijst met afbeeldingen die visueel vergelijkbaar zijn met de ingevoerde afbeelding. |
| Visueel vergelijkbare producten            | Producten die visueel vergelijkbaar zijn met het weergegeven product.            |
| Winkelbronnen                     | Een lijst met plaatsen waar u het artikel kunt kopen dat wordt weergegeven op de ingevoerde afbeelding.            |
| Verwante zoekopdrachten                     | Een lijst met verwante zoekopdrachten die door anderen zijn uitgevoerd, of die zijn gebaseerd op de inhoud van de afbeelding.            |
| Webpagina's met de installatiekopie van het opnemen     | Webpagina's die de ingevoerde afbeelding bevatten.            |
| Recepten                              | Webpagina's met recepten voor het maken van het gerecht weergegeven in de invoer afbeelding bevatten.            |

Naast inzicht retourneert de Bing visuele zoekopdrachten tal van termen (dat wil zeggen, tags) worden afgeleid van de afbeelding. De tags kunnen gebruikers om te verkennen concepten gevonden in de afbeelding. Bijvoorbeeld, als de afbeelding van een beroemde atleet, één van de labels kan overeenkomen met de naam van de atleet, een andere label Sport kan worden. Of, als de afbeelding van een cirkel apple, de labels kunnen worden Apple cirkel, Pies en Desserts.

Resultaten van de Bing visuele zoekopdrachten bevatten ook omsluitende kaders voor regio's in de installatiekopie van het van belang. Bijvoorbeeld, als de afbeelding verschillende beroemdheden bevat, bevatten de resultaten vakken begrenzingsvak voor elk van de herkende beroemdheden. Of, als Bing een product of de kledingfabrikanten in de afbeelding herkent, het resultaat een begrenzingsvak voor het herkende item bevatten.

## <a name="workflow"></a>Werkstroom

Bing Visual Search-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de REST-API of de SDK gebruiken voor de service.

1. Maak een [Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) voor toegang tot de Bing zoeken-API's. Als u geen Azure-abonnement hebt, kunt u [gratis een account maken](https://azure.microsoft.com/free/). U krijgt de abonnementssleutel van uw uit de [Azure-portal](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account#access-your-resource) nadat u uw account hebt of [Azure-website](https://azure.microsoft.com/try/cognitive-services/my-apis) na het activeren van een gratis proefversie.
2. Een aanvraag verzenden naar de API met een geldige zoekquery.
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.

## <a name="next-steps"></a>Volgende stappen

Probeer eerst de Bing visuele zoekopdrachten-API [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/).
De demo toont hoe u snel een zoekquery aanpassen en Doorzoek Internet op afbeeldingen.

Zie deze quickstarts om snel aan de slag te gaan met uw eerste aanvraag: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [Node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).

## <a name="see-also"></a>Zie ook

* De [installatiekopieën - visuele zoekopdrachten](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) verwijzing beschrijft definities en informatie over de eindpunten aanvraagheaders, antwoorden en queryparameters die u gebruiken voor aanvragen op basis van een installatiekopie van kunt de zoekresultaten.

* De [Bing zoeken-API gebruiken en weergavevereisten](../bing-web-search/use-display-requirements.md) aanvaardbaar gebruik van de inhoud en informatie die is opgedaan met de Bing zoeken-API's opgeven.
