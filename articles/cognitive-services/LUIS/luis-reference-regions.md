---
title: Publicatie-regio's en -eindpunten
titleSuffix: Azure Cognitive Services
description: 3 authoring regio's en hun portals ondersteuning voor alle veel publishing regio's. De regio waarin u uw LUIS-app publiceren overeenkomt met de regio of locatie die u in de Azure-portal opgeeft wanneer u een Azure LUIS-eindpuntsleutel maakt. Wanneer u een app publiceert, genereert LUIS automatisch een eindpunt-URL voor de regio die is gekoppeld aan de sleutel.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: 20ea2eb632a6d685351178691cc3d0f58a567902
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891409"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Ontwerpen en het publiceren van regio's en de bijbehorende sleutels

Drie ontwerpen regio's worden ondersteund door de bijbehorende LUIS portals. Als u wilt een LUIS-app publiceren in meer dan één regio, moet u ten minste één sleutel per regio. 

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUIS ontwerpen regio 's
Er zijn drie LUIS authoring portals, op basis van regio. U moet maken en publiceren in dezelfde regio. 

|LUIS|Regio ontwerpen|De naam van de Azure-regio|
|--|--|--|
|[www.Luis.AI][www.luis.ai]|V.S.<br>niet-Europa<br>niet-Australië| `westus`|
|[AU.Luis.AI][au.luis.ai]|Australië| `australiaeast`|
|[EU.Luis.AI][eu.luis.ai]|Europa|`westeurope`|

