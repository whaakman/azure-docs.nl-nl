---
title: Publicatie-regio's en -eindpunten
titleSuffix: Azure Cognitive Services
description: De regio waarin u uw LUIS-app publiceren overeenkomt met de regio of locatie die u in de Azure-portal opgeeft wanneer u een Azure LUIS-eindpuntsleutel maakt. Wanneer u een app publiceert, genereert LUIS automatisch een eindpunt-URL voor de regio die is gekoppeld aan de sleutel.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 995300fe2a82fb41078d1c66435f0fb006d5f8cf
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53603688"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Ontwerpen en het publiceren van regio's en de bijbehorende sleutels

De regio waarin u uw LUIS-app publiceren overeenkomt met de regio of locatie die u in de Azure-portal opgeeft wanneer u een Azure LUIS-eindpuntsleutel maakt. Wanneer u [een app publiceren](./luis-how-to-publish-app.md), LUIS genereert automatisch een eindpunt-URL voor de regio die is gekoppeld aan de sleutel. Als u wilt een LUIS-app publiceren in meer dan één regio, moet u ten minste één sleutel per regio. 

## <a name="luis-website"></a>LUIS-website
Er zijn drie LUIS-websites, op basis van regio. U moet maken en publiceren in dezelfde regio. 

|LUIS|Regio|
|--|--|
|[www.Luis.AI][www.luis.ai]|V.S.<br>niet-Europa<br>niet-Australië|
|[AU.Luis.AI][au.luis.ai]|Australië|
|[EU.Luis.AI][eu.luis.ai]|Europa|

## <a name="regions-and-azure-resources"></a>Regio's en Azure-resources
De app wordt gepubliceerd naar alle regio's die zijn gekoppeld aan de LUIS-resources in de portal LUIS toegevoegd. Bijvoorbeeld, voor een app gemaakt op [www.luis.ai][www.luis.ai], als u een LUIS-resource in **westus** en toe te voegen aan de app als een resource, de app wordt gepubliceerd in die regio. 

## <a name="public-apps"></a>Openbare apps
Een openbare app is gepubliceerd in alle regio's, zodat een gebruiker met een sleutel op basis van een regio LUIS resource toegang heeft tot de app in welke regio gekoppeld aan de resource-sleutel is.

## <a name="publishing-regions"></a>Publicatie-regio 's

LUIS-apps die zijn gemaakt op https://www.luis.ai kunnen worden gepubliceerd naar alle eindpunten, behalve de [Europese](#publishing-to-europe) en [Australische](#publishing-to-australia) regio's. 

De ontwerphandleiding regio app kan alleen worden gepubliceerd naar een bijbehorende regio publiceren. Als uw app zich momenteel in de verkeerde regio voor schrijven bevindt, de app exporteren en importeren in de juiste authoring regio voor uw regio publiceren. 

 Globale regio | Regio ontwerpen<br>`API region name` | Publiceren en uitvoeren van query's regio<br>`API region name`   |   LUIS-website | Eindpunt-URL-indeling   |
|-----|------|------|------|------|
| Azië | US - west<br>`westus`| Azië - oost<br>`eastasia`     | [www.Luis.AI][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | US - west<br>`westus`| Azië - zuidoost<br>`souteastasia`     | [www.Luis.AI][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australië](#publishing-to-australia) | Australië - oost<br>`australiaeast`| Australië - oost<br>`australiaeast`     |   [AU.Luis.AI][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa -west<br>`westeurope`| Europa - noord<br>`northeurope`     | [EU.Luis.AI][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa -west<br>`westeurope`| Europa -west<br>`westeurope`     | [EU.Luis.AI][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | US - west<br>`westus` | US - oost<br>`eastus`      |[www.Luis.AI][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west<br>`westus` | US - oost 2<br>`eastus2`     | [www.Luis.AI][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west<br>`westus` | US - zuid-centraal<br>`southcentralus`     | [www.Luis.AI][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | US - west<br>`westus` | US - west-centraal<br>`westcentralus`     |[www.Luis.AI][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west<br>`westus` | US - west<br>`westus`  |  [www.Luis.AI][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Noord-Amerika | US - west<br>`westus` | US - west 2<br>`westus2`    | [www.Luis.AI][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Zuid-Amerika | US - west<br>`westus` | Brazilië - zuid<br>`brazilsouth`     | [www.Luis.AI][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Publiceren naar Europa

Als u wilt publiceren naar de Europese regio's, u LUIS apps maken op https://eu.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de regio Europa, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://eu.luis.ai. LUIS-apps die zijn gemaakt op [ https://eu.luis.ai ] [ eu.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="publishing-to-australia"></a>Publiceren naar Australië

Als u wilt publiceren naar de Australische regio's, u LUIS apps maken op https://au.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de Australische regio, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://au.luis.ai. LUIS-apps die zijn gemaakt op [ https://au.luis.ai ] [ au.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="endpoints"></a>Eindpunten

LUIS heeft momenteel 2 eindpunten: één voor het ontwerpen en één voor analyse van tekst.

|Doel|URL|
|--|--|
|Ontwerpen|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Analyse van tekst (voorspelling query)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

De volgende tabel beschrijft de parameters, aangeduid met behulp van accolades `{}`, in de vorige tabel.

|Parameter|Doel|
|--|--|
|regio|Azure-regio - ontwerpen en publiceren van hebben verschillende regio 's|
|toepassings-id|LUIS-app-ID in URL route gebruikt en gevonden op app-dashboard|
|q|utterance tekst verzonden vanuit de clienttoepassing zoals chatbot|


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de vooraf gemaakte entiteiten](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai