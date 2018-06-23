---
title: Wacht even en niet-wait-acties gebruiken met de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik wacht en niet-wait-acties met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bb2cbd55b6c8be51213095926bb592169613d1fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345262"
---
# <a name="wait-and-non-wait-actions"></a>Wacht even en niet-wait-acties

Deze zelfstudie ziet het verschil tussen de wachttijd acties en niet-wait-acties in de conversatie cursist.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Actie wachten: nadat het systeem heeft een actie 'wacht', wordt u acties stoppen en wachten op invoer van gebruiker.
- Niet-wait-actie: nadat het systeem heeft een actie 'niet-wait', wordt onmiddellijk gekozen nog een actie (zonder te wachten gebruiker inpu eerst in).

## <a name="steps"></a>Stappen

### <a name="create-a-new-app"></a>Een nieuwe app maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam WaitNonWait. Klik vervolgens op maken.

### <a name="create-the-first-wait-action"></a>De eerste actie in de wacht maken

1. Klik op acties en vervolgens nieuwe actie.
2. In het antwoord, voer 'welke dieren wilt u?'.
    - Dit is een actie wachten, dus laat de wachten op reactie selectievakje is ingeschakeld.
3. Klik op gereed.

### <a name="create-a-non-wait-action"></a>Maken van een niet-Wait-actie

1. Klik op nieuwe actie
2. In het antwoord, typ 'Moo dat spreken'.
3. Ongedaan maken-controle van de wachttijd voor het selectievakje in als antwoord.
4. Klik op Maken

### <a name="create-a-second-non-wait-action"></a>Maak een tweede Non-Wait-actie

1. Klik op nieuwe actie
2. In het antwoord, typ 'Eenden spreken quack'.
3. Ongedaan maken-controle van de wachttijd voor het selectievakje in als antwoord.
4. Klik op Maken

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Type 'Hallo'
3. Klik op Score acties en selecteer 'welke dieren wilt u?'.
4. Voer de ook'
5. Klik op Score acties en selecteer 'Moo dat spreken'.
    - Houd er rekening mee dat de bot niet op invoer wacht en zal de volgende actie ondernemen.
2. Selecteer 'welke dieren wilt u?'.
3. Voer 'duck'
5. Klik op Score acties en selecteer 'Eenden spreken quack'.

![](../media/tutorial2_dialogs.PNG)

Noteer de volgorde van de antwoorden bot met betrekking tot de wachttijd en niet-wait-acties.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot entiteiten](./3-introduction-to-entities.md)
