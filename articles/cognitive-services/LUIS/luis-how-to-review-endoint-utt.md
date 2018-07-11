---
title: Voorgestelde uitingen van LUIS label | Microsoft Docs
description: Language Understanding (LUIS) gebruiken om te labelen voorgestelde uitingen en helpen verhogen actieve machine learning.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2017
ms.author: v-geberr
ms.openlocfilehash: c695aed4bc6d2553c719f8dfe664634e2f3b2613
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37966846"
---
# <a name="review-endpoint-utterances"></a>Eindpuntuitingen controleren

De functie baanbrekende van LUIS is de [concept](luis-concept-review-endpoint-utterances.md) van actief leren. Zodra uw LUIS eindpunt-query's heeft, LUIS gebruikt active leren om de kwaliteit van de resultaten te verbeteren. In het proces actief leren, LUIS onderzoekt alle uitingen van het eindpunt en uitingen die het niet zeker weet is geselecteerd. Als u deze uitingen label, trainen en publiceren, en vervolgens LUIS uitingen nauwkeuriger identificeert. 

## <a name="filter-utterances"></a>Uitingen filteren
1. Open uw app (bijvoorbeeld TravelAgent) door te selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **bouwen** in de bovenste balk.

2. Onder de **verbeterde app-prestaties**, selecteer **bekijken eindpunt uitingen**.

    ![Uitingen bekijken](./media/label-suggested-utterances/review.png)

3. Op de **bekijken eindpunt uitingen** weergeeft, schakelt u in de **filterlijst met opzet of entiteit** in het tekstvak. Deze vervolgkeuzelijst bevat alle intents onder **INTENTS** en alle entiteiten in **entiteiten**.

    ![Uitingen filteren](./media/label-suggested-utterances/filter.png)

4. Selecteer een categorie (intents of entiteiten) in de vervolgkeuzelijst en bekijk de uitingen.

    ![Intentie uitingen](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Label-entiteiten
LUIS vervangt entiteit tokens (woorden) met de namen van entiteiten in het blauw is gemarkeerd. Als een utterance is zonder label entiteiten, kunt u ze in de utterance label. 

1. Selecteer op de woorden in de utterance. 

2. Selecteer een entiteit in de lijst.

    ![Label-entiteit](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Één utterance uitlijnen

Elke utterance heeft een voorgestelde bedoeling weergegeven in de **uitgelijnd bedoeling** kolom. 

1. Als u akkoord met deze suggestie gaat, selecteert u op het vinkje hebt geklikt.

    ![Uitgelijnde intentie behouden](./media/label-suggested-utterances/align-intent-check.png)

2. Als u niet eens met het voorstel bent, selecteert u het juiste doel in de uitgelijnde, intentie vervolgkeuzelijst en selecteer op het vinkje aan de rechterkant van het uitgelijnde doel. 

    ![Bedoeling uitlijnen](./media/label-suggested-utterances/align-intent.png)

3. Nadat u hebt geselecteerd op het vinkje hebt geklikt, wordt de utterance verwijderd uit de lijst. 

## <a name="align-several-utterances"></a>Verschillende uitingen uitlijnen

Om te worden uitgelijnd verschillende uitingen, schakel het selectievakje aan de linkerkant van de uitingen en selecteer vervolgens op de **toevoegen geselecteerd** knop. 

![Verschillende uitlijnen](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Controleer of uitgelijnde doel
U kunt controleren of de utterance is afgestemd op de juiste intentie door te gaan naar de **Intents** pagina, selecteert u de naam van de intentie en controleren van de uitingen. De utterance van **bekijken eindpunt uitingen** is in de lijst.

## <a name="delete-utterance"></a>Utterance verwijderen
Elke utterance kan worden verwijderd uit de lijst controleren. Zodra verwijderd, wordt deze niet weergegeven in de lijst met het opnieuw. Dit geldt zelfs als de gebruiker de dezelfde utterance van het eindpunt voert. 

Als u niet zeker of u de utterance verwijdert, verplaatsen naar de intentie, geen of maak een nieuw doel, zoals "gemengde" en de utterance verplaatsen naar dit doel. 

## <a name="delete-several-utterances"></a>Verschillende uitingen verwijderen
Als u wilt verwijderen van verschillende uitingen, selecteert u elk item en selecteer in de Prullenbak rechts van de **toevoegen geselecteerd** knop.

![Verschillende verwijderen](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Volgende stappen

Als u wilt testen hoe prestaties worden verbeterd wanneer u de voorgestelde uitingen labels, kunt u de testconsole openen door het selecteren van **testen** in het bovenste deelvenster. Zie voor instructies over het testen van uw app met behulp van de testconsole [trainen en testen van uw app](interactive-test.md).
