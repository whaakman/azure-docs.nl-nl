---
title: Understanding (LUIS) API HTTP-antwoord taalcodes - Azure | Microsoft Docs
titleSuffix: Azure
description: Inzicht in welke HTTP-antwoord-codes zijn geretourneerd door de LUIS ontwerp- en eindpunt API 's
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: v-geberr
ms.openlocfilehash: 9c7381d9dc2ecf302c85c6b4f1f24b24a28d9ebe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344848"
---
# <a name="luis-api-http-response-codes"></a>LUIS API HTTP-reactiecodes
De [ontwerpen](https://aka.ms/luis-authoring-apis) en [eindpunt](https://aka.ms/luis-endpoint-apis) API's HTTP-antwoord retourcodes. Terwijl antwoordberichten informatie die specifiek zijn voor een aanvraag bevatten, is de HTTP-statuscode van antwoord algemeen. 

## <a name="common-status-codes"></a>Algemene statuscodes
De volgende tabel vindt u enkele van de meest voorkomende HTTP-antwoord statuscodes voor met de [ontwerpen](https://aka.ms/luis-authoring-apis) en [eindpunt](https://aka.ms/luis-endpoint-apis) API's:

|Code|API|Uitleg|
|:--|--|--|
|400|Ontwerpen, eindpunt|de aanvraag-parameters zijn onjuist wat betekent dat de vereiste parameters ontbreken, ongeldig of zijn te groot zijn|
|400|Ontwerpen, eindpunt|de aanvraagtekst is onjuist wat betekent dat de JSON ontbreken, ongeldig of zijn te groot|
|401|Ontwerpen|abonnementssleutel eindpunt, gebruikt in plaats van de sleutel ontwerpen|
|401|Ontwerpen, eindpunt|de sleutel ongeldig, leeg of onjuist gevormd|
|401|Ontwerpen, eindpunt| sleutel komt niet overeen met de regio|
|401|Ontwerpen|u bent niet de eigenaar of een medewerker|
|401|Ontwerpen|Ongeldige volgorde van de API-aanroepen|
|403|Ontwerpen, eindpunt|totale maandelijkse sleutel quotum is overschreden|
|409|Eindpunt|toepassing wordt nog steeds geladen.|
|410|Eindpunt|toepassing moet worden retrained en gepubliceerd|
|414|Eindpunt|query overschrijdt de toegestane aantal tekens|
|429|Ontwerpen, eindpunt|Frequentielimiet is overschreden (aanvragen/seconde)|