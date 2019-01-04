---
title: Pre-Trained entiteiten toevoegen aan een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het toevoegen van Pre-trained entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c198dfc19a350188f500af86c531be9a9ac424ce
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796740"
---
# <a name="how-to-add-pre-trained-entities"></a>Pre-trained entiteiten toevoegen
In deze zelfstudie laat zien hoe Pre-Trained entiteiten toevoegen aan uw Model van de cursist conversatie.

## <a name="video"></a>Video

[![Vooraf getrainde entiteiten zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Vooraf getrainde entiteiten herkennen algemene typen entiteiten, zoals getallen, datums, monetaire bedragen en anderen.  Ze werken "out-of-the-box," hoeven niet alle training en hun gedrag kan niet worden gewijzigd in tegenstelling tot aangepaste entiteiten.  Standaard Pre-Trained entiteiten zijn meerdere waarden samenvoegen van elke geïdentificeerde exemplaar van de entiteit.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'PretrainedEntities' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Pre-Trained/datetimeV2' voor het "entiteitstype'.
3. Het selectievakje 'Meerdere waarden'.
    - Entiteiten van meerdere waarden worden verzameld van een of meer waarden in de entiteit.
    - Negatable eigenschappen zijn uitgeschakeld voor Pre-Trained entiteiten.
4. Klik op de knop 'Maken'.

![](../media/tutorial7_entities_a.PNG)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u ' de datum is $builtin-datetimev2 "
3. Klik op de knop 'Maken'.

![](../media/tutorial7_actions_a.PNG)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u 'Wat is de datum?'
    - Vooraf getrainde entiteiten kunnen vereiste entiteiten niet als ze worden herkend die standaard voor alle uitingen van de gebruiker.
3. Typ in het veld "Diskwalificeren voorziet", "builtin-datetimev2."
4. Klik op de knop 'Maken'.

![](../media/tutorial7_actions2_a.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Hallo".
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, 'Wat is de datum?'
5. In het deelvenster chat, waar de status "Type uw bericht …", typt u 'nu'
    - De vandaag utterance wordt automatisch herkend door vooraf getrainde modellen in LUIS.
    - Waarden van Pre-Trained entiteiten aanwijzen, ziet u aanvullende gegevens van LUIS.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteit Resolvers](./09-entity-resolvers.md)
