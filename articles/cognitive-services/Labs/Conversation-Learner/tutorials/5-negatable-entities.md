---
title: Het gebruik van negatable entiteiten met een toepassing conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van negatable entiteiten met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3d65376c9c43ee1407468f3e8bf3e058048bd556
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345302"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-application"></a>Het gebruik van negatable entiteiten met een conversatie cursist-toepassing

Deze zelfstudie wordt de eigenschap 'negatable' entiteiten.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Een actie als 'negatable' markeren als de gebruiker '' een entiteitswaarde, zoals in "Nee, ik wil niet $entity wissen kan" of "Nee, geen $entity." Bijvoorbeeld, "Nee, ik niet wil leave van Boston."

Concrete invulling te geven, als de eigenschap 'negatable' van een entiteit is ingesteld:

- Als labels entiteit vermeldingen, kunt u zowel normale (positief) exemplaren van een entiteit en een 'negatieve' exemplaren van de entiteit een label
- LUIS leert twee entiteitmodellen: één voor positieve exemplaren en een tweede voor negatieve exemplaren
- Het effect van een negatieve exemplaar van een entiteit is het wissen van die waarde van de variabele entiteit (indien aanwezig)

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam NegatableEntity. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer de naam in de naam van de entiteit.
3. Controle van Negatable.
    - Dit geeft aan de gebruiker een waarde opgeven voor de entiteit of spreek iets zijn *niet* de waarde van de entiteit. In het laatste geval is resulteert dit in een overeenkomende waarde van de entiteit te verwijderen.
3. Klik op Maken.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. In het antwoord, typ 'Ik weet niet de naam van uw'.
3. Voer de naam in tot uitsluiting entiteiten.
3. Klik op Maken

Vervolgens maakt u de tweede actie.

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord, ' Ik weet dat uw naam. Het is $name'.
4. Klik op Maken

U hebt nu twee acties.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'Ik weet niet de naam van uw'
    - Merk op dat is het ophalen van een score 100% omdat dit de enige geldige actie.
2. Typ 'Mijn naam is david'
3. Selecteer 'david', en kies het label ' + naam '
    - Er zijn twee exemplaren van 'name': '+ naam' en '-naam '.  Plus betekent dat we die waarde opgeeft. Min betekent dat we aan dat het systeem iets is niet de waarde.
5. Klik op Score-acties
    - Opmerking naamwaarde is nu in het geheugen van de bot.
    - "Ik weet dat uw naam. Het $name is, is het alleen beschikbaar antwoord. 
6. Selecteer ' Ik weet dat uw naam. Het is $name'.

We gaan de negatable entiteit wissen:

7. Typ 'Mijn naam is niet david'.
    - Bericht is 'geen' geselecteerd als de naam op basis van het vorige patroon. Dit is onjuist.
2. Klik op 'not' en vervolgens de rode x. 
3. Klik op 'david'.
    - Dit is nu een negatieve entiteit is dit niet de waarde van de entiteit naam communiceren.
2. Selecteer '-naam '.
3. Klik op Score acties.
    - U ziet de waarde uit het geheugen is gewist.
2. Selecteer 'Ik weet niet de naam van uw', dat is de enige actie.

Vervolgens laten we zien hoe een nieuwe waarde voor de naam kan worden ingevoerd.

3. Voer 'Jan' als naam. Selecteert u 'Jan' en klik op naam.
4. Klik op Score acties.
5. Selecteer ' Ik weet dat uw naam. Het is $name'.

Nu kunt u proberen de ingevoerde naam vervangen.

6. Typ 'Mijn naam is susan'.
7. Selecteer ' Ik weet dat uw naam. Het is $name'.
7. Klik op Score acties.
8. Kennisgeving **susan** heeft overschreven **john** in de entiteit-waarden.
9. Typ 'Mijn naam is niet susan'.
    - Let op dat het systeem heeft dit gelabeld als een negatieve exemplaar.
2. Klik op Score acties.
3. Selecteer 'Ik weet niet de naam van uw', dat is de enige actie.
7. Klik op het onderwijs gereed.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten met meerdere waarden](./6-multi-value-entities.md)
