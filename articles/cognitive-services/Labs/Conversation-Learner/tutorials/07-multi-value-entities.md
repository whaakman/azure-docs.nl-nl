---
title: Entiteiten met meerdere waarden gebruiken met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van entiteiten met meerdere waarden met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 7911dd561da0f1e7b62b1b457ae5b059d5d54767
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704090"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Entiteiten met meerdere waarden gebruiken met een Conversation Learner model
In deze zelf studie wordt de eigenschap met meerdere waarden van entiteiten weer gegeven.

## <a name="video"></a>Video

[![Preview-versie van zelf studie voor Multi-Value-entiteiten](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Met meerdere waarden worden waarden in een lijst verzameld, in plaats van een enkele waarde op te slaan.  Deze entiteiten zijn handig wanneer gebruikers meer dan één waarde kunnen opgeven. Toppings voor een pizza.

Entiteiten die als meerdere waarden zijn gemarkeerd, hebben elk herkend exemplaar van de entiteit toegevoegd aan een lijst in het geheugen van de bot. De volgende herkenning wordt toegevoegd aan de waarde van de entiteit, in plaats van te worden overschreven.

## <a name="steps"></a>Stappen

Start op de start pagina in de gebruikers interface van het web.

### <a name="create-the-model"></a>Het model maken

1. Selecteer **Nieuw model**.
2. Voer **MultiValueEntities** in als **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linkerdeel venster en vervolgens **nieuwe entiteit**.
2. Selecteer **aangepast** getraind voor **entiteits type**.
3. Voer **toppings** in als **entiteits naam**.
4. Controleer op **meerdere waarden** om de entiteit in te scha kelen voor een of meer waarden.
5. Controleer **negatiable**.
6. Selecteer **Maken**.

![](../media/T07_entity_create.png)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Typ **hier uw toppings: $toppings** voor **de reactie van de bot...** . Het voorloop dollar teken duidt op een verwijzing naar een entiteit.
3. Selecteer **Maken**.

![](../media/T07_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Geef op **welk toppings u wilt?** voor **het antwoord van de bot...** .
3. Voer **toppings** invoor diskwalificerende recht.
4. Selecteer **Maken**.

U hebt nu twee acties.

![](../media/T07_action_create_2.png)

### <a name="train-the-model"></a>Het model trainen

1. Selecteer **dialoog vensters trainen** in het linkerpaneel en klik vervolgens op **Nieuw trein dialoog venster**.
2. Geef **Hi** op voor de utterance van de gebruiker in het linkerdeel venster.
3. Selecteer **Score acties**.
4. Selecteer **wat toppings u wilt?** in de lijst met acties. Het percentiel is 100% als de enige geldige actie op basis van de beperkingen.
5. Voer **kaas en champignons** in voor de utterance van de gebruiker in het linkerdeel venster.
6. Markeer **kaas** en selecteer vervolgens **+ toppings**.
7. Markeer **champignons** en selecteer vervolgens **+ toppings**.
8. Selecteer **Score acties**.
9. Selecteer **hier uw toppings: $toppings** uit de lijst met acties.
10. Voer een **peper toe** voor de volgende utterance van de gebruiker in het linkerdeel venster.
11. Markeer **peper** en selecteer vervolgens **+ toppings**.
12. Selecteer **Score acties**.
13. Selecteer **hier uw toppings: $toppings** uit de lijst met acties.
14. Voer **kaas verwijderen** in voor de derde utterance van de gebruiker in het linkerdeel venster.
15. Markeer **kaas** en selecteer vervolgens **-toppings**.
16. Selecteer **Score acties**.
17. Selecteer **hier uw toppings: $toppings** uit de lijst met acties.

![](../media/T07_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vooraf getrainde entiteiten](./08-pre-trained-entities.md)
