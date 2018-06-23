---
title: Ondersteunde landen en talen voor Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Ontdek welke landen en talen worden ondersteund door Bing Visual zoeken-API.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 4723d028cc22caf8be3eb294b52506ec112cbab5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345327"
---
# <a name="bing-visual-search-countries-and-languages"></a>Bing Visual zoeken landen en talen

Bing Visual zoeken-API ondersteunt meer dan drie dozijn landen kunnen veel met meer dan één taal. De land en de taal van de keuze van de gebruiker bevatten elke aanvraag. Markt van de gebruiker weet, kunt u Bing juiste resultaten geretourneerd. Als u geen land- en taalinstellingen opgeeft, wordt Bing een zo goed mogelijke poging om te bepalen van de gebruiker land- en taalinstellingen. Omdat de resultaten bevatten koppelingen naar Bing te sturen kunnen, kan weet de land- en taalinstellingen bieden een voorkeur gelokaliseerde Bing gebruikerservaring als de gebruiker op de Bing-koppelingen.

Als de land- en taalinstellingen opgeven, stelt u de `mkt` queryparameter (markt) aan de code van de **markten** in de volgende tabel. De markt Hiermee geeft u een land of de taal. Als de gebruiker wil Zie tekst weergeven in een andere taal, stelt u `setLang` queryparameter met de juiste taalcode.

U kunt ook opgeven het land met behulp van de `cc` queryparameter. Als u een land opgeeft, moet u ook een of meer taalcodes met opgeven de `Accept-Language` HTTP-header. De ondersteunde talen verschillen per land; voor elk land in de tabel markten wordt verstrekt.



> [!NOTE]
> De volgende markt beperkingen toepassen:
> 
> - Afbeelding erkenning aantekeningen zijn alleen beschikbaar in het Engels. 
> - Recept, aankopen en pagina's, inclusief inzicht zijn beschikbaar in de markt en-US alleen. 


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
