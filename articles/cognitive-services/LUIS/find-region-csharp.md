---
title: LUIS gebied met C# zoeken binnen de grenzen van de taal Understanding (LUIS) | Microsoft Docs
description: Programmatisch zoeken gebied met een eindpuntsleutel en de toepassing publiceren-ID voor LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/31/2018
ms.author: v-geberr
ms.openlocfilehash: f0df14736e0ed47957999e3aa7c6a22b0b0c0a35
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110403"
---
# <a name="region-can-be-determined-from-api-call"></a>Regio kan worden bepaald van de API-aanroep 
Als u de LUIS app-ID en de LUIS abonnements-ID hebt, kunt u welke regio moet worden gebruikt voor het eindpunt query's kunt vinden.

> [!NOTE] 
> De volledige C#-oplossing is beschikbaar via de [ **LUIS-Samples** Github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>C#-klasse-code gebied vinden
De consoletoepassing duurt de LUIS app-ID en de endpoint-sleutel en retourneert alle regio's die zijn gekoppeld. Op dit moment een eindpuntsleutel is gemaakt door regio zodat slechts één regio moet retourneren.

De afhankelijkheden van .net-bibliotheek omvatten:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Deze aangepaste LUIS klasse is ontworpen om te zoeken naar de regio toevoegen. Vervang de waarden van de variabelen voor `luisAppId` en `luisSubscriptionKey` met uw eigen waarden. Alle regio's die 401 retourneren wordt geschreven naar de Foutopsporingsconsole. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Dit is een voorbeeld van een aanroep van de aangepaste LUIS klasse in de consoletoepassing Main-methode:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Wanneer de toepassing wordt uitgevoerd, de console wordt weergegeven voor de regio voor de app-ID.

![Schermafbeelding van de console-app met LUIS regio](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over LUIS [regio's](luis-reference-regions.md).
