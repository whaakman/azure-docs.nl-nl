---
title: Het registreren van dialoogvensters in de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het dialoogvensters aanmelden voor een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 477545c48aeca05d56fdae28ac65a8f381a482fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345318"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-application"></a>Dialoogvensters vastleggen in een toepassing conversatie cursist

Deze zelfstudie wordt beschreven hoe u eindgebruikers testen in de interface van de conversatie cursist; hoe dialoogvensters worden geregistreerd; en hoe corrigeren dialoogvensters vastgelegd om uw model te verbeteren.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
U kunt de dialoogvensters logboek om te controleren en correcties in dialoogvensters uitgevoerd met eindgebruikers kunt gebruiken.  U kunt in het bijzonder entiteit labels en actie selecties voor het verbeteren van de prestaties van het getrainde model en het hele systeem oplossen. 

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam LogDialogs. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer plaats in de naam van de entiteit.
3. Klik op Maken.

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ in het antwoord, welke plaats?.
3. Voer $city in tot uitsluiting entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens nieuwe actie.
3. In het antwoord, typt u 'het in $city mooi weer is waarschijnlijk'.
4. Vereiste entiteiten invoeren $city.
4. Klik op Maken.

U hebt nu twee acties.

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Wat is het weer'.
3. Klik op Score acties, en selecteer welke plaats?
2. Voer 'Seattle'.
3. Dubbelklik op "Seattle" en selecteer plaats.
    - Dit gemarkeerd als een entiteit plaats.
5. Klik op Score-acties
6. Selecteer 'het in $city mooi weer is waarschijnlijk'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe actie en vervolgens nieuwe Train dialoogvenster.
2. Typ 'Wat is het weer in Haarlem?'. U ziet dat Seattle wordt gemarkeerd als een entiteit.
5. Klik op Score-acties 
6. Selecteer 'het in $city mooi weer is waarschijnlijk'.
7. Klik op het onderwijs gereed.

### <a name="try-the-bot-as-the-user"></a>Probeer de bot als de gebruiker
Stel we dat we deze bot geïmplementeerd voor gebruikers.

1. Klik op logboek dialoogvensters.
2. Klik op nieuwe Chat-sessie.
    - Dit geeft de bot als de gebruiker deze in de web-chat-besturingselement aan de linkerkant van de gebruikersinterface optreden zou. U kunt het gebied spatietekens aan de rechterkant negeren.
3. Typ 'Hallo'.
4. Bot-antwoord: welke plaats?
4. Type 'Boston'.
5. Bot-antwoord: welke plaats?
    - Dit lijkt niet rechts. Sla dus laten we dit dialoogvenster.
2. Klik op gereed testen.

Laten we een nieuwe sessie starten:

2. Klik op nieuwe Chat-sessie.
3. Type 'Prognose voor Boston'.
4. Bot-antwoord: welke plaats?
2. Klik op het onderwijs gereed.

Nu gaan we correcties in het tweede dialoogvenster aanbrengen:

1. Klik op 'Boston prognose' onder logboek-dialoogvensters.
    - Hiermee opent u de conversatie.
    - Als u klikt u op de gebruiker-zijde van de conversatie (hier van 'voorspelling voor Boston'), kunt u de entiteit labels wijzigen.
    - Als u op aan de kant van het systeem (hier in welke stad'), u kunt wijzigen welke actie is geselecteerd.
5. Klik op 'Prognose voor Boston'. 
    - Hier kunt u de hoofdoorzaak is dat Boston niet is gemarkeerd als een entiteit. We moeten wijzigen.
    - Dubbelklik op 'Boston' en selecteer vervolgens plaats.
    - Wijzigingen verzenden op en klik op opslaan. Hiermee wordt een dialoogvenster met training op basis van de wijzigingen, en u zet deze neer in de dialoogvensters training op het moment van de wijziging.
6. Selecteer 'het in $city mooi weer is waarschijnlijk.'
7. Klik op het onderwijs gereed. Als u nu naar Train-dialoogvensters gaat, ziet u dat de nieuwe actie wordt toegevoegd.

![](../media/tutorial9_logdiag1.PNG)

Nu we correcties aan te brengen naar het andere dialoogvenster maken:

1. Klik op 'Hallo' onder logboek-dialoogvensters.
    - Hiermee opent u de conversatie.
3. Opmerking: het antwoord 'Hallo' is 'welke stad'. Maar we wilt wijzigen die aan een item dat meer zinvol is. Een beter antwoord iets zou zijn, zoals 'hello, ik ben de bot weer'. Maar er is geen actie die die biedt zodat we hebben een maken.
4. Klik op de actie.
    - Typ in het antwoord ' Ik wil de bot weer. Ik kan helpen bij prognoses.'
6. Ongedaan maken-controle van de wachttijd voor het antwoord selectievakje in zodat het een niet-wait-actie.
7. Klik op Maken.
8. Klik vervolgens op deze nieuwe actie. Klik vervolgens op Save.
    - Hiermee wordt u terug naar dat punt in de trainingssessie.
6. Klik op selecteren welke plaats?
7. Type in 'Boston'. Dubbelklik op label Boston als een entiteit als dat niet al.
8. Klik op Score acties.
9. Schakel 'het in $city mooi weer is waarschijnlijk.'
10. Klik op het onderwijs gereed.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Detectie-callback entiteit](./10-entity-detection-callback.md)
