---
title: Wat is Bing Aangepaste zoekopdrachten? | Microsoft Docs
description: Bevat een overzicht van Bing Custom Search
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: b6f50844d6571cca6d63c1db7a85863e3d22d411
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948074"
---
# <a name="what-is-bing-custom-search"></a>Wat is Bing Aangepaste zoekopdrachten?

Bing aangepaste zoekopdrachten kunt maken op maat gemaakte zoekervaringen voor onderwerpen waarin u geïnteresseerd bent. Bijvoorbeeld, als u over een website die u een zoekfunctie kunt, kunt u de domeinen, websites en webpagina's die Bing zoeken. Uw gebruikers zien de resultaten die zijn afgestemd op de inhoud die ze geïnteresseerd in plaats van naar de pagina met zoekresultaten die niet van belang inhoud bevatten.

Gebruik voor het maken van uw aangepaste weergave van de website de Bing Custom Search [portal](https://customsearch.ai). De portal kunt u een exemplaar voor aangepast zoeken die Hiermee geeft u de domeinen, websites, en webpagina's die u wilt dat Bing om te zoeken naar en de websites die u niet wilt dat deze om te zoeken naar maken. Naast het opgeven van de URL's van de inhoud die u kent, kunt u de portal ook gebruiken om te zoeken naar relevante inhoud die u wilt toevoegen.

De portal kunt u een specifieke webpagina aan het begin van het zoekresultaat vastmaken als de gebruiker een specifieke zoekterm invoert. 

Na het definiëren van uw exemplaar, kunt u aangepaste zoekopdrachten integreren in uw website, desktop-app of mobiele app door de aangepaste zoekopdrachten-API aan te roepen. Als u een web gebaseerde site of toepassing hebt, kunt u de gehoste-gebruikersinterface weergegeven van de search-interface voor u.

De volgende afbeelding toont de eenvoud van de aangepaste zoekopdrachten-integratie.

![afbeelding alt](./media/bcs-overview.png "hoe Bing Custom Search werkt.")

## <a name="adding-custom-search-box-suggestions"></a>Toevoegen van aangepaste zoekopdrachten vak suggesties

U kunt uw aangepaste zoekervaring met aangepaste-vak zoeksuggesties verrijken. Deze functie beschikt u over aangepaste zoeksuggesties relevant zijn voor uw zoekervaring. Als de gebruiker typt in het zoekvak in de vervolgkeuzelijst voorgestelde querytekenreeksen op basis van de gedeeltelijke queryreeks van de gebruiker bevat. U kunt opgeven of u wilt retourneren alleen uw aangepaste suggesties of bevat ook suggesties voor Bing. [Meer informatie](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Toe te voegen aangepaste installatiekopie zoekervaring

U kunt uw ervaring aangepast zoeken met afbeeldingen verrijken. Dit is vergelijkbaar met webresultaten, aangepaste zoekopdrachten ondersteunt zoeken voor afbeeldingen in uw exemplaar van de lijst met websites. [Meer informatie](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Toevoegen van aangepaste video's zoeken-ervaring

U kunt uw aangepaste zoekervaring met video's verrijken. Dit is vergelijkbaar met webresultaten, aangepaste zoekopdrachten ondersteunt het zoeken naar video's in uw exemplaar van de lijst met websites. [Meer informatie](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Uw exemplaar voor aangepast zoeken met anderen te delen

U kunt eenvoudig toestaan gezamenlijke bewerken en het testen van uw exemplaar door met leden van uw team te delen. [Meer informatie](share-your-custom-search.md).

## <a name="next-steps"></a>Volgende stappen

Als u wilt snel aan de slag, Zie [maken van uw eerste exemplaar van de Bing Custom Search](quick-start.md).

Zie voor meer informatie over het aanpassen van uw exemplaar van de search [definiëren van een exemplaar voor aangepast zoeken](define-your-custom-view.md).

Raken met de referentie-inhoud voor elk van de aangepaste zoekopdrachten-eindpunten. De verwijzing bevat de eindpunten, kopteksten en queryparameters die u gebruiken wilt om aan te vragen van zoekresultaten. Daarnaast vindt u hier definities van de responsobjecten.

- [Custom Search-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [Aangepaste afbeeldingen-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [Aangepaste Vidoe-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [Aangepaste automatische suggestie-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Lees [Gebruiks- en weergavevereisten voor Bing](./use-and-display-requirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.