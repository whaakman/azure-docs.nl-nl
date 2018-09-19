---
title: Landen, regio's en talen die worden ondersteund door de Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Ontdek welke landen/regio's en talen worden ondersteund door de Bing webzoekopdrachten-API.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: b954814d1c26e8afd24e0bb13e74c493f0fd2c55
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125116"
---
# <a name="countries-regions-and-languages-supported-by-the-bing-web-search-api"></a>Landen, regio's en talen die worden ondersteund door de Bing webzoekopdrachten-API

De Bing webzoekopdrachten-API ondersteunt meer dan drie tientallen landen of regio's, veel met meer dan één taal. Een land of regio op te geven met een query kunt verfijnen zoekresultaten op basis van de interesses van dat land of regio's. De resultaten kunnen koppelingen naar Bing zijn en deze koppelingen kunnen lokaliseren van de gebruikerservaring van Bing op basis van het opgegeven land/regio of taal.

Kunt u een land of regio met behulp van de `cc` queryparameter. Wanneer een land of regio is opgegeven, moet u een of meer taalcodes met de [ `Accept-Language` header](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Gebruik de [markten tabel](#Markets) voor een lijst van ondersteunde talen in elke markt.

U kunt ook opgeven de markt op met de `mkt` queryparameter, en een code van de **markten** tabel. Tegelijkertijd een markt op te geven, geeft een land of regio en taal van voorkeur. U kunt expliciet instellen met de taal met de `setLang` queryparameter.

## <a name="countriesregions"></a>Landen/regio's

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
|Zwitserland|Frans|FR-h|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Nederlands|en-GB|
|Verenigde Staten|Nederlands|nl-NL|
|Verenigde Staten|Spaans|ES-US|
