---
title: Ondersteunde landen en talen voor Bing installatiekopie API van zoekservice in Azure | Microsoft Docs
description: Ontdek welke landen en talen worden ondersteund door de API van Bing installatiekopie zoeken.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: d0d33ee714ba5cd1ce4e846b96c04f755933bee1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345447"
---
# <a name="bing-image-search-countries-and-languages"></a>Zoeken naar Bing-afbeelding landen en talen

De Bing installatiekopie zoeken-API ondersteunt meer dan drie dozijn landen kunnen veel met meer dan één taal. Een land geven met een query fungeert voor het verfijnen van de zoekresultaten op basis van interesses in dat land. Bovendien worden de resultaten kunnen bevatten koppelingen naar Bing en deze koppelingen kunnen lokalisatie van de gebruikerservaring Bing volgens de opgegeven land of de taal.

Als de land- en taalinstellingen opgeven, stelt u de `mkt` queryparameter (markt) aan de code van de **markten** in de volgende tabel. De markt Hiermee geeft u een land of de taal. Als de gebruiker wil Zie tekst weergeven in een andere taal, stelt u `setLang` queryparameter met de juiste taalcode.

U kunt ook opgeven het land met behulp van de `cc` queryparameter. Als u een land opgeeft, moet u ook een of meer taalcodes met opgeven de `Accept-Language` HTTP-header. De ondersteunde talen verschillen per land; voor elk land in de tabel markten wordt verstrekt.

> [!NOTE]
> De afbeeldingen trends API ondersteunt momenteel alleen de volgende markten:
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
|Brazilië|BRAZILIË|
|Canada|CA|
|Chili|CL|
|Denemarken|DK|
|Finland|FI|
|Frankrijk|FR|
|Duitsland|DE|
|Hongkong|HK|
|India|IN|
|Indonesië|ID|
|Italië|IT|
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
|Filippijnen|PH|
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


## <a name="markets"></a>Markten

|Land/regio|Taal|Markt Code|
|-------|--------|-----------|
|Argentinië|Spaans|ES-AR|
|Australië|Engels|en-AU|
|Oostenrijk|Duits|de AT|
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
|Hongkong|Traditioneel Chinees|zh-HK|
|India|Engels|NL-IN|
|Indonesië|Engels|NL-ID|
|Italië|Italiaans|IT-IT|
|Japan|Japans|ja-JP|
|Zuid-Korea|Koreaans|ko-KR|
|Maleisië|Engels|NL mijn|
|Mexico|Spaans|es-MX|
|Nederland|Nederlands|NL-NL|
|Nieuw-Zeeland|Engels|NL NZ|
|China|Chinees|zh-CN|
|Polen|Pools|pl-PL|
|Portugal|Portugees|pt-PT|
|Filippijnen|Engels|NL-PH|
|Rusland|Russisch|ru-RU|
|Saudi-Arabië|Arabisch|ar-SA|
|Zuid-Afrika|Engels|NL-ZA|
|Spanje|Spaans|es-ES|
|Zweden|Zweeds|SV-SE|
|Zwitserland|Frans|FR CH|
|Zwitserland|Duits|de CH|
|Taiwan|Traditioneel Chinees|zh-TW.|
|Turkije|Turks|tr-TR|
|Verenigd Koninkrijk|Engels|NL GB|
|Verenigde Staten|Engels|nl-NL|
|Verenigde Staten|Spaans|ES VS|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de eindpunten Bing nieuws zoeken [nieuws installatiekopie Search API v7-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
