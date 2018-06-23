---
title: Veelgestelde vragen over de Bing Spell selectievakje API - cognitieve Azure-Services | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over de Bing-API voor het controleren van spellen op Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 87b1f3ed3e0aaa9f3c3c804dc9eac3ee60b4a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345596"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Veelgestelde vragen over de spellingcontrole van Bing API controleren

 Antwoorden op veelgestelde vragen over de concepten, code en scenario's die zijn gerelateerd aan de Bing-API voor het controleren van spellen voor cognitieve Microsoft-Services in Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Hoe krijg ik-headers van de client optioneel bij het aanroepen van de Bing-API voor het controleren van spellen van JavaScript?

De volgende headers zijn optioneel, maar we raden u ze behandelen als vereist. Deze koppen helpen de Bing spellen controleren API meer nauwkeurige resultaten geretourneerd.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-client-IP

Wanneer u de Bing-API voor het controleren van spellen vanuit JavaScript aanroept, ingebouwde beveiligingsfuncties van uw browser mogelijk kunt u echter toegang krijgen tot de waarden van de headers.

U lost dit probleem, kunt u de Bing spellen controleren API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers` header die whitelists antwoordheaders en deze beschikbaar voor JavaScript.

Het is gemakkelijk voor het installeren van een proxy CORS om toe te staan de [zelfstudie app](tutorials/spellcheck.md) voor toegang tot de optionele client-headers. Eerste, als u dit nog niet hebt [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Wijzig vervolgens de Bing spellen controleren API-eindpunt in het HTML-bestand in:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Ten slotte de CORS-proxy starten met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken u sluit het venster stopt de proxy. In de uitbreidbare HTTP-Headers gedeelte onder de zoekresultaten, nu ziet u de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op de [UserVoice website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Cognitieve Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
