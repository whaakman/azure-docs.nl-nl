---
title: Toepassingendashboard voor LUIS apps | Microsoft Docs
description: Meer informatie over het dashboard, een gevisualiseerde reporting hulpprogramma dat u kunt uw apps in één oogopslag controleren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 4e117aa734c551f76c3602f81e3e2ebc6aa9f337
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345873"
---
# <a name="application-dashboard"></a>Toepassingendashboard
Het app-dashboard kunt u uw app in één oogopslag controleren. De **Dashboard** wordt weergegeven wanneer u een app openen door te klikken op de naam van de toepassing in **mijn Apps** pagina selecteert **Dashboard** van bovenpaneel. 

> [!CAUTION]
> Als u wilt dat de meest recente metrische gegevens voor LUIS, moet u naar:
> * Gebruik een LUIS [eindpuntsleutel](luis-how-to-azure-subscription.md) gemaakt in Azure
> * Gebruik LUIS eindpuntsleutel voor alle endpoint-aanvragen, met inbegrip van LUIS [API](https://aka.ms/luis-endpoint-apis) en bot
> * Gebruik andere endpoint-sleutel voor elke app LUIS. Gebruik een sleutel één eindpunt niet voor alle apps. De eindpuntsleutel wordt getraceerd op het niveau van sleutels niet op het niveau van de app.  

De **Dashboard** pagina geeft een overzicht van de LUIS-app met inbegrip van het huidige model status, evenals [eindpunt](luis-glossary.md#endpoint) gebruik gedurende een bepaalde periode. <!--The following image shows the **Dashboard** page.-->

<!-- TBD: Get a working screen shot
![The Dashboard](./media/luis-how-to-use-dashboard/dashboard.png)
-->

<!-- TBD: IS THIS STILL TRUE?
At the top of the **Dashboard** page, a contextual notification bar constantly displays notifications to update you on the required or recommended actions appropriate for the current state of your app. It also provides useful tips and alerts as needed. A detailed description of the data reported on the **Dashboard** page follows.
-->
  
## <a name="app-status"></a>App-status
Het dashboard toont de training voor de toepassing en publicatiestatus, met inbegrip van de datum en tijd waarop de app voor het laatst getraind en gepubliceerd.  

![Dashboard - App-Status](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Gegevens modelstatistieken
Het dashboard toont het totale aantal intents en entiteiten gelabelde utterances bestaande in de app. 

![App-gegevens statistiek](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Eindpunt treffers
Het dashboard toont de totale eindpunt treffers dat de app LUIS ontvangt en weer te geven treffers binnen een periode die u kunt opgeven. Het totaal aantal treffers weergegeven is de som van de endpoint-treffers die gebruikmaken van een [eindpuntsleutel](./luis-concept-keys.md#endpoint-key) en eindpunt treffers die gebruikmaken van een [ontwerpen sleutel](./luis-concept-keys.md#authoring-key).

<!-- TBD: this image is old but I don't have a new one based on usage -->
![Eindpunt treffers](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> Telling van het meest recente eindpunt is in de Azure portal op het overzicht van de service LUIS. 
 
### <a name="total-endpoint-hits"></a>Totaal aantal eindpunt treffers
Het totaal aantal treffers in eindpunt naar uw app ontvangen sinds het maken van de app tot de huidige datum.

### <a name="endpoint-hits-per-period"></a>Eindpunt treffers per periode
Het aantal treffers ontvangen binnen een periode van afgelopen weergegeven per dag. De punten tussen de begin- en einddatums vertegenwoordigen de dagen die vallen in deze periode. Beweeg de muisaanwijzer de muisaanwijzer over elk punt om te zien de treffers in elke dag binnen de periode tellen. 

Selecteer een periode op de grafiek weergeven:
 
1. Klik op **extra instellingen** ![knop Extra instellingen](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst met punten. U kunt selecteren perioden variërend van één week tot een jaar. 

    ![Eindpunt treffers per periode](./media/luis-how-to-use-dashboard/timerange.png)

2. Selecteer een periode in de lijst en klik vervolgens op de pijl naar links ![Pijl naar links](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek weergeven.

### <a name="key-usage"></a>Sleutelgebruik
Het aantal treffers uit de sleutel voor het abonnement van de toepassing verbruikt. Zie voor meer informatie over abonnement sleutels [sleutels in LUIS](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Opzet uitsplitsing
De **bedoeling uitsplitsing** een uitsplitsing van intents op basis van gelabelde utterances of eindpunt treffers worden weergegeven. Deze overzichtsgrafiek toont het relatieve belang van elk doel in de app. Als u de muisaanwijzer boven een segment, ziet u de naam van de opzet en het percentage dat deze vertegenwoordigt het totaal aantal treffers in gelabelde utterances/het eindpunt. 

![Opzet uitsplitsing](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Om te bepalen of de uitsplitsing is gebaseerd op gelabelde utterances of eindpunt treffers:

1. Klik op **extra instellingen** ![knop Extra instellingen](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst als in de volgende afbeelding:

    ![Lijst met opzet uitsplitsing](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Selecteer een waarde in de lijst en klik vervolgens op de pijl naar links ![Pijl naar links](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek weergeven.

## <a name="entity-breakdown"></a>Uitsplitsing van de entiteit
Het dashboard geeft een overzicht van de entiteiten die zijn gebaseerd op gelabelde utterances of eindpunt treffers weer. Deze overzichtsgrafiek toont het relatieve belang van elke entiteit in de app. Als u de muisaanwijzer boven een segment, ziet u de entiteitsnaam van de en het percentage in treffers gelabelde utterances/het eindpunt. 

![Uitsplitsing van de entiteit](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Om te bepalen of de uitsplitsing is gebaseerd op gelabelde utterances of eindpunt treffers:

1. Klik op **extra instellingen** ![knop Extra instellingen](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) voor toegang tot de lijst als in de volgende afbeelding:

    ![Lijst van de uitsplitsing van entiteit](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Selecteer een waarde in de lijst en klik vervolgens op de pijl naar links ![Pijl naar links](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) de grafiek dienovereenkomstig weergeven.
