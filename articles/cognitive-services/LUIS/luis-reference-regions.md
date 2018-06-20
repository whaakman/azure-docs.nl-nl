---
title: Language Understanding (LUIS) gebieden | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat een lijst met de LUIS regio's voor de website LUIS, Azure-abonnementen en world regio's.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 86a20770178707f72cf2991ca08b6b98eaeaf0cf
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36237003"
---
# <a name="regions-and-keys"></a>Regio's en sleutels

De regio waarin u uw app LUIS publiceren overeenkomt met de regio of de locatie die u in de Azure portal opgeeft bij het maken van een Azure-LUIS endpoint-sleutel. Wanneer u [een app publiceren](./PublishApp.md), LUIS genereert automatisch een eindpunt-URL voor de regio die aan de sleutel gekoppeld. Als u wilt een app LUIS publiceert naar meer dan één regio, moet u ten minste één sleutel per regio. 

## <a name="luis-website"></a>LUIS website
Er zijn drie LUIS websites, op basis van regio. U moet maken en publiceren in dezelfde regio. 

|LUIS|Regio|
|--|--|
|[www.Luis.AI][www.luis.ai]|V.S.<br>niet-Europa<br>geen Australië|
|[AU.Luis.AI][au.luis.ai]|Australië|
|[EU.Luis.AI][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Publishing regio 's

LUIS apps die zijn gemaakt op https://www.luis.ai kan worden gepubliceerd naar alle eindpunten, behalve de [Europese](#publishing-to-europe) en [Australische](#publishing-to-australia) regio's. 

De authoring regio-app kan alleen worden gepubliceerd naar een bijbehorende regio publiceren. Als uw app zich momenteel in de verkeerde regio ontwerpen, de app exporteren en importeren in de juiste authoring regio voor uw publishing regio. 

 Globale regio | Regio ontwerpen | Publiceren & opvragen van regio   |   LUIS website | Eindpunt-URL-indeling   |
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

Als u wilt publiceren naar de Europese gebieden, hebt u LUIS apps op https://eu.luis.ai alleen. Als u probeert te publiceren ergens anders met een sleutel in de regio Europa, wordt LUIS een waarschuwingsbericht weergegeven. Gebruik in plaats daarvan https://eu.luis.ai. LUIS apps die zijn gemaakt op [ https://eu.luis.ai ] [ eu.luis.ai] niet automatisch kunnen worden gemigreerd naar andere regio's. Exporteer en importeer vervolgens de app LUIS om te migreren.

## <a name="publishing-to-australia"></a>Publiceren naar Australië

Als u wilt publiceren naar de Australische regio's, hebt u LUIS apps op https://au.luis.ai alleen. Als u probeert te publiceren ergens anders met een sleutel in de Australische regio, wordt LUIS een waarschuwingsbericht weergegeven. Gebruik in plaats daarvan https://au.luis.ai. LUIS apps die zijn gemaakt op [ https://au.luis.ai ] [ au.luis.ai] niet automatisch kunnen worden gemigreerd naar andere regio's. Exporteer en importeer vervolgens de app LUIS om te migreren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vooraf gedefinieerde entiteiten verwijzing](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai