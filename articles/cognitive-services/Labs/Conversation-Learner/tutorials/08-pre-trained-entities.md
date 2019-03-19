---
title: Pre-Trained entiteiten toevoegen aan een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het toevoegen van Pre-trained entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f5b3234c45a9ee80bc5a2c2afe67046896270802
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58163783"
---
# <a name="how-to-add-pre-trained-entities"></a>Pre-trained entiteiten toevoegen
In deze zelfstudie laat zien hoe Pre-Trained entiteiten toevoegen aan uw Model van de cursist conversatie.

## <a name="video"></a>Video

[![Vooraf getrainde entiteiten zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Vooraf getrainde entiteiten herkennen algemene typen entiteiten, zoals getallen, datums, monetaire bedragen en anderen.  Ze werken "out-of-the-box," hoeven niet alle training en hun gedrag kan niet worden gewijzigd in tegenstelling tot aangepaste entiteiten.  Standaard Pre-Trained entiteiten zijn meerdere waarden samenvoegen van elke geïdentificeerde exemplaar van de entiteit.

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het Model maken

1. Selecteer **nieuw Model**.
2. Voer **PretrainedEntities** voor **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linker deelvenster, klikt u vervolgens **nieuwe entiteit**.
2. Selecteer **Pre-Trained/datetimeV2** voor **entiteitstype**.
3. Controleer **meerdere waarden** om in te schakelen door de entiteit een of meer waarden worden verzameld. Houd er rekening mee, mag niet Pre-Trained entiteiten negatable.
4. Selecteer **Maken**.

![](../media/T08_entity_create.png)

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **de datum is $builtin-datetimev2** voor **van Bot-antwoord...** .
3. Selecteer **Maken**.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **wat is de datum?** voor **van Bot-antwoord...** . Vooraf getrainde entiteiten kunnen niet worden **vereist entiteiten** zoals deze standaard voor alle uitingen worden herkend.
3. Voer **builtin datetimev2** voor **diskwalificeren voorziet**.
4. Selecteer **Maken**.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Het Model te trainen

1. Selecteer **Train-dialoogvensters** in het linker deelvenster, klikt u vervolgens **nieuwe dialoogvenster van de trein**.
2. Voer **hello** voor utterance van de gebruiker in het deelvenster links chat.
3. Selecteer **Score acties**.
4. Selecteer **wat is de datum?** uit de lijst met acties
5. Voer **vandaag** voor utterance van de gebruiker in het deelvenster links chat.
    - De **vandaag** utterance wordt automatisch herkend door vooraf getrainde modellen in LUIS.
    - Waarden van Pre-Trained entiteiten aanwijzen, ziet u aanvullende gegevens van LUIS.

![](../media/T08_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteit Resolvers](./09-entity-resolvers.md)
