---
title: Taal ondersteuning-Bing Webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Een lijst met natuurlijke talen, landen en regio's die worden ondersteund door de Bing Nieuws zoeken-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 9425de6e75a9a46d71ff85ce49b0650c8e7a9a16
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882675"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Ondersteuning van talen en regio's voor de Bing Webzoekopdrachten-API

Het Bing Webzoekopdrachten-API ondersteunt meer dan drie dozijn landen of regio's, veel met meer dan één taal. Het opgeven van een land of regio met een query helpt de zoek resultaten te verfijnen op basis van de interesses van die landen of regio's. De resultaten kunnen koppelingen naar Bing bevatten en deze koppelingen kunnen de Bing-gebruikers ervaring lokaliseren op basis van de opgegeven land/regio of taal.

U kunt een land of regio opgeven met behulp van de `cc` query parameter. Wanneer u een land of regio hebt opgegeven, moet u een of meer taal codes met de [ `Accept-Language` header](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#headers)opgeven. Gebruik de [tabel Markets](#markets) voor een lijst met talen die op elke markt worden ondersteund.

U kunt ook de-markt met de `mkt` query parameter en een code uit de tabel **Markets** opgeven. Als u een markt opgeeft, geeft u een land of regio en een voorkeurs taal op. U kunt de taal expliciet instellen met de `setLang` query parameter.

## <a name="countriesregions"></a>Landen/regio's

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
|Indonesië|id|
|Italië|it|
|Japan|JP|
|Zuid-Korea|KR|
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
|Saudi-Arabië|SA|
|Zuid-Afrika|ZA|
|Spanje|ES|
|Zweden|SE|
|Zwitserland|CH|
|Taiwan|TW|
|Turkije|TR|
|Verenigd Koninkrijk|GB|
|Verenigde Staten|VS|

## <a name="markets"></a>Landen

|Land/regio|Taal|Markt code|
|-------|--------|-----------|
|Argentinië|Spaans|es-AR|
|Australië|Engels|en-AU|
|Oostenrijk|Duits|de-AT|
|België|Nederlands|nl-BE|
|België|Frans|fr-worden|
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
|Indonesië|Engels|en-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Zuid-Korea|Koreaans|ko-KR|
|Maleisië|Engels|en-mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Engels|NL-NZ|
|Noorwegen|Noors|Nee-Nee|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filipijnen|Engels|en-PH|
|Rusland|Russisch|ru-RU|
|Saudi-Arabië|Arabisch|ar-SA|
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

* [Naslag voor Bing Afbeeldingen zoeken-API](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
