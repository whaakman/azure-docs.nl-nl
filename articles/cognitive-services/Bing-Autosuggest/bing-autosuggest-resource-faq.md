---
title: Veelgestelde vragen (FAQ) over Azure-API voor Automatische suggestie | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Azure cognitieve Services voor Automatische suggestie API in Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 00b91728bcfec52ff30697f080d5c2619bab79a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345438"
---
# <a name="frequently-asked-questions-faq-about-autosuggest-api-cognitive-services"></a>Veelgestelde vragen (FAQ) over Automatische suggestie van API (cognitieve Services)
 
 Antwoorden op veelgestelde vragen over de concepten, code en scenario's die zijn gerelateerd aan de API voor Automatische suggestie voor cognitieve Azure-Services.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Hoe krijg ik-headers van de client optioneel bij het aanroepen van de API voor Automatische suggestie van Bing van JavaScript?

De volgende headers zijn optioneel, maar we raden u ze behandelen als vereist. Deze koppen helpen de Bing voor Automatische suggestie van API-nauwkeurige resultaten geretourneerd.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-client-IP

Wanneer u de Automatische suggestie van Bing-API vanuit JavaScript aanroept, ingebouwde beveiligingsfuncties van uw browser mogelijk kunt u echter toegang krijgen tot de waarden van de headers.

U kunt dit oplossen kunt u de Automatische suggestie van Bing-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers` header die whitelists antwoordheaders en deze beschikbaar voor JavaScript.

Het is gemakkelijk voor het installeren van een proxy CORS om toe te staan onze [zelfstudie app](tutorials/autosuggest.md) voor toegang tot de optionele client-headers. Eerste, als u dit nog niet hebt [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Wijzig vervolgens het eindpunt voor Automatische suggestie van Bing-API in het HTML-bestand in:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Ten slotte de CORS-proxy starten met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken u sluit het venster stopt de proxy. In de uitbreidbare HTTP-Headers gedeelte onder de zoekresultaten, nu ziet u de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

- [Stack-overloop: Cognitieve Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
