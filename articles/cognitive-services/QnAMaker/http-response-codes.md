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
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d5b97f9c3b29732ba901f783ac04a3ada338ab69
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866437"
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
