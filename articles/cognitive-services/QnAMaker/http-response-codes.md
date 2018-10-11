---
title: API-HTTP-responscodes - QnA Maker
titleSuffix: Azure Cognitive Services
description: Inzicht in welke HTTP-responscodes worden geretourneerd van de QnA Maker-API 's
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: 96782d32817e4989c02e0ed098d7772c80aa26c8
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079679"
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