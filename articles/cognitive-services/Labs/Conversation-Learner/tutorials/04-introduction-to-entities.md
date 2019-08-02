---
title: Entiteiten gebruiken met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van entiteiten met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: cba12b6c09c1bdbf4e8f7841676a609c34109d93
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707343"
---
# <a name="introduction-to-entities"></a>Inleiding tot entiteiten

In deze zelf studie worden entiteiten geïntroduceerd, entiteiten in aanmerking nemen, vereiste entiteiten en hun gebruik binnen Conversation Learner.

## <a name="video"></a>Video

[![Preview-versie van de zelf studie over entiteiten](https://aka.ms/cl_Tutorial_v3_IntroEntities_Preview)](https://aka.ms/cl_Tutorial_v3_IntroEntities)

## <a name="requirements"></a>Vereisten

Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Entiteiten leggen de gegevens vast die de bot nodig heeft om de taak uit te voeren, hetzij door middel van extractie van gebruikers uitingen of-toewijzing op aangepaste code. Entiteiten zelf kunnen ook de beschik baarheid van acties beperken door expliciet te worden geclassificeerd als "vereist" of "diskwalificerende".

- De vereiste entiteiten moeten aanwezig zijn in het geheugen van het model om de actie beschikbaar te maken
- Het niet kwalificeren van entiteiten mag *niet* aanwezig zijn in het geheugen van het model om de actie beschikbaar te maken

Deze zelf studie is gericht op aangepaste entiteiten. Voor pretrainde, met meerdere waarden afgeleide entiteiten en programmatische entiteiten worden in andere zelf studies geïntroduceerd.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik in de gebruikers interface van de web op ' nieuw model '.
2. Typ ' IntroToEntities ' in het veld ' naam ' en druk op ENTER.
3. Klik op de knop maken.

### <a name="entity-creation"></a>Entiteit maken

1. Klik in het linkerdeel venster op entiteiten en vervolgens op de knop nieuwe entiteit.
2. Selecteer aangepast getraind voor het entiteits type.
3. Typ ' City ' voor de ' entiteits naam '.
4. Klik op de knop maken.

> [!NOTE]
> Het entiteits type aangepast getraind geeft aan dat deze entiteit kan worden getraind, in tegens telling tot andere typen entiteiten.

### <a name="create-the-actions"></a>De acties maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' Ik weet niet welke plaats je wilt. '
3. Typ ' City ' in het veld ' entiteiten kwalificeren '.
4. Klik op de knop maken.

> [!NOTE]
> Als de entiteit ' City ' wordt toegevoegd aan ' diskwalificerende entiteiten ', zou deze actie niet in aanmerking komen voor de overweging van de bot wanneer de entiteit ' City ' is gedefinieerd in het geheugen van de bot.

Maak nu een tweede actie.

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' het weer in de $city is waarschijnlijk zon. '
3. Klik op de knop maken.

> [!NOTE]
> De entiteit ' City ' is automatisch toegevoegd aan de lijst met vereiste entiteiten, op basis van de verwijzing in het antwoord.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-model"></a>Het model trainen

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht "Typ uw boodschap..." typt u "Hallo".
    - Hiermee wordt de kant van de gebruiker van de conversatie gesimuleerd.
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie ' Ik weet niet welke plaats je wilt. '
5. Reageer als gebruiker op ' Seattle '.
6. Klik op de knop ' Score acties '.
7. Selecteer de reactie: ' weer in $city is waarschijnlijk zon. '
8. Klik op de knop Opslaan.

![](../media/tutorial3_entities.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verwachte entiteit](./05-expected-entity.md)
