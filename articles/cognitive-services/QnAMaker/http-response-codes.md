---
title: API-HTTP-responscodes - QnA Maker
titleSuffix: Azure Cognitive Services
description: Krijg inzicht in welke HTTP-responscodes worden geretourneerd van de QnA Maker-API's. Dit helpt u los eventuele fouten.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 319623487f37308b5b8fe3fd107b01733825184d
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454715"
---
# <a name="qna-maker-api-http-response-codes"></a>QnA Maker API HTTP-responscodes
De [management](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) en voorspelling API's retourneren HTTP-responscodes. Terwijl antwoordberichten informatie die specifiek zijn voor een aanvraag bevatten, is de HTTP-antwoordstatuscode algemeen. 

### <a name="management"></a>Beheer

|Code|Uitleg|
|:--|--|
|2xx|Geslaagd|
|400|de aanvraagparameters zijn onjuist wat betekent dat de vereiste parameters ontbreken, onjuist gevormd of te groot zijn|
|400|de aanvraagtekst is onjuist wat betekent dat de JSON is ontbreekt, is onjuist gevormd of te groot|
|401|Ongeldige sleutel|
|403|Het is niet toegestaan - bent u niet gemachtigd juiste|
|404|KB bestaat niet|
|410|Deze API is afgeschaft en is niet meer beschikbaar|
