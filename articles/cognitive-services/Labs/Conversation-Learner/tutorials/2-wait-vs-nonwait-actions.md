---
title: Wacht even en niet-wait-acties gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van wacht even en niet-wait-acties met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f349dab23b9156d3a5656e8275533ebe6a82cdf9
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45540897"
---
# <a name="wait-and-non-wait-actions"></a>Wacht even en acties voor niet-wait

Deze zelfstudie leert het verschil tussen wacht acties en niet-wait-acties in de Conversatiecursist.

## <a name="video"></a>Video

[![Zelfstudie 2-Preview](http://aka.ms/cl-tutorial-02-preview)](http://aka.ms/blis-tutorial-02)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Actie wachten: nadat het systeem heeft een actie 'wait', wordt niet meer acties en wachten op invoer van de gebruiker.
- Niet-wait-actie: nadat het systeem heeft een actie 'niet-wait', wordt deze onmiddellijk nog een actie kiezen (zonder te wachten op gebruikersinvoer eerst).

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam WaitNonWait. Klik vervolgens op maken.

### <a name="create-the-first-wait-action"></a>De eerste actie in de wacht maken

1. Klik op acties en vervolgens nieuwe actie.
2. Voer 'welke dieren wilt u dat?-antwoord.
    - Dit is een actie wachten, dus laat het wachten op reactie selectievakje is ingeschakeld.
3. Klik op Maken.

### <a name="create-a-non-wait-action"></a>Een niet-Wait-actie maken

1. Klik op nieuwe actie
2. Typ 'Koeien zeg moo' antwoord.
3. Schakel de wachttijd voor het selectievakje in als antwoord.
4. Klik op Maken

### <a name="create-a-second-non-wait-action"></a>Maak een tweede actie voor niet-Wait

1. Klik op nieuwe actie
2. Typ 'Eenden zeg quack' antwoord.
3. Schakel de wachttijd voor het selectievakje in als antwoord.
4. Klik op Maken

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Type 'Hallo'
3. Klik op Score acties en selecteer 'welke dieren wilt u dat?'.
4. Voer 'ook'
5. Klik op Score acties en selecteer 'Koeien zeg moo'.
    - De bot wordt niet wachten op invoer en zal de volgende actie ondernemen.
2. Selecteer 'welke dieren wilt u dat?'.
3. Voer 'duck'
5. Klik op Score acties en selecteer 'Eenden zeg quack'.

![](../media/tutorial2_dialogs.PNG)

> [!NOTE]
> De volgorde van de bot antwoorden met betrekking tot wachten en acties voor niet-wait.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot entiteiten](./3-introduction-to-entities.md)
