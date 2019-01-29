---
title: Taalondersteuning - Bing Custom Search-API
titleSuffix: Azure Cognitive Services
description: Een lijst met ondersteunde talen en regio's voor de Bing Custom Search-API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: v-gedod
ms.openlocfilehash: 24b14c4f0ca185a7916aeb19671a22ff46a23ee2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194845"
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
|Australië|Nederlands|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|nl-BE|
|België|Frans|fr-BE|
|Brazilië|Portugees|pt-BR|
|Canada|Nederlands|NL-CA|
|Canada|Frans|fr-CA|
|Chili|Spaans|es-CL|
|Denemarken|Deens|da-DK|
|Finland|Fins|fi-FI|
|Frankrijk|Frans|fr-FR|
|Duitsland|Duits|de-DE|
|Hongkong SAR|Traditioneel Chinees|zh-HK|
|India|Nederlands|NL-IN|
|Indonesië|Nederlands|NL-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Korea|Koreaans|ko-KR|
|Maleisië|Nederlands|en Mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Nederlands|NL-NZ|
|Noorwegen|Noors|no-NO|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filipijnen|Nederlands|NL-PH|
|Rusland|Russisch|ru-RU|
|Saoedi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Nederlands|en-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR-h|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Verenigde Staten|Nederlands|en-US|
|Verenigde Staten|Spaans|es-US|
