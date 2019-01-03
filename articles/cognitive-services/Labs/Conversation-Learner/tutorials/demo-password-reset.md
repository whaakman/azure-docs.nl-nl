---
title: Demo Conversatiecursist model, wachtwoord opnieuw instellen - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 409647da146a2844384204cb03de5028d45e5763
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792387"
---
# <a name="demo-password-reset"></a>Demo: Wachtwoord opnieuw instellen
In deze zelfstudie worden een bot eenvoudig technische ondersteuning die u bij het opnieuw instellen van wachtwoorden mogelijk gemaakt door Conversatiecursist helpen kan gepresenteerd. Model van de bot kunt leren essentieel dialoogvenster stromen en schakelt u meerdere reeksen met inbegrip van de klassen van een out-van-domein. De taak kan worden bereikt zonder code of entiteiten.

## <a name="video"></a>Video

[![Voorbeeld van de demo-wachtwoord](https://aka.ms/cl_Tutorial_v3_DemoPassword_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPassword)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat het wachtwoord opnieuw instellen van bot wordt uitgevoerd

    npm run demo-password

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op zelfstudie Demo wachtwoord opnieuw instellen. 

### <a name="actions"></a>Acties

Het Model bevat een set acties die zijn ontworpen om u te helpen gebruikers oplossen van veelvoorkomende problemen met wachtwoord.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Dialoogvensters voor training

Het Model bevat ook enkele Train-dialoogvensters, inclusief een aantal die laten buiten het domein klasse training zien. Bijvoorbeeld, zoals gebruikers die kunnen u vragen een routebeschrijving. De voorbeeld-bot is getraind op een enkele voor demonstratiedoeleinden te gebruiken, en gewoon reageert door deze met de mededeling 'kunnen niet te helpen bij die." De lijst met bestaande Train-dialoogvensters bevindt zich onder het 'Train-dialoogvensters' in het linkerpaneel.

![](../media/tutorial_pw_reset_entities.PNG)

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "ik verloren mijn wachtwoord."
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Is die voor uw lokaal account of een Microsoft-account?"
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u in "lokaal account."
6. Klik op de knop 'Acties Score'.
7. Selecteer het antwoord, "welke versie van Windows hebt u?"
8. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'windows xp'
9. Klik op de knop 'Acties Score'.
10. Klik op het "+ actie" knop.
11. In de 'van de Bot response...' veld, typt u ' oplossing: Het wachtwoord opnieuw instellen op Windows XP.."
12. Klik op de knop 'Maken'.

### <a name="training-dialogs-for-out-of-domain-scenarios"></a>Dialoogvensters voor Out-van-domein-scenario's voor training

1. In het linkerdeelvenster, klik op "Train-dialoogvensters" en vervolgens de bestaande 'software winkels' dialoogvenster van de trein.
2. Klik op de utterance 'software winkels' in het deelvenster chat.
3. In het veld 'Toevoegen... alternatieve invoer' enter type 'zoeken op het web' en druk op.
4. In het veld 'Toevoegen alternatieve invoer...' enter 'vlucht reserveren' en druk op.
5. Klik op de knop 'Wijzigingen opslaan'.
6. Klik op de knop 'Opslaan bewerken'.
7. Klik op "Log-dialoogvensters" en vervolgens de knop 'Dialoogvenster voor een nieuwe Log' in het linkerdeelvenster.
8. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Ik kan mijn wachtwoord niet vinden"
9. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Microsoft-account
10. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Bedankt"
11. Klik op de knop 'Gedaan testen'.
12. Klik op het logboek "Ik kan mijn wachtwoord niet vinden" dialoogvenster van de rasterweergave.
13. Klik op het niet correct weergegeven in het deelvenster chat ' oplossing: Het antwoord van het opnieuw instellen van het wachtwoord van een Microsoft-Account'.
14. Klik op het "+ actie" knop.
15. In de 'van de Bot response...' veld, typt u "U bent Welkom"
16. Klik op de knop 'Maken'.
17. Klik op de knop 'Opslaan als Train dialoogvenster'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - pizza volgorde](./demo-pizza-order.md)
