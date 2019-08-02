---
title: Inleiding tot het trainen van een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het trainen van een model, inclusief vertakkingen en het bewerken van een vorige training via Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: c657025ce588363cf76ce10868d809a9aff69222
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705614"
---
# <a name="introduction-to-training"></a>Inleiding tot training

In deze zelf studie ziet u de basis beginselen van het trainen van een model, het uitdelen van een nieuwe training op basis van een vorige training en het bewerken van een bot-antwoord om het te wijzigen.

## <a name="video"></a>Video

[![Preview-zelf studie voor training](https://aka.ms/cl_Tutorial_v3_IntroTraining_Preview)](https://aka.ms/cl_Tutorial_v3_IntroTraining)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Regelen Een bot-reactie op invoer van de gebruiker.
- Trainen: De manier waarop we een bot leren om te reageren op invoer van de gebruiker.
- Vertakkingen De wijziging van een gebruikers invoer in een opgeslagen trein dialoog venster voor het maken van een nieuw trein dialoogvenster dat hetzelfde start als het origineel, maar neemt de conversatie in een andere richting.

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw model maken

1. Klik in de gebruikers interface van de web op nieuw model
2. Typ voor de naam ' bot inspireren '. Klik vervolgens op maken.

### <a name="create-an-action"></a>Een actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. Geef in het veld ' bot ' op ' Hallo! Wil je vandaag geïnspireerd zijn? '.
    - Wijzig alle andere velden en schakel de standaard instelling in.
3. Klik op Maken.

### <a name="first-training-and-creating-another-action-while-training"></a>Eerste training en maken van een andere actie tijdens de training

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' Hallo '. 
    - Hiermee wordt de kant van de gebruiker van de conversatie gesimuleerd.
3. Klik op ' Score acties '.
4. Selecteer het antwoord ' Hallo! Wil je vandaag geïnspireerd zijn? '.
5. Reageer als gebruiker op ' ja '.
6. Klik op ' Score acties '.
7. Klik op de knop + actie. 
    - Hiermee gaat u naar het vertrouwde ' actie maken ' in het dialoog venster.
8. Typ de reactie van de bot als ' u bent geweldig! '
9. Klik op Maken.
10. U ziet dat de bot direct reageert.
11. Klik op de knop Opslaan.

### <a name="branch-a-second-training-off-of-the-first-training"></a>Vertakking een tweede training van de eerste training
1. Klik op de rasterrij met een overzicht van de eerste training. 
    - Zo kunt u de bestaande training bekijken en bewerken.
2. Klik op het antwoord van de gebruiker op Ja. 
    - Dit maakt het bewerken van besturings elementen zichtbaar.
3. Klik op het vertakkings pictogram. 
    - Hiermee wordt een prompt weer gegeven voor een andere gebruikers invoer voor een nieuwe conversatie.
4. Typ ' nee ', druk op ENTER of klik op de knop maken. 
    - Op dit moment hebt u een nieuw exemplaar van een trein dialoogvenster. het oorspronkelijke venster blijft ongewijzigd.
5. Klik op ' Score acties '.
6. Klik op het onjuiste antwoord van de bot dat zojuist is verschenen.
7. Klik op de knop + actie 
    - zodat we een nieuwe actie voor de bot kunnen maken om te reageren met.
8. Typ de reactie van de bot als ' geen probleem! Hebt u een goede dag! '
9. Klik op Maken
10. U ziet dat de bot direct reageert.
11. Klik op de knop Opslaan.

### <a name="test-the-trainings"></a>De trainingen testen
1. Klik in het linkerdeel venster op dialoog vensters logboeken en vervolgens op nieuw logboek venster.
2. Typ ' hi ' in het bericht. 
3. U ziet dat de bot automatisch reageert op de manier waarop we deze hebben getraind.
4. Typ ' ja ' in het antwoord van de gebruiker.
5. Let op het antwoord van de bot. Dit betekent dat de eerste training werkt.
6. Klik op de knop sessietime time-out. Dit geeft aan Conversation Learner u opnieuw wilt beginnen, waardoor de conversatie wordt genegeerd.
7. Typ ' hi ' in het bericht. 
8. U ziet dat de bot automatisch reageert op de manier waarop we deze hebben getraind.
9. Typ het antwoord van de gebruiker, "nee".
10. Let op het bot-antwoord, het laat zien dat de tweede training werkt.
11. Klik op de knop testen is voltooid.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wacht tijden en acties zonder wacht tijd](./03-wait-vs-nonwait-actions.md)
