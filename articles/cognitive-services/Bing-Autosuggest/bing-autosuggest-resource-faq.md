---
title: Frequently Asked Questions (FAQ) - Bing Automatische suggestie-API
titlesuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Automatische suggestie-API.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: b0ec10bbf03e8a8d005eece4b6496b74b2943233
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173255"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Veelgestelde vragen (FAQ) over Bing Automatische suggestie-API
 
 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Automatische suggestie-API voor Azure Cognitive Services.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Hoe ontvang ik de headers van clientcertificaten desgewenst bij het aanroepen van de Automatische suggestie-API van JavaScript?

De volgende headers zijn optioneel, maar wij raden u ze behandelen als vereist. Deze headers helpen bij de Automatische suggestie-API voor Bing meer nauwkeurige resultaten worden geretourneerd.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

Echter, wanneer u de Automatische suggestie-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties die mogelijk voorkomen dat u toegang tot de waarden van deze headers.

U kunt dit oplossen kunt u de Automatische suggestie-API-aanvraag via een proxy CORS. Het antwoord van deze proxy heeft een `Access-Control-Expose-Headers` header die antwoordheaders accounttoewijzing en maakt ze beschikbaar voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](tutorials/autosuggest.md) voor toegang tot de optionele client-headers. Als u [Node.js](https://nodejs.org/en/download/) nog niet hebt, moet u dit eerst installeren. Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens het Bing Automatische suggestie-API-eindpunt in het HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Start ten slotte de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster geopend terwijl u de zelfstudie-app gebruikt. Als u het venster sluit, wordt de proxy gestopt. In de uitbreidbare sectie met HTTP-headers onder de zoekresultaten ziet u nu (onder andere) de `X-MSEdge-ClientID`-header en kunt u controleren of deze voor elke aanvraag gelijk is.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [User Voice-website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

- [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
