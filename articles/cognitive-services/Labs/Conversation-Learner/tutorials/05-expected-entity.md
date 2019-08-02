---
title: De eigenschap verwachte entiteit van Conversation Learner acties gebruiken-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van de eigenschap ' verwachte entiteit ' van een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 5fbe4f09d377b9f157368184ab26341782e9aed1
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707327"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>De eigenschap ' verwachte entiteit ' van acties gebruiken

In deze zelf studie wordt de eigenschap verwachte entiteit van acties gedemonstreerd.

## <a name="video"></a>Video

[![Preview-versie van verwachte zelf studie voor entiteit](https://aka.ms/cl_Tutorial_v3_ExpectedEntity_Preview)](https://aka.ms/cl_Tutorial_v3_ExpectedEntity)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Gebruik de eigenschap verwachte entiteit van een actie om het antwoord van de gebruiker op deze actie op te slaan in een entiteit.

Wanneer entiteiten worden toegevoegd aan de eigenschap verwachte entiteit van een actie, wordt het volgende door het systeem uitgevoerd:

1. Begin door te proberen entiteiten te koppelen met behulp van de op machine learning gebaseerde entiteit extractie model
2. Wijs het hele gebruikers utterance toe aan $entity op basis van heuristiek als er geen entiteiten worden gevonden
3. Roep `EntityDetectionCallback`aan en ga door met actie selectie.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik in de gebruikers interface van de web op ' nieuw model '.
2. Typ ' ExpectedEntities ' in het veld ' naam ' en druk op ENTER.
3. Klik op de knop maken.

### <a name="entity-creation"></a>Entiteit maken

1. Klik in het linkerdeel venster op entiteiten en vervolgens op de knop nieuwe entiteit.
2. Selecteer aangepast getraind voor het entiteits type.
3. Typ ' naam ' voor de ' entiteits naam '.
4. Klik op de knop maken.

> [!NOTE]
> Het entiteits type aangepast getraind geeft aan dat deze entiteit kan worden getraind, in tegens telling tot andere typen entiteiten.

![](../media/tutorial4_entities.PNG)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' wat is uw naam? '
3. Typ ' naam ' in het veld ' verwachte entiteiten '.
4. Klik op de knop maken.

> [!NOTE]
> Entiteiten die zijn gedetecteerd en opgehaald van het antwoord van de gebruiker, worden opgeslagen in de entiteit ' naam ' als deze actie wordt gekozen. Als er geen entiteiten worden gedetecteerd, wordt het volledige antwoord opgeslagen naar deze entiteit.

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' Hi $name! '
3. Klik op de knop maken.

> [!NOTE]
> De entiteit ' naam ' is automatisch toegevoegd als een ' vereiste entiteiten ' op basis van de verwijzing in het antwoord.

U hebt nu twee acties.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-model"></a>Het model trainen

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht "Typ uw boodschap..." typt u "Hallo".
    - Hiermee wordt de kant van de gebruiker van de conversatie gesimuleerd.
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie ' wat is uw naam? '
    - De ' Hi $name! ' het antwoord kan niet worden geselecteerd omdat dit antwoord vereist dat de entiteit ' naam ' nu in het geheugen van het model moet worden gedefinieerd.
5. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' Frank '.
    - "Frank" is gemarkeerd als een entiteit op basis van de heuristisch die we eerder hebben ingesteld om het antwoord als de entiteit op te slaan.
6. Klik op de knop ' Score acties '.
    - De entiteit ' name ' is nu gedefinieerd als ' Frank ' in het geheugen van het model, dus de actie ' Hello $name ' kan worden geselecteerd als een actie.
7. Selecteer het antwoord ' Hi $name! '
8. Klik op de knop Opslaan.

Door alternatieve invoer toe te voegen, wordt het model verder getraind.

1. In de "alternatieve invoer toevoegen..." typt u ' Ik ben Jose '.
    - Het model herkent de naam niet als een entiteit, zodat het hele tekst blok als de waarde van de entiteit wordt geselecteerd
2. Klik op de woord groep ' Ik ben ' en klik vervolgens op het pictogram Prullenbak.
3. Klik op ' Jose ' en klik vervolgens op ' naam ' in de lijst met entiteiten.
4. Klik op Score acties.
5. Selecteer het antwoord "Hallo Frank!"
6. Klik op de knop Opslaan.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontkennende entiteiten](./06-negatable-entities.md)
