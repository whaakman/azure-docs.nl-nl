---
title: Wat is Bing Visual Search?
titleSuffix: Azure Cognitive Services
description: Bing Visual Search biedt details of inzichten over een afbeelding, zoals vergelijkbare afbeeldingen of winkelbronnen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: aab17f8a0213c213920ebdfc2b2fe31248657a91
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742844"
---
# <a name="what-is-the-bing-visual-search-api"></a>Wat is Bing Visual Search-API?

Bing Visual Search-API biedt afbeeldingsdetails die vergelijkbaar zijn met die op Bing.com/images. Als u een afbeelding uploadt of een URL naar een afbeelding opgeeft, kan met deze API een verscheidenheid aan details over de afbeelding worden geïdentificeerd, waaronder visueel vergelijkbare afbeeldingen, winkelbronnen, webpagina’s waarop de afbeelding wordt weergegeven, en meer. Als u de [ Bing Afbeeldingen zoeken-API](../bing-image-search/overview.md) gebruikt, kunt u inzichttokens gebruiken die zijn bijgevoegd bij de zoekresultaten van de API, in plaats van dat u een afbeelding uploadt.

## <a name="insights"></a>Inzichten

U kunt de volgende inzichten ontdekken met Visual Search:

| Inzicht                              | Beschrijving |
|--------------------------------------|-------------|
| Visueel vergelijkbare afbeeldingen              | Een lijst met afbeeldingen die visueel vergelijkbaar zijn met de ingevoerde afbeelding. |
| Visueel vergelijkbare producten            | Producten die visueel vergelijkbaar zijn met het weergegeven product.            |
| Winkelbronnen                     | Een lijst met plaatsen waar u het artikel kunt kopen dat wordt weergegeven op de ingevoerde afbeelding.            |
| Verwante zoekopdrachten                     | Een lijst met verwante zoekopdrachten die door anderen zijn uitgevoerd, of die zijn gebaseerd op de inhoud van de afbeelding.            |
| Webpagina's met de afbeelding     | Webpagina's die de ingevoerde afbeelding bevatten.            |
| Recepten                              | Een lijst met webpagina's met recepten voor het maken van het gerecht dat wordt weergegeven op de ingevoerde afbeelding            |

Naast deze inzichten retourneert Visual Search ook een diverse reeks termen (tags) die zijn afgeleid van de ingevoerde afbeelding. Aan de hand van deze tags kunnen gebruikers in de afbeelding gevonden concepten verkennen. Als de ingevoerde afbeelding bijvoorbeeld van een beroemde atleet is, kan een van de tags de naam van de atleet zijn, een andere tag kan Sport zijn. Of als de ingevoerde afbeelding een appeltaart is, kunnen de tags Appeltaart, Taarten, Toetjes zijn, zodat gebruikers gerelateerde concepten kunnen verkennen.

De resultaten van Visual Search bevatten ook begrenzingsvakken voor interessegebieden in de afbeelding. Als de afbeelding bijvoorbeeld meerdere beroemdheden bevat, kunnen de resultaten begrenzingsvakken bevatten voor elk van de herkende beroemdheden in de afbeelding. Of, als Bing een product of kledingstuk herkent in de afbeelding, kan het resultaat een begrenzingsvak bevatten voor het herkende product of kledingstuk.

> [!IMPORTANT]
> Als u afbeeldingsinzichten ophaalt met behulp van Bing Afbeeldingen zoeken-API, kunt u overwegen om over te schakelen naar Bing Visual Search-API, omdat deze uitgebreidere inzichten biedt.

## <a name="workflow"></a>Werkstroom

Bing Visual Search-API is een RESTful-webservice die eenvoudig kan worden aangeroepen vanuit elke programmeertaal waarmee HTTP-aanvragen kunnen worden gedaan en JSON kan worden geparseerd. U kunt de service gebruiken met de REST API of de SDK.

1. Maak een account voor Cognitive Services-API met toegang tot de Bing Zoeken-API's. Als u geen Azure-abonnement hebt, kunt u gratis een account maken.
2. Verzend een aanvraag naar de API met een geldige zoekquery.
3. Verwerk de API-reactie door het geretourneerde JSON-bericht te parseren.


## <a name="next-steps"></a>Volgende stappen

Probeer eerst de [interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/) van de Bing Afbeeldingen zoeken-API uit.
In deze demo ziet u hoe u snel een zoekquery kunt aanpassen en internet kunt doorzoeken op afbeeldingen.

Wanneer u klaar bent om de API aan te roepen, maakt u een [account voor Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Als u geen Azure-abonnement hebt, kunt u gratis [een account maken](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Zie deze quickstarts om snel aan de slag te gaan met uw eerste aanvraag: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [Node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).


## <a name="see-also"></a>Zie ook

* De documentatie voor [Bing Visual Search](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) bevat definities en informatie over de eindpunten, headers, API-antwoorden en queryparameters waarmee u afbeeldingszoekresultaten kunt opvragen.

* In de [Bing-vereisten voor gebruik en weergave](./use-and-display-requirements.md) staan het acceptabele gebruik van de inhoud en informatie die is verkregen via de Bing Zoeken-API's.
