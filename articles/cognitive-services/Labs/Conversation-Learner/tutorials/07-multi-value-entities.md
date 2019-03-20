---
title: Over het gebruik van meerdere waarden entiteiten met een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van meerdere waarden entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 07550ed9c78aa584e55403d225eef4bcff33b057
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58167419"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Over het gebruik van meerdere waarden entiteiten met een model Conversatiecursist
In deze zelfstudie leert de eigenschap met meerdere waarden van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie Preview entiteiten van meerdere waarden](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Meerdere waarden entiteiten oplopen waarden in een lijst, in plaats van een enkele waarde opslaan.  Deze entiteiten zijn nuttig wanneer gebruikers meer dan één waarde kunnen opgeven. Toppings op een pizza bijvoorbeeld.

Entiteiten die zijn gemarkeerd als meerdere waarden elk herkende exemplaar van de entiteit toegevoegd aan een lijst in het geheugen van de Bot heeft. Volgende erkenning toegevoegd aan van de entiteit-waarde, in plaats van worden overschreven.

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het Model maken

1. Selecteer **nieuw Model**.
2. Voer **MultiValueEntities** voor **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linker deelvenster, klikt u vervolgens **nieuwe entiteit**.
2. Selecteer **aangepaste getraind** voor **entiteitstype**.
3. Voer **toppings** voor **entiteitnaam**.
4. Controleer **meerdere waarden** om in te schakelen door de entiteit een of meer waarden worden verzameld.
5. Controleer **Negatable**.
6. Selecteer **Maken**.

![](../media/T07_entity_create.png)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **Hier vindt u uw toppings: $toppings** voor **van Bot-antwoord...** . De toonaangevende dollarteken geeft aan dat de verwijzing naar een entiteit.
3. Selecteer **Maken**.

![](../media/T07_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **welke toppings wilt u dat?** voor **van Bot-antwoord...** .
3. Voer **toppings** voor **diskwalificeren voorziet**.
4. Selecteer **Maken**.

U hebt nu twee acties.

![](../media/T07_action_create_2.png)

### <a name="train-the-model"></a>Het Model te trainen

1. Selecteer **Train-dialoogvensters** in het linker deelvenster, klikt u vervolgens **nieuwe dialoogvenster van de trein**.
2. Voer **Hallo** voor utterance van de gebruiker in het deelvenster links chat.
3. Selecteer **Score acties**.
4. Selecteer **welke toppings wilt u dat?** uit de lijst met acties. Het percentiel is 100% als de enige geldige actie op basis van de beperkingen.
5. Voer **kaas- en paddestoelen** voor utterance van de gebruiker in het deelvenster links chat.
6. Markeer **kaas** Selecteer **+ toppings**.
7. Markeer **paddestoelen** Selecteer **+ toppings**.
8. Selecteer **Score acties**.
9. Selecteer **Hier vindt u uw toppings: $toppings** uit de lijst met acties.
10. Voer **toevoegen peper** voor de volgende utterance van de gebruiker in het deelvenster links chat.
11. Markeer **peper** Selecteer **+ toppings**.
12. Selecteer **Score acties**.
13. Selecteer **Hier vindt u uw toppings: $toppings** uit de lijst met acties.
14. Voer **kaas verwijderen** voor derde utterance van de gebruiker in het deelvenster links chat.
15. Markeer **kaas** Selecteer **-toppings**.
16. Selecteer **Score acties**.
17. Selecteer **Hier vindt u uw toppings: $toppings** uit de lijst met acties.

![](../media/T07_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vooraf getrainde entiteiten](./08-pre-trained-entities.md)
