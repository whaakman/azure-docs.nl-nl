---
title: Over het gebruik van entiteiten met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteiten met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4b1c2d9390890618a9aa61eb425fbd132917f414
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229045"
---
# <a name="introduction-to-entities"></a>Kennismaking met entiteiten

In deze zelfstudie bevat entiteiten en ziet u hoe u de 'Disqualifying entiteiten' en 'Entiteiten is vereist' velden in acties.

## <a name="video"></a>Video

[![Zelfstudie 3-Preview](https://aka.ms/cl-tutorial-03-preview)](https://aka.ms/blis-tutorial-03)

## <a name="requirements"></a>Vereisten

In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

In deze zelfstudie ziet u twee veelvoorkomende toepassingen voor entiteiten.  Entiteiten kunnen eerst subtekenreeksen extraheren uit een gebruikersbericht, zoals het identificeren van een plaats in 'Wat is het weer in Seattle?'.  Entiteiten kunnen ten tweede beperken wanneer er acties beschikbaar zijn.  Een actie kan met name een entiteit wordt 'vereist' of 'diskwalificeren' lijst:
- Vereiste entiteiten van een actie moeten aanwezig zijn in het geheugen van de bot in volgorde voor de actie moet beschikbaar zijn
- Entiteiten diskwalificeren moet *niet* aanwezig zijn in het geheugen van de bot in volgorde voor de actie beschikbaar

Andere zelfstudies betrekking hebben op andere aspecten van entiteiten, zoals de vooraf gemaakte entiteiten, meerdere waarden en negatable entiteiten, programmatische entiteiten en manipuleren entiteiten in de code.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam IntroToEntities. Klik vervolgens op maken.

### <a name="create-entity"></a>Entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer in de naam van de entiteit, plaats.
3. Klik op Maken

> [!NOTE]
> Het entiteitstype is 'custom'--Dit betekent dat de entiteit kan worden getraind.  Er zijn ook vooraf gemaakte entiteiten, wat betekent dat hun gedrag kan niet worden aangepast--deze worden behandeld in een andere zelfstudie.

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ 'Ik weet niet welke plaats die u wilt' in het antwoord.
3. Voer $city in diskwalificeren van entiteiten. Klik op Opslaan.
    - Dit betekent dat als deze entiteit is gedefinieerd in het geheugen van de bot, klikt u vervolgens deze actie wordt *niet* beschikbaar zijn.
2. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ 'het weer in de $city is waarschijnlijk zonnige' in het antwoord.
4. In de vereiste entiteiten, is stad entiteit automatisch toegevoegd omdat deze is bedoeld.
5. Op Opslaan klikken

U hebt nu twee acties.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'Ik weet niet welke plaats die u wilt?'
    - Het antwoord waar de entiteit plaats vereist is, kan niet worden geselecteerd omdat de entiteit plaats is niet gedefinieerd in het geheugen van de bot.
2. Selecteer 'Ik weet niet welke plaats die u wilt'.
4. Voer 'seattle'. Markeer seattle, en klik vervolgens plaats.
5. Klik op Score acties
    - Waarde van stad is nu in het geheugen van de bot.
    - 'Weer in $city is waarschijnlijk zonnige' is nu beschikbaar als een antwoord. 
6. Selecteer 'Weer in $city is waarschijnlijk zonnige'.

Stel dat de gebruiker voert 'herhalen op die'. 
1. Typ die en invoeren. Plaats entiteit en de waarde ervan is in het geheugen beschikbaar.
2. Selecteer 'Weer in $city is waarschijnlijk zonnige'.

![](../media/tutorial3_entities.PNG)

U hebt nu een entiteit gemaakt en exemplaren van deze gebruiker berichten met het label.  U hebt ook de aanwezigheid/afwezigheid van de entiteit in het geheugen om te bepalen van de bot gebruikt voor het acties beschikbaar via een van de actie diskwalificeren en entiteitsvelden van de vereiste zijn.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verwachte entiteit](./4-expected-entity.md)
