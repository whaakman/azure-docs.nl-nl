---
title: Het gebruik van Negatiable-entiteiten met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van Negatiable-entiteiten met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 4edbfc396d4ab32b991b084a7b738f04b5205418
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704109"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Negatie-entiteiten gebruiken met een Conversation Learner model

In deze zelf studie wordt de eigenschap ' Negatiable ' van entiteiten gedemonstreerd.

## <a name="video"></a>Video

[![Preview van zelf studie voor negatie-entiteiten](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Met de eigenschap ' Negatiable ' van een entiteit kunt u een label maken van zowel normale (positieve) als negatieve exemplaren van de entiteit, op basis van positieve en negatieve modellen en de waarde van een bestaande entiteit wissen. Entiteiten waarvoor de eigenschap ' Negatiable ' is ingesteld, worden genegeerde entiteiten genoemd in de conversatie-leaner.

## <a name="steps"></a>Stappen

Start op de start pagina in de gebruikers interface van het web.

### <a name="create-the-model"></a>Het model maken

1. Selecteer **Nieuw model**.
2. Voer **NegatableEntity** in als **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linkerdeel venster en vervolgens **nieuwe entiteit**.
2. Selecteer **aangepast** getraind voor **entiteits type**.
3. Voer een **naam** in voor de naam van de **entiteit**.
4. Controleer **negatie** om gebruikers in staat te stellen een entiteits waarde op te geven of iets anders is dan de waarde van een entiteit, waardoor de overeenkomende entiteits *waarde wordt verwijderd* .
5. Selecteer **Maken**.

![](../media/T06_entity_create.png)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Voer **de naam in die ik niet weet.** **reactie van bot...** .
3. Voer een **naam** in voor diskwalificerende recht.
4. Selecteer **Maken**.

![](../media/T06_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Voer **de naam in die ik weet. Het is $name.** **reactie van bot...** .
3. Selecteer **Maken**.

> [!NOTE]
> De **naam** entiteit is automatisch toegevoegd als **vereiste entiteiten** op referentie in het antwoord utterance.

U hebt nu twee acties.

![](../media/T06_action_create_2.png)

### <a name="train-the-model"></a>Het model trainen

1. Selecteer **dialoog vensters trainen** in het linkerpaneel en klik vervolgens op **Nieuw trein dialoog venster**.
2. Voer **Hallo** in voor de utterance van de gebruiker in het linkerdeel venster.
3. Selecteer **Score acties**.
4. Selecteer **Ik heb uw naam niet kennen.** uit de lijst met acties. Het percentiel is 100% als de enige geldige actie op basis van de beperkingen.
5. **Mijn naam invoeren is Frank** voor de utterance van de gebruiker in het linkerdeel venster.
6. Markeer **Frank** en selecteer vervolgens **+ naam**. Niet-verwerkte entiteiten hebben twee instanties: (+) plus de waarde wordt toegevoegd of overschreven. (-) min de waarde wordt verwijderd.
7. Selecteer **Score acties**. De **naam** entiteit is nu gedefinieerd als **Frank** in het geheugen van het model **, dus ik weet uw naam. Het is $name** actie beschikbaar is.
8. Selecteer **ik ken je naam toe. Het is $name.** uit de lijst met acties.
9. **Mijn naam invoeren is niet Frank.** voor de utterance van de gebruiker in het linkerdeel venster.
10. Markeer **Frank** then select **-name** om de waarde van de entiteit **name** te wissen.
11. Selecteer **Score acties**.
12. Selecteer **Ik heb uw naam niet kennen.** uit de lijst met acties.
13. Voer **Mijn naam in als Susan.** voor de derde utterance van de gebruiker in het linkerdeel venster.
14. Selecteer **Susan** en **naam** 

![](../media/T06_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten met meerdere waarden](./07-multi-value-entities.md)
