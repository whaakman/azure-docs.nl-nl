---
title: Patronen toevoegen nauwkeurigheid
titleSuffix: Language Understanding - Azure Cognitive Services
description: Patroon sjablonen ter verbetering van nauwkeurigheid in Language Understanding (LUIS)-toepassingen toevoegen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 202b9632b7a7faaf955874a0300edbe5134b7fa1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60196660"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Patronen voor het verbeteren van nauwkeurigheid toevoegen
Nadat een LUIS-app eindpunt uitingen ontvangt, gebruikt u een [patroon](luis-concept-patterns.md) voor het verbeteren van nauwkeurigheid voor uitingen dat een patroon in de woordvolgorde en word keuze. Patronen gebruiken specifieke [syntaxis](luis-concept-patterns.md#pattern-syntax) om aan te geven van de locatie van: [entiteiten](luis-concept-entity-types.md), entiteit [rollen](luis-concept-roles.md), en optionele tekst.

## <a name="add-template-utterance-to-create-pattern"></a>Sjabloon utterance voor het maken van patroon toevoegen
1. Open uw app door het selecteren van de naam ervan op **mijn Apps** pagina en selecteer vervolgens **patronen** in het linkerdeelvenster onder **verbeterde app-prestaties**.

    ![Schermafbeelding van de lijst met patronen](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Selecteer het juiste type voor het patroon. 

    ![Doel selecteren](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. In het tekstvak sjabloon, typt u de sjabloon utterance en selecteer Enter. Als u de naam van de entiteit opgeven wilt, gebruikt u de juiste patroon entiteit-syntaxis. Beginnen met de syntaxis van de entiteit met `{`. De lijst met entiteiten weergegeven. Selecteer de juiste entiteit en selecteer vervolgens Enter. 

    ![Schermafbeelding van de entiteit voor het patroon](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Als uw entiteit bevat een [rol](luis-concept-roles.md), geven de rol met een één dubbele punt, `:`, nadat de entiteit-naam, zoals `{Location:Origin}`. De lijst met functies voor de entiteiten in een lijst weergegeven. Selecteer de rol en selecteer vervolgens Enter. 

    ![Schermafbeelding van de entiteit met de rol](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Nadat u de juiste entiteit hebt geselecteerd, klaar bent met het invoeren van het patroon en selecteer vervolgens Enter. Wanneer u klaar bent u patronen [trainen](luis-how-to-train.md) uw app.

    ![Schermopname van het opgegeven patroon met beide typen entiteiten](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Uw app trainen na het wijzigen van model met patronen
Nadat u toevoegen, bewerken, verwijderen of opnieuw toewijzen van een patroon [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>Contextuele werkbalk gebruiken

De contextuele werkbalk boven de lijst met patronen kunt u:

* Zoeken naar patronen
* Een patroon bewerken
* Opnieuw toewijzen van afzonderlijke patroon naar een ander doel
* Opnieuw toewijzen van verschillende patronen naar een ander doel
* Delete a single patroon
* Verschillende patronen verwijderen
* Patroon lijst filteren door entiteit
* Filter-pattern-list-by-intent
* Entiteit of intentie filter verwijderen
* Patroon van bestaande utterance op de pagina kunt u lezen wat of entiteit toevoegen

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [bouwen van een patroon](luis-tutorial-pattern.md) met een pattern.any en rollen met een zelfstudie.
* Meer informatie over het [trainen](luis-how-to-train.md) uw app.
