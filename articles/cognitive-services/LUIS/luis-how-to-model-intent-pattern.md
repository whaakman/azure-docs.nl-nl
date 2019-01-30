---
title: Patronen toevoegen nauwkeurigheid
titleSuffix: Language Understanding - Azure Cognitive Services
description: Informatie over het toevoegen van patroon-sjablonen in toepassingen voor het verbeteren van nauwkeurigheid Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 46b7e7757c2bf1b01bebeff059f0028b9e9d189c
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220753"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Patronen voor het verbeteren van nauwkeurigheid toevoegen
Nadat een LUIS-app eindpunt uitingen ontvangt, gebruikt u de [concept](luis-concept-patterns.md) van patronen voor het verbeteren van nauwkeurigheid voor uitingen dat een patroon in de woordvolgorde en word keuze. Gebruik patronen [entiteiten](luis-concept-entity-types.md) en hun rollen om gegevens met behulp van specifieke patroon syntaxis te extraheren. 

## <a name="add-template-utterance-to-create-pattern"></a>Sjabloon utterance voor het maken van patroon toevoegen
1. Open uw app door het selecteren van de naam ervan op **mijn Apps** pagina en selecteer vervolgens **patronen** in het linkerdeelvenster onder **verbeterde app-prestaties**.

    ![Schermafbeelding van de lijst met patronen](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Selecteer het juiste type voor het patroon. 

    ![Doel selecteren](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. In het tekstvak sjabloon, typt u de sjabloon utterance en selecteer Enter. Als u de naam van de entiteit opgeven wilt, gebruikt u de juiste patroon entiteit-syntaxis. Beginnen met de syntaxis van de entiteit met `{`. De lijst met entiteiten weergegeven. Selecteer de juiste entiteit en selecteer vervolgens Enter. 

    ![Schermafbeelding van de entiteit voor het patroon](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Als uw entiteit een rol bevat, geeft u de functie met een één dubbele punt, `:`, nadat de entiteit-naam, zoals `{Location:Origin}`. De lijst met functies voor de entiteiten in een lijst weergegeven. Selecteer de rol en selecteer vervolgens Enter. 

    ![Schermafbeelding van de entiteit met de rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Nadat u de juiste entiteit hebt geselecteerd, klaar bent met het invoeren van het patroon en selecteer vervolgens Enter. Wanneer u klaar bent u patronen [trainen](luis-how-to-train.md) uw app.

    ![Schermopname van het opgegeven patroon met beide typen entiteiten](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Zoekpatronen
Zoeken, kunt u het vinden van patronen die bepaalde tekst bevatten.  

1. Selecteer het pictogram met Vergrootglas.

    ![Schermafbeelding van de patronen-pagina met hulpprogramma zoekpictogram gemarkeerd](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Typ de zoektekst in het zoekvak in de rechterbovenhoek van de lijst met patronen en selecteer Enter. De lijst met patronen wordt bijgewerkt om alleen de patronen met inbegrip van uw zoektekst weer te geven.

    ![Schermafbeelding van de patronen-pagina met zoeken naar tekst in het zoekvak gemarkeerd](./media/luis-how-to-model-intent-pattern/search-text.png)

    De zoekopdracht te annuleren en herstellen van de volledige lijst met patronen, verwijderen van de tekst die u hebt getypt.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Een patroon bewerken
1. Als u wilt bewerken een patroon, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de regel voor dit patroon knop en selecteer vervolgens **bewerken**. 

    ![Menu-item in de rij patroon Screenshot van bewerken](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Voer alle wijzigingen in het tekstvak in. Wanneer u klaar bent, voert u selecteert. Wanneer u klaar bent bewerken patronen [trainen](luis-how-to-train.md) uw app.

    ![Schermafbeelding van het patroon bewerken](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Opnieuw toewijzen van afzonderlijke patroon naar een ander doel

Als u wilt toewijzen van een enkel patroon naar een ander doel, selecteert u de intentie keuzelijst met invoervak rechts van de tekst van het patroon en selecteer een ander doel.

![Schermopname van het opnieuw toewijzen van afzonderlijke patroon naar een ander doel](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Opnieuw toewijzen van verschillende patronen naar een ander doel

Als u wilt toewijzen verschillende patronen naar een ander doel, schakelt u het selectievakje links van elk patroon of schakel het selectievakje in de bovenste. De **opnieuw toewijzen van intentie** optie wordt weergegeven op de werkbalk. Selecteer het juiste type voor de patronen. 

![Schermopname van het opnieuw toewijzen van verschillende patronen naar een ander doel](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Een enkel patroon verwijderen

1. Als u wilt verwijderen een patroon, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de regel voor dit patroon knop en selecteer vervolgens **verwijderen**. 

    ![Schermafbeelding van verwijderen utterance](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Selecteer **Ok** om de verwijdering te bevestigen.

    ![Schermafbeelding van verwijderen bevestigen](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Verschillende patronen verwijderen

1. Als u wilt verwijderen van verschillende patronen, schakelt u het selectievakje links van elk patroon of schakel het selectievakje in de bovenste. De **patronen (s) verwijderen** optie wordt weergegeven op de werkbalk. Selecteer **patronen (s) verwijderen**.  

    ![Schermopname van het verwijderen van verschillende patronen](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. De **patronen verwijderen** bevestigingsvenster weergegeven. Selecteer **Ok** voltooien van de verwijdering.

    ![Schermopname van het verwijderen van verschillende patronen bevestigingsvenster](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Patroon lijst filteren door entiteit

Als u wilt de lijst met patronen filteren op een bepaalde entiteit, selecteer **entiteit filters** in de werkbalk boven de patronen. 

![Schermopname van het filteren van patronen door entiteit](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Nadat het filter wordt toegepast, wordt de naam van de entiteit wordt weergegeven onder de werkbalk. 

## <a name="filter-pattern-list-by-intent"></a>Patroon lijst filteren op doel

Als u wilt de lijst met patronen filteren op een bepaald doel, selecteer **Intent-filters** in de werkbalk boven de patronen. 

![Schermafbeelding van patronen te filteren op doel](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Nadat het filter wordt toegepast, wordt de naam van de intentie wordt weergegeven onder de werkbalk. 

## <a name="remove-entity-or-intent-filter"></a>Entiteit of intentie filter verwijderen
Wanneer het patroon lijst is gefilterd, de entiteit of de naam van de intentie wordt weergegeven onder de werkbalk. Als u wilt verwijderen van het filter, de naam te selecteren.

![Schermafbeelding van het verwijderen van entiteit van filter](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Het filter verwijderd en alle patronen weer te geven. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Patroon van bestaande utterance op de pagina kunt u lezen wat of entiteit toevoegen
U kunt een patroon maken van een bestaande utterance in elk de **bedoeling** of **entiteit** pagina. Alle uitingen op een willekeurige pagina doel of de entiteit worden weergegeven in een lijst met in de rechterkolom bevindt die toegang geeft tot utterance-opties zoals **bewerken**, **verwijderen**, en **als patroontoevoegen**.

1. Op de geselecteerde rij van de utterance, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de utterance en selecteer **toevoegen als patroon**.

    [![Schermafbeelding van de tabel met Add-patroon gemarkeerd in het optiemenu uitingen](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "schermafbeelding van de tabel uitingen met Add-patroon gemarkeerd in het optiemenu")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Wijzigen van het patroon volgens de [syntaxisregels](luis-concept-patterns.md#pattern-syntax). Als de utterance die u hebt geselecteerd, wordt aangeduid met entiteiten, zijn deze entiteiten al in het patroon met de juiste syntaxis.

    ![Schermafbeelding van gefilterde patronen van entiteit](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Uw app trainen na het wijzigen van model met patronen
Nadat u toevoegen, bewerken, verwijderen of opnieuw toewijzen van een patroon [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [bouwen van een patroon](luis-tutorial-pattern.md) met een pattern.any en rollen.
* Meer informatie over het [trainen](luis-how-to-train.md) uw app.
