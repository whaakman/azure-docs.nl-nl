---
title: Een Hallo wereld Conversation Learner model maken-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het maken van een model voor ' Hallo wereld ' Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bb1d053af8813f05872c56d3b5609f2d7d7d5d8c
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705642"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Een model ' Hallo wereld ' maken met Conversation Learner

In deze zelf studie leert u hoe u aan de slag gaat met Conversation Learner, zoals het maken van acties, het interactief samen stellen van de bot en het aanbrengen van correcties van geregistreerde dialoog vensters die afkomstig zijn van eind gebruikers.

## <a name="video"></a>Video

[![Preview van Hallo wereld-zelf studie](https://aka.ms/cl_Tutorial_v3_HelloWorld_Preview)](https://aka.ms/cl_tutorial_v3_helloworld)


## <a name="requirements"></a>Vereisten
Als u dat nog niet hebt gedaan, controleert u eerst of alle installatie stappen zijn voltooid `.env` , inclusief het maken van een bestand met uw Luis-ontwerp sleutel.  Zie [Quick](../quickstart.md) start voor meer informatie.

Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="steps"></a>Stappen

Start op de start pagina in de gebruikers interface van het web.

### <a name="create-the-model"></a>Het model maken
1. Klik op de knop Nieuw model.
2. Voer Hallo wereld in het veld naam in.
3. Klik op de knop maken.

U ziet nu de weer gave van het model dat u hebt gemaakt.

### <a name="create-an-action"></a>Een actie maken
1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
    - Een actie kan een tekst bericht zijn dat Conversation Learner terugkeert naar de gebruiker, een API-aanroep of een kaart.
2. In het antwoord van de bot... veld type "Hallo".
    - Dit is de reactie die wordt geretourneerd door de bot.
3. Klik op de knop maken.

U hebt de eerste actie gemaakt die de bot kan uitvoeren, d.w.z. het retour neren van een tekst antwoord.

### <a name="train-dialogs"></a>Dialoog vensters trainen
Hier traint u het model om te reageren op gebruikers uitingen.

#### <a name="first-training-dialog"></a>Eerste trainings venster

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. Typ ' hi ', druk op ENTER.
    - Als voor beeld van wat de gebruiker aan het begin van een gesprek kan zeggen.
3. Klik op de knop ' Score acties '.
4. Selecteer Hallo.
    - U hebt zojuist een volledige turn-functie in dit voor beeld voltooid. 
5. Typ het antwoord van de gebruiker, "tot ziens".
6. Klik op de knop ' Score acties '.
7. Klik op de knop + actie.
8. Typ "tot ziens" in het antwoord van de bot... en klik vervolgens op de knop maken.
    - U ziet dat de bot heeft gereageerd met de actie die u zojuist hebt gemaakt.
9. Klik op de knop Opslaan. 
    - Hiermee wordt het dialoog venster voor trainingen beëindigd en opgeslagen.

Nu hebt u één trainings venster in het model, samen met één entiteit en twee acties.

#### <a name="second-training-dialog"></a>Dialoog venster voor tweede training
Laten we nog een training volgen en zien hoe de bot reageert.

1. Klik op de knop Nieuw trainen dialoog venster.
2. Typ in, Hi
    - Dit is vergelijkbaar met het eerste dialoog venster en we verwachten een goede Score van de bot te krijgen.
3. Klik op de knop ' Score acties '.
    - De positie en Score zijn mogelijk nog niet nauw keurig genoeg en vereisen mogelijk extra training.
4. Selecteer Hallo.
5. Typ het antwoord van de gebruiker.
6. Klik op de knop ' Score acties '.
7. Selecteer "tot ziens"
8. Klik op de knop Opslaan.

### <a name="log-dialogs"></a>Dialoog vensters logboek
Hier kunt u uw gesprekken testen, weer geven en corrigeren die u of echte gebruikers hebben gehad met uw bot.

#### <a name="test-the-model-as-an-end-user"></a>Het model testen als een eind gebruiker
1. Klik in het linkerdeel venster op dialoog vensters logboeken en vervolgens op de knop nieuw logboek.
2. Typ ' Hallo daar '.
3. Een korte tijd geduld; de bot moet automatisch reageren met ' Hallo '
4. Voer ' Byebye ' in
5. Wacht even en opnieuw moet de bot automatisch reageren met ' Hallo '.
6. Klik op de knop testen is voltooid.

#### <a name="view-and-correct-a-user-conversation"></a>Een gebruikers conversatie bekijken en corrigeren
Met de dialoog vensters voor Logboeken kunt u de lijst met conversaties weer geven die gebruikers met uw bot hebben ondergaan. U kunt deze ook bewerken om de reacties van de bot te corrigeren en de interacties als trainings dialoogvensters op te slaan. Hiervoor doet u het volgende:
1. Klik in het raster op het logboek van de conversatie.
2. Klik op de laatste bot-actie, bijvoorbeeld "Hallo".
3. Selecteer "tot ziens" om de bot te corrigeren.
4. Klik op de knop Opslaan als trein venster.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot training](./02-intro-to-training.md)
