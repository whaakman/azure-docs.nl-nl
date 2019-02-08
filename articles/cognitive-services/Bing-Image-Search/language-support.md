---
title: Taalondersteuning - Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: Ontdek welke landen/regio's en talen worden ondersteund door de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: aahi
ms.openlocfilehash: 3b1c4108b18d744eb526a765a98f740d6e27a444
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868392"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Ondersteuning voor taal en regio voor de Bing afbeeldingen zoeken-API

De Bing afbeeldingen zoeken-API biedt ondersteuning voor meer dan drie tientallen landen/regio's, veel met meer dan één taal. Een land/regio met een query op te geven fungeert voornamelijk om zoekresultaten te verfijnen op basis van de interesses in dat land/regio. Bovendien worden de resultaten kunnen koppelingen naar Bing bevatten en deze koppelingen kunnen lokaliseren van de gebruikerservaring van Bing op basis van de opgegeven land/regio's of de taal.

Als de land/regio en taal opgeven, stelt u de `mkt` queryparameter (markt) naar een code van de **markten** in de volgende tabel. De markt Hiermee geeft u een land/regio en taal. Als de gebruiker geeft de voorkeur aan om te zien in een andere taal-tekst weergeven, stelt u `setLang` queryparameter in de juiste taal-code.

U kunt ook opgeven de land/regio met behulp van de `cc` queryparameter. Als u een land/regio opgeeft, moet u ook opgeven een of meer taalcodes die met behulp van de `Accept-Language` HTTP-header. De ondersteunde talen variëren per land/regio; voor elk land/regio in de tabel markten wordt verstrekt.

> [!NOTE]
> De Trending afbeeldingen-API ondersteunt momenteel alleen de volgende markten:
> - en-US (Engels, Verenigde Staten)
> - NL-CA (Engels, Canada)
> - en-AU (Engels, Australië)
> - zh-CN (Chinees, China)

## <a name="countries"></a>Landen/regio's

|Land/regio|Code|
|-------|----|
|Argentinië|AR|
|Australië|AU|
|Oostenrijk|AT|
|België|BE|
|Brazilië|BR|
|Canada|CA|
|Chili|CL|
|Denemarken|DK|
|Finland|FI|
|Frankrijk|FR|
|Duitsland|DE|
|Hongkong SAR|HK|
|India|IN|
|Indonesië|Id|
|Italië|IT|
|Japan|JP|
|Korea|KR|
|Maleisië|MY|
|Mexico|MX|
|Nederland|NL|
|Nieuw-Zeeland|NZ|
|Noorwegen|NO|
|China|CN|
|Polen|PL|
|Portugal|PT|
|Filipijnen|PH|
|Rusland|RU|
|Saoedi-Arabië|SA|
|Zuid-Afrika|ZA|
|Spanje|ES|
|Zweden|SE|
|Zwitserland|CH|
|Taiwan|TW|
|Turkije|TR|
|Verenigd Koninkrijk|GB|
|Verenigde Staten|VS|


## <a name="markets"></a>Markten

|Land/regio|Taal|Code van de markt|
|-------|--------|-----------|
|Argentinië|Spaans|es-AR|
|Australië|Engels|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|nl-BE|
|België|Frans|fr-BE|
|Brazilië|Portugees|pt-BR|
|Canada|Engels|NL-CA|
|Canada|Frans|fr-CA|
|Chili|Spaans|es-CL|
|Denemarken|Deens|da-DK|
|Finland|Fins|fi-FI|
|Frankrijk|Frans|fr-FR|
|Duitsland|Duits|de-DE|
|Hongkong SAR|Traditioneel Chinees|zh-HK|
|India|Engels|NL-IN|
|Indonesië|Engels|NL-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Korea|Koreaans|ko-KR|
|Maleisië|Engels|en Mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Engels|NL-NZ|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filipijnen|Engels|NL-PH|
|Rusland|Russisch|ru-RU|
|Saoedi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Engels|en-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR-h|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Engels|en-GB|
|Verenigde Staten|Engels|en-US|
|Verenigde Staten|Spaans|es-US|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het zoeken in Bing nieuws-eindpunten [nieuws afbeeldingen zoeken-API voor Bing versie 7 verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
