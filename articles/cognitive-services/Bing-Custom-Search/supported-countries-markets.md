---
title: Ondersteunde landen en talen voor Bing aangepaste API van zoekservice in Azure | Microsoft Docs
description: Ontdek welke landen en talen worden ondersteund door de API van Bing aangepaste zoeken.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/19/2017
ms.author: v-gedod
ms.openlocfilehash: 7ff309f9b789662c4ebd791dffaa2bc2e440763e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344910"
---
# <a name="bing-custom-search-countries-and-languages"></a>Bing aangepaste zoekactie landen en talen

De Bing aangepaste zoeken-API ondersteunt meer dan drie dozijn landen kunnen veel met meer dan één taal. 

Hoewel dit optioneel, de aanvraag moet opgeven, de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Zie voor een lijst van optionele queryparameters [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)

U kunt opgeven van een land via de `cc` queryparameter. Als u een land opgeeft, moet u ook een of meer taalcodes met opgeven de `Accept-Language` header. De ondersteunde talen verschillen per land; ze zijn opgegeven voor elk land in de **markten** tabel.

De `Accept-Language` header en het `setLang` queryparameter sluiten elkaar wederzijds uit, niet beide opgeeft. Zie voor meer informatie [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

## <a name="countries"></a>Landen

|Land|Code|
|-------|----|
|Argentinië|AR|
|Australië|AUSTRALIË|
|Oostenrijk|AT|
|België|WORDEN|
|Brazilië|BRAZILIË|
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
|Zweden|SE|
|Zwitserland|CH|
|Taiwan|TW|
|Turkije|TR|
|Verenigd Koninkrijk|GB|
|Verenigde Staten|VS|


## <a name="markets"></a>Markten

|Land|Taal|Markt Code|
|-------|--------|-----------|
|Argentinië|Spaans|ES-AR|
|Australië|Nederlands|en-AU|
|Oostenrijk|Duits|de AT|
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
|Maleisië|Nederlands|NL mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Nederlands|NL NZ|
|Noorwegen|Noors|uit den boze|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filippijnen|Nederlands|NL-PH|
|Rusland|Russisch|ru-RU|
|Saoedi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Nederlands|NL-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR CH|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW.|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Nederlands|NL GB|
|Verenigde Staten|Nederlands|nl-NL|
|Verenigde Staten|Spaans|ES VS|
