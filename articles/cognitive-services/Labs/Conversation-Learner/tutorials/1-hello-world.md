---
title: Het maken van een toepassing 'Hallo wereld' conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een toepassing 'Hallo wereld' conversatie cursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 30026285ac6dda45d2f5e3718aae741b928cf242
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345313"
---
# <a name="how-to-create-a-hello-world-application-with-conversation-learner"></a>Het maken van een 'Hallo wereld'-toepassing met de conversatie cursist

Deze zelfstudie laat zien hoe u aan de slag met de conversatie cursist, zoals het maken van acties, interactief onderwijs en correcties van geregistreerde dialoogvensters aanbrengen van de eindgebruikers.

## <a name="requirements"></a>Vereisten
Als u nog niet gedaan hebt, controleert u eerst alle installatiestappen zijn voltooid, zoals het maken van een `.env` bestand met uw LUIS sleutel ontwerpen.  Zie [Quick Start](https://github.com/Microsoft/ConversationLearner-Samples) voor meer informatie.

Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="steps"></a>Stappen

Start op de startpagina van de Webgebruikersinterface.

### <a name="create-the-app"></a>De app maken
1. Klik op nieuwe App
2. Voer in het veld Naam Hallo wereld
3. Klik op Maken

### <a name="create-an-action"></a>Een actie maken

1. Klik op de Hallo wereld-app te starten
2. Klik op acties en vervolgens nieuwe actie
    - Een actie is een SMS-bericht conversatie cursist aan de gebruiker, een API-aanroep of een kaart retourneert.
3. Typ in het antwoord, 'Hello World!'
    - Dit is het antwoord dat de bot wordt geretourneerd
4. Klik op Maken

U hebt gemaakt eerst de bot kunt doen, dat wil zeggen een tekstantwoord retourneren.

### <a name="train-the-bot"></a>De bot trainen

#### <a name="create-the-first-dialog"></a>Het eerste dialoogvenster maken

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster
2. Geef een voorbeeld van wat de gebruiker in de begging van de conversatie, bijvoorbeeld 'Hallo' zeggen.
3. Klik op Score-acties
4. Selecteer 'Hello World!'
    - Hiermee maakt u een voorbeeld van een inschakelen van dit dialoogvenster. 
2. Voer 'goodbye'
3. Klik op Score-acties
4. Klik op de actie toevoegen, voert u 'Tot ziens!' in het antwoord, klik vervolgens op 'Maken'
5. Klik op het onderwijs gereed. Hiermee beëindigt u dit dialoogvenster training.

U hebt nu een dialoogvenster voor onderwijs in het systeem.

#### <a name="continue-teaching-the-bot"></a>Gaan de bot onderwijs
We doen één meer training en Zie hoe de bot reageert.

1. Klik op nieuwe Train dialoogvenster
2. Voer ' Hallo er '
    - Dit is vergelijkbaar met het eerste dialoogvenster en naar verwachting krijgt een goede score van de bot ophalen.
2. Klik op Score actie
    - De positie en score nog niet genoeg juist en aanvullende onderwijs vereisen.
3. Klik op selecteren naast 'Hello World!'
4. Voer vervolgens "bye"
5. Klik op Score-acties
6. Selecteer 'Tot ziens!'
7. Klik op gereed onderwijs

![](../media/tutorial1_actions.PNG)

Een andere sessie voor onderwijs om te zien hoe de bot werkt wordt gedaan.

Herhaal de bovenstaande stappen met 'Hallo' en 'byebye' en noteert u de wijzigingen in de positie en de score van het antwoord bots wanneer u klikt op Score in te grijpen.

U kunt nu Herhaal de stappen die met 'howdy' en "goede bye" en houd er rekening mee dat de scoreprofiel bevat verbeteringen in die aangeeft van de bot scores deze interactie heeft geleerd.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>De bot testen als een eindgebruiker

1. Klik op logboek dialoogvensters, klikt u vervolgens nieuwe logboek dialoogvenster
2. Typ 'Hallo er'
3. Vervolgens bye

U kunt ook proberen te starten van een conversatie met "bye" en noteer de bot antwoord.

### <a name="view-conversations-in-the-log-dialogs"></a>Weergave conversaties in het logboek-dialoogvensters

In logboek dialoogvensters, kunt u de lijst weergeven van conversaties, bijwerken en de interacties als training-dialoogvensters opslaan. Dat doet:

1. Klik op het logboek van een conversatie
2. Als de conversatie er goed uitziet, klikt u op de laatste actie bv. "Goodbye".
3. Klik op het voorgestelde antwoord. 
    - U kunt ook selecteren of een andere actie toevoegen.
4. Klik vervolgens op gereed opslaan als een dialoogvenster training.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wacht even en niet-wait-acties](./2-wait-vs-nonwait-actions.md)