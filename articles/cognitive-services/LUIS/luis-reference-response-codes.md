---
title: API-HTTP-responscodes
titleSuffix: Azure
description: Inzicht in welke HTTP-responscodes zijn geretourneerd door de LUIS ontwerpen en de eindpunt-API 's
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: f6742bf64ce26e6cce93dfcdfd06756f3c340d9e
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522973"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Algemene API-responscodes en hun betekenis

De [ontwerpen](https://go.microsoft.com/fwlink/?linkid=2092087) en [eindpunt](https://go.microsoft.com/fwlink/?linkid=2092356) API's geven de HTTP-responscodes. Terwijl antwoordberichten informatie die specifiek zijn voor een aanvraag bevatten, is de HTTP-antwoordstatuscode algemeen. 

## <a name="common-status-codes"></a>Algemene statuscodes
De volgende tabel bevat enkele van de meest voorkomende HTTP-antwoord-statuscodes voor de [ontwerpen](https://go.microsoft.com/fwlink/?linkid=2092087) en [eindpunt](https://go.microsoft.com/fwlink/?linkid=2092356) API's:

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

## <a name="next-steps"></a>Volgende stappen

* REST-API [ontwerpen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) en [eindpunt](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) documentatie
