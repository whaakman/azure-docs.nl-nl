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
ms.openlocfilehash: f633dd375d690a1c3e66a2a6e02ae69665dbe960
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170630"
---
# <a name="demo-password-reset"></a>Demo: Wachtwoord opnieuw instellen
Deze demonstratie ziet u een eenvoudige technische ondersteuning-bot die u bij het opnieuw instellen van wachtwoorden helpen kan. 

Hier ziet u hoe Conversatiecursist essentieel dialoogvenster stromen, zet meerdere reeksen, met inbegrip van een klasse out-van-domein kunt leren. Deze demonstratie gebruikt geen eventuele code of entiteiten.

## <a name="video"></a>Video

[![Voorbeeld van de demo-wachtwoord](http://aka.ms/cl-demo-password-preview)](http://aka.ms/blis-demo-password)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat het wachtwoord opnieuw instellen van bot wordt uitgevoerd

    npm run demo-password

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op zelfstudie Demo wachtwoord opnieuw instellen. 

### <a name="actions"></a>Acties

We hebt set acties waarbij de gebruiker hulp nodig met het wachtwoord hebt is, inclusief oplossingen gemaakt.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Dialoogvensters voor training

Er zijn een aantal training-dialoogvensters. Er zijn ook demonstraties van een out-of domain-klasse--bijvoorbeeld aanvragen van gebruikers, zoals 'routebeschrijvingen' zich buiten het domein. de bot voorbeelden van enkele buiten het domein aanvragen heeft gekregen en kan reageren met 'Ik niet kan helpen bij die.'

![](../media/tutorial_pw_reset_entities.PNG)

Bijvoorbeeld: we proberen een sessie voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'Ik mijn wachtwoord verloren'.
2. Klik op Score actie.
3. Klik om te selecteren, Is dat u voor uw lokaal account of een Microsoft-account?'
4. Voer 'Lokale account'.
5. Klik op Score acties.
3. Klik om te selecteren, welke versie van Windows hebt u?"
4. Voer ' Windows 8'.
5. Klik op Score acties.
6. Selecteer ' oplossing: het wachtwoord opnieuw instellen op Windows 8.'
4. Klik op het onderwijs gereed.

We proberen de bot leert hoe een klasse out-van-domein.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'zoeken op het web'.
    - Dit is een voorbeeld van de klasse out-van-domein. 
2. Klik op Score actie.
3. Klik om te selecteren 'Sorry, ik niet kan helpen bij die'.
    - U ziet dat de score voor deze optie is momenteel laag. Maar na iets meer onderwijs, de score hoger wordt ontvangen.
4. Klik op het onderwijs gereed.

U hebt nu gezien over het maken van een basic-technische ondersteuning-demo en hoe deze oplossingen bieden, en ook verwerken buiten voorbeeldquery's leert.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - pizza volgorde](./demo-pizza-order.md)
