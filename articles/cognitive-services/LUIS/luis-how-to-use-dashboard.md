---
title: App-dashboard
titleSuffix: Language Understanding - Azure Cognitive Services
description: Meer informatie over het dashboard, een gevisualiseerde rapportagetool in te voeren die u kunt uw apps in één oogopslag controleren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 6f6de278e802b83623413b0bf70e556d083818c8
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140605"
---
# <a name="model-and-usage-statistics-in-the-dashboard"></a>Model en het gebruik van statistieken in het dashboard
Het app-dashboard kunt u uw app in één oogopslag controleren. De **Dashboard** wordt weergegeven wanneer u een app openen door te klikken op de naam van de toepassing op **mijn Apps** pagina selecteert **Dashboard** uit boven in het venster. 

> [!CAUTION]
> Als u wilt dat de meest recente metrische gegevens voor LUIS, moet u naar:
> * Gebruik een LUIS [eindpuntsleutel](luis-how-to-azure-subscription.md) gemaakt in Azure
> * Gebruik LUIS eindpuntsleutel voor alle endpoint-aanvragen, met inbegrip van LUIS [API](https://aka.ms/luis-endpoint-apis) en bot
> * Gebruik verschillende eindpuntsleutel voor elke LUIS-app. Gebruik een sleutel van één eindpunt niet voor alle apps. De eindpuntsleutel wordt bijgehouden op het niveau van de sleutel, niet op het niveau van de app.  

De **Dashboard** pagina geeft een overzicht van de LUIS-app met inbegrip van het huidige model staat, evenals [eindpunt](luis-glossary.md#endpoint) gebruik na verloop van tijd. 
  
## <a name="app-status"></a>App-status
Het dashboard wordt weergegeven van de toepassing training en publicatiestatus, met inbegrip van de datum en tijd wanneer de app het laatst getraind en gepubliceerd.  

![Dashboard - App-Status](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Model-data-statistieken
Het dashboard toont het totale aantal intenties en entiteiten gelabelde uitingen bestaan in de app. 

![App-gegevens-statistieken](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Eindpunt treffers
Het dashboard toont de totale eindpunt treffers die de LUIS-app ontvangt en kunt u om weer te geven treffers binnen een periode die u opgeeft. Het totale aantal hits weergegeven is de som van eindpunt treffers die gebruikmaken van een [eindpuntsleutel](./luis-concept-keys.md#endpoint-key) en eindpunt treffers die gebruikmaken van een [ontwerpen sleutel](./luis-concept-keys.md#authoring-key).

![Eindpunt treffers](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> Het eindpunt van de meest recente aantal treffers in de Azure-portal in het overzicht van LUIS-service is. 
 
### <a name="total-endpoint-hits"></a>Eindpunt van het totale aantal hits
Het totaal aantal treffers in eindpunt en aan uw app ontvangen na het maken van apps tot de huidige datum.

### <a name="endpoint-hits-per-period"></a>Eindpunt treffers per periode
Het aantal treffers ontvangen binnen een periode van afgelopen per dag weergegeven. De punten tussen de begin- en einddatums vertegenwoordigen de dagen vallen in deze periode. Beweeg de muisaanwijzer de muisaanwijzer over elk punt om te zien de treffers tellen per dag binnen de periode. 

Selecteer een periode op de grafiek weergeven:
 
1. Klik op **extra instellingen** ![knop Extra instellingen](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst met perioden. Perioden, variërend van één week van één jaar, kunt u selecteren. 

    ![Eindpunt treffers per periode](./media/luis-how-to-use-dashboard/timerange.png)

2. Selecteer een periode in de lijst en klik vervolgens op de pijl-terug ![Pijl-terug](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek weergeven.

### <a name="key-usage"></a>Sleutelgebruik
Het aantal treffers gebruikt vanaf de eindpuntsleutel van de toepassing. Zie voor meer informatie over endpoint-codes [sleutels in LUIS](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Uitsplitsing van de intentie
De **uitsplitsing van de intentie** geeft een overzicht van de intenties op basis van gelabelde uitingen of eindpunt treffers weer. Deze overzichtsgrafiek toont u het relatieve belang van elk doel in de app. Als u de muisaanwijzer boven een segment, ziet u de naam van de intentie en het percentage staat voor het totale aantal hits gelabelde uitingen/het eindpunt. 

![Uitsplitsing van de intentie](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Om te bepalen of de uitsplitsing van de is gebaseerd op gelabelde uitingen of eindpunt treffers:

1. Klik op **extra instellingen** ![extra instellingen knop aan de toegangslijst](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst met zoals in de volgende afbeelding:

    ![Uitsplitsing van de intentie-lijst](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Selecteer een waarde in de lijst en klik vervolgens op de pijl-terug ![Pijl-terug](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek weergeven.

## <a name="entity-breakdown"></a>Uitsplitsing van de entiteit
Het dashboard geeft een overzicht van de entiteiten op basis van gelabelde uitingen of eindpunt treffers weer. Deze overzichtsgrafiek toont u het relatieve belang van elke entiteit in de app. Als u de muisaanwijzer boven een segment, ziet u de naam van de entiteit en het percentage treffers in gelabelde uitingen/het eindpunt. 

![Uitsplitsing van de entiteit](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Om te bepalen of de uitsplitsing van de is gebaseerd op gelabelde uitingen of eindpunt treffers:

1. Klik op **extra instellingen** ![knop Extra instellingen voor lijst](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst met zoals in de volgende afbeelding:

    ![Uitsplitsing van de entiteitenlijst](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Selecteer een waarde in de lijst en klik vervolgens op de pijl-terug ![Pijl-terug](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek dienovereenkomstig weergeven.
