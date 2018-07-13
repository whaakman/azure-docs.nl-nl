---
title: Ondersteunde landen/regio's en talen voor de Bing afbeeldingen zoeken-API op Azure | Microsoft Docs
description: Ontdek welke landen/regio's en talen worden ondersteund door de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 97e2bed509146172c10aa9ac2658b99ed7610fcc
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004435"
---
# <a name="bing-image-search-countriesregions-and-languages"></a>Bing afbeeldingen zoeken-landen/regio's en talen

De Bing afbeeldingen zoeken-API biedt ondersteuning voor meer dan drie tientallen landen/regio's, veel met meer dan één taal. Een land/regio met een query op te geven fungeert voornamelijk om zoekresultaten te verfijnen op basis van de interesses in dat land/regio. Bovendien worden de resultaten kunnen koppelingen naar Bing bevatten en deze koppelingen kunnen lokaliseren van de gebruikerservaring van Bing op basis van de opgegeven land/regio's of de taal.

Als de land/regio en taal opgeven, stelt u de `mkt` queryparameter (markt) naar een code van de **markten** in de volgende tabel. De markt Hiermee geeft u een land/regio en taal. Als de gebruiker geeft de voorkeur aan om te zien in een andere taal-tekst weergeven, stelt u `setLang` queryparameter in de juiste taal-code.

U kunt ook opgeven de land/regio met behulp van de `cc` queryparameter. Als u een land/regio opgeeft, moet u ook opgeven een of meer taalcodes die met behulp van de `Accept-Language` HTTP-header. De ondersteunde talen variëren per land/regio; voor elk land/regio in de tabel markten wordt verstrekt.

> [!NOTE]
> De Trending afbeeldingen-API ondersteunt momenteel alleen de volgende markten:
> - en-US (Engels, Verenigde Staten) 
> - NL-CA (Engels, Canada) 
> - en-AU (Engels, Australië) 
> - zh-CN (Chinees, China)

## <a name="countries"></a>Landen

|Land/regio|Code|
|-------|----|
|Argentinië|AR|
|Australië|AUSTRALIË|
|Oostenrijk|AT|
|België|WORDEN|
|Brazilië|BR|
|Canada|CA|
|Chili|CL|
|Denemarken|DK|
|Finland|FI|
|Frankrijk|FR|
|Duitsland|DE|
|Hongkong|HK|
|India|IN|
|Indonesië|Id|
|Italië|IT|
|Japan|JP|
|Korea|KR|
|Maleisië|MIJN|
|Mexico|MX|
|Nederland|NL|
|Nieuw-Zeeland|NZ|
|Noorwegen|NEE|
|China|ALGEMENE NAAM|
|Polen|PL|
|Portugal|PT|
|Filippijnen|PH|
|Rusland|RU|
|Saoedi-Arabië|SA|
|Zuid-Afrika|ZA|
|Spanje|ES|
|Zweden|TWEEDE EDITIE|
|Zwitserland|CH|
|Taiwan|TWEE|
|Turkije|TR|
|Verenigd Koninkrijk|GB|
|Verenigde Staten|VS|


## <a name="markets"></a>Markten

|Land/regio|Taal|Code van de markt|
|-------|--------|-----------|
|Argentinië|Spaans|ES-AR|
|Australië|Nederlands|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|NL-worden|
|België|Frans|FR-worden|
|Brazilië|Portugees|pt-BR|
|Canada|Nederlands|NL-CA|
|Canada|Frans|fr-CA|
|Chili|Spaans|ES-CL|
|Denemarken|Deens|da-DK|
|Finland|Fins|fi-FI|
|Frankrijk|Frans|fr-FR|
|Duitsland|Duits|de-DE|
|Hongkong|Traditioneel Chinees|zh-HK|
|India|Nederlands|NL-IN|
|Indonesië|Nederlands|NL-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Korea|Koreaans|ko-KR|
|Maleisië|Nederlands|en Mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Nederlands|NL-NZ|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filippijnen|Nederlands|NL-PH|
|Rusland|Russisch|ru-RU|
|Saoedi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Nederlands|NL-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR-h|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Verenigde Staten|Nederlands|nl-NL|
|Verenigde Staten|Spaans|ES-US|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het zoeken in Bing nieuws-eindpunten [nieuws afbeeldingen zoeken-API voor Bing versie 7 verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
