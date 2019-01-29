---
title: Veelgestelde vragen over de Bing Spell Check-API
titlesuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Bing Spell Check-API op Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 90acc80b19058c2a7eb963f9e423883846519b2c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164925"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Veelgestelde vragen over de Bing Spell Check-API

 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Bing Spell Check-API voor Microsoft Cognitive Services op Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Hoe ontvang ik de headers van clientcertificaten desgewenst bij het aanroepen van de Bing Spell Check-API van JavaScript?

De volgende headers zijn optioneel, maar wij raden u ze behandelen als vereist. Deze headers helpen de Bing Spell Check-API meer nauwkeurige resultaten retourneren.

- X-Search-locatie
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

Echter, wanneer u de Bing Spell Check-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties die mogelijk voorkomen dat u toegang tot de waarden van deze headers.

U lost dit probleem, kunt u de Bing Spell Check-API-aanvraag via een proxy CORS. Het antwoord van deze proxy heeft een `Access-Control-Expose-Headers` header die antwoordheaders accounttoewijzing en maakt ze beschikbaar voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan de [zelfstudie app](tutorials/spellcheck.md) voor toegang tot de optionele client-headers. Als u [Node.js](https://nodejs.org/en/download/) nog niet hebt, moet u dit eerst installeren. Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens de Bing Spell Check-API-eindpunt in de HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Start ten slotte de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster geopend terwijl u de zelfstudie-app gebruikt. Als u het venster sluit, wordt de proxy gestopt. In de uitbreidbare HTTP-Headers sectie hieronder de zoekresultaten, kunt u nu zien de `X-MSEdge-ClientID` header (onder andere) en controleer of het is hetzelfde voor elke aanvraag.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op de [UserVoice-website](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
