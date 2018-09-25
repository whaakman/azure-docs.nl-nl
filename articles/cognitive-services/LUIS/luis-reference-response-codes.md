---
title: Language Understanding (LUIS) API HTTP-responscodes - Azure | Microsoft Docs
titleSuffix: Azure
description: Inzicht in welke HTTP-responscodes zijn geretourneerd door de LUIS ontwerpen en de eindpunt-API 's
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: 07982716ff655f980844ca3dbcf7e8d7df1827c4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031217"
---
# <a name="luis-api-http-response-codes"></a>LUIS API HTTP-responscodes
De [ontwerpen](https://aka.ms/luis-authoring-apis) en [eindpunt](https://aka.ms/luis-endpoint-apis) API's geven de HTTP-responscodes. Terwijl antwoordberichten informatie die specifiek zijn voor een aanvraag bevatten, is de HTTP-antwoordstatuscode algemeen. 

## <a name="common-status-codes"></a>Algemene statuscodes
De volgende tabel bevat enkele van de meest voorkomende HTTP-antwoord-statuscodes voor de [ontwerpen](https://aka.ms/luis-authoring-apis) en [eindpunt](https://aka.ms/luis-endpoint-apis) API's:

|Code|API|Uitleg|
|:--|--|--|
|400|Ontwerpen, eindpunt|de aanvraagparameters zijn onjuist wat betekent dat de vereiste parameters ontbreken, onjuist gevormd of te groot zijn|
|400|Ontwerpen, eindpunt|de aanvraagtekst is onjuist wat betekent dat de JSON is ontbreekt, is onjuist gevormd of te groot|
|401|Ontwerpen|eindpuntsleutel-abonnement, gebruikt in plaats van de sleutel ontwerpen|
|401|Ontwerpen, eindpunt|sleutel ongeldig, ongeldig of leeg zijn|
|401|Ontwerpen, eindpunt| sleutel komt niet overeen met de regio|
|401|Ontwerpen|u bent niet de eigenaar of de samenwerker|
|401|Ontwerpen|Ongeldige volgorde van de API-aanroepen|
|403|Ontwerpen, eindpunt|totale maandelijkse sleutel quotum overschreden|
|409|Eindpunt|toepassing is geladen.|
|410|Eindpunt|toepassing moet worden retrained en gepubliceerd|
|414|Eindpunt|query groter is dan het maximum aantal tekens|
|429|Ontwerpen, eindpunt|Limiet wordt overschreden (aanvragen/seconde)|