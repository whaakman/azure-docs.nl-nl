---
title: Gebruiker uitingen bekijken
titleSuffix: Language Understanding - Azure Cognitive Services
description: Actief leren legt eindpunt query's en van de gebruiker eindpunt uitingen die het niet zeker weet is geselecteerd. U bekijken deze uitingen voor het selecteren van het doel en markeren van entiteiten voor deze lezen-wereld-uitingen. Deze wijzigingen in uw voorbeeld-uitingen accepteren en vervolgens trainen en publiceren. LUIS identificeert dan uitingen nauwkeuriger.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: 8fac360682ef11c438cdec333fac21d6f8cfc117
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895903"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal-for-active-learning"></a>Over het bekijken van de eindpunt-uitingen in portal LUIS, actief leren

[Actief leren](luis-concept-review-endpoint-utterances.md) vastgelegd eindpunt query's en van de gebruiker eindpunt uitingen die het niet zeker weet is geselecteerd. U bekijken deze uitingen voor het selecteren van het doel en markeren van entiteiten voor deze lezen-wereld-uitingen. Deze wijzigingen in uw voorbeeld-uitingen accepteren en vervolgens trainen en publiceren. LUIS identificeert dan uitingen nauwkeuriger.


## <a name="enable-active-learning"></a>Actief leren inschakelen

Om in te schakelen actief leren, meld u query's van gebruikers. Dit wordt bereikt door het instellen van de [eindpunt query](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) met de `log=true` querystring-parameter en waarde.

## <a name="disable-active-learning"></a>Actief leren uitschakelen

Als u wilt uitschakelen actief leren, meld u niet query's van gebruikers. Dit wordt bereikt door het instellen van de [eindpunt query](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) met de `log=false` querystring-parameter en waarde.

## <a name="filter-utterances"></a>Uitingen filteren

1. Open uw app (bijvoorbeeld TravelAgent) door te selecteren van de naam op **mijn Apps** pagina en selecteer vervolgens **bouwen** in de bovenste balk.

1. Onder de **verbeterde app-prestaties**, selecteer **bekijken eindpunt uitingen**.

1. Op de **bekijken eindpunt uitingen** weergeeft, schakelt u in de **filterlijst met opzet of entiteit** in het tekstvak. Deze vervolgkeuzelijst bevat alle intents onder **INTENTS** en alle entiteiten in **entiteiten**.

    ![Uitingen filteren](./media/label-suggested-utterances/filter.png)

1. Selecteer een categorie (intents of entiteiten) in de vervolgkeuzelijst en bekijk de uitingen.

    ![Intentie uitingen](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Label-entiteiten
LUIS vervangt entiteit tokens (woorden) met de namen van entiteiten in het blauw is gemarkeerd. Als een utterance is zonder label entiteiten, kunt u ze in de utterance label. 

1. Selecteer op de woorden in de utterance. 

1. Selecteer een entiteit in de lijst.

    ![Label-entiteit](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Één utterance uitlijnen

Elke utterance heeft een voorgestelde bedoeling weergegeven in de **uitgelijnd bedoeling** kolom. 

1. Als u akkoord met deze suggestie gaat, selecteert u op het vinkje hebt geklikt.

    ![Uitgelijnde intentie behouden](./media/label-suggested-utterances/align-intent-check.png)

1. Als u niet eens met het voorstel bent, selecteert u het juiste doel in de uitgelijnde, intentie vervolgkeuzelijst en selecteer op het vinkje aan de rechterkant van het uitgelijnde doel. 

    ![Bedoeling uitlijnen](./media/label-suggested-utterances/align-intent.png)

1. Nadat u hebt geselecteerd op het vinkje hebt geklikt, wordt de utterance verwijderd uit de lijst. 

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

Als u wilt testen hoe prestaties worden verbeterd wanneer u de voorgestelde uitingen labels, kunt u de testconsole openen door het selecteren van **testen** in het bovenste deelvenster. Zie voor instructies over het testen van uw app met behulp van de testconsole [trainen en testen van uw app](luis-interactive-test.md).
