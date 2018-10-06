---
title: Frequently Asked Questions (FAQ) - Bing Automatische suggestie-API
titlesuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Automatische suggestie-API.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 84f1b0555922119e9de4addc3d51ac233e7bae65
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831352"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Veelgestelde vragen (FAQ) over Bing Automatische suggestie-API
 
 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Automatische suggestie-API voor Azure Cognitive Services.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Hoe ontvang ik de headers van clientcertificaten desgewenst bij het aanroepen van de Automatische suggestie-API van JavaScript?

De volgende headers zijn optioneel, maar wij raden u ze behandelen als vereist. Deze headers helpen bij de Automatische suggestie-API voor Bing meer nauwkeurige resultaten worden geretourneerd.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-client-IP

Echter, wanneer u de Automatische suggestie-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties die mogelijk voorkomen dat u toegang tot de waarden van deze headers.

U kunt dit oplossen kunt u de Automatische suggestie-API-aanvraag via een proxy CORS. Het antwoord van deze proxy heeft een `Access-Control-Expose-Headers` header die antwoordheaders accounttoewijzing en maakt ze beschikbaar voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](tutorials/autosuggest.md) voor toegang tot de optionele client-headers. Ten eerste, als u dit nog niet, [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens het Bing Automatische suggestie-API-eindpunt in het HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Ten slotte, start u de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken het venster sluit, stopt de proxy. In de uitbreidbare HTTP-Headers sectie hieronder de zoekresultaten, kunt u nu zien de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

- [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
