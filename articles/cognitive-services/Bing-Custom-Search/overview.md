---
title: Wat is Bing Aangepaste zoekopdrachten?
titlesuffix: Azure Cognitive Services
description: Biedt op hoog niveau een overzicht van Bing Aangepaste zoekopdrachten.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: overview
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: f2946918f0c1a7a516788989042825e8f49d7b0b
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318721"
---
# <a name="what-is-bing-custom-search"></a>Wat is Bing Aangepaste zoekopdrachten?

Met Bing Aangepaste zoekopdrachten kunt u op maat gemaakte zoekervaringen maken voor onderwerpen die u interesseren. Als u bijvoorbeeld een website hebt die een zoekervaring biedt, kunt u de domeinen, websites en webpagina's opgeven waarin Bing zoekt. Uw gebruikers zien dan zoekresultaten die zijn afgestemd op de inhoud die ze interessant vinden in plaats van dat ze door pagina's met zoekresultaten moeten bladeren die niet-relevante inhoud bevatten.

Als u een aangepaste weergave van het web wilt maken, gebruikt u de [portal](https://customsearch.ai) van Bing Aangepaste zoekopdrachten. In de portal kunt u een exemplaar voor aangepaste zoekopdrachten maken waarin u de domeinen, websites en webpagina's opgeeft waarin Bing moet zoeken. U geeft ook aan welke websites moeten worden uitgesloten. Naast het opgeven van de URL's van de inhoud die u kent, kunt u de portal ook gebruiken om relevante inhoud te zoeken die u mogelijk wilt toevoegen.

In de portal kunt u een specifieke webpagina vastmaken aan de bovenkant van de zoekresultaten voor wanneer de gebruiker een specifieke zoekterm invoert. 

Wanneer u een exemplaar hebt gedefinieerd, kunt u aangepast zoeken integreren op uw website, in uw desktop-app of in uw mobiele app. Roep hiervoor de Aangepaste zoekopdrachten-API aan. Als u beschikt over een site of toepassing op internet, kunt u de gehoste gebruikersinterface de zoekinterface voor u laten weergeven.

In de volgende afbeelding ziet u hoe eenvoudig aangepast zoeken kan worden geïntegreerd.

![picture alt](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/cognitive-services/Bing-Custom-Search/media/BCS-Overview.png "Hoe Bing Aangepaste zoekopdrachten werkt.")

## <a name="adding-custom-search-box-suggestions"></a>Aangepaste zoekvaksuggesties toevoegen

U kunt uw ervaring met aangepaste zoekopdrachten verrijken met aangepaste zoekvaksuggesties. Met deze functie kunt u aangepaste zoeksuggesties opgeven die relevant zijn voor uw zoekervaring. Wanneer de gebruiker in het zoekvak begint te typen, worden in de vervolgkeuzelijst gesuggereerde queryreeksen weergegeven op basis van de gedeeltelijk ingevoerde queryreeks. U kunt opgeven of er alleen aangepaste suggesties mogen worden geretourneerd, of ook Bing-suggesties. [Meer informatie](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Ervaring voor aangepast afbeeldingen zoeken toevoegen

U kunt uw ervaring met aangepaste zoekopdrachten verrijken met afbeeldingen. Bij aangepast zoeken, kunt u (net als bij webresultaten) zoeken naar afbeeldingen in de lijst websites van uw exemplaar. [Meer informatie](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Ervaring voor aangepast video's zoeken toevoegen

U kunt uw ervaring met aangepaste zoekopdrachten verrijken met video's. Bij aangepast zoeken, kunt u (net als bij webresultaten) zoeken naar video's in de lijst websites van uw exemplaar. [Meer informatie](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Uw exemplaar voor aangepast zoeken delen met anderen

U kunt het gezamenlijk bewerken en testen van uw exemplaar eenvoudig toestaan door het te delen met leden van uw team. [Meer informatie](share-your-custom-search.md).

## <a name="next-steps"></a>Volgende stappen

Zie [Uw eerste Bing Aangepaste zoekopdrachten-exemplaar maken](quick-start.md) om snel aan de slag te gaan.

Zie [Een exemplaar voor aangepast zoeken definiëren](define-your-custom-view.md) voor informatie over het aanpassen van uw zoekexemplaar.

Lees u in in de referentie-inhoud van alle aangepaste zoekeindpunten. De handleiding bevat de eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen. Daarnaast vindt u hier definities van de responsobjecten.

- [Custom Search-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Custom Image-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Aangepaste Video-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Custom Autosuggest-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Lees [Gebruiks- en weergavevereisten voor Bing](./use-and-display-requirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.
