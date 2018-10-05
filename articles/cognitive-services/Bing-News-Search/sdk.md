---
title: Zoeken in Bing nieuws SDK
titleSuffix: Azure Cognitive Services
description: Bing nieuws zoeken-SDK voor toepassingen die op het web zoeken.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: b1d9eaa35416adfa11647f2116171256f82fe095
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801230"
---
# <a name="bing-search-sdk"></a>Zoeken in Bing SDK
De Bing nieuws zoeken-API-voorbeelden vindt u scenario's die:
1. Nieuws op te vragen voor zoektermen met `market` en `count` parameters, Controleer of het aantal resultaten en afdrukken `totalEstimatedMatches`, naam, URL, beschrijving, Publicatietijd en de naam van de provider van het eerste nieuws-resultaat.
2. Meest recent nieuws op te vragen voor zoektermen met `freshness` en `sortBy` parameters, Controleer of het aantal resultaten en afdrukken `totalEstimatedMatches`, URL, beschrijving, Publicatietijd en de naam van de provider van het eerste nieuws-resultaat.
3. Query uitvoeren op categorie nieuws voor `movie` en `TV entertainment` met veilig zoeken, het aantal resultaten controleren en afdrukken categorie, naam, URL, beschrijving, Publicatietijd en de naam van de provider van het eerste nieuws-resultaat.
4. Query actuele onderwerpen in Bing, nieuws aantal resultaten controleren en de naam, de tekst van de query, afdrukken `webSearchUrl`, `newsSearchUrl` en afbeelding van de URL van het eerste nieuws-resultaat.

De Bing zoeken-SDK's moet u de functionaliteit voor het web zoeken gemakkelijk toegankelijk zijn in de volgende programmeertalen:
* Aan de slag met [.NET-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Zie ook [.NET-bibliotheken](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) voor definities en afhankelijkheden.
* Aan de slag met [Node.js-voorbeelden](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Zie ook [Node.js clientbibliotheken](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) voor definities en afhankelijkheden.
* Aan de slag met [Java-voorbeelden](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Zie ook [Java-bibliotheken](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) voor definities en afhankelijkheden.
* Aan de slag met [voorbeelden van Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Zie ook [Python-bibliotheken](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) voor definities en afhankelijkheden.

SDK-voorbeelden voor elke taal omvatten een Leesmij-bestand met informatie over vereisten en installatie/uitvoeren van de voorbeelden.