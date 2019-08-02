---
title: Vooraf getrainde entiteiten toevoegen aan een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het toevoegen van vooraf getrainde entiteiten aan een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: fceca459b80725e1a9c8b7dbc6a4387ea98dbb7b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704124"
---
# <a name="how-to-add-pre-trained-entities"></a>Vooraf getrainde entiteiten toevoegen
Deze zelf studie laat zien hoe u vooraf getrainde entiteiten kunt toevoegen aan uw Conversation Learner model.

## <a name="video"></a>Video

[![Preview-versie van zelf studie voor vooraf getrainde entiteiten](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Vooraf getrainde entiteiten herkennen veelvoorkomende typen entiteiten, zoals getallen, datums, geld bedragen en anderen.  Ze werken "out-of-the-box", geen training vereist en hun gedrag kan niet worden gewijzigd, zoals bij aangepaste entiteiten.  Vooraf getrainde entiteiten zijn standaard meerdere waarden, waarbij elke geïdentificeerde instantie van de entiteit wordt gecumuleerd.

## <a name="steps"></a>Stappen

Start op de start pagina in de gebruikers interface van het web.

### <a name="create-the-model"></a>Het model maken

1. Selecteer **Nieuw model**.
2. Voer **PretrainedEntities** in als **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linkerdeel venster en vervolgens **nieuwe entiteit**.
2. Selecteer **vooraf getraind/datetimeV2 voor het** **entiteits type**.
3. Controleer op **meerdere waarden** om de entiteit in te scha kelen voor een of meer waarden. Opmerking: vooraf getrainde entiteiten kunnen niet worden ontkrachtigd.
4. Selecteer **Maken**.

![](../media/T08_entity_create.png)

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Voer **de datum in $Builtin datetimev2** voor **de reactie van de bot...**
3. Selecteer **Maken**.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Opgeven **Wat is de datum?** voor **de reactie van de bot...** Vooraf getrainde entiteiten kunnen geen **entiteiten** zijn die standaard worden herkend voor alle uitingen.
3. Voer **Builtin-datetimev2** in voor het diskwalificeren van **entiteiten**.
4. Selecteer **Maken**.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Het model trainen

1. Selecteer **dialoog vensters trainen** in het linkerpaneel en klik vervolgens op **Nieuw trein dialoog venster**.
2. Voer **Hallo** in voor de utterance van de gebruiker in het linkerdeel venster.
3. Selecteer **Score acties**.
4. Selecteer **Wat is de datum?** in de lijst acties
5. Voer **vandaag nog** in voor de utterance van de gebruiker in het linkerdeel venster.
    - De **huidige** utterance wordt automatisch herkend door vooraf getrainde modellen in Luis.
    - Bij het aanwijzen van de waarden van vooraf getrainde entiteiten worden aanvullende gegevens weer gegeven door LUIS.

![](../media/T08_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteits conflicten](./09-entity-resolvers.md)
