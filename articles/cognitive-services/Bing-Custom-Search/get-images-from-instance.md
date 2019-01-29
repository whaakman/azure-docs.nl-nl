---
title: Afbeeldingen ophalen uit de aangepaste weergave - Bing Custom Search
titleSuffix: Azure Cognitive Services
description: Overzicht op hoog niveau over het gebruik van Bing Custom Search om op te halen van afbeeldingen uit de aangepaste weergave van het Web.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: scottwhi
ms.openlocfilehash: 742436b3314886cc81bdefcf9a2961d25e268a02
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55148554"
---
# <a name="get-images-from-your-custom-view"></a>Afbeeldingen ophalen uit de aangepaste weergave

Zoeken in Bing aangepaste installatiekopieën kunt u uw aangepaste zoekervaring met installatiekopieën te verrijken. Bij aangepast zoeken, kunt u (net als bij webresultaten) zoeken naar afbeeldingen in de lijst websites van uw exemplaar. U kunt de afbeeldingen met behulp van Bing aangepaste afbeeldingen zoeken-API ophalen of functie via de gebruikersinterface die wordt gehost. Met behulp van de gehoste UI-functie is eenvoudig te gebruiken en aanbevolen voor de zoekfunctie om aan de slag en uitvoeren in een korte volgorde.  Zie voor meer informatie over het configureren van uw gebruikersinterface die wordt gehost om op te nemen van installatiekopieën [configureren van uw gehoste gebruikersinterface-ervaring](hosted-ui.md).

Als u meer controle over het weergeven van de lijst met zoekresultaten wilt, kunt u afbeeldingen van Bing Custom Search-API. Omdat aanroepen van de API vergelijkbaar is met de Bing afbeeldingen zoeken-API aanroepen afhandeling [Bing afbeeldingen zoeken](../Bing-Image-Search/overview.md) voor voorbeelden van de API aanroept. Maar voordat u dit doen, Maak uzelf vertrouwd met de [aangepaste afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference) inhoud. De belangrijkste verschillen zijn de ondersteunde queryparameters (u moet de queryparameter customConfig bevatten) en het verzenden van aanvragen voor het eindpunt.

<!--
## Next steps

[Call your custom view](search-your-custom-view.md)
-->
