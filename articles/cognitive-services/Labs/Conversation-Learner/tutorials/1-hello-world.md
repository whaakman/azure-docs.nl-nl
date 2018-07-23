---
title: Over het maken van een model Conversatiecursist "Hallo wereld" - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een model Conversatiecursist "Hallo wereld".
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 70b8f25bd699cbdb069892d65bf766ef3953f59d
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170868"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Over het maken van een 'Hallo wereld'-model met Conversatiecursist

Deze zelfstudie laat zien hoe u aan de slag met de conversatie Learner, met inbegrip van het maken van acties, geven interactief en correcties van geregistreerde dialoogvensters van eindgebruikers te maken.

## <a name="video"></a>Video

[![Zelfstudie 1-Preview](http://aka.ms/cl-tutorial-01-preview)](http://aka.ms/blis-tutorial-01)


## <a name="requirements"></a>Vereisten
Als u niet hebt gedaan, eerst controleert u of alle stappen van de setup zijn voltooid, met inbegrip van het maken van een `.env` bestand met uw LUIS sleutel ontwerpen.  Zie [snelstartgids](https://github.com/Microsoft/ConversationLearner-Samples) voor meer informatie.

In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het model maken
1. Klik op Nieuw Model
2. Voer in het veld naam Hello World
3. Klik op Maken

### <a name="create-an-action"></a>Een actie maken

1. Klik op het ' Hallo wereld '-model te starten
2. Klik op acties en vervolgens nieuwe actie
    - Een actie is een SMS-bericht die Conversatiecursist geretourneerd naar de gebruiker, een API-aanroep of een kaart.
3. Typ in antwoord 'Hallo wereld!'
    - Dit is het antwoord dat de bot wordt geretourneerd
4. Klik op Maken

U hebt gemaakt dat het eerste wat de bot kunt doen, dat wil zeggen een tekstantwoord retourneren.

### <a name="train-the-bot"></a>De bot te trainen

#### <a name="create-the-first-dialog"></a>Het eerste dialoogvenster maken

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein
2. Voer een voorbeeld van wat de gebruiker in de begging van de conversatie, bijvoorbeeld 'Hallo' zeggen.
3. Klik op Score acties
4. Selecteer "Hallo wereld!"
    - Hiermee maakt u een dialoogvenster Voorbeeld van een inschakelen. 
2. Voer "tot ziens"
3. Klik op Score acties
4. Klik op actie toevoegen, voert u "Tot ziens!" in het antwoord, en klik op 'Maken'
5. Klik op het onderwijs gereed. Dit verloopt dit dialoogvenster training.

U hebt nu een dialoogvenster voor onderwijs in het systeem.

#### <a name="continue-teaching-the-bot"></a>Doorgaan met de bot onderwijs
Laten we doen een meer training en Zie hoe de bot reageert.

1. Klik op het dialoogvenster Nieuw Train
2. Voer ' Hallo er '
    - Dit is vergelijkbaar met het eerste dialoogvenster en we verwachten dat een goede score ophalen van de bot.
2. Klik op Score actie
    - De positie en de score nog niet genoeg nauwkeurige en aanvullende onderwijs zijn vereist.
3. Klik op selecteren naast 'Hallo wereld!'
4. Voer vervolgens 'bye'
5. Klik op Score acties
6. Selecteer "Tot ziens!"
7. Klik op gereed onderwijs

![](../media/tutorial1_actions.PNG)

Er wordt een andere sessie voor onderwijs als u wilt zien hoe de bot werkt doen.

Herhaal de bovenstaande stappen met behulp van 'Hallo' en 'byebye', en noteer de wijzigingen in de positie en de score van het antwoord bots wanneer u klikt op Score actie.

U kunt nu de stappen met behulp van "howdy" en "tot ziens" en houd er rekening mee dat de score bevat verbeteringen in scores die wijzen op de bot deze interactie heeft geleerd.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Testen van de bot als een eindgebruiker

1. Klik op logboek dialoogvensters, en vervolgens Nieuw Log-dialoogvenster
2. Type "Hallo allemaal"
3. Vervolgens 'bye'

U kunt ook proberen te starten van een gesprek met 'bye', en noteer de reactie van de bot.

### <a name="view-conversations-in-the-log-dialogs"></a>Weergave-conversaties in het logboek-dialoogvensters

In de dialoogvensters logboek, kunt u de lijst weergeven van gesprekken, bijwerken en opslaan van de interacties als training-dialoogvensters. Om dat te doen:

1. Klik op het logboek van een gesprek
2. Als de conversatie ziet er goed uit, klikt u op de laatste actie bijvoorbeeld "Tot ziens".
3. Klik op het voorgestelde antwoord. 
    - U kunt ook selecteren of een andere actie toevoegen.
4. Vervolgens klikt u op gereed om op te slaan dit als een dialoogvenster training.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wacht even en acties voor niet-wait](./2-wait-vs-nonwait-actions.md)