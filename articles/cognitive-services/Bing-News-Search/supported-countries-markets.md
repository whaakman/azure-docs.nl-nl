---
title: Ondersteunde landen en talen voor Bing nieuws API van zoekservice in Azure | Microsoft Docs
description: Ontdek welke landen en talen worden ondersteund door de API van Bing installatiekopie zoeken.
services: cognitive-services
author: MikeDodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 12/20/2017
ms.author: v-gedod
ms.openlocfilehash: 80326d66e509b64efd5d386fe793bc9942b29ae3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344580"
---
# <a name="bing-news-search-countries-and-languages"></a>Bing nieuws zoeken landen en talen

De Bing nieuws zoeken-API ondersteunt meerdere landen kunnen veel met meer dan één taal. Een land geven met een query fungeert voor het verfijnen van de zoekresultaten op basis van interesses in dat land. Bovendien worden de resultaten kunnen bevatten koppelingen naar Bing en deze koppelingen kunnen lokalisatie van de gebruikerservaring Bing volgens de opgegeven land of de taal.

U kunt opgeven van een land via de `cc` queryparameter. Als u een land opgeeft, moet u ook een of meer taalcodes met opgeven de `Accept-Language` HTTP-header. De ondersteunde talen verschillen per land; voor elk land in de tabel markten wordt verstrekt.

U kunt ook opgeven de markt met behulp van de `mkt` query parameter en een code van de **markten** tabel. Een markt tegelijkertijd opgeven, geeft een land en een voorkeurstaal. De `setLang` queryparameter kan worden ingesteld op een taalcode in dit geval; dit is meestal de dezelfde taal die is opgegeven door `mkt` tenzij de gebruiker geeft de voorkeur aan om te zien van Bing in een andere taal.

## <a name="supported-markets-for-news-search-endpoint"></a>Ondersteunde markten voor het eindpunt van nieuws zoeken

Voor de `/news/search` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst kan worden gewijzigd.  
  
Voor een lijst met land codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  
  
|Land/regio|Taal|Markt code|  
|---------------------|--------------|-----------------| 
|Denemarken|Deens|da-DK|
|Oostenrijk|Duits|de AT| 
|Zwitserland|Duits|de CH|
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Canada|Nederlands|NL-CA|
|Verenigd Koninkrijk|Nederlands|NL GB|
|Indonesië|Nederlands|NL-ID|
|Ierland|Nederlands|NL-IE|
|India|Nederlands|NL-IN|
|Maleisië|Nederlands|NL mijn|
|Nieuw-Zeeland|Nederlands|NL NZ|
|Republiek der Filippijnen|Nederlands|NL-PH|
|Singapore|Nederlands|NL-SG|
|Verenigde Staten|Nederlands|nl-NL|
|Nederlands|Algemeen|NL WW|
|Nederlands|Algemeen|NL XA|
|Zuid-Afrika|Nederlands|NL-ZA|
|Argentinië|Spaans|ES-AR|
|Chili|Spaans|ES-CL|
|Spanje|Spaans|es-ES|
|Mexico|Spaans|es-MX|
|Verenigde Staten|Spaans|ES VS| 
|Spaans|Algemeen|ES Excel|
|Finland|Fins|fi-FI|  
|Frankrijk|Frans|FR-worden|
|Canada|Frans|fr-CA| 
|België|Nederlands|NL-worden|
|Zwitserland|Frans|FR CH|
|Frankrijk|Frans|fr-FR|  
|Italië|Italiaans|IT-IT| 
|Hongkong SAR|Traditioneel Chinees|zh-HK|  
|Taiwan|Traditioneel Chinees|zh-TW.|
|Japan|Japans|ja-JP|  
|Korea|Koreaans|ko-KR|  
|Nederland|Nederlands|NL-NL|  
|Volksrepubliek China|Chinees|zh-CN|  
|Brazilië|Portugees|pt-BR|
|Rusland|Russisch|ru-RU|  
|Zweden|Zweeds|SV-SE|  
|Turkije|Turks|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Ondersteunde markten voor nieuws eindpunt
Voor de `/news` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst kan worden gewijzigd.  
  
Voor een lijst met land codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  
  
|Land/regio|Taal|Markt code|  
|---------------------|--------------|-----------------| 
|Denemarken|Deens|da-DK|
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Verenigd Koninkrijk|Nederlands|NL GB|
|Verenigde Staten|Nederlands|nl-NL|
|Nederlands|Algemeen|NL WW|
|Chili|Spaans|ES-CL|
|Mexico|Spaans|es-MX|
|Verenigde Staten|Spaans|ES VS| 
|Finland|Fins|fi-FI|  
|Canada|Frans|fr-CA|
|Frankrijk|Frans|fr-FR|  
|Italië|Italiaans|IT-IT| 
|Brazilië|Portugees|pt-BR|
|Volksrepubliek China|Chinees|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Ondersteunde markten voor nieuws trends eindpunt
Voor de `/news/trendingtopics` eindpunt, de volgende tabel bevat de markt code-waarden die u gebruiken kunt om op te geven de `mkt` queryparameter. Bing retourneert de inhoud voor alleen deze markten. De lijst kan worden gewijzigd.  
  
Voor een lijst met land codes die u kunt opgeven de `cc` queryparameter, Zie [landcodes](#countrycodes).  
  
|Land/regio|Taal|Markt code|  
|---------------------|--------------|-----------------| 
|Duitsland|Duits|de-DE|
|Australië|Nederlands|en-AU|
|Verenigd Koninkrijk|Nederlands|NL GB|
|Verenigde Staten|Nederlands|nl-NL|
|Canada|Nederlands|NL-CA|
|India|Nederlands|NL-IN|
|Frankrijk|Frans|fr-FR|
|Canada|Frans|fr-CA|
|Brazilië|Portugees|pt-BR|
|Volksrepubliek China|Chinees|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Landcodes  

Hieronder vindt u de landcodes die u kunt opgeven in de `cc` queryparameter. De lijst kan worden gewijzigd.  
  
|Land/regio|Landnummer|  
|---------------------|------------------|  
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
|Hongkong SAR|HK|  
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
|Volksrepubliek China|ALGEMENE NAAM|  
|Polen|PL|  
|Portugal|PT|  
|Republiek der Filippijnen|PH|  
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

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de eindpunten Bing nieuws zoeken [nieuws Search API v7-verwijzingen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).