---
title: Wacht even en niet-wait-acties gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van wacht even en niet-wait-acties met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2869e6c59388adf548e5f239d3a831a5a3f060dd
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214259"
---
# <a name="wait-and-non-wait-actions"></a>Wacht even en acties voor niet-wait

Deze zelfstudie leert het verschil tussen wacht acties en niet-wait-acties in de Conversatiecursist.

## <a name="video"></a>Video

[![Wacht versus Preview van niet-Wait-zelfstudie](https://aka.ms/cl_Tutorial_v3_WaitnonWait_Preview)](https://aka.ms/cl_Tutorial_v3_WaitnonWait)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Wachten op actie: Nadat het systeem heeft een actie 'wait', wordt niet meer acties en wachten op invoer van de gebruiker.
- Niet-wait-actie: Nadat het systeem heeft een actie 'niet-wait', wordt deze onmiddellijk nog een actie kiezen (zonder te wachten op invoer van de gebruiker).

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw model maken

1. Klik op Nieuw Model in de Web-UI
2. In het veld 'Name', ' wachten Non-Wait ' typt, drukt u op of klik op de knop 'Maken'.

### <a name="create-the-first-two-wait-actions"></a>De eerste twee wacht acties maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. Veld, typt u 'welke pizza wilt u dat?' in de 'van de Bot response...'.
    - Dit is een actie wachten, dus laat het selectievakje 'Wacht voor Response' is ingeschakeld.
3. Klik op de knop 'Maken'.
4. Deze stappen te herhalen, maak nog een actie met "Pizza op de manier waarop!" Als het antwoord van de Bot.

### <a name="train-using-those-wait-actions"></a>Trainen met behulp van deze acties wachten

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hi'. 
    - Dit simuleert de kant van de gebruiker van de conversatie.
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "welke pizza wilt u?".
5. Als de gebruiker reageren met 'Margherita'.
6. Klik op de knop 'Acties Score'.
7. Selecteer het antwoord, "Pizza op de manier waarop!".
8. Klik op de knop 'Opslaan'.

### <a name="create-a-non-wait-action-while-training"></a>Maken van een niet-Wait-actie tijdens het trainen
Hoewel u kunt de niet-Wait-actie maken kan, zoals u eerder hebt gedaan, kunt u het ook binnen een trainingssessie maken.
1. Klik op de knop 'Nieuwe Train dialoogvenster'.
2. Als het type van de gebruiker in "Hallo".
3. Klik op de knop 'Acties Score'.
4. Klik op het "+ actie" knop. 
    - Hiermee gaat u naar de bekend in het dialoogvenster 'Een actie maken'.
5. Typ in het antwoord van de Bot als "Welkom bij Pizza Bot!"
6. Schakel het selectievakje 'Wacht voor Response'.
7. Klik op de knop 'Maken'.
    - U ziet dat de Bot reageert onmiddellijk met 'Welkom bij Pizza-Bot." en dat u opnieuw bent gevraagd om een ander Bot-antwoord. Dit is omdat het antwoord van de Bot is de niet-Wait-actie die we zojuist hebben gemaakt.
9. Selecteer het antwoord, "welke pizza wilt u?".
10. Als de gebruiker reageren met 'Margherita'.
11. Klik op de knop 'Acties Score'.
12. Selecteer het antwoord, "Pizza op de manier waarop!".
13. Klik op de knop 'Opslaan'.

> [!NOTE]
> De volgorde van de bot antwoorden met betrekking tot wachten en acties voor niet-wait.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot entiteiten](./04-introduction-to-entities.md)
