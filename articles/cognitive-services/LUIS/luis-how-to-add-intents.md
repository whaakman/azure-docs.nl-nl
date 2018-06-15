---
title: Intents in LUIS toepassingen toevoegen | Microsoft Docs
description: Language Understanding (LUIS) om toe te voegen intents om apps gebruikersaanvragen begrijpen en reageren op deze correct te gebruiken.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.service: cognitive-services
ms.openlocfilehash: 6e013e994a3bcb60c3104aa10cd7bad1535706f1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345868"
---
# <a name="manage-intents"></a>Intents beheren 
Voeg [intents](luis-concept-intent.md) aan uw app LUIS voor het identificeren van groepen van vragen of opdrachten die het hetzelfde doel hebben. 

U toevoegen en beheren van uw intents uit de **Intents** pagina beschikbaar zijn vanuit **Intents** in het linkerpaneel van LUIS. 

De volgende procedure laat zien hoe de bedoeling 'Bookflight' in de app TravelAgent toevoegen.

## <a name="add-intent"></a>Doel toevoegen

1. Uw app (bijvoorbeeld TravelAgent) openen door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **Intents** in het linkerdeelvenster. 
2. Op de **Intents** pagina, klikt u op **maken van nieuwe bedoeling**.

    ![Intents lijst](./media/luis-how-to-add-intents/IntentsList.png)
