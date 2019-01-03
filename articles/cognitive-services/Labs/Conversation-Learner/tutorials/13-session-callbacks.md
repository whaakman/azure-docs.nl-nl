---
title: Sessie callbacks gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de callbacks sessie met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 5458d83fcc9fad50a2a52273ea6b5573ed90a97e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796739"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Sessie callbacks gebruiken met een model Conversatiecursist

Deze zelfstudie maakt u kennis sessies, hoe ze worden verwerkt en de Conversatiecursist onSessionStart en onSessionEnd callbacks.

## <a name="video"></a>Video

[![Sessie Callbacks zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_SessionCallbacks_Preview)](https://aka.ms/cl_Tutorial_v3_SessionCallbacks)

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialSessionCallbacks' wordt uitgevoerd.

    npm run tutorial-session-callbacks

## <a name="details"></a>Details
In deze zelfstudie bevat informatie over het concept van een sessie, hoe sessies standaard worden verwerkt en hoe u dit gedrag kunt negeren.

Een sessie vertegenwoordigt in Conversatiecursist, ononderbroken interactieve exchange met de bot. Sessies kan meerdere schakelt hebben, maar worden via een programma is gestopt vanwege inactiviteit als meer dan dertig minuten is opgegeven.  Zie de help-pagina op "Limieten" voor informatie over het wijzigen van deze time-out standaardduur van de sessie.

Deze lange periode van inactiviteit zorgt ervoor dat de bot te maken van een nieuwe sessie opnieuw instellen van het terugkerende neurale netwerk naar de oorspronkelijke staat. Standaard worden alle entiteitswaarden worden gewist. Dit standaardgedrag van het wissen van de entiteitswaarden kan worden gewijzigd zoals hieronder wordt weergegeven.

### <a name="load-the-demo-model"></a>Laden van de Demo-Model

In de web-UI, klik op "Zelfstudies importeren" en selecteer het model met de naam '-zelfstudie-13-SessionCallbacks'.

### <a name="code-for-the-callbacks"></a>Code voor de callbacks

Voorbeeldcode voor twee callbacks van dit Model kunnen u vinden in: `c:\<installedpath>\src\demos\tutorialSessionCallbacks.ts`.

![](../media/tutorial11_code.PNG)

- OnSessionStartCallback: deze methode stelt de BotName-entiteit.
- OnSessionEndCallback: u kunt opgeven wat u wilt behouden. Hiermee worden alle entiteiten met uitzondering van de gebruikersnaam en het telefoonnummer van de gebruiker gewist.

Elke retouraanroep is optioneel.

### <a name="actions"></a>Acties

Vier acties zijn gedefinieerd in het Model. De bestaande acties worden weergegeven in de rasterweergave voor 'Acties'

![](../media/tutorial11_actions.PNG)

### <a name="creating-an-end-session-action-for-callback-invocation"></a>Het maken van een End-sessie actie (voor de callback-aanroep)

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. Selecteer 'ENDSESSION' voor het "entiteitstype'.
3. Typ in het veld 'Gegevens...', 'Gereed'
4. Klik op de knop 'Maken'.

### <a name="edit-an-existing-action"></a>Een bestaande actie bewerken

1. Selecteer de "dus $UserName, u bent in $UserLocation" actie van de rasterweergave.
2. Schakel het selectievakje 'Wacht voor Response'.
3. Klik op de knop 'Opslaan'.

### <a name="chaining-actions"></a>Koppeling van acties

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo.'
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Hallo, ik ben Botty. Wat is de naam van uw?'
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Lars"
6. Selecteer het antwoord 'Hallo Lars. Wat is uw telefoonnummer?'
7. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "555-555-5555"
8. Klik op de knop 'Acties Score'.
9. Selecteer het antwoord, "Kunt u zien Botty uw locatie, Lars?"
10. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Seattle"
11. Klik op de knop 'Acties Score'.
12. Selecteer het antwoord "Zodat Lars, u in Seattle bent"
13. Selecteer het antwoord, "Voltooid"
14. Klik op de knop 'Opslaan'.

### <a name="testing-the-model"></a>Het Model testen

1. Klik op "Log-dialoogvensters" en vervolgens de knop 'Dialoogvenster voor een nieuwe Log' in het linkerdeelvenster.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo'
3. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Lars"
4. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "555-555-5555"
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Seattle"
    - Op dit moment moeten alle entiteitswaarden, met uitzondering van de locatie zijn bewaard.
6. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Hallo"
7. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Detroit"
8. Klik op de knop 'Sessietime-out'.
    - Met deze knop oefeningen van de bot-antwoord voor de lange perioden van inactiviteit
9. Klik op de knop 'OK'.
10. Klik op de knop 'Gedaan testen'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [API-aanroepen](./14-api-calls.md)
