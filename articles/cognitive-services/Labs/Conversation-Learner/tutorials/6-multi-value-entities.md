---
title: Over het gebruik van meerdere waarden entiteiten met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van meerdere waarden entiteiten met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a80577bb226cbec080edf5e06dbd0f31c80a5890
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321424"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Over het gebruik van meerdere waarden entiteiten met een model Conversatiecursist
In deze zelfstudie leert de eigenschap 'meerdere waarden' van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie 6-Preview](http://aka.ms/cl-tutorial-06-preview)](http://aka.ms/blis-tutorial-06)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Een entiteit die "meerdere waarde" stelt waarden in een lijst, in plaats van een enkele waarde opslaan samen.  Dit is handig voor entiteiten waarin de gebruiker meer dan één waarde, zoals toppings op een pizza kunt opgeven.

Concrete invulling te geven, als een entiteit is gemarkeerd als 'meerdere waarden', herkend klikt u vervolgens elke exemplaar van de entiteit wordt toegevoegd aan een lijst in van de bot geheugen (in plaats voor het overschrijven van de waarde van een enkele entiteit).

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam MultiValueEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer in de naam van de entiteit, Toppings.
3. Controle van meerdere waarden.
    - Entiteiten van meerdere waarden worden verzameld van een of meer waarden in de entiteit.
2. Controleer de Negatable.  
    - Hierdoor wordt de gebruiker toppings verwijderen uit de lijst met samengevoegde pizza toppings.
3. Klik op Maken.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ 'welke toppings wilt u dat?-antwoord.
3. Voer Toppings in diskwalificeren van entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie.

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord, "Hier vindt u uw toppings: $Toppings'.
4. Klik op Maken

U hebt nu twee acties.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'welke toppings wilt u dat?'
2. Voer 'paddestoelen en kaas'. 
    - U kunt een nul, een of meer dan één van de entiteiten label.
3. Klik op 'paddestoelen' en selecteer Toppings.
4. Klik op 'kaas' en selecteer Toppings.
5. Klik op Score acties
    - De twee waarden zijn nu aanwezig in de entiteit Toppings. 
6. Selecteer "Hier worden de toppings: $Toppings'.

We kunnen meer toevoegen aan dit:

7. Voer 'add peppers'.
    - Klik op 'peppers' onder de detectie van de entiteit en Toppings selecteren.
3. Klik op Score acties.
    - 'peppers' wordt nu weergegeven als een extra waarde in Toppings.
6. Selecteer "Hier worden de toppings: $Toppings'.

We verwijderen een toegenomen en voeg een:

2. Typ 'peppers verwijderen en worst toevoegen'.
1. Klik op 'peppers' en klik op de rode x om deze te verwijderen.
2. Klik op 'peppers' en selecteer '-'Toppings.
3. Klik op Score acties.
    - 'peppers' is verwijderd en 'worst' is toegevoegd.
6. Selecteer "Hier worden de toppings: $Toppings'.

Nu gaan we proberen alles verwijderen:

6. Voer 'paddestoelen verwijderen, verwijdert kaas en worst verwijderen'.
7. Klik op elk van de drie en selecteer '-'Toppings.
7. Klik op Score acties.
    - Alle toppings worden gewist.
2. Selecteer 'welke toppings wilt u dat?'
3. Klik op gereed onderwijs

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ingebouwde entiteiten](./7-built-in-entities.md)
