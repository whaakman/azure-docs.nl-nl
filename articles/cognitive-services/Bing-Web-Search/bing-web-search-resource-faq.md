---
title: Veelgestelde vragen (FAQ) - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over het gebruik van de Bing webzoekopdrachten-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: troubleshooting
ms.date: 10/06/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: d11165b22665eec09e5025a4daa5ecd1171b7d80
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159026"
---
# <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen

 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Bing webzoekopdrachten-API voor Microsoft Cognitive Services op Azure.

## <a name="response-headers-in-javascript"></a>Antwoordheaders in JavaScript

De volgende headers kunnen optreden in de reacties van de Bing webzoekopdrachten-API.

|||
|-|-|
|`X-MSEdge-ClientID`|De unieke ID die Bing is toegewezen aan de gebruiker|
|`BingAPIs-Market`|De markt die is gebruikt om te voldoen aan de aanvraag|
|`BingAPIs-TraceId`|De vermelding op de server-API voor Bing voor deze aanvraag (voor ondersteuning)|

Dit is vooral belangrijk voor het behouden van de client-ID en retourneer het met de volgende aanvragen. Als u dit doet, wordt de zoekopdracht gebruikt voorbij context in volgorde van zoekresultaten en bieden ook een consistente gebruikerservaring.

Echter, wanneer u de Bing webzoekopdrachten-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties (CORS) kunnen voorkomen dat u toegang tot de waarden van deze headers.

Om toegang te krijgen tot de headers, kunt u de Bing webzoekopdrachten-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers`-header waardoor antwoordheaders worden opgenomen in de whitelist en beschikbaar gemaakt voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](tutorial-bing-web-search-single-page-app.md) voor toegang tot de optionele client-headers. Als u [Node.js](https://nodejs.org/en/download/) nog niet hebt, moet u dit eerst installeren. Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens de Bing webzoekopdrachten-API-eindpunt in de HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Start ten slotte de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster geopend terwijl u de zelfstudie-app gebruikt. Als u het venster sluit, wordt de proxy gestopt. In de uitbreidbare sectie met HTTP-headers onder de zoekresultaten ziet u nu (onder andere) de `X-MSEdge-ClientID`-header en kunt u controleren of deze voor elke aanvraag gelijk is.

## <a name="response-headers-in-production"></a>Antwoordheaders in productie

De CORS-proxy-benadering beschreven in het vorige antwoord is geschikt voor ontwikkeling, testen en leren.

In een productieomgeving, moet u echter een script op de server in hetzelfde domein als de webpagina die gebruikmaakt van de Bing webzoekopdrachten-API hosten. Met dit script moet daadwerkelijk uitvoeren van de API-aanroepen op verzoek van de webpagina JavaScript en alle resultaten, met inbegrip van headers, terug naar de client. Aangezien de twee resources (pagina en script) een oorsprong delen, CORS wordt niet geleverd aan de orde en de speciale headers zijn toegankelijk voor de JavaScript op de webpagina wordt weergegeven.

Deze benadering ook voorkomt dat uw API-sleutel blootstelling aan het publiek, omdat alleen het script op de server nodig heeft. Het script kunt u een andere methode gebruiken om te controleren of dat de aanvraag is geautoriseerd.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Zie ook

 [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/bing-api)
