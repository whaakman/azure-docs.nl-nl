---
title: Geografische grenzen gebruiken voor het filteren van resultaten van de Bing API voor zoeken van lokale bedrijven | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel voor informatie over het filteren van resultaten van de Bing API voor zoeken van lokale bedrijven.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 5861ec60790cbdbf1144996b1d46eb223d5935d4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158023"
---
# <a name="use-geographic-boundaries-to-filter-results-from-the-bing-local-business-search-api"></a>Geografische grenzen gebruiken voor het filteren van resultaten van de Bing API voor zoeken van lokale bedrijven

De Bing lokale bedrijven zoeken-API kunt u grenzen instellen op de specifieke geografische regio die u zoeken wilt met behulp van de `localCircularView` of `localMapView` queryparameters. Zorg ervoor dat slechts één parameter gebruiken in uw query's. 

Als u een zoekterm een expliciete geografische locatie bevat, worden de lokale Business-API voor Bing automatisch gebruikt deze om in te stellen grenzen voor de lijst met zoekresultaten. Bijvoorbeeld, als de zoekterm is `sailing in San Diego`, klikt u vervolgens `San Diego` wordt gebruikt als de locatie en andere locaties in de queryparameters opgegeven of gebruiker headers worden genegeerd. 

Als een geografische locatie is niet in de zoekterm gevonden en geen geografische locatie wordt opgegeven met behulp van de queryparameters, probeert de Bing lokale bedrijven zoeken-API om te bepalen van de locatie van de aanvraag `X-Search-ClientIP` of `X-Search-Location` headers. Als geen van beide header is opgegeven, de locatie van de client-IP van de aanvraag wordt bepaald door de API of GPS-coördinaten voor mobiele apparaten.

## <a name="localcircularview"></a>localCircularView

De `localCircularView` parameter maakt u een circulaire geografische gebied rond een set van breedtegraad/lengtegraad coördinaten, gedefinieerd door een radius. Wanneer u deze parameter gebruikt, antwoorden van de Bing API voor zoeken van lokale bedrijven alleen bevat locaties binnen deze cirkel, in tegenstelling tot de `localMapView` parameter waaronder locaties iets buiten het zoekgebied.

Als u een circulaire geografische zoekgebied, kies een breedtegraad en lengtegraad om te fungeren als het midden van de cirkel en een radius in meters. Deze parameter kan vervolgens worden toegevoegd aan een querytekenreeks, bijvoorbeeld: `q=Restaurants&localCircularView=47.6421,-122.13715,5000`.

Volledige query:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localCircularView=47.6421,-122.13715,5000&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="localmapview"></a>localMapView

De `localMapView` parameter geeft u een rechthoekig geografische gebied te zoeken, met behulp van twee sets coördinaten van de Zuidoost en noordwest hoeken op te geven. Wanneer u deze parameter gebruikt, antwoorden van de Bing API voor zoeken van lokale bedrijven locaties binnen en buiten het opgegeven gebied kunnen bevatten, in tegenstelling tot de `localCircularView` parameter, die alleen locaties binnen het zoekgebied bevat.

Als u een rechthoekige zoekgebied, twee sets breedtegraad/lengtegraad coördinaten om te fungeren als de Zuidoost en noordwest hoeken van de grens te kiezen. Zorg ervoor dat u definieert de Zuidoost coördinaten eerst, zoals in het volgende voorbeeld: `localMapView=47.619987,-122.181671,47.6421,-122.13715`.

Volledige query:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localMapView=47.619987,-122.181671,47.6421,-122.13715&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="next-steps"></a>Volgende stappen
- [Lokale bedrijven zoeken Java-snelstartgids](quickstarts/local-search-java-quickstart.md)
- [Lokale bedrijven zoeken C# Quick Start](quickstarts/local-quickstart.md)
- [Lokale bedrijven zoeken Node-Quickstart](quickstarts/local-search-node-quickstart.md)
- [Local Business Search Python quickstart](quickstarts/local-search-python-quickstart.md)
