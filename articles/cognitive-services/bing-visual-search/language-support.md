---
title: Taalondersteuning - Bing visuele zoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen, landen en regio's die worden ondersteund door de Bing visuele zoekopdrachten-API. De Bing visuele zoekopdrachten-API biedt ondersteuning voor meer dan drie tientallen landen/regio's, veel met meer dan één taal.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: edfd3ba88da16ff283096af3c5a4929dd06417e2
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435459"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Ondersteuning voor taal en regio voor de Bing visuele zoekopdrachten-API

Bing visuele zoekopdrachten-API biedt ondersteuning voor meer dan drie tientallen landen/regio's, veel met meer dan één taal. Elke aanvraag moet bevatten voor land/regio en taal naar keuze van de gebruiker. Markt van de gebruiker weet, kunt u Bing juiste resultaten worden geretourneerd. Als u een land/regio en taal niet opgeeft, maakt Bing een best-effort om te bepalen van de gebruiker land/regio en taal. Omdat de resultaten koppelingen naar Bing bevatten kunnen, kan weet de land/regio en taal bieden een voorkeur gelokaliseerde Bing gebruikerservaring als de gebruiker op de Bing-koppelingen.

Als de land/regio en taal opgeven, stelt u de `mkt` queryparameter (markt) naar een code van de **markten** in de volgende tabel. De markt Hiermee geeft u een land/regio en taal. Als de gebruiker geeft de voorkeur aan om te zien in een andere taal-tekst weergeven, stelt u `setLang` queryparameter in de juiste taal-code.

U kunt ook opgeven de land/regio met behulp van de `cc` queryparameter. Als u een land/regio opgeeft, moet u ook opgeven een of meer taalcodes die met behulp van de `Accept-Language` HTTP-header. De ondersteunde talen variëren per land/regio; voor elk land in de tabel markten wordt verstrekt.



> [!NOTE]
> Er gelden de volgende beperkingen in de markt:
>
> - Image recognition aantekeningen zijn alleen beschikbaar in het Engels.
> - Recept, aankopen en pagina's, inclusief inzichten zijn beschikbaar in de Amerikaanse markt.


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
