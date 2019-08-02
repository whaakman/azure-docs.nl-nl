---
title: Sessie-Call backs gebruiken met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van sessie-Call backs met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 656d028082d56b8f094e83363e5189b163581c53
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703949"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Sessie-Call backs gebruiken met een Conversation Learner model

In deze zelf studie worden sessies geïntroduceerd, de manier waarop deze worden verwerkt en de onSessionStart en onSessionEnd-Call backs worden Conversation Learner.

## <a name="video"></a>Video

[![Zelf studie voor sessie-Call backs](https://aka.ms/cl_Tutorial_v3_SessionCallbacks_Preview)](https://aka.ms/cl_Tutorial_v3_SessionCallbacks)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot ' tutorialSessionCallbacks ' worden uitgevoerd.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
In deze zelf studie wordt het concept van een sessie beschreven, hoe sessies standaard worden verwerkt en hoe u dit gedrag kunt negeren.

In Conversation Learner wordt een sessie vertegenwoordigd door één, niet-onderbroken interactieve uitwisseling met de bot. Sessies kunnen meerdere beurten hebben, maar via een programma worden beëindigd wegens inactiviteit als deze langer dan dertig minuten zijn verstreken.  Zie de Help-pagina op ' limieten ' voor informatie over het wijzigen van de time-outwaarde voor de standaard sessie.

Deze lange periode van inactiviteit zorgt ervoor dat de bot een nieuwe sessie maakt en het huidige Neural-netwerk opnieuw instelt op de oorspronkelijke staat. Standaard worden alle entiteits waarden gewist. Dit standaard gedrag van het wissen van entiteits waarden kan worden gewijzigd, zoals hieronder wordt weer gegeven.

### <a name="load-the-demo-model"></a>Het demo model laden

Klik in de webgebruikersinterface op zelf studies importeren en selecteer het model met de naam ' zelf studie-13-SessionCallbacks '.

### <a name="code-for-the-callbacks"></a>Code voor de retour aanroepen

Voorbeeld code voor de twee Call backs van dit model vindt u in `c:\<installedpath>\src\demos\tutorialSessionCallbacks.ts`:.

![](../media/tutorial11_code.PNG)

- OnSessionStartCallback: met deze methode wordt de entiteit BotName ingesteld.
- OnSessionEndCallback: u kunt opgeven wat u wilt behouden. Hiermee worden alle entiteiten gewist, met uitzonde ring van gebruikers naam en gebruikers nummer.

Elke retour aanroep is optioneel.

### <a name="actions"></a>Acties

In het model worden vier acties gedefinieerd. De bestaande acties worden weer gegeven in de raster weergave voor acties

![](../media/tutorial11_actions.PNG)

### <a name="creating-an-end-session-action-for-callback-invocation"></a>Een actie voor een eind sessie maken (voor het aanroepen van een aanroep)

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. Selecteer ' ENDSESSION ' voor het entiteits type '.
3. In de gegevens... typt u "Gereed"
4. Klik op de knop maken.

### <a name="edit-an-existing-action"></a>Een bestaande actie bewerken

1. Selecteer de actie ' so, $UserName, u bevindt zich in $UserLocation ' in de raster weergave.
2. Schakel het selectie vakje ' wachten op antwoord ' uit.
3. Klik op de knop Opslaan.

### <a name="chaining-actions"></a>Koppelings acties

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht "Typ uw boodschap..." typt u "Hallo".
3. Klik op de knop ' Score acties '.
4. Selecteer het antwoord ' Hallo, ik ben Botty. Wat is je naam? '
5. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Lars '
6. Selecteer het antwoord ' Hi Lars. Wat is uw telefoon nummer? "
7. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' 555-555-5555 '
8. Klik op de knop ' Score acties '.
9. Selecteer de reactie ' kunt u Botty uw locatie Lars? '
10. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Seattle '
11. Klik op de knop ' Score acties '.
12. Selecteer de reactie "so, Lars, u in Seattle"
13. Selecteer de reactie ' gereed '
14. Klik op de knop Opslaan.

### <a name="testing-the-model"></a>Het model testen

1. Klik in het linkerdeel venster op dialoog vensters logboeken en vervolgens op de knop nieuw logboek.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Hallo '
3. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Lars '
4. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' 555-555-5555 '
5. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Seattle '
    - Op dit moment moeten alle entiteits waarden met uitzonde ring van locatie behouden blijven.
6. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Hallo '
7. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Detroit '
8. Klik op de knop sessietime time-out.
    - Als u op deze knop klikt, wordt het antwoord van de bot genoteerd op lange Peri Oden van inactiviteit
9. Klik op de knop OK.
10. Klik op de knop testen is voltooid.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [API-aanroepen](./14-api-calls.md)
