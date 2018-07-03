---
title: Language Understanding (LUIS)-regio's | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat een lijst met de LUIS-regio's voor de website LUIS, Azure-abonnementen en regio's ter wereld.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: d81fbc03689788066fb9275523a5e96647117c58
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37346441"
---
# <a name="regions-and-keys"></a>Regio's en sleutels

De regio waarin u uw LUIS-app publiceren overeenkomt met de regio of locatie die u in de Azure-portal opgeeft wanneer u een Azure LUIS-eindpuntsleutel maakt. Wanneer u [een app publiceren](./luis-how-to-publish-app.md), LUIS genereert automatisch een eindpunt-URL voor de regio die is gekoppeld aan de sleutel. Als u wilt een LUIS-app publiceren in meer dan één regio, moet u ten minste één sleutel per regio. 

## <a name="luis-website"></a>LUIS-website
Er zijn drie LUIS-websites, op basis van regio. U moet maken en publiceren in dezelfde regio. 

|LUIS|Regio|
|--|--|
|[www.Luis.AI][www.luis.ai]|V.S.<br>niet-Europa<br>niet-Australië|
|[AU.Luis.AI][au.luis.ai]|Australië|
|[EU.Luis.AI][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Publicatie-regio 's

LUIS-apps die zijn gemaakt op https://www.luis.ai kunnen worden gepubliceerd naar alle eindpunten, behalve de [Europese](#publishing-to-europe) en [Australische](#publishing-to-australia) regio's. 

De ontwerphandleiding regio app kan alleen worden gepubliceerd naar een bijbehorende regio publiceren. Als uw app zich momenteel in de verkeerde regio voor schrijven bevindt, de app exporteren en importeren in de juiste authoring regio voor uw regio publiceren. 

 Globale regio | Regio ontwerpen | Publiceren en uitvoeren van query's regio   |   LUIS-website | Eindpunt-URL-indeling   |
|-----|------|------|------|------|
| Azië | VS - west| Oost-Azië     | [www.Luis.AI][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | VS - west| Zuidoost-Azië     | [www.Luis.AI][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australië](#publishing-to-australia) | Australië - oost| Australië - oost     |   [AU.Luis.AI][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| West-Europa| Noord-Europa     | [EU.Luis.AI][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | West-Europa| West-Europa     | [EU.Luis.AI][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | VS - west | VS - oost      |[www.Luis.AI][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | VS - west | VS - oost 2     | [www.Luis.AI][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | VS - west | Zuid-centraal VS     | [www.Luis.AI][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | VS - west | West-centraal VS     |[www.Luis.AI][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | VS - west | VS - west |  [www.Luis.AI][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Noord-Amerika | VS - west | VS - west 2    | [www.Luis.AI][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Zuid-Amerika | VS - west | Brazilië - zuid     | [www.Luis.AI][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Publiceren naar Europa

Als u wilt publiceren naar de Europese regio's, u LUIS apps maken op https://eu.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de regio Europa, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://eu.luis.ai. LUIS-apps die zijn gemaakt op [ https://eu.luis.ai ] [ eu.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="publishing-to-australia"></a>Publiceren naar Australië

Als u wilt publiceren naar de Australische regio's, u LUIS apps maken op https://au.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de Australische regio, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://au.luis.ai. LUIS-apps die zijn gemaakt op [ https://au.luis.ai ] [ au.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de vooraf gemaakte entiteiten](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai