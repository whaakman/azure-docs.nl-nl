---
title: Demo conversatie cursist-toepassing voor wachtwoordherstel - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo conversatie cursist toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 24d61787a79ee1a1a9737c417aa966cc8fd75930
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345278"
---
# <a name="demo-password-reset"></a>Demo: Wachtwoord opnieuw instellen
Deze demo ziet u een eenvoudige technische ondersteuning bot die u bij het opnieuw instellen van wachtwoorden helpen kan. 

Er wordt weergegeven hoe conversatie cursist meer informatie over niet-triviale dialoogvenster stromen, schakel meerdere reeksen, met inbegrip van een klasse buiten het domein. Deze demonstratie gebruikt geen eventuele code of entiteiten.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat het wachtwoord opnieuw instellen van bot wordt uitgevoerd

    npm run demo-password

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op zelfstudie Demo voor wachtwoord opnieuw instellen. 

### <a name="actions"></a>Acties

Wij hebben set acties waarbij de gebruiker is op zoek naar help met hun wachtwoord inclusief oplossingen gemaakt.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Training dialoogvensters

Er zijn een aantal training dialoogvensters. Er zijn ook demonstraties van een out-of domain klasse--bijvoorbeeld gebruikersaanvragen zijn 'een routebeschrijving' buiten het domein. de bot voorbeelden van enkele buiten het domein aanvragen heeft gekregen en beantwoorden met 'Ik niet kan helpen bij die.'

![](../media/tutorial_pw_reset_entities.PNG)

Als u bijvoorbeeld een sessie onderwijs eens te proberen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'Ik mijn wachtwoord verloren'.
2. Klik op de actie Score.
3. Klik om te selecteren, Is dat u voor uw Microsoft-account of lokale account?'
4. Voer 'Lokale account'.
5. Klik op Score acties.
3. Schakel 'welke versie van Windows hebt u?'
4. Voer ' Windows 8'.
5. Klik op Score acties.
6. Selecteer ' oplossing: het wachtwoord opnieuw instellen op Windows 8.'
4. Klik op het onderwijs gereed.

We gaan de bot Ontdek hoe een klasse buiten het domein.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'zoeken op het web'.
    - Dit is een voorbeeld van de klasse buiten het domein. 
2. Klik op de actie Score.
3. Schakel 'Helaas, ik niet kan helpen bij die'.
    - U ziet dat de score voor deze optie is momenteel laag. Maar na wat meer onderwijs de score ontvangt hoger.
4. Klik op het onderwijs gereed.

U hebt nu het maken van een demo basic technische ondersteuning en hoe deze oplossingen bieden en ook verwerken buiten voorbeeldquery's meer gezien.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - pizza volgorde](./demo-pizza-order.md)
