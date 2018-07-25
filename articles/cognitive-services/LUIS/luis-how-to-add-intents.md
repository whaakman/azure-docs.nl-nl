---
title: Intents toevoegen in toepassingen LUIS | Microsoft Docs
description: Language Understanding (LUIS) om toe te voegen intents om apps te begrijpen van gebruikersaanvragen en reageren op deze correct te gebruiken.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0ebf15ea49467674ab3c56aa7983131593cf5c9a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225883"
---
# <a name="manage-intents"></a>Intents beheren 
Voeg [intents](luis-concept-intent.md) aan uw LUIS-app voor het identificeren van groepen van vragen of de opdrachten die het hetzelfde doel hebben. 

U toevoegen en beheren van uw intenties van de **Intents** pagina beschikbaar is via **Intents** in het linkerdeelvenster van LUIS. 

De volgende procedure laat zien hoe u het doel "Bookflight" in de app TravelAgent toevoegen.

## <a name="add-intent"></a>Doel toevoegen

1. Open uw app (bijvoorbeeld TravelAgent) door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **Intents** in het linkerpaneel. 
2. Op de **Intents** pagina, klikt u op **maken van nieuwe bedoeling**.

    ![Intents lijst](./media/luis-how-to-add-intents/IntentsList.png)
3. In de **maken van nieuwe bedoeling** in het dialoogvenster, type de bedoeling naam 'BookFlight' en klikt u op **gedaan**.

    ![Doel toevoegen](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Op de detailpagina van de intentie van de zojuist toegevoegde intentie [utterances toevoegen](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Wijzig de naam van doel

1. Op de **bedoeling** pagina, klikt u op het pictogram wijzigen ![Wijzig de naam van intentie](./media/luis-how-to-add-intents/Rename-Intent-btn.png) naast de naam van de intentie. 

2. Op de **bedoeling** pagina, de naam van de huidige intentie wordt weergegeven in het dialoogvenster. Bewerk de naam van de intentie en druk op enter. De nieuwe naam wordt opgeslagen en weergegeven op de pagina intentie.

    ![Doel bewerken](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Verwijderen van doel
Bij het verwijderen van een doel dan de intentie geen, kunt u alle uitingen toevoegen aan de intentie geen. Dit is handig als u wilt verplaatsen van de uitingen in plaats van deze worden verwijderd.   

1. Op de **bedoeling** pagina, klikt u op de **verwijderen kunt u lezen wat** knop naast de rechterkant van de naam van de intentie. 

    ![Intentie knop verwijderen](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Klik op de knop 'Ok' in het bevestigingsvenster.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Een utterance op intentie pagina toevoegen

Voer een relevante utterance die u kunt van uw gebruikers, zoals verwachten op de pagina intentie `book 2 adult business tickets to Paris tomorrow on Air France` in het tekstvak onder de naam van de intentie en druk op Enter. 
 
>[!NOTE]
>LUIS worden alle uitingen geconverteerd naar kleine letters.

![Schermafbeelding van de intenties pagina, met utterance gemarkeerd](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Uitingen worden toegevoegd aan de lijst met uitingen voor het huidige doel. Wanneer een utterance is toegevoegd, [label van de entiteiten](luis-how-to-add-example-utterances.md) binnen de uitingen en [trainen](luis-how-to-train.md) uw app. 

## <a name="create-a-pattern-from-an-utterance"></a>Maken van een patroon van een utterance
Zie [toevoegen patroon uit bestaande utterance op de pagina kunt u lezen wat of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Een utterance op intentie pagina bewerken

Als u wilt een utterance bewerken, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de regel voor die utterance en selecteer vervolgens **bewerken**. Wijzig de tekst en druk op Enter op het toetsenbord.

![Schermafbeelding van de intenties pagina, met de knop met weglatingsteken gemarkeerd](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Uitingen op intentie pagina opnieuw toewijzen
U kunt het doel van een of meer uitingen wijzigen door deze opnieuw toewijzen aan een ander doel. 

Als u wilt toewijzen van een enkele utterance naar een ander doel, aan de rechterkant van de rij van de utterance, selecteert u de juiste naam van de intentie onder de **met het label bedoeling** kolom. De utterance is verwijderd uit de huidige intentie utterance lijst. 

![Schermafbeelding van BookFlight intentie pagina met de bedoeling van een utterance onder Labeled intentie kolom geselecteerd](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Als u wilt wijzigen van de intentie van verschillende uitingen, schakel de selectievakjes in aan de linkerkant van de uitingen en selecteer vervolgens **opnieuw toewijzen van intentie**. Selecteer het juiste doel in de lijst.

![Schermafbeelding van BookFlight intentie pagina met een utterance gecontroleerd en de intentie knop gemarkeerd voor opnieuw toewijzen](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Uitingen op intentie pagina verwijderen

Als u wilt verwijderen een utterance, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de regel voor die utterance en selecteer vervolgens **verwijderen**. De utterance wordt verwijderd uit de lijst en de LUIS-app.

![Schermafbeelding van de intenties pagina, met de optie voor het verwijderen is gemarkeerd](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Verschillende uitingen verwijderen:

1. Schakel de selectievakjes in aan de linkerkant van de uitingen en selecteer vervolgens **uitingen (s) verwijderen**. 

    ![Schermafbeelding van de intenties pagina, met uitingen gecontroleerd en utterance(s) knop gemarkeerd verwijderen](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Selecteer **gedaan** in de **uitingen verwijderen?** pop-upvenster.

## <a name="search-in-utterances-on-intent-page"></a>Zoeken in uitingen op intentie pagina
U kunt zoeken in een doel voor uitingen die tekst (woorden of zinsdelen) bevatten. Bijvoorbeeld, ziet u mogelijk een foutbericht weergegeven dat betrekking heeft op een bepaald woord, en u wilt alle voorbeelden die bepaald woord bevatten. 

1. Selecteer het pictogram met Vergrootglas in de werkbalk.

    ![Schermafbeelding van de intenties pagina met zoekpictogram van Vergrootglas gemarkeerd](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Een Zoektekstvak wordt weergegeven. Typ het woord of zinsdeel in het zoekvak in de rechterbovenhoek van de lijst met uitingen. De uitingen lijst met updates, zodat alleen de uitingen die uw zoektekst bevatten. 

    ![Schermafbeelding van de intenties pagina met Zoektekstvak gemarkeerd](./media/luis-how-to-add-intents/search-textbox.png)

    De zoekopdracht te annuleren en de volledige lijst met uitingen herstellen, verwijderen van de zoektekst die u hebt getypt. Als u wilt sluiten in het Zoektekstvak, selecteert u het pictogram met Vergrootglas opnieuw op de werkbalk.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Voorspelling discrepantie fouten op intentie pagina
Een utterance in een intent mogelijk een discrepantie tussen het geselecteerde doel en de score voorspelling. LUIS geeft aan dat dit verschil met een rood kader rond de score. 

![Schermafbeelding van de intentie van de BookFlight pagina met voorspelling discrepantie score gemarkeerd](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filteren op intentie voorspelling discrepantie fouten op intentie pagina
Als u wilt filteren op de lijst utterance alleen uitingen met een discrepantie intentie voorspelling, in-/ uitschakelen van **Alles weergeven** naar **alleen fouten** in de werkbalk. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filteren op entiteitstype op intentie pagina
Gebruik de **entiteit filters** vervolgkeuzelijst op de werkbalk om de uitingen filteren op entiteit. 

![Schermafbeelding van de intenties pagina met entiteit type filter gemarkeerd](./media/luis-how-to-add-intents/filter-by-entities.png) 

Als u wilt verwijderen van het filter, selecteert u het blauwe filtervak met dat woord of woordgroep onder de werkbalk.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Overschakelen naar de token weergave op intentie pagina
In-/ uitschakelen **Tokens weergave** om de tokens in plaats van de namen van het type entiteit weer te geven. Op het toetsenbord, kunt u ook gebruiken **besturingselement + E** in de weergave-of uitschakelen. 

![Schermafbeelding van BookFlight doel met Token weergeven die zijn gemarkeerd](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Uw app na het wijzigen van model met intents trainen
Na het toevoegen, bewerken of verwijderen van intents, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Na het intents toevoegen aan uw app, uw volgende taak bestaat uit om te beginnen met het toevoegen [voorbeeld uitingen](luis-how-to-add-example-utterances.md) voor de die u hebt toegevoegd. 