Ontwerphandleiding regio's hebt [gekoppelde regio's voor failover](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Publicatie-regio's en Azure-resources
De app wordt gepubliceerd naar alle regio's die zijn gekoppeld aan de LUIS-resources in de portal LUIS toegevoegd. Bijvoorbeeld, voor een app gemaakt op [www.luis.ai][www.luis.ai], als u een LUIS of Cognitive Service resource in **westus** en [toe te voegen aan de app als een resource ](luis-how-to-azure-subscription.md), de app wordt gepubliceerd in die regio. 

## <a name="public-apps"></a>Openbare apps
Een openbare app is gepubliceerd in alle regio's, zodat een gebruiker met een sleutel op basis van een regio LUIS resource toegang heeft tot de app in welke regio gekoppeld aan de resource-sleutel is.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Publicatie-regio's zijn gekoppeld aan het ontwerpen van regio 's

De ontwerphandleiding regio app kan alleen worden gepubliceerd naar een bijbehorende regio publiceren. Als uw app zich momenteel in de verkeerde regio voor schrijven bevindt, de app exporteren en importeren in de juiste authoring regio voor uw regio publiceren. 

LUIS-apps die zijn gemaakt op https://www.luis.ai kunnen worden gepubliceerd naar alle eindpunten, behalve de [Europese](#publishing-to-europe) en [Australische](#publishing-to-australia) regio's. 

## <a name="publishing-to-europe"></a>Publiceren naar Europa

Als u wilt publiceren naar de Europese regio's, u LUIS apps maken op https://eu.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de regio Europa, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://eu.luis.ai. LUIS-apps die zijn gemaakt op [ https://eu.luis.ai ] [ eu.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="europe-publishing-regions"></a>Publicatie-Europa

 Globale regio | API-gebied voor ontwerp en ontwerpen van website| Publiceren en uitvoeren van query's regio<br>`API region name`   |  Eindpunt-URL-indeling   |
|-----|------|------|------|
| [Europa](#publishing-to-europe)| `westeurope`<br>[EU.Luis.AI][eu.luis.ai]| Frankrijk - centraal<br>`francecentral`     | https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Europa](#publishing-to-europe)| `westeurope`<br>[EU.Luis.AI][eu.luis.ai]| Europa - noord<br>`northeurope`     | https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Europa](#publishing-to-europe) | `westeurope`<br>[EU.Luis.AI][eu.luis.ai]| Europa -west<br>`westeurope`    |  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| [Europa](#publishing-to-europe) | `westeurope`<br>[EU.Luis.AI][eu.luis.ai]| Verenigd Koninkrijk Zuid<br>`uksouth`    |  https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-australia"></a>Publiceren naar Australië

Als u wilt publiceren naar de Australische regio's, u LUIS apps maken op https://au.luis.ai alleen. Als u probeert te publiceren ergens anders met behulp van een sleutel in de Australische regio, weergegeven LUIS een waarschuwingsbericht. In plaats daarvan gebruik https://au.luis.ai. LUIS-apps die zijn gemaakt op [ https://au.luis.ai ] [ au.luis.ai] niet automatisch gemigreerd naar andere regio's. Exporteren en vervolgens de LUIS-app importeren als u wilt deze migreren.

## <a name="australia-publishing-regions"></a>Regio's in Australië publiceren

 Globale regio | API-gebied voor ontwerp en ontwerpen van website| Publiceren en uitvoeren van query's regio<br>`API region name`   |  Eindpunt-URL-indeling   |
|-----|------|------|------|
| [Australië](#publishing-to-australia) | `australiaeast`<br>[AU.Luis.AI][au.luis.ai]| Australië - oost<br>`australiaeast`     |  https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-other-regions"></a>Publiceren naar een andere regio 's

Als u wilt publiceren naar de andere regio's, u LUIS apps maken op [ https://www.luis.ai ](https://www.luis.ai) alleen. 

## <a name="other-publishing-regions"></a>Andere publishing regio 's

 Globale regio | API-gebied voor ontwerp en ontwerpen van website| Publiceren en uitvoeren van query's regio<br>`API region name`   |  Eindpunt-URL-indeling   |
|-----|------|------|------|
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| India - centraal<br>`centralindia` |  https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| Azië - oost<br>`eastasia`     |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| Japan - oost<br>`japaneast`     |   https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| Japan - west<br>`japanwest`     |   https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| Korea - centraal<br>`koreacentral`     |   https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Azië | `westus`<br>[www.Luis.AI][www.luis.ai]| Azië - zuidoost<br>`southeastasia`     |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika |`westus`<br>[www.Luis.AI][www.luis.ai] | Canada - midden<br>`canadacentral`     |   https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika |`westus`<br>[www.Luis.AI][www.luis.ai] | US - centraal<br>`centralus`     |   https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika |`westus`<br>[www.Luis.AI][www.luis.ai] | US - oost<br>`eastus`      |  https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | `westus`<br>[www.Luis.AI][www.luis.ai] | US - oost 2<br>`eastus2`     |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | `westus`<br>[www.Luis.AI][www.luis.ai] | US - noord-centraal<br>`northcentralus`  |  https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika | `westus`<br>[www.Luis.AI][www.luis.ai] | US - zuid-centraal<br>`southcentralus`  |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Noord-Amerika |`westus`<br>[www.Luis.AI][www.luis.ai] | US - west-centraal<br>`westcentralus`    |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Noord-Amerika | `westus`<br>[www.Luis.AI][www.luis.ai] | US - west<br>`westus`  |   https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Noord-Amerika |`westus`<br>[www.Luis.AI][www.luis.ai] | US - west 2<br>`westus2`    |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Zuid-Amerika | `westus`<br>[www.Luis.AI][www.luis.ai] | Brazilië - zuid<br>`brazilsouth`    |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="endpoints"></a>Eindpunten

LUIS heeft momenteel 2 eindpunten: één voor het ontwerpen en één voor de Queryanalyse voorspelling.

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

## <a name="failover-regions"></a>Failover-regio 's

Elke regio heeft een secundaire regio om te fungeren als failover voor. Europa mislukt via in Europa en Australië overgenomen wordt in Australië.

Ontwerphandleiding regio's hebt [gekoppelde regio's voor failover](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de vooraf gemaakte entiteiten](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
