---
title: Ondersteunde landen en talen voor Bing Web-API voor zoeken in Azure | Microsoft Docs
description: Ontdek welke landen en talen worden ondersteund door de Bing Web zoeken-API.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 7b62c4a4feb7144662a8fe4d692f11f1efe5db1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344549"
---
# <a name="bing-web-search-countries-and-languages"></a>Bing zoeken op het Web landen en talen

De Bing Web zoeken-API ondersteunt meer dan drie dozijn landen kunnen veel met meer dan één taal. Een land geven met een query fungeert voor het verfijnen van de zoekresultaten op basis van interesses in dat land. Bovendien worden de resultaten kunnen bevatten koppelingen naar Bing en deze koppelingen kunnen lokalisatie van de gebruikerservaring Bing volgens de opgegeven land of de taal.

U kunt opgeven van een land via de `cc` queryparameter. Als u een land opgeeft, moet u ook een of meer taalcodes met opgeven de `Accept-Language` HTTP-header. De ondersteunde talen verschillen per land; voor elk land in de tabel markten wordt verstrekt.

U kunt ook opgeven de markt met behulp van de `mkt` query parameter en een code van de **markten** tabel. Een markt tegelijkertijd opgeven, geeft een land en een voorkeurstaal. De `setLang` queryparameter kan worden ingesteld op een taalcode in dit geval; dit is meestal de dezelfde taal die is opgegeven door `mkt` tenzij de gebruiker geeft de voorkeur aan om te zien van Bing in een andere taal.

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
