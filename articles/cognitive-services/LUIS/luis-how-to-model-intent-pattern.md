---
title: Patroon sjablonen in LUIS apps toevoegen | Microsoft Docs
titleSuffix: Azure
description: Informatie over het patroon sjablonen in toepassingen Language Understanding (LUIS) om de nauwkeurigheid van de voorspelling te toevoegen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 68c0ea1fd3f2e60e0adec631f33c8bd09a3d9960
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35350061"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Patronen voor een betere nauwkeurigheid toevoegen
Nadat een app LUIS eindpunt utterances ontvangt, gebruikt u de [concept](luis-concept-patterns.md) van patronen voor het verbeteren van de nauwkeurigheid voor utterances die op basis van een patroon in word volgorde, en woord keuze duidelijk. Gebruik patronen [entiteiten](luis-concept-entity-types.md) en hun functies met specifieke patroon syntaxis gegevens moeten worden opgehaald. 

## <a name="add-template-utterance-to-create-pattern"></a>Sjabloon utterance voor het maken van patroon toevoegen
1. Uw app openen door te selecteren van de naam ervan op **mijn Apps** pagina en selecteer vervolgens **patronen** in het linkerdeelvenster onder **app sneller**.

    ![Schermafbeelding van de lijst met patronen](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Selecteer het juiste type voor het patroon. 

    ![Doel selecteren](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. In het tekstvak voor de sjabloon, typt u de sjabloon utterance en selecteer Enter. Als u wilt de entiteitsnaam op te geven, gebruikt u de syntaxis van de entiteit correcte patroon. Beginnen met de syntaxis van de entiteit met `{`. De lijst met entiteiten weergegeven. Selecteer de juiste entiteit en selecteer vervolgens op Enter. 

    ![Schermafbeelding van de entiteit voor patroon](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Als uw entiteit een rol bevat, geeft u de functie met een dubbele één `:`nadat de entiteit een naam geven, zoals `{Location:Origin}`. De lijst met rollen voor de entiteiten in een lijst weergegeven. Selecteer de rol en selecteer vervolgens op Enter. 

    ![Schermafbeelding van de entiteit met de rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Nadat u de juiste entiteit hebt geselecteerd, hebt ingevoerd, het patroon en schakelt u op Enter. Wanneer u klaar bent invoeren patronen [trainen](luis-how-to-train.md) uw app.

    ![Schermopname van het opgegeven patroon met beide typen entiteiten](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Zoekpatronen
Zoeken, kunt u zoeken naar patronen die de opgegeven tekst bevatten.  

1. Selecteer het pictogram Vergrootglas.

    ![Schermopname van patronen pagina met hulpprogramma zoekpictogram gemarkeerd](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Typ de zoektekst in het zoekvak op de rechterbovenhoek van de lijst met patronen en selecteer Enter. De lijst met patronen is bijgewerkt zodat alleen de patronen waaronder de zoektekst.

    ![Schermopname van patronen pagina met zoeken naar tekst in het zoekvak gemarkeerd](./media/luis-how-to-model-intent-pattern/search-text.png)

    De zoekopdracht te annuleren en herstellen van de volledige lijst met patronen, verwijderen van de tekst die u hebt getypt.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Een patroon bewerken
1. Als u wilt bewerken in een patroon, selecteer het pictogram van de drie puntjes (...) aan de rechterkant van de regel voor dit patroon en selecteer vervolgens **bewerken**. 

    ![Menu-item in de rij patroon schermafbeelding bewerken](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Breng de gewenste wijzigingen in het tekstvak. Wanneer u klaar bent, voert u selecteert. Wanneer u klaar bent bewerken patronen [trainen](luis-how-to-train.md) uw app.

    ![Schermopname van het patroon bewerken](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Opnieuw toewijzen van afzonderlijke patroon naar een ander doel

Opnieuw toewijzen van een enkel patroon naar een ander doel, schakelt u het in opzet lijst aan de rechterkant van de tekst van het patroon en selecteer een ander doel.

![Schermopname van afzonderlijke patroon naar een ander doel opnieuw toewijzen](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Opnieuw toewijzen van verschillende patronen naar een ander doel

Schakel het selectievakje links van elke patroon om toe te wijzen verschillende patronen naar een ander doel, of schakel het selectievakje in de bovenste. De **opnieuw toewijzen bedoeling** optie wordt weergegeven op de werkbalk. Selecteer het juiste type voor de patronen. 

![Schermopname van het opnieuw toewijzen van verschillende patronen naar een ander doel](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Een enkel patroon verwijderen

1. Als u wilt verwijderen een patroon, selecteer het pictogram van de drie puntjes (...) aan de rechterkant van de regel voor dit patroon en selecteer vervolgens **verwijderen**. 

    ![Schermopname van verwijderen utterance](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Selecteer **Ok** om de verwijdering te bevestigen.

    ![Schermopname van verwijderen bevestigen](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Verschillende patronen verwijderen

1. Schakel het selectievakje links van elke patroon of het bovenste selectievakje selecteert voor het verwijderen van verschillende patronen. De **patronen (s) verwijderen** optie wordt weergegeven op de werkbalk. Selecteer **patronen (s) verwijderen**.  

    ![Schermopname van het verwijderen van verschillende patronen](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. De **patronen verwijderen** bevestigingsvenster weergegeven. Selecteer **Ok** verwijderen voltooid.

    ![Schermopname van het verwijderen van verschillende patronen](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Patroon filterlijst door entiteit

U kunt de lijst met patronen filteren door een specifieke entiteit selecteren **entiteit filters** in de werkbalk boven de patronen. 

![Schermopname van het filteren van patronen met entiteit](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Nadat het filter is toegepast, wordt de naam van de entiteit weergegeven onder de werkbalk. 

## <a name="filter-pattern-list-by-intent"></a>Patroon lijst filteren op doel

Selecteer om de lijst met patronen filteren op een specifiek doel, **bedoeling filtert** in de werkbalk boven de patronen. 

![Schermopname van het filteren van patronen met opzet](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Nadat het filter is toegepast, wordt de naam van de opzet weergegeven onder de werkbalk. 

## <a name="remove-entity-or-intent-filter"></a>Verwijderen van entiteit of opzet filter
Wanneer de lijst patroon is gefilterd, wordt de entiteit of de naam van de opzet weergegeven onder de werkbalk. Selecteer de naam voor het verwijderen van het filter.

![Schermopname van gefilterde patronen door entiteit](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Het filter verwijderd en alle patronen weer te geven. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Patroon van bestaande utterance op doel of entiteit pagina toevoegen
U kunt een patroon maken van een bestaande utterance van een de **bedoeling** of **entiteit** pagina. Alle utterances op elke pagina doel of de entiteit worden weergegeven in een lijst met de rechterkolom toegang verlenen tot utterance niveau opties zoals **bewerken**, **verwijderen**, en **toevoegen als patroon**.

1. Selecteer de drie puntjes (...) rechts van de utterance op de geselecteerde rij van de utterance, en selecteer **toevoegen als patroon**.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Schermafbeelding van de tabel utterances met Add-patroon in het optiemenu gemarkeerd")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Wijzigen van het patroon volgens de [syntaxisregels](luis-concept-patterns.md#pattern-syntax). Als de geselecteerde utterance is gelabeld met entiteiten, zijn deze entiteiten al in het patroon met de juiste syntaxis.

    ![Schermopname van gefilterde patronen door entiteit](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Training van uw app na het model met patronen wijzigen
Nadat u toevoegen, bewerken, verwijderen of opnieuw toewijzen van een patroon [trainen](luis-how-to-train.md) en [publiceren](PublishApp.md) uw app om uw wijzigingen te invloed hebben op eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [bouwen van een patroon](luis-tutorial-pattern.md) met een pattern.any en rollen.
* Meer informatie over hoe [trainen](luis-how-to-train.md) uw app.