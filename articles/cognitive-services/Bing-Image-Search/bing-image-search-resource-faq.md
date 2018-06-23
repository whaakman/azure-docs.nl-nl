---
title: Veelgestelde vragen (FAQ) over Bing zoeken naar afbeelding API in Azure | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Microsoft cognitieve Services Bing installatiekopie zoeken-API in Azure.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 15d3def94312fbde0bf3443cba54edfacafa7ab3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344407"
---
# <a name="frequently-asked-questions-faq-about-bing-image-search-api-cognitive-services"></a>Veelgestelde vragen (FAQ) over Bing installatiekopie zoeken-API (cognitieve Services)
 
Antwoorden op veelgestelde vragen over de concepten, code en scenario's die zijn gerelateerd aan de installatiekopie Search-API van Bing voor cognitieve Microsoft-Services in Azure.

## <a name="response-headers-in-javascript"></a>Antwoordheaders in JavaScript

De volgende headers optreden in antwoorden van de API van Bing installatiekopie zoeken.

|||
|-|-|
|`X-MSEdge-ClientID`|De unieke ID die Bing is toegewezen aan de gebruiker|
|`BingAPIs-Market`|De markt, die is gebruikt om te voldoen aan het verzoek|
|`BingAPIs-TraceId`|De vermelding op de server Bing-API voor deze aanvraag (voor ondersteuning)|

Het is vooral belangrijk voor het behouden van de client-ID en het terugzetten met de volgende aanvragen. Als u dit doet, wordt de zoekopdracht voorbij context in zoekresultaten rangschikking gebruiken en bieden ook een consistente gebruikerservaring.

Wanneer u de Bing installatiekopie zoeken-API vanuit JavaScript aanroept, ingebouwde beveiligingsfuncties van uw browser (CORS) mogelijk kunt u echter toegang krijgen tot de waarden van de headers.

Als u wilt toegang krijgen tot de headers, kunt u de Bing installatiekopie zoeken-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers` header die whitelists antwoordheaders en deze beschikbaar voor JavaScript.

Het is gemakkelijk voor het installeren van een proxy CORS om toe te staan onze [zelfstudie app](tutorial-bing-image-search-single-page-app.md) voor toegang tot de optionele client-headers. Eerste, als u dit nog niet hebt [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Wijzig vervolgens het Bing installatiekopie zoeken-API-eindpunt in het HTML-bestand in:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Ten slotte de CORS-proxy starten met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken u sluit het venster stopt de proxy. In de uitbreidbare HTTP-Headers gedeelte onder de zoekresultaten, nu ziet u de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="response-headers-in-production"></a>Antwoordheaders in productie

De CORS-proxy-benadering wordt beschreven in het vorige antwoord is geschikt voor ontwikkeling, testen en leren. 

In een productieomgeving, moet u echter een script serverzijde in hetzelfde domein als de webpagina die gebruikmaakt van de Web zoeken-API van Bing hosten. Het script moet daadwerkelijk komen de API-aanroepen op verzoek van de webpagina JavaScript en geeft alle resultaten, inclusief de kopteksten, terug naar de client. Omdat de twee resources (pagina en script) een oorsprong delen, CORS komen niet in het spel en de speciale headers zijn acessible naar JavaScript op de webpagina. 

Deze aanpak ook beveiligt uw API-sleutel van blootstelling aan de openbare omdat alleen het script op de server nodig heeft. Het script kunt u een andere methode (zoals de HTTP-verwijzende site) gebruiken om ervoor te zorgen dat de aanvraag is geautoriseerd.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Zie ook

 [Stack-overloop: Cognitieve Services](http://stackoverflow.com/questions/tagged/bing-api)