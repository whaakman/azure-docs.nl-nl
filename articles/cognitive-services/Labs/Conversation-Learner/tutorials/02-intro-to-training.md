---
title: Inleiding tot het trainen van een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Leer hoe u een zoals vertakkingen en bewerken van vorige trainingen via Conversatiecursist model te trainen.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1e6dea521207ec00efe948a879c705bd28a75d57
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213188"
---
# <a name="introduction-to-training"></a>Inleiding tot Training

In deze zelfstudie leert de basisprincipes van het trainen van een Model, vertakkingen maken uit een nieuwe training op basis van een vorige trainingen en bewerken van een Bot-antwoord om te wijzigen.

## <a name="video"></a>Video

[![Inleiding tot zelfstudie Preview Training](https://aka.ms/cl_Tutorial_v3_IntroTraining_Preview)](https://aka.ms/cl_Tutorial_v3_IntroTraining)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Acties: Een Bot-reactie op gebruikersinvoer.
- Train: De manier waarop we leren een Bot om te reageren op invoer van de gebruiker.
- Vertakking: De wijziging van de invoer van een gebruiker binnen een opgeslagen dialoogvenster van de trein ten behoeve van het maken van een nieuw dialoogvenster van de trein die gelijk zijn aan de oorspronkelijke instantie, maar duurt het gesprek in een andere richting.

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw model maken

1. Klik op Nieuw Model in de Web-UI
2. Typ voor de 'naam', 'Bot inspireren'. Klik vervolgens op maken.

### <a name="create-an-action"></a>Een actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot-response' Voer "Hallo! Wilt u inspireren vandaag? ".
    - Laat andere velden en selectievakjes op de standaardinstelling.
3. Klik op Maken.

### <a name="first-training-and-creating-another-action-while-training"></a>Eerst trainings- en het maken van een andere actie tijdens het trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Hallo". 
    - Dit simuleert de kant van de gebruiker van de conversatie.
3. Klik op 'Acties Score'.
4. Selecteer het antwoord, "Hallo! Wilt u inspireren vandaag? ".
5. Beantwoorden 'Ja', als de gebruiker.
6. Klik op 'Acties Score'.
7. Klik op het "+ actie" knop. 
    - Hiermee gaat u naar de bekend in het dialoogvenster 'Een actie maken'.
8. Typ in de reactie van de Bot als "U bent geweldige."
9. Klik op Maken.
10. U ziet dat de Bot direct reageert.
11. Klik op de knop 'Opslaan'.

### <a name="branch-a-second-training-off-of-the-first-training"></a>Een tweede Training af bij de eerste Training vertakking
1. Klik op de rasterrij met een overzicht van de eerste training. 
    - Hiermee kunt u bekijken en bewerken van de bestaande training.
2. Klik op het antwoord "Ja" gebruiker. 
    - Deze wordt besturingselementen bewerken weergegeven.
3. Klik op het pictogram vertakking. 
    - Er verschijnt nu een prompt voor de invoer van een andere gebruiker voor een nieuwe discussie.
4. Type in "Nee" hits invoeren of klik op de knop 'Maken'. 
    - Op dit moment hebt u een nieuw exemplaar van een dialoogvenster van de trein, de oorspronkelijke map blijft ongewijzigd.
5. Klik op 'Acties Score'.
6. Klik op onjuist antwoord van de Bot die net binnenkomen.
7. Klik op het "+ actie" knop 
    - zodat we een nieuwe actie voor de Bot reageert met maken kunt.
8. Typ in het antwoord van de Bot als 'geen probleem! Een prettige dag hebben."
9. Klik op Maken
10. U ziet dat de Bot direct reageert.
11. Klik op de knop 'Opslaan'.

### <a name="test-the-trainings"></a>U kunt de testen
1. Klik op "Log-dialoogvensters" en 'Dialoogvenster voor een nieuwe Log' in het linkerdeelvenster.
2. Typ in het bericht 'Hallo'. 
3. U ziet dat de Bot automatisch in de manier waarop die we deze getraind reageert.
4. Typ in het antwoord van de gebruiker, 'Ja'.
5. U ziet het antwoord Bot Hier ziet u dat de eerste training functioneert.
6. Klik op de knop 'Sessietime-out'. Hiermee wordt aangegeven Conversatiecursist dat we opnieuw wilt starten, negeert de conversatie schakelt die zojuist plaatsgevonden heeft.
7. Typ in het bericht 'Hallo'. 
8. U ziet dat de Bot automatisch in de manier waarop die we deze getraind reageert.
9. Typ in het antwoord van de gebruiker, "Nee".
10. U ziet het antwoord Bot Hier ziet u dat de tweede training functioneert.
11. Klik op de knop 'Gedaan testen'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wacht even en acties voor niet-wait](./03-wait-vs-nonwait-actions.md)