3. In de **maken van nieuwe bedoeling** in het dialoogvenster, typ de bedoeling naam 'BookFlight' en klik op **gedaan**.

    ![Doel toevoegen](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Op de pagina opzet details van de zojuist toegevoegde bedoeling [utterances toevoegen](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Wijzig de naam van doel

1. Op de **bedoeling** pagina, klikt u op het pictogram Rename ![bedoeling wijzigen](./media/luis-how-to-add-intents/Rename-Intent-btn.png) naast de naam van de opzet. 

2. Op de **bedoeling** pagina de naam van de huidige opzet wordt weergegeven in het dialoogvenster. Bewerk de naam van de opzet en druk op enter. De nieuwe naam wordt opgeslagen en weergegeven op de pagina met opzet.

    ![Bedoeling bewerken](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Bedoeling verwijderen
Wanneer u de opzet dan de opzet geen verwijdert, kunt u alle utterances toevoegen aan de opzet geen. Dit is handig als u wilt verplaatsen van de utterances in plaats van deze worden verwijderd.   

1. Op de **bedoeling** pagina, klikt u op de **bedoeling verwijderen** knop naast het recht van de naam van de opzet. 

    ![Opzet knop verwijderen](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Klik op de knop 'Ok' in het dialoogvenster voor bevestiging.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Een utterance op opzet pagina toevoegen

Voer op de pagina opzet een relevante utterance u van uw gebruikers, zoals verwachten `book 2 adult business tickets to Paris tomorrow on Air France` in het tekstvak onder de naam van de opzet en druk op Enter. 
 
>[!NOTE]
>Alle utterances converteert LUIS naar kleine letters.

![Schermopname van Intents pagina met details met utterance gemarkeerd](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Utterances worden toegevoegd aan de lijst met utterances voor het huidige doel. Wanneer een utterance is toegevoegd, [label alle entiteiten](luis-how-to-add-example-utterances.md) binnen de utterances en [trainen](luis-how-to-train.md) uw app. 

## <a name="create-a-pattern-from-an-utterance"></a>Een patroon van een utterance maken
Zie [toevoegen patroon van bestaande utterance op pagina bedoeling of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Een utterance op opzet pagina bewerken

Selecteer het pictogram van de drie puntjes (...) aan de rechterkant van de lijn voor die utterance een utterance bewerken, en selecteer vervolgens **bewerken**. Wijzig de tekst en druk op Enter op het toetsenbord.

![Schermopname van Intents pagina met details met drie punten pictogram gemarkeerd](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Utterances op opzet pagina opnieuw toewijzen
U kunt de bedoeling van een of meer utterances wijzigen door ze opnieuw toewijzen aan een ander doel. 

Als u wilt toewijzen van een enkele utterance naar een ander doel, aan de rechterkant van de utterance rij, selecteert u de naam van de juiste opzet onder de **bedoeling gelabeld** kolom. De utterance is verwijderd uit de bedoeling utterance lijst. 

![Schermopname van BookFlight opzet pagina met een utterance opzet onder Labeled opzet kolom geselecteerd](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Als u wilt wijzigen de bedoeling van verschillende utterances, schakel de selectievakjes in aan de linkerkant van de utterances en selecteer vervolgens **opnieuw toewijzen bedoeling**. Selecteer het juiste doel in de lijst.

![Schermopname van BookFlight opzet pagina met een utterance gecontroleerd en het opnieuw toewijzen opzet knop gemarkeerd](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Utterances op opzet pagina verwijderen

Als u wilt verwijderen een utterance, selecteer het pictogram van de drie puntjes (...) aan de rechterkant van de lijn voor die utterance en selecteer vervolgens **verwijderen**. De utterance wordt verwijderd uit de lijst en de app LUIS.

![Schermopname van Intents pagina met details met de optie verwijderen gemarkeerd](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Verschillende utterances verwijderen:

1. Schakel de selectievakjes in aan de linkerkant van de utterances en selecteer vervolgens **utterances (s) verwijderen**. 

    ![Schermopname van Intents pagina met details met utterances gecontroleerd en utterance(s) knop gemarkeerd verwijderen](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Selecteer **gedaan** in de **utterances verwijderen?** pop-upvenster.

## <a name="search-in-utterances-on-intent-page"></a>Zoeken in de utterances op opzet pagina
In de opzet, kunt u zoeken naar utterances die tekst (woorden of zinnen) bevatten. Bijvoorbeeld, ziet u mogelijk een foutbericht weergegeven dat betrekking heeft op een bepaald woord en wordt gezocht naar alle voorbeelden die dat bepaalde woord bevatten. 

1. Selecteer het vergrootglaspictogram in de werkbalk.

    ![Schermopname van Intents pagina met zoekpictogram van Vergrootglas gemarkeerd](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Een Zoektekstvak wordt weergegeven. Typ het woord of zinsdeel in het zoekvak op de rechterbovenhoek van de lijst utterances. De utterances lijst van updates, zodat alleen de utterances waaronder de zoektekst. 

    ![Schermopname van Intents pagina met Zoektekstvak gemarkeerd](./media/luis-how-to-add-intents/search-textbox.png)

    De zoekopdracht te annuleren en de volledige lijst met utterances terugzetten, verwijderen van de zoektekst die u hebt getypt. Als u wilt sluiten in het Zoektekstvak, selecteer u het pictogram Vergrootglas opnieuw op de werkbalk.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Voorspelling discrepantie fouten op opzet pagina
Een utterance in de opzet wellicht een discrepantie tussen het geselecteerde doel en de score van de voorspelling. LUIS geeft aan dat dit verschil met een rood kader rondom de score. 

![Schermafbeelding van de intentie van de BookFlight pagina met voorspelling discrepantie score gemarkeerd](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filteren op opzet voorspelling discrepantie fouten op opzet pagina
Filter de lijst utterance alleen utterances met een discrepantie opzet voorspelling: in-of uitschakelen van **Alles weergeven** naar **alleen fouten** op de werkbalk. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filteren op entiteittype op opzet pagina
Gebruik de **entiteit filters** vervolgkeuzelijst op de werkbalk om de utterances filteren op entiteit. 

![Schermopname van Intents pagina met entiteit type filter gemarkeerd](./media/luis-how-to-add-intents/filter-by-entities.png) 

U verwijdert het filter, schakelt u het in blauw filter met dat woord of woordgroep onder de werkbalk.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Overschakelen naar de token weergave op opzet pagina
Wisselknop **Tokens weergave** om de tokens in plaats van de namen van het type entiteit weer te geven. Op het toetsenbord, kunt u ook gebruiken **besturingselement + E** in de weergave-of uitschakelen. 

![Schermopname van BookFlight hetzelfde doel aan Token weergave gemarkeerd](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Training van uw app na het model met intents wijzigen
Nadat u toevoegen, bewerken of verwijderen van intents, [trainen](luis-how-to-train.md) en [publiceren](PublishApp.md) uw app om uw wijzigingen te invloed hebben op eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Na het intents toevoegen aan uw app, is uw volgende taak om toe te voegen [voorbeeld utterances](luis-how-to-add-example-utterances.md) voor de die u hebt toegevoegd. 
