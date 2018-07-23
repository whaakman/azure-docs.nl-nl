---
title: Over het gebruik van negatable entiteiten met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van negatable entiteiten met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2fd00d53755e44e3a3d86782c40aa6a53ff4d378
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171398"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Over het gebruik van negatable entiteiten met een model Conversatiecursist

In deze zelfstudie ziet u de eigenschap "negatable" van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie 5-Preview](http://aka.ms/cl-tutorial-05-preview)](http://aka.ms/blis-tutorial-05)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Een actie als wanneer u 'negatable' markeren als de gebruiker '' de entiteitswaarde van een, zoals in "Nee, ik wil niet $entity wissen kan" of "Nee, geen $entity." Bijvoorbeeld, "Nee, ik niet wil laten in Boston."

Concrete invulling te geven, als de eigenschap 'negatable' van een entiteit is ingesteld:

- Als labels entiteit vermeldingen, kunt u zowel normale (positief) exemplaren van een entiteit en een 'negatieve' exemplaren van de entiteit van label
- LUIS leert twee entiteitmodellen: een positieve exemplaren en een tweede voor negatieve exemplaren
- Het effect van een negatieve exemplaar van een entiteit is het wissen van die waarde van de variabele entiteit (indien aanwezig)

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam NegatableEntity. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer de naam in de naam van de entiteit.
3. Controleer de Negatable.
    - Deze eigenschap geeft aan dat de gebruiker een waarde opgeven voor de entiteit of Stel dat er iets zijn *niet* de waarde van de entiteit. In dat laatste geval resulteert dit in een overeenkomende waarde van de entiteit te verwijderen.
3. Klik op Maken.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ 'Ik weet niet de naam van uw' in het antwoord.
3. Voer de naam in diskwalificeren van entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie.

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord, "Ik weet dat uw naam. Het is $name'.
4. Klik op Maken

U hebt nu twee acties.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'Ik weet niet de naam van uw'
    - De score is 100%, omdat het de enige geldige actie.
2. Typ 'Mijn naam is david'
3. Selecteer 'david', en kies het label "+ naam"
    - Er zijn twee exemplaren van 'name': '+ naam' en '-naam '.  (+) Plus toegevoegd of de waarde wordt overschreven. (-) Min Hiermee verwijdert u de waarde.
5. Klik op Score acties
    - De naamwaarde is nu in het geheugen van de bot.
    - "Ik weet dat uw naam. Het is $name' is de enige beschikbare reactie. 
6. Selecteer ' Ik weet dat uw naam. Het is $name'.

We proberen de negatable entiteit uit te schakelen:

7. Typ 'Mijn naam is niet david'.
    - U ziet is 'not' geselecteerd als de naam op basis van het vorige patroon. Dit label is onjuist.
2. Klik op 'not' en klik op de rode x. 
3. Klik op 'david'.
    - Dit is nu een negatieve entiteit communicatie is dit niet de waarde van de naam van entiteit.
2. Selecteer '-naam '.
3. Klik op Score acties.
    - U ziet dat de waarde is van het geheugen gewist.
2. Selecteer 'Ik weet niet de naam van uw', is de enige actie.

Vervolgens laten we zien hoe een nieuwe waarde voor de naam kan worden ingevoerd.

3. Voer 'john' als naam. Vervolgens selecteert u 'john' en klik op de naam.
4. Klik op Score acties.
5. Selecteer ' Ik weet dat uw naam. Het is $name'.

Probeer nu de ingevoerde naam vervangen.

6. Typ 'Mijn naam is susan'.
7. Selecteer ' Ik weet dat uw naam. Het is $name'.
7. Klik op Score acties.
8. U ziet dat **susan** is overschreven door **john** in de entiteitswaarden.
9. Typ 'Mijn naam is niet susan'.
    - U ziet dat het systeem heeft dit aangeduid als een negatieve exemplaar.
2. Klik op Score acties.
3. Selecteer 'Ik weet niet de naam van uw', is de enige actie.
7. Klik op het onderwijs gereed.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten van meerdere waarden](./6-multi-value-entities.md)
