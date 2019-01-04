---
title: Zoeken naar een aangepaste weergave - Bing Custom Search
titlesuffix: Azure Cognitive Services
description: Hierin wordt beschreven hoe u kunt een aangepaste weergave van het web zoeken.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 77a1756aba0d8473051cdf335f33ed9ca5a8fb24
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53558325"
---
# <a name="call-your-bing-custom-search-instance-from-the-portal"></a>Uw Bing Custom Search-instantie aanroepen vanuit de Portal

Nadat u uw aangepaste zoekervaring hebt geconfigureerd, kunt u deze uit testen binnen de Bing Custom Search [portal](https://customsearch.ai). 

![een schermafbeelding van de portal van bing aangepaste zoekopdrachten](media/portal-search-screen.png)
## <a name="create-a-search-query"></a>Maken van een zoekquery 

Nadat u bent aangemeld bij de Bing Custom Search [portal](https://customsearch.ai), selecteer uw exemplaar van de zoekopdracht en klikt u op de **productie** tabblad. Onder **eindpunten**, selecteert u een API-eindpunt (bijvoorbeeld Web-API). Uw abonnement bepaalt welke eindpunten worden weergegeven.

Voer de parameterwaarden voor het eindpunt voor het maken van een zoekquery. Houd er rekening mee dat de parameters weergegeven in de portal kunnen worden gewijzigd, afhankelijk van het eindpunt dat u kiest. Zie de[Custom Search-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) voor meer informatie. 

Er zijn enkele belangrijke parameters hieronder:


|Parameter  |Description  |
|---------|---------|
|Query’s uitvoeren     | De zoekterm die moet worden gezocht. Alleen beschikbaar voor Web, installatiekopie, Video en Automatische suggesties-eindpunten |
|Aangepaste configuratie-ID | De configuratie-ID van het geselecteerde exemplaar van de aangepaste zoekopdrachten. Dit veld is alleen-lezen. |
|Market     | De markt die het resultaat zal zijn afkomstig uit. Alleen beschikbaar voor de eindpunten Web, installatiekopie, Video en gebruikersinterface die wordt gehost.        |
|Abonnementssleutel | De abonnementssleutel om mee te testen. U kunt een sleutel in de vervolgkeuzelijst selecteren of een handmatig invoeren.          |

Te klikken op **extra Parameters** blijkt dat de volgende parameters:  

|Parameter  |Description  |
|---------|---------|
|Veilig zoeken     | Een filter dat wordt gebruikt voor het filteren van webpagina's voor de inhoud voor volwassenen. Alleen beschikbaar voor de eindpunten Web, installatiekopie, Video en gebruikersinterface die wordt gehost.        |
|Taal van de gebruikersinterface    | De taal die wordt gebruikt voor tekenreeksen voor interface. Bijvoorbeeld, als u inschakelt, afbeeldingen en video's in de gebruikersinterface die wordt gehost, de **installatiekopie** en **Video** tabbladen de opgegeven taal gebruikt.        |
|Count     | Het aantal zoekresultaten te retourneren in de reactie. Alleen beschikbaar voor Web, afbeeldingen en Video-eindpunten.         |
|Offset    | Het aantal zoekresultaten om over te slaan voordat het retourneren van resultaten. Alleen beschikbaar voor Web, afbeeldingen en Video-eindpunten.        |
    
Nadat u alle vereiste opties hebt opgegeven, klikt u op **aanroepen** om de JSON-antwoord in het rechterdeelvenster weer te geven. Als u het eindpunt van de gebruikersinterface die wordt gehost selecteert, kunt u de zoekervaring testen in het onderste deelvenster.

## <a name="next-steps"></a>Volgende stappen

- [Aanroepen van de aangepaste weergave met C#](./call-endpoint-csharp.md)
- [Aanroepen van de aangepaste weergave met Java](./call-endpoint-java.md)
- [Aanroepen van de aangepaste weergave met NodeJs](./call-endpoint-nodejs.md)
- [Aanroepen van de aangepaste weergave met Python](./call-endpoint-python.md)

- [Aanroepen van de aangepaste weergave met de SDK voor C#](./sdk-csharp-quick-start.md)