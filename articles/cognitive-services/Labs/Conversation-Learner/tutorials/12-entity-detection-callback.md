---
title: Entiteit detectie callback gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteit detectie callback met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 147887151abd5c1f7455b0efbf9aadbc2d884183
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796741"
---
# <a name="how-to-use-entity-detection-callback"></a>Het gebruik van entiteit detectie terugbellen

In deze zelfstudie ziet u de callback van de detectie van entiteit en ziet u een algemeen patroon voor het oplossen van entiteiten.

## <a name="video"></a>Video

[![Entiteit detectie Callback zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_EntityDetection_Preview)](https://aka.ms/cl_Tutorial_v3_EntityDetection)

## <a name="requirements"></a>Vereisten
In deze zelfstudie vereist dat de `tutorialEntityDetectionCallback` bot wordt uitgevoerd.

    npm run tutorial-entity-detection

## <a name="details"></a>Details
Entiteit detectie callbacks inschakelen wijziging van het gedrag van entiteiten erkenning en manipuleren van de entiteit via code. We zullen deze functionaliteit uitproberen door te gaan met een ontwerppatroon van typische entiteit detectie callback uw. Bijvoorbeeld, het oplossen van 'de big apple' naar 'Antwerpen'.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'EntityDetectionCallback' en druk op enter.
3. Klik op de knop 'Maken'.

Drie entiteiten nodig zijn in dit voorbeeld, maken we de drie.

### <a name="create-the-custom-trained-entity"></a>De aangepaste getrainde entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepaste getraind' voor het "entiteitstype'.
3. Type "plaats" voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

### <a name="create-the-first-programmatic-entity"></a>De eerste programmatische entiteit maken

1. Klik op de knop 'Nieuwe entiteit'.
2. Selecteer 'Programmatische' voor het "entiteitstype'.
3. Type 'CityUnknown' voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

### <a name="create-the-first-programmatic-entity"></a>De eerste programmatische entiteit maken

1. Klik op de knop 'Nieuwe entiteit'.
2. Selecteer 'Programmatische' voor het "entiteitstype'.
3. Type 'CityResolved' voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

Maak nu de drie acties.

### <a name="action-creation"></a>Actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Welke stad wilt u?"
3. Typ in het veld 'Verwachte entiteit in antwoord op gebruiker...', 'Plaats'.
4. Typ in het veld 'Entiteiten diskwalificeren', 'Plaats'.
5. Klik op de knop 'Maken'.
6. Klik op de knop 'Nieuwe actie'.
7. In de 'van de Bot response...' veld, typt u "Welke stad wilt u?"
8. Typ in het veld 'Verwachte entiteit in antwoord op gebruiker...', "Ik weet niet deze plaats."
9. Klik op de knop 'Maken'.
10. Klik op de knop 'Nieuwe actie'.
11. In de 'van de Bot response...' veld, typt u "$CityResolved is heel fijn."
12. Klik op de knop 'Maken'.

Dit is de callback-code:

![](../media/tutorial10_callbackcode.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo'
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Welke stad wilt u?"
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'big apple'
6. Klik op de knop 'Acties Score'.
    - Op de knop activeert de callback van de detectie van entiteit
    - De callback-code stelt u de waarde CityResolved entiteit correct naar "new york"
7. Selecteer de respons ' new york is is heel fijn. "

Dit patroon is karakteristiek voor veel bot-scenario's. Gebruiker uitingen en uitgepakte entiteiten worden geleverd aan uw bedrijfslogica en die logica transformeert de utterance in canonieke vorm, die vervolgens wordt opgeslagen in programmatische entiteiten voor latere wordt van het dialoogvenster.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sessie retouraanroepen](./13-session-callbacks.md)
