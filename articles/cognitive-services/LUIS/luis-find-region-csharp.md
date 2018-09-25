---
title: Eindpunt-regio met C# niet vinden in LUIS
titleSuffix: Azure Cognitive Services
description: Via een programma zoeken regio met de eindpuntsleutel en toepassing publiceren-ID van LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 53c3d1abb24ae0d5b33a2a100dda07fd20ae92d1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039629"
---
# <a name="find-endpoint-region-with-c"></a>Eindpunt-gebied met C# zoeken 
Als u de LUIS-app-ID en de LUIS abonnements-ID hebt, kunt u welke regio u moet gebruiken voor eindpunt query's kunt vinden.

> [!NOTE] 
> De volledige C#-oplossing is beschikbaar via de [ **LUIS-Samples** Github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>C#-klassecode regio zoeken
De consoletoepassing neemt de LUIS-app-ID en de eindpuntsleutel en retourneert alle regio's die zijn gekoppeld. Op dit moment een eindpuntsleutel per regio wordt gemaakt, zodat slechts één regio moet worden geretourneerd.

De afhankelijkheden van .net-bibliotheek opnemen:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Deze aangepaste LUIS-klasse gebouwd om te vinden van de regio toevoegen. Vervang de waarden van variabelen voor `luisAppId` en `luisSubscriptionKey` door uw eigen waarden. Alle regio's die 401 retourneren worden, geschreven naar de console voor foutopsporing. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Dit is een voorbeeld van het aanroepen van de aangepaste LUIS-klasse in de consoletoepassing Main-methode:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Wanneer de toepassing wordt uitgevoerd, de-console wordt aangegeven in de regio voor de app-ID.

![Schermopname van console-app met LUIS regio](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over LUIS [regio's](luis-reference-regions.md).
