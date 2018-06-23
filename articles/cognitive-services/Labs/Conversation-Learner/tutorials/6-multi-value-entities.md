---
title: Entiteiten met meerdere waarden gebruiken met de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van meerdere waarden entiteiten met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 865b50747b2c9574b5f88d4902bea9e4c8e0e032
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345283"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-application"></a>Het gebruik van meerdere waarden entiteiten met een conversatie cursist-toepassing
Deze zelfstudie ziet de eigenschap 'met meerdere waarden' van entiteiten.

##<a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Een entiteit die 'met meerdere waarden' een tijdje staan waarden in de lijst in plaats van een enkele waarde opslaan.  Dit is handig voor entiteiten waarbij de gebruiker meer dan één waarde, zoals toppings op een pizza kunt opgeven.

Concrete invulling te geven, als een entiteit is gemarkeerd als 'met meerdere waarden', herkend klikt u vervolgens elk exemplaar van de entiteit worden toegevoegd aan een lijst aan in de bot geheugen (in plaats wordt een waarde van één entiteit overschreven).

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam MultiValueEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer in de naam van de entiteit Toppings.
3. Controle van meerdere waarden.
    - Entiteiten met meerdere waarden worden verzameld van een of meer waarden in de entiteit.
2. Controle van Negatable.  
    - Hierdoor wordt de gebruiker toppings verwijderen uit de lijst met elkaar opgetelde pizza toppings.
3. Klik op Maken.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. In het antwoord, typ 'welke toppings wilt u?'.
3. Voer Toppings in tot uitsluiting entiteiten.
3. Klik op Maken

Vervolgens maakt u de tweede actie.

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord, ' Hier vindt u uw toppings: $Toppings'.
4. Klik op Maken

U hebt nu twee acties.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'welke toppings wilt u?'
2. Voer 'paddestoelen en kaas'. 
    - U kunt een label voorzien voor nul, een of meer dan één van de entiteiten.
3. Klik op 'paddestoelen' en selecteer Toppings.
4. Klik op 'kaas' en selecteer Toppings.
5. Klik op Score-acties
    - Houd er rekening mee dat de twee waarden nu aanwezig in de entiteit Toppings zijn. 
6. Selecteer ' Hier vindt u uw toppings: $Toppings'.

Er kan meer toevoegen aan dit:

7. Voer 'add pepers'.
    - Klik op 'pepers' onder entiteit detectie en Toppings selecteren.
3. Klik op Score acties.
    - Houd er rekening mee dat pepers nu worden weergegeven als een extra waarde in Toppings.
6. Selecteer ' Hier vindt u uw toppings: $Toppings'.

We gaan een toegenomen verwijderen en voeg een:

2. Typ 'pepers verwijderen en worst toevoegen'.
1. Klik op 'pepers' en klik op de rode x te verwijderen.
2. Klik op 'pepers' en selecteer '-'Toppings.
3. Klik op Score acties.
    - Houd er rekening mee dat 'pepers' is verwijderd en 'worst' is toegevoegd.
6. Selecteer ' Hier vindt u uw toppings: $Toppings'.

Nu gaan we proberen alles verwijderen:

6. Voer 'paddestoelen verwijderen, verwijdert u kaas en worst verwijderen'.
7. Klik op elk van de drie en selecteer '-'Toppings.
7. Klik op Score acties.
    - Houd er rekening mee dat alle toppings zijn uitgeschakeld.
2. Selecteer 'welke toppings wilt u?'
3. Klik op gereed onderwijs

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ingebouwde entiteiten](./7-built-in-entities.md)
