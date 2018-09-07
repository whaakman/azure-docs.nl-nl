---
title: Eindpunt-regio met behulp van Node.js niet vinden in LUIS
titleSuffix: Azure Cognitive Services
description: Via een programma zoeken regio met de eindpuntsleutel en toepassing publiceren-ID van LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 2b978b8459bbf248f7702076c78c1948b036aec6
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057358"
---
# <a name="find-endpoint-region-with-nodejs"></a>Eindpunt-gebied met behulp van Node.js zoeken
Als u de LUIS-app-ID en de LUIS abonnements-ID hebt, kunt u welke regio u moet gebruiken voor eindpunt query's kunt vinden.

> [!NOTE] 
> De volledige Node.js-oplossing is beschikbaar via de [ **LUIS-Samples** Github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>LUIS-eindpunt querystrategie
Elke query LUIS-eindpunt is vereist:

* Een eindpuntsleutel
* Een app-ID
* Een regio

Als de query van LUIS-eindpunt maakt gebruik van het juiste eindpunt sleutel- en app-ID, maar de verkeerde regio, wordt de responscode 401. De 401-aanvraag wordt niet meegerekend mee voor het abonnementenquotum. Deze aanvraag omzetten in een strategie te vragen voor alle regio's om de juiste regio. De juiste regio is de enige aanvraag die een statuscode 2xx retourneert. 

|responscode|Parameters|
|--|--|
|2xx|juiste eindpuntsleutel<br>juiste app-ID<br>regio van de juiste host|
|401|juiste eindpuntsleutel<br>juiste app-ID<br>_onjuiste_ hostregio|

## <a name="nodejs-code-to-find-region"></a>Node.js-code regio zoeken
De consoletoepassing neemt de LUIS-app-ID en de eindpuntsleutel en retourneert alle regio's die zijn gekoppeld. Op dit moment een eindpuntsleutel per regio wordt gemaakt, zodat slechts één regio moet worden geretourneerd.

De NPM-afhankelijkheden zijn:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Voeg constanten toe. Vervang de waarden van variabelen voor `subscriptionKey` en `appId` door uw eigen waarden.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Voeg `searchRegions` functie voor het vinden van de regio. Alle onjuist regio's retourneren 401, dat is aangetroffen en genegeerd.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Roep de `searchRegions` functioneren en één regio retourneren:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Wanneer de toepassing wordt uitgevoerd, de terminal wordt aangegeven in de regio voor de app-ID.

![Schermopname van console-app met LUIS regio](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Volgende stappen

Meer informatie over LUIS [regio's](luis-reference-regions.md).
