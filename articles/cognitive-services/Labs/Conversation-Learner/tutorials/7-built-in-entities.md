---
title: Vooraf samengestelde entiteiten toevoegen aan de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het vooraf samengestelde entiteiten toevoegen aan een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f014464419bfac39a9e57e679fcd28a737e9ebdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345643"
---
# <a name="how-to-add-pre-built-entities"></a>Het toevoegen van vooraf samengestelde entiteiten
Deze zelfstudie laat zien hoe entiteiten 'vooraf gemaakte' toevoegen aan uw toepassing conversatie cursist.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Vooraf samengestelde entiteiten herkennen entiteiten, zoals getallen, datums, geldbedrag en andere algemene typen.  In tegenstelling tot aangepaste entiteiten ze werken 'out-of-the-box' en hoeven niet alle training.  In tegenstelling tot aangepaste entiteiten kan niet hun gedrag worden gewijzigd.  Standaard, vooraf samengestelde entiteiten met meerdere waarden zijn: dat wil zeggen, worden de bot geheugen verzameld voor elke geïdentificeerde exemplaar van de entiteit.

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam BuiltInEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Klik op de vervolgkeuzelijst EntityType en datetimev2 selecteren.
    - Programmeerbare en Negatable opties zijn uitgeschakeld, omdat ze niet van toepassing vooraf bouwen entiteiten.
3. Klik op Maken.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ in het antwoord, is de datum is $luis-datetimev2'.
3. Klik op Maken.

![](../media/tutorial7_actions.PNG)

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. In het antwoord, typ 'Wat is de datum?'.
4. Voer in tot uitsluiting entiteiten in 'luis datetimev2'.
4. Klik op Maken

![](../media/tutorial7_actions2.PNG)

U hebt nu twee acties.

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'Wat is de datum'?
2. Voer 'vandaag'. 
    - Kennisgeving vandaag is gecodeerd en weergegeven in de tweede regel omdat het een vooraf samengestelde entiteit en niet-bewerkbare.
5. Klik op Score-acties
    - U ziet dat nu de datum wordt weergegeven in de sectie entiteit geheugen. 
    - Als u de muis op de datum, kunt u zien welke extra gegevens LUIS die kan gebruikt worden en meer in de code kan worden bewerkt. 
6. Selecteer, de datum is $luis-datetimev2'.
7. Klik op gereed onderwijs

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Alternatieve invoer](./8-alternative-inputs.md)
