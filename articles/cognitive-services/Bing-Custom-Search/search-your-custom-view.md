---
title: 'Bing aangepaste zoekactie: Een aangepaste weergave zoeken | Microsoft Docs'
description: Hierin wordt beschreven hoe u een aangepaste weergave van het web te zoeken
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 75f6c8d299c7eed901dda0631fca74b040f72e30
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344897"
---
# <a name="call-your-custom-search"></a>Uw aangepaste zoekopdracht aanroepen
Voordat u uw eerste aanroep aan de aangepaste zoeken-API voor het ophalen van zoekresultaten voor uw exemplaar, moet u een abonnement cognitieve Services code. Als u een sleutel voor aangepaste zoeken-API, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).

> [!NOTE]
> Bestaande Bing aangepaste zoekactie klanten met een preview-sleutel ingericht op of vóór 15 oktober 2017 zich met hun sleutels tot 30 2017 November of totdat ze zijn het maximum aantal query's zijn toegestaan. Daarna nodig ze hebben om te migreren naar de versie van het algemeen beschikbaar op Azure.

## <a name="try-it-out"></a>Probeer het
Nadat u uw aangepaste zoekervaring hebt geconfigureerd, kunt u de configuratie van binnen de portal aangepaste zoekactie testen. Meld u aan bij [aangepaste zoekactie](https://customsearch.ai), klikt u op een exemplaar van de aangepaste zoekactie en klik op de **productie** tabblad. De **eindpunten** tabblad wordt weergegeven. Uw abonnement wordt bepaald welke eindpunten zijn beschikbaar om te proberen, Zie [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/). Moet een eindpunt testen, selecteert u deze uit de vervolgkeuzelijst en de bijbehorende configuratieopties. 

Hieronder vindt u de beschikbare opties.

- **Query**: de zoekterm om naar te zoeken. Alleen beschikbaar voor Web-, afbeelding- en Automatische suggestie eindpunten.
- **Aangepaste configuratie-ID**: de configuratie-ID van het geselecteerde exemplaar van de aangepaste zoekactie. Dit veld is alleen-lezen.
- **Markt**: de herkomst van de resultaten van markt. Alleen beschikbaar voor Web-, afbeelding- en gehost UI-eindpunten.
- **Abonnementssleutel**: de abonnementssleutel om mee te testen. U kunt een sleutel uit de vervolgkeuzelijst selecteren of een handmatig invoeren.
- **Veilig zoeken**: een filter dat wordt gebruikt voor het filteren van webpagina's voor de inhoud voor volwassenen. Alleen beschikbaar voor Web-, afbeelding- en gehost UI-eindpunten.
- **Aantal**: het aantal zoekresultaten te retourneren in het antwoord. Alleen beschikbaar voor Web- en eindpunten.
- **Offset**: het aantal zoekresultaten te retourneren in het antwoord. Alleen beschikbaar voor Web- en eindpunten.

Nadat u alle vereiste opties voor Web-, Image of Automatische suggestie hebt opgegeven, klikt u op **aanroepen** om weer te geven van de JSON-antwoord in het rechterdeelvenster. 

Als u het eindpunt van de gebruikersinterface gehost selecteert, kunt u de zoekfunctie in het rechterdeelvenster kunt testen.

## <a name="next-steps"></a>Volgende stappen
- [Aanroepen van de aangepaste weergave met C#](./call-endpoint-csharp.md)
- [Aanroepen van de aangepaste weergave met Java](./call-endpoint-java.md)
- [De aangepaste weergave met NodeJs aanroepen](./call-endpoint-nodejs.md)
- [Aanroepen van de aangepaste weergave met behulp van Python](./call-endpoint-python.md)