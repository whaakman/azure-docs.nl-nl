---
title: Het formaat en bijsnijden miniaturen van Bing - Bing afbeeldingen zoeken-API
description: Informatie over het vergroten of verkleinen en bijgesneden miniaturen opgenomen in een reactie van de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: F4FFAE91-A003-4F7C-8E60-83A142485E28
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: de82cc5554af91294dda3826dfb394cc94dbf3d0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296224"
---
# <a name="resizing-and-cropping-thumbnail-images"></a>Vergroten/verkleinen en bijsnijden miniatuurafbeeldingen

Bij het verwerken van een zoekquery parameterselectie Bing miniaturen gegevens voor alle installatiekopieën in de [antwoord](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#bing-image-search-response-format). Deze informatie kan worden gebruikt voor weergave alle of een subset van de geretourneerde miniatuurweergaven. Als u een subset wordt weergegeven, bieden u een optie om de resterende installatiekopieën weer te geven.


<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Als de gebruiker op de miniatuur klikt, kunt u [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) gebruiken om de afbeelding op volledige grootte weer te geven aan de gebruiker. Vergeet niet om een geschikt kenmerk toe te wijzen aan de afbeelding.

Als `shoppingSourcesCount` of `recipeSourcesCount` groter is dan nul, voeg dan een visueel element, zoals een winkelwagen, toe aan de miniatuur om aan te geven dat er artikelen of recepten bestaan voor het item in de afbeelding.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Als u inzichten over de afbeelding wilt opvragen, zoals webpagina's die de afbeelding bevatten of personen die zijn herkend in de afbeelding, gebruikt u [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken). Zie voor meer informatie [Inzichten over afbeeldingen](image-insights.md).

## <a name="resizing-and-cropping-thumbnails"></a>Vergroten/verkleinen en miniaturen bijsnijden

U kunt ook miniaturen, zoals wanneer de cursor van een gebruiker de muisaanwijzer boven het uitvouwen en vergroten of verkleinen.
> [!NOTE]
> Vergeet niet om een geschikt kenmerk toe wijzen aan de afbeelding als u dat wilt doen. U kunt bijvoorbeeld de host van [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) extraheren en deze weergeven onder de afbeelding.

[!INCLUDE [cognitive-services-bing-resize-crop-thumbnails](../../../includes/cognitive-services-bing-resize-crop-thumbnails.md)]
