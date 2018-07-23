---
title: Vooraf gemaakte entiteiten toevoegen aan een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Leer hoe u vooraf gemaakte entiteiten toevoegen aan een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 84d73add5586aaaf130253a8122a4152e39bcbe9
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171364"
---
# <a name="how-to-add-pre-built-entities"></a>Vooraf gemaakte entiteiten toevoegen
Deze zelfstudie laat zien hoe u entiteiten 'vooraf gemaakte' toevoegen aan uw model Conversatiecursist.

## <a name="video"></a>Video

[![Zelfstudie 7-Preview](http://aka.ms/cl-tutorial-07-preview)](http://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Vooraf gemaakte entiteiten herkennen algemene typen entiteiten, zoals getallen, datums, monetaire bedragen en anderen.  In tegenstelling tot aangepaste entiteiten, ze "out-of-the-box" werk en hoeven niet alle training.  In tegenstelling tot aangepaste entiteiten, kan niet hun gedrag worden gewijzigd.  Standaard, vooraf gemaakte entiteiten zijn meerdere waarden - dat wil zeggen, het geheugen van de bot elke geïdentificeerde exemplaar van de entiteit wordt verzameld.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam BuiltInEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Klik op de EntityType-omlaag en selecteer datetimev2.
    - Opties voor programmeerbare en Negatable zijn uitgeschakeld, omdat ze niet van toepassing voor het bouwen van vooraf entiteiten.
3. Klik op Maken.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ in het antwoord is de datum is $luis-datetimev2'.
3. Klik op Maken.

![](../media/tutorial7_actions.PNG)

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ 'Wat is de datum?' in het antwoord.
4. Voer 'luis-datetimev2' in diskwalificeren van entiteiten.
4. Klik op Maken

![](../media/tutorial7_actions2.PNG)

U hebt nu twee acties.

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'Wat is de datum?'
2. Voer 'nu'. 
    - U ziet dat vandaag is gemarkeerd en wordt weergegeven in de tweede regel omdat dit een vooraf gemaakte entiteit en niet-bewerkbare.
5. Klik op Score acties
    - U ziet dat nu de datum wordt weergegeven in de sectie entiteit geheugen. 
    - Als u met de muisaanwijzer over de datum, kunt u zien welke aanvullende gegevens geleverd door LUIS, die kan worden gebruikt en kan verder worden bewerkt in de code. 
6. Selecteer ' de datum is $luis-datetimev2'.
7. Klik op gereed onderwijs

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Alternatieve invoer](./8-alternative-inputs.md)
