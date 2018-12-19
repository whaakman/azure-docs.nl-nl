---
title: Taalondersteuning - Bing Custom Search-API
titleSuffix: Azure Cognitive Services
description: Een lijst met ondersteunde talen en regio's voor de Bing Custom Search-API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: v-gedod
ms.openlocfilehash: 8fa70705d1dff5c3150fa5868938a3c50ef863fe
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48815560"
---
# <a name="language-and-region-support-for-the-bing-custom-search-api"></a>Ondersteuning voor de Bing webzoekopdrachten-API voor aangepaste taal en regio

De Bing Custom Search-API biedt ondersteuning voor meer dan drie tientallen landen/regio's, veel met meer dan één taal.

Hoewel dit optioneel is, de aanvraag moet opgeven de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) queryparameter, waarmee de markt waar u de resultaten afkomstig zijn van uniek wordt geïdentificeerd. Zie voor een lijst van optionele queryparameters [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)

U kunt geeft u een land/regio met de `cc` queryparameter. Als u een land/regio opgeeft, moet u ook opgeven een of meer taalcodes die met behulp van de `Accept-Language` header. De ondersteunde talen variëren per land/regio; ze worden gegeven voor elk land/regio in de **markten** tabel.

De `Accept-Language` header en het `setLang` queryparameter zijn sluiten elkaar wederzijds uit, niet beide opgeeft. Zie voor meer informatie, [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

## <a name="countries"></a>Landen/regio's

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
|Hongkong SAR|HK|
|India|IN|
|Indonesië|ID|
|Italië|it|
|Japan|JP|
|Zuid-Korea|KR|
|Maleisië|MIJN|
|Mexico|MX|
|Nederland|NL|
|Nieuw-Zeeland|NZ|
|Noorwegen|NEE|
|China|ALGEMENE NAAM|
|Polen|PL|
|Portugal|PT|
|Filipijnen|PH|
|Rusland|RU|
|Saudi-Arabië|SA|
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
|Australië|Engels|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|NL-worden|
|België|Frans|FR-worden|
|Brazilië|Portugees|pt-BR|
|Canada|Engels|NL-CA|
|Canada|Frans|fr-CA|
|Chili|Spaans|ES-CL|
|Denemarken|Deens|da-DK|
|Finland|Fins|fi-FI|
|Frankrijk|Frans|fr-FR|
|Duitsland|Duits|de-DE|
|Hongkong SAR|Traditioneel Chinees|zh-HK|
|India|Engels|NL-IN|
|Indonesië|Engels|NL-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Zuid-Korea|Koreaans|ko-KR|
|Maleisië|Engels|en Mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Engels|NL-NZ|
|Noorwegen|Noors|uit den boze|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filipijnen|Engels|NL-PH|
|Rusland|Russisch|ru-RU|
|Saudi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Engels|NL-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR-h|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Engels|en-GB|
|Verenigde Staten|Engels|nl-NL|
|Verenigde Staten|Spaans|ES-US|
