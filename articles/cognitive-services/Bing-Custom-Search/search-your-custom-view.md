---
title: Zoeken naar een aangepaste weergave - Bing Custom Search
titlesuffix: Azure Cognitive Services
description: Hierin wordt beschreven hoe u kunt een aangepaste weergave van het web zoeken.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 235062c1b3e54843b5e64f4ef16091ae5d630894
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814352"
---
# <a name="call-your-custom-search"></a>Aanroepen van uw aangepaste zoekopdrachten

Voordat u de eerste aanroep van de Custom Search-API om op te halen van zoekresultaten voor uw exemplaar, moet u een code van Cognitive Services-abonnement. Als u een sleutel voor Custom Search-API, Zie [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).


## <a name="try-it-out"></a>Uitproberen

Nadat u uw aangepaste zoekervaring hebt geconfigureerd, kunt u de configuratie van binnen de portal zoeken in aangepaste testen. 

1. Meld u aan bij [Custom Search](https://customsearch.ai).
2. Klik op een Custom Search-exemplaar uit de lijst van exemplaren.
3. Klik op de **productie** tabblad. 
4. Onder de **eindpunten** tabblad, selecteert u een eindpunt (bijvoorbeeld Web-API). Uw abonnement bepaalt welke eindpunten worden weergegeven (Zie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) voor opties voor abonnementen). 
5. Geef de parameterwaarden. 

    Hieronder vindt u de mogelijke parameters die u kunt instellen (de daadwerkelijke lijst is afhankelijk van het eindpunt van de geselecteerde). Zie voor meer informatie over deze parameters [Custom Search-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) verwijzing.

    - **Query**: de zoekterm moet worden gezocht. Alleen beschikbaar voor Web, installatiekopie, Video en Automatische suggesties-eindpunten.
    - **Aangepaste configuratie-ID**: de configuratie-ID van het geselecteerde exemplaar van de aangepaste zoekopdrachten. Dit veld is alleen-lezen.
    - **Handel**: de markt waarin de resultaten afkomstig zijn uit. Alleen beschikbaar voor Web, installatiekopie, Video en gebruikersinterface die wordt gehost eindpunten.
    - **Abonnementssleutel**: de abonnementssleutel om mee te testen. U kunt een sleutel in de vervolgkeuzelijst selecteren of een handmatig invoeren.  
      
    Te klikken op **extra Parameters** blijkt dat de volgende parameters:  
      
    - **Veilig zoeken**: een filter dat wordt gebruikt voor het filteren van webpagina's voor de inhoud voor volwassenen. Alleen beschikbaar voor Web, installatiekopie, Video en gebruikersinterface die wordt gehost eindpunten.
    - **Taal van de gebruikersinterface**: de taal die wordt gebruikt voor tekenreeksen voor interface. Bijvoorbeeld, als u inschakelt, afbeeldingen en video's in de gebruikersinterface die wordt gehost, de **installatiekopie** en **Video** tabbladen de opgegeven taal gebruikt.
    - **Aantal**: het aantal zoekresultaten te retourneren in de reactie. Alleen beschikbaar voor Web, afbeeldingen en Video-eindpunten.
    - **Offset**: het aantal zoekresultaten om over te slaan voordat het retourneren van resultaten. Alleen beschikbaar voor Web, afbeeldingen en Video-eindpunten.

6. Nadat u alle vereiste opties hebt opgegeven, klikt u op **aanroepen** om de JSON-antwoord in het rechterdeelvenster weer te geven. 

Als u het eindpunt van de gebruikersinterface die wordt gehost selecteert, kunt u de zoekervaring testen in het onderste deelvenster.

## <a name="next-steps"></a>Volgende stappen

- [Aanroepen van de aangepaste weergave met C#](./call-endpoint-csharp.md)
- [Aanroepen van de aangepaste weergave met Java](./call-endpoint-java.md)
- [Aanroepen van de aangepaste weergave met NodeJs](./call-endpoint-nodejs.md)
- [Aanroepen van de aangepaste weergave met Python](./call-endpoint-python.md)

- [Aanroepen van de aangepaste weergave met de SDK voor C#](./sdk-csharp-quick-start.md)