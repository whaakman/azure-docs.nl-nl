---
title: Het gebruik van entiteiten met een toepassing conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteiten met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 85df31c2e2ff3ca81698921a1f17f415daefb6c5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345281"
---
# <a name="introduction-to-entities"></a>Inleiding tot entiteiten

Deze zelfstudie maakt u kennis met entiteiten en laat zien hoe u de 'Disqualifying entiteiten' en 'Vereist entiteiten' velden in acties.

## <a name="requirements"></a>Vereisten

Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Deze zelfstudie ziet u twee veelvoorkomende toepassingen voor entiteiten.  Ten eerste kunnen entiteiten subtekenreeksen extraheren uit een gebruikersbericht, zoals een plaats in 'Wat is het weer in Haarlem?' te identificeren.  Entiteiten kunnen ten tweede beperken wanneer er acties beschikbaar zijn.  Een actie kunt in het bijzonder een entiteit wordt 'vereist' of 'tot ' uitsluiting weergeven:
- Een actie vereist entiteiten moet aanwezig zijn in het geheugen van de bot in volgorde voor de actie beschikbaar
- Tot entiteiten uitsluiting moet *niet* aanwezig zijn in de bot geheugen om de actie beschikbaar

Andere zelfstudies betrekking hebben op andere aspecten van entiteiten, zoals de vooraf gemaakte entiteiten, met meerdere waarden en negatable entiteiten, programmatische entiteiten en manipuleren entiteiten in code.

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam IntroToEntities. Klik vervolgens op maken.

### <a name="create-entity"></a>Entiteit maken

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer plaats in de naam van de entiteit.
3. Klik op Maken

Houd er rekening mee voor het entiteitstype is 'custom'--Dit betekent dat de entiteit kan worden getraind.  Er zijn ook vooraf samengestelde entiteiten, wat betekent dat hun gedrag kan niet worden gecorrigeerd--deze in een andere zelfstudie vallen.

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. In het antwoord, typ 'Ik niet weet welke stad gewenste'.
3. Voer $city in tot uitsluiting entiteiten. Klik op opslaan.
    - Dit betekent dat als deze entiteit is gedefinieerd in het geheugen van de bot, klikt u vervolgens deze actie wordt *niet* beschikbaar.
2. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. In het antwoord, typt u 'het in de $city mooi weer is waarschijnlijk'.
4. Houd er rekening mee dat stad entiteit automatisch is toegevoegd sinds het is bedoeld in entiteiten vereist.
5. Op Opslaan klikken

U hebt nu twee acties.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'Ik niet weet welke stad gewenste?'
    - Houd er rekening mee dat het antwoord waar de entiteit plaats vereist is omdat de plaats entiteit is niet gedefinieerd in het geheugen van de bot kan niet worden geselecteerd.
2. Selecteer 'Ik niet weet welke stad gewenste'.
4. Voer 'seattle'. Markeer seattle en klik vervolgens op plaats.
5. Klik op Score-acties
    - Opmerking stad waarde is nu in het geheugen van de bot.
    - ' In $city mooi weer is waarschijnlijk' is nu beschikbaar als antwoord. 
6. Selecteer ' In $city mooi weer is waarschijnlijk'.

Stel dat de gebruiker invoert, die repeat". 
1. Die te typen en. Houd er rekening mee dat stad entiteit en de waarde in het geheugen en beschikbaar is.
2. Selecteer ' In $city mooi weer is waarschijnlijk'.

![](../media/tutorial3_entities.PNG)

U hebt nu een entiteit gemaakt en exemplaren ervan in de gebruiker berichten met het label.  U hebt ook de aanwezigheid/afwezigheid van de entiteit gebruikt in het geheugen van de bot om te bepalen wanneer acties beschikbaar via de actie tot uitsluiting en vereiste entiteitsvelden zijn.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verwachte entiteit](./4-expected-entity.md)
