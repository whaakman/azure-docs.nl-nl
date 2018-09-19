---
title: Veelgestelde vragen (FAQ) - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Bing webzoekopdrachten-API.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: troubleshooting
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: b0b8fd3eb80d7418546788565402042de20ab3e7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129315"
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

Om toegang te krijgen tot de headers, kunt u de Bing webzoekopdrachten-API-aanvraag via een proxy CORS. Het antwoord van deze proxy heeft een `Access-Control-Expose-Headers` header die antwoordheaders accounttoewijzing en maakt ze beschikbaar voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](tutorial-bing-web-search-single-page-app.md) voor toegang tot de optionele client-headers. Ten eerste, als u dit nog niet, [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens de Bing webzoekopdrachten-API-eindpunt in de HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Ten slotte, start u de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken het venster sluit, stopt de proxy. In de uitbreidbare HTTP-Headers sectie hieronder de zoekresultaten, kunt u nu zien de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="response-headers-in-production"></a>Antwoordheaders in productie

De CORS-proxy-benadering beschreven in het vorige antwoord is geschikt voor ontwikkeling, testen en leren.

In een productieomgeving, moet u echter een script op de server in hetzelfde domein als de webpagina die gebruikmaakt van de Bing webzoekopdrachten-API hosten. Met dit script moet daadwerkelijk uitvoeren van de API-aanroepen op verzoek van de webpagina JavaScript en alle resultaten, met inbegrip van headers, terug naar de client. Aangezien de twee resources (pagina en script) een oorsprong delen, CORS wordt niet geleverd aan de orde en de speciale headers zijn acessible aan de JavaScript op de webpagina wordt weergegeven.

Deze benadering ook voorkomt dat uw API-sleutel blootstelling aan het publiek, omdat alleen het script op de server nodig heeft. Het script kunt u een andere methode gebruiken om te controleren of dat de aanvraag is geautoriseerd.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Zie ook

 [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/bing-api)
