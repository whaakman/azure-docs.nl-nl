---
title: Hoe u zich aanmeldt dialoogvensters in een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Leer hoe u aan te melden dialoogvensters in een model Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 6ceeb9683a979256a8a52347fc74ab758fd1d348
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171432"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Hoe u zich aanmeldt dialoogvensters in een model Conversatiecursist

In deze zelfstudie leert u hoe u doet eindgebruiker testen in de interface Conversatiecursist; hoe dialoogvensters worden geregistreerd; en hoe u correcties in de dialoogvensters om te verbeteren van uw model vastgelegd.

## <a name="video"></a>Video

[![Zelfstudie 9-Preview](http://aka.ms/cl-tutorial-09-preview)](http://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
U kunt de dialoogvensters log gebruiken om te controleren en corrigeren dialoogvensters die wordt uitgevoerd met eindgebruikers.  Specifiek, kunt u oplossen entiteit labels en actie selecties voor het verbeteren van de prestaties van het getrainde model en het hele systeem. 

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam LogDialogs. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer in de naam van de entiteit, plaats.
3. Klik op Maken.

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. Typ in het antwoord 'Welke plaats?'.
3. Voer $city in diskwalificeren van entiteiten.
3. Klik op Maken

Maak vervolgens de tweede actie:

1. Klik op acties en vervolgens nieuwe actie.
3. Typ 'het weer in $city is waarschijnlijk zonnige' in het antwoord.
4. Vereiste entiteiten, voer $city.
4. Klik op Maken.

U hebt nu twee acties.

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Wat is het weer'.
3. Klik op Score acties, en selecteer welke plaats?
2. Voer 'Seattle'.
3. Dubbelklik op "Seattle" en selecteer plaats.
    - Hiermee markeert als een entiteit plaats.
5. Klik op Score acties
6. Selecteer 'het weer in $city is waarschijnlijk zonnige'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe actie en vervolgens nieuwe dialoogvenster van de trein.
2. Typ 'Wat is het weer in Seattle?'. U ziet dat Seattle is gemarkeerd als een entiteit.
5. Klik op Score acties 
6. Selecteer 'het weer in $city is waarschijnlijk zonnige'.
7. Klik op het onderwijs gereed.

### <a name="try-the-bot-as-the-user"></a>Probeer de bot als de gebruiker
Stel dat we deze bot hebt geïmplementeerd voor gebruikers.

1. Klik op logboek dialoogvensters.
2. Klik op nieuwe Chat-sessie.
    - Dit geeft de bot als de gebruiker deze in het besturingselement voor webchat aan de linkerkant van de gebruikersinterface optreden zou. U kunt het spatietekens gebied aan de rechterkant negeren.
3. Typ 'Hallo'.
4. Bot-antwoord: welke plaats?
4. Het type 'Boston'.
5. Bot-antwoord: welke plaats?
    - Dit lijkt niet rechts. Laten we dit dialoogvenster opslaan.
2. Klik op gereed testen.

Laten we een nieuwe sessie starten:

2. Klik op nieuwe Chat-sessie.
3. Het type 'Prognose voor Boston'.
4. Bot-antwoord: welke plaats?
2. Klik op het onderwijs gereed.

Nu maken we gaan correcties in het tweede dialoogvenster:

1. Klik op 'Prognose voor Boston' onder het Log-dialoogvensters.
    - Hiermee opent u de conversatie.
    - Als u klikt u op aan de gebruiker van de conversatie (hier op 'Prognose voor Boston'), kunt u labels van de entiteit.
    - Als u op het systeem aan clientzijde klikt (hier op 'welke plaats'), u kunt wijzigen welke actie wordt geselecteerd.
5. Klik op 'Prognose voor Boston'. 
    - Hier kunt u de hoofdoorzaak is dat Boston niet is gemarkeerd als een entiteit. We moeten wijzigen.
    - Dubbelklik op 'Boston' en selecteer vervolgens plaats.
    - Wijzigingen verzenden, en klik op opslaan. Dit maakt een training dialoogvenster op basis van de wijzigingen die u hebt gemaakt, en u zet deze neer in dialoogvensters training op het moment van de wijziging die u hebt gemaakt.
6. Selecteer 'het weer in $city is waarschijnlijk zonnige'.
7. Klik op het onderwijs gereed. Als u nu naar de dialoogvensters van de trein gaat, ziet u dat de nieuwe actie wordt toegevoegd.

![](../media/tutorial9_logdiag1.PNG)

Nu maken we gaan correcties aan de andere dialoogvenster:

1. Klik op 'Hallo' onder het Log-dialoogvensters.
    - Hiermee opent u de conversatie.
3. Het antwoord om 'Hallo' is 'welke plaats'. Maar we willen wijzigen in iets dat logischer. Een beter antwoord zou iets zoals 'Hallo, ik ben de bot weer'. Maar er is geen actie dat dit doet, zodat we hoeven te maken.
4. Klik op de actie.
    - Typ in het antwoord, "Ik ben de bot weer. Ik kan helpen bij de prognoses.'
6. Schakel de wachttijd voor het selectievakje in als antwoord zodat deze een niet-wait-actie.
7. Klik op Maken.
8. Klik om te selecteren van deze nieuwe actie. Klik vervolgens op Save.
    - Hiermee wordt u terug naar dat punt in de trainingssessie.
6. Klik op selecteren welke plaats?
7. Typ 'Boston'. Dubbelklik op label Boston als een entiteit als dit nog niet.
8. Klik op Score acties.
9. Klik om te selecteren 'het weer in $city is waarschijnlijk zonnige'.
10. Klik op het onderwijs gereed.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteit detectie terugbellen](./10-entity-detection-callback.md)
