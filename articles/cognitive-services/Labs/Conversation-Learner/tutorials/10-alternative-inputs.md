---
title: Alternatieve invoer gebruiken met Conversation Learner-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van alternatieve invoer met Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 7416939db8d6552f79fba700b8432de2ad228846
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704201"
---
# <a name="how-to-use-alternative-inputs"></a>Alternatieve invoer gebruiken

In deze zelf studie ziet u hoe u het veld alternatieve invoer gegevens gebruikt voor gebruikers uitingen in de interface voor onderwijs.

## <a name="video"></a>Video

[![Preview van zelf studie voor alternatieve invoer](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Alternatieve invoer is een alternatieve, semantisch gelijkwaardige gebruiker uitingen die de gebruiker op een bepaald punt in een trainings dialoogvenster kan hebben genoemd. Met deze alternatieve invoer kunt u op een flexibele manier uitingen opgeven zonder dat u elke variatie in afzonderlijke trainings dialoogvensters hoeft op te lossen.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik in de gebruikers interface van de web op ' nieuw model '.
2. Typ ' AlternativeInputs ' in het veld ' naam ' en druk op ENTER.
3. Klik op de knop maken.

### <a name="entity-creation"></a>Entiteit maken

1. Klik in het linkerdeel venster op entiteiten en vervolgens op de knop nieuwe entiteit.
2. Selecteer aangepast getraind voor het entiteits type.
3. Typ ' City ' voor de ' entiteits naam '.
4. Klik op de knop maken.

![](../media/T10_actions.png)

Nu gaan we drie acties maken.

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... veld, typt u "welke plaats?"
3. In de ' verwachte entiteit in antwoord van de gebruiker... ' typt u ' City. '
4. Typ ' City ' in het veld ' kwalificeering '.
5. Klik op de knop maken.

![](../media/T10_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' het weer in $city waarschijnlijk zon. '
3. Klik op de knop maken.

![](../media/T10_action_create_2.png)

### <a name="create-the-third-action"></a>De derde actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' Probeer om het weer te vragen '.
3. Typ ' City ' in het veld ' kwalificeering '.
4. Klik op de knop maken.

![](../media/T10_action_create_3.png)

U hebt nu drie acties.

![](../media/T10_actions.png)

### <a name="train-the-model"></a>Het model trainen

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' wat is het weer? '
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie "welke plaats?"
5. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Denver '
6. Klik op de knop ' Score acties '.
7. Selecteer de reactie: ' het weer in Denver is waarschijnlijk zon. '
8. Klik op de knop Wijzigingen verzenden.

![](../media/T10_training_1.png)

We gaan het model verder trainen door een ander trein dialoogvenster te maken.

### <a name="second-model-train-dialog"></a>Tweede model trein, dialoog venster

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' wat kunt u doen? '
3. Klik op de knop ' Score acties '.
4. Selecteer het antwoord ' Probeer om het weer te vragen '.
5. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' wat is het weer in Seattle? '
6. Klik op Seattle en klik vervolgens in de lijst met entiteiten op plaats.
7. Klik op de knop ' Score acties '.
8. Selecteer de reactie: ' het weer in Seattle is waarschijnlijk zon. '
9. Klik op de knop Wijzigingen verzenden.

![](../media/T10_training_2.png)

### <a name="third-model-train-dialog-using-alternative-input"></a>Dialoog venster voor het trainen van het derde model met alternatieve invoer

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Help '
3. Klik op de knop ' Score acties '.
    - Het model is niet zeker van de beste optie. Daarom kiest u standaard het hoogste percentiel.
4. Klik op de knop leer annuleren en vervolgens op bevestigen.

![](../media/T10_training_3.png)

U kunt het systeem beter afstemmen met alternatieve invoer. U kunt alternatieve invoer toevoegen tijdens het onderwijs of later.

1. Klik in het linkerpaneel op ' dialoog vensters trainen ' en selecteer vervolgens ' wat kunt u doen? '. in de lijst met dialoog vensters voor de trein.
1. Klik op het ' wat kunt u doen? ' utterance in het chat venster.
1. In de "alternatieve invoer toevoegen..." typt u "Help" en druk op ENTER.
1. Klik op de knop Wijzigingen opslaan.

![](../media/T10_training_4.png)

Laten we nog een alternatieve invoer toevoegen om Houston te verwerken.

1. Klik op het ' wat is het weer in Seattle? ' utterance in het chat venster.
1. In de "alternatieve invoer toevoegen..." typt u "forecast for Houston" en druk op ENTER.
   - Het fout bericht markeert de alternatieve invoer voor het feit moet semantisch gelijkwaardig zijn en dezelfde entiteiten als de oorspronkelijke utterance bevatten. niet alleen dezelfde waarden van entiteiten. De aanwezigheid van dezelfde entiteiten is vereist.
1. Klik op ' Houston ' en selecteer ' City ' in de lijst met entiteiten.
1. In de "alternatieve invoer toevoegen..." typt u "prognose voor Seattle" en druk op ENTER.
1. Klik op Seattle en selecteer plaats in de lijst met entiteiten.
1. Klik op de knop Wijzigingen opslaan.
1. Klik op de knop bewerken opslaan.

![](../media/T10_training_5.png)

### <a name="testing-the-model"></a>Het model testen

1. Klik in het linkerdeel venster op dialoog vensters logboeken en vervolgens op het dialoog venster nieuw logboek.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Help mij '
3. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' prognose voor Denver '

![](../media/T10_logdialog.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Dialoog vensters logboek](./11-log-dialogs.md)
