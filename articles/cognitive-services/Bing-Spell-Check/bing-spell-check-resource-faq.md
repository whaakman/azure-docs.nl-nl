---
title: Veelgestelde vragen over de Bing Spell Check-API
titlesuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Bing Spell Check-API op Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: e6662ffcbab9ea274a67bc4437ca1600f1625ff1
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801502"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Veelgestelde vragen over de Bing Spell Check-API

 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Bing Spell Check-API voor Microsoft Cognitive Services op Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Hoe ontvang ik de headers van clientcertificaten desgewenst bij het aanroepen van de Bing Spell Check-API van JavaScript?

De volgende headers zijn optioneel, maar wij raden u ze behandelen als vereist. Deze headers helpen de Bing Spell Check-API meer nauwkeurige resultaten retourneren.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-client-IP

Echter, wanneer u de Bing Spell Check-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties die mogelijk voorkomen dat u toegang tot de waarden van deze headers.

U lost dit probleem, kunt u de Bing Spell Check-API-aanvraag via een proxy CORS. Het antwoord van deze proxy heeft een `Access-Control-Expose-Headers` header die antwoordheaders accounttoewijzing en maakt ze beschikbaar voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan de [zelfstudie app](tutorials/spellcheck.md) voor toegang tot de optionele client-headers. Ten eerste, als u dit nog niet, [Installeer Node.js](https://nodejs.org/en/download/). Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens de Bing Spell Check-API-eindpunt in de HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Ten slotte, start u de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster open terwijl u de zelfstudie app; gebruiken het venster sluit, stopt de proxy. In de uitbreidbare HTTP-Headers sectie hieronder de zoekresultaten, kunt u nu zien de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op de [UserVoice-website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
