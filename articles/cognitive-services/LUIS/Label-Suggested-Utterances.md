---
title: Voorgestelde utterances met LUIS label | Microsoft Docs
description: Language Understanding (LUIS) te labelen voorgestelde utterances en versterking actieve machine learning te gebruiken.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2017
ms.author: v-geberr
ms.openlocfilehash: 91a2fe738743d359677392bfb79aac885702b440
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350081"
---
# <a name="review-endpoint-utterances"></a>Eindpunt utterances controleren

De functie innovatieve van LUIS is de [concept](luis-concept-review-endpoint-utterances.md) van actieve leren. Als uw LUIS eindpunt-query's heeft, LUIS maakt gebruik van actieve leren om de kwaliteit van de resultaten te verbeteren. In het actieve leerproces LUIS alle endpoint-utterances moet worden gecontroleerd en utterances dat het niet zeker weet wat selecteert. Als u deze utterances label, trainen en te publiceren, en vervolgens LUIS utterances nauwkeuriger identificeert. 

## <a name="filter-utterances"></a>Filter utterances
1. Open uw app (bijvoorbeeld TravelAgent) door het selecteren van de naam ervan op **mijn Apps** pagina en selecteer vervolgens **bouwen** in de bovenste balk.

2. Onder de **app sneller**, selecteer **bekijken eindpunt utterances**.

    ![Utterances controleren](./media/label-suggested-utterances/review.png)

3. Op de **bekijken eindpunt utterances** pagina in de **filterlijst met opzet of entiteit** in het tekstvak. Deze vervolgkeuzelijst bevat alle intents onder **INTENTS** en alle entiteiten onder **entiteiten**.

    ![Utterances filteren](./media/label-suggested-utterances/filter.png)

4. Selecteer een categorie (intents of entiteiten) in de vervolgkeuzelijst en bekijk de utterances.

    ![Opzet utterances](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Label entiteiten
Entiteit-tokens (woorden) vervangt LUIS door de namen van entiteiten in blauw is gemarkeerd. Als een utterance heeft zonder label entiteiten, kunt u ze in de utterance label. 

1. Selecteer op de tekst in de utterance. 

2. Selecteer een entiteit in de lijst.

    ![Label entiteit](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Één utterance uitlijnen

Elke utterance heeft een voorgestelde opzet weergegeven in de **uitgelijnd bedoeling** kolom. 

1. Als u akkoord met suggestie gaat, selecteert u op het vinkje.

    ![Uitgelijnde bedoeling houden](./media/label-suggested-utterances/align-intent-check.png)

2. Als u niet akkoord met de suggestie, selecteert u het juiste doel in de uitgelijnde, opzet vervolgkeuzelijst en selecteer vervolgens op het vinkje rechts van de bedoeling uitgelijnd. 

    ![Bedoeling uitlijnen](./media/label-suggested-utterances/align-intent.png)

3. Nadat u hebt geselecteerd op het selectievakje is ingeschakeld, wordt de utterance verwijderd uit de lijst. 

## <a name="align-several-utterances"></a>Verschillende utterances uitlijnen

Om te worden uitgelijnd verschillende utterances, schakel het selectievakje links van de utterances en selecteer vervolgens op de **toevoegen geselecteerd** knop. 

![Verschillende uitlijnen](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Controleer of uitgelijnde bedoeling
U kunt controleren of de utterance is afgestemd op de juiste bedoeling door te gaan naar de **Intents** pagina, selecteert u de naam van de opzet en controleren van de utterances. De utterance van **bekijken eindpunt utterances** staat in de lijst.

## <a name="delete-utterance"></a>Utterance verwijderen
Elke utterance kan worden verwijderd uit de lijst controleren. Wanneer verwijderd, wordt niet weergegeven in de lijst opnieuw. Dit geldt zelfs als de gebruiker de dezelfde utterance van het eindpunt voert. 

Als u zeker weet of u de utterance moet verwijderen, verplaatsen naar de opzet, geen of maak een nieuwe bedoeling zoals 'diverse' en de utterance naar die intentie verplaatsen. 

## <a name="delete-several-utterances"></a>Verschillende utterances verwijderen
Voor het verwijderen van verschillende utterances, selecteert u elk item en selecteer in de Prullenbak rechts van de **toevoegen geselecteerd** knop.

![Verschillende verwijderen](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Volgende stappen

Als u wilt testen hoe prestaties worden verbeterd wanneer u de voorgestelde utterances labels, kunt u de test-console openen door te selecteren **testen** in het bovenste deelvenster. Zie voor instructies voor het testen van uw app met de console test [Train en uw app testen](Train-Test.md).
