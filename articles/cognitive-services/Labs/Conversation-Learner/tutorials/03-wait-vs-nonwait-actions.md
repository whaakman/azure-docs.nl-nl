---
title: Wacht tijden en acties zonder wacht tijd gebruiken met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van wachtende en niet-wacht acties met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 5d9a67f0b413b624baec369a219547cf18e9dbd2
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705604"
---
# <a name="wait-and-non-wait-actions"></a>Wacht tijden en acties zonder wacht tijd

In deze zelf studie wordt het verschil weer gegeven tussen wacht acties en acties zonder wacht tijd in de Conversation Learner.

## <a name="video"></a>Video

[![Wachten op voor beeld van niet-wachtende zelf studie](https://aka.ms/cl_Tutorial_v3_WaitnonWait_Preview)](https://aka.ms/cl_Tutorial_v3_WaitnonWait)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Wacht actie: Nadat het systeem een actie ' wait ' heeft uitgevoerd, worden er geen acties meer uitgevoerd en wachten op invoer van de gebruiker.
- Niet-wachtende actie: Nadat het systeem de actie ' niet-wachten ' heeft uitgevoerd, wordt er onmiddellijk een andere actie gekozen (zonder te wachten op invoer van de gebruiker).

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw model maken

1. Klik in de gebruikers interface van de web op nieuw model
2. Typ in het veld naam ' wacht woord niet wachten ', druk op ENTER of klik op de knop maken.

### <a name="create-the-first-two-wait-actions"></a>De eerste twee wacht acties maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' wat pizza wilt u bevinden? '.
    - Dit is een wacht actie, dus zorg ervoor dat het selectie vakje wachten op antwoord is ingeschakeld.
3. Klik op de knop maken.
4. Herhaal deze stappen, maak een andere actie met ' pizza op de manier! ' Als reactie van de bot.

### <a name="train-using-those-wait-actions"></a>Train het gebruik van deze acties

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' hi '. 
    - Hiermee wordt de kant van de gebruiker van de conversatie gesimuleerd.
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie ' wat pizza wilt u bevallen? '.
5. Reageer als gebruiker op ' Margherita '.
6. Klik op de knop ' Score acties '.
7. Selecteer de reactie "pizza op de manier!".
8. Klik op de knop Opslaan.

### <a name="create-a-non-wait-action-while-training"></a>Een niet-wacht actie maken tijdens de training
Hoewel u de niet-wacht actie zoals eerder hebt gemaakt, kunt u deze ook in een trainings sessie maken.
1. Klik op de knop Nieuw trainen dialoog venster.
2. Als het gebruikers type in, "Hallo".
3. Klik op de knop ' Score acties '.
4. Klik op de knop + actie. 
    - Hiermee gaat u naar het vertrouwde ' actie maken ' in het dialoog venster.
5. Typ de reactie van de bot als ' Welkom bij de pizza-bot! '
6. Schakel het selectie vakje ' wachten op antwoord ' uit.
7. Klik op de knop maken.
    - U ziet dat de bot direct reageert met, ' Welkom bij de pizza-bot! ' en dat u opnieuw wordt gevraagd naar een andere bot-reactie. Dit komt doordat de reactie van de bot de niet-wacht actie is die we zojuist hebben gemaakt.
9. Selecteer de reactie ' wat pizza wilt u bevallen? '.
10. Reageer als gebruiker op ' Margherita '.
11. Klik op de knop ' Score acties '.
12. Selecteer de reactie "pizza op de manier!".
13. Klik op de knop Opslaan.

> [!NOTE]
> De volg orde van de bot-antwoorden met betrekking tot wachtende en niet-wacht acties.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot entiteiten](./04-introduction-to-entities.md)
