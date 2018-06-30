---
title: LUIS gebied met behulp van Node.js zoeken binnen de grenzen van de taal Understanding (LUIS) | Microsoft Docs
description: Programmatisch zoeken gebied met een eindpuntsleutel en de toepassing publiceren-ID voor LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: v-geberr
ms.openlocfilehash: 6d85e6007b3e85a1b55997541e721ad57c22dddf
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111742"
---
# <a name="region-can-be-determined-from-api-call"></a>Regio kan worden bepaald van de API-aanroep 
Als u de LUIS app-ID en de LUIS abonnements-ID hebt, kunt u welke regio moet worden gebruikt voor het eindpunt query's kunt vinden.

> [!NOTE] 
> De volledige Node.js-oplossing is beschikbaar via de [ **LUIS-Samples** Github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>LUIS eindpunt querystrategie
Elke LUIS endpoint-query is vereist:

* Een eindpuntsleutel
* Een app-ID
* Een regio

Als de LUIS eindpunt query de juiste eindpunt sleutel en app-ID maar de verkeerde regio gebruikt, is de antwoordcode 401. De aanvraag 401 is telt niet mee voor het quotum voor het abonnement. Deze aanvraag omzetten in een strategie voor het pollen van alle regio's om te zoeken naar de juiste regio. De juiste regio is de enige aanvraag waarmee 2xx statuscode geretourneerd. 

|responscode|Parameters|
|--|--|
|2xx|juiste eindpuntsleutel<br>app-ID corrigeren<br>juiste hostregio|
|401|juiste eindpuntsleutel<br>app-ID corrigeren<br>_onjuiste_ hostregio|

## <a name="nodejs-code-to-find-region"></a>Node.js-code gebied vinden
De consoletoepassing duurt de LUIS app-ID en de endpoint-sleutel en retourneert alle regio's die zijn gekoppeld. Op dit moment een eindpuntsleutel is gemaakt door regio zodat slechts één regio moet retourneren.

De NPM-afhankelijkheden zijn:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Constanten toevoegen. Vervang de waarden van de variabelen voor `subscriptionKey` en `appId` met uw eigen waarden.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Voeg `searchRegions` functioneren op het gebied vinden. Alle onjuist regio's retourneren 401, dat is gevonden en genegeerd.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Roep de `searchRegions` functioneren en retourneert één regio:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Wanneer de toepassing wordt uitgevoerd, ziet de terminal u de regio voor de app-ID.

![Schermafbeelding van de console-app met LUIS regio](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Volgende stappen

Meer informatie over LUIS [regio's](luis-reference-regions.md).
