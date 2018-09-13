---
title: Publiceren van regio's en -eindpunten - LUIS
titleSuffix: Azure Cognitive Services
description: De regio waarin u uw LUIS-app publiceren overeenkomt met de regio of locatie die u in de Azure-portal opgeeft wanneer u een Azure LUIS-eindpuntsleutel maakt. Wanneer u een app publiceert, genereert LUIS automatisch een eindpunt-URL voor de regio die is gekoppeld aan de sleutel. Als u wilt een LUIS-app publiceren in meer dan één regio, moet u ten minste één sleutel per regio.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/11/2018
ms.author: diberry
ms.openlocfilehash: 48e1e19d2d425fe123e5a0c369ecebf623b74eb2
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721299"
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

## <a name="regions-and-azure-resources"></a>Regio's en Azure-resources
De app wordt gepubliceerd naar alle regio's die zijn gekoppeld aan de LUIS-resources in de portal LUIS toegevoegd. Bijvoorbeeld, voor een app gemaakt op [www.luis.ai][www.luis.ai], als u een LUIS-resource in **westus** en toe te voegen aan de app als een resource, de app wordt gepubliceerd in die regio. 

## <a name="public-apps"></a>Openbare apps
Een openbare app is gepubliceerd in alle regio's, zodat een gebruiker met een sleutel op basis van een regio LUIS resource toegang heeft tot de app in welke regio gekoppeld aan de resource-sleutel is.

## <a name="publishing-regions"></a>Publicatie-regio 's

LUIS-apps die zijn gemaakt op https://www.luis.ai kunnen worden gepubliceerd naar alle eindpunten, behalve de [Europese](#publishing-to-europe) en [Australische](#publishing-to-australia) regio's. 

De ontwerphandleiding regio app kan alleen worden gepubliceerd naar een bijbehorende regio publiceren. Als uw app zich momenteel in de verkeerde regio voor schrijven bevindt, de app exporteren en importeren in de juiste authoring regio voor uw regio publiceren. 

 Globale regio | Regio ontwerpen | Publiceren en uitvoeren van query's regio   |   LUIS-website | Eindpunt-URL-indeling   |
|-----|------|------|------|------|
| Azië | US - west| Azië - oost     | [www.Luis.AI][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | US - west| Azië - zuidoost     | [www.Luis.AI][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Australië](#publishing-to-australia) | Australië - oost| Australië - oost     |   [AU.Luis.AI][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa -west| Europa - noord     | [EU.Luis.AI][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa -west| Europa -west     | [EU.Luis.AI][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | US - west | US - oost      |[www.Luis.AI][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west | US - oost 2     | [www.Luis.AI][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west | US - zuid-centraal     | [www.Luis.AI][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | US - west | US - west-centraal     |[www.Luis.AI][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | US - west | US - west |  [www.Luis.AI][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Noord-Amerika | US - west | US - west 2    | [www.Luis.AI][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Zuid-Amerika | US - west | Brazilië - zuid     | [www.Luis.AI][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

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