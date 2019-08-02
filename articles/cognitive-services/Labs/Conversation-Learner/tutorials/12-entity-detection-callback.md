---
title: Het gebruik van de call back van entiteits detectie met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van de call back van entiteits detectie met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 3f6094da7b103f1c99e9bb3fa9313354fcb2a8bc
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703987"
---
# <a name="how-to-use-entity-detection-callback"></a>Het gebruik van de functie voor het terugbellen van entiteits detectie

In deze zelf studie ziet u de retour aanroep van entiteits detectie en illustreert u een gemeen schappelijk patroon voor het omzetten van entiteiten.

## <a name="video"></a>Video

[![Preview-zelf studie voor het detecteren van entiteits detectie](https://aka.ms/cl_Tutorial_v3_EntityDetection_Preview)](https://aka.ms/cl_Tutorial_v3_EntityDetection)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet `tutorialEntityDetectionCallback` de bot worden uitgevoerd.

    npm run tutorial-entity-detection

## <a name="details"></a>Details
Met de functie voor het detecteren van de entiteits detectie kunt u het gedrag van entiteits herkenning en entiteits manipulatie via code wijzigen. Deze functionaliteit wordt gedemonstreerd door een typisch query patroon voor het terugbellen van de entiteits detectie door te lopen. Voor beeld: ' de Big Apple ' naar ' New York ' oplossen.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik in de gebruikers interface van de web op ' nieuw model '.
2. Typ ' EntityDetectionCallback ' in het veld ' naam ' en druk op ENTER.
3. Klik op de knop maken.

In dit voor beeld zijn drie entiteiten nodig, dus maken we de drie.

### <a name="create-the-custom-trained-entity"></a>De aangepaste getrainde entiteit maken

1. Klik in het linkerdeel venster op entiteiten en vervolgens op de knop nieuwe entiteit.
2. Selecteer aangepast getraind voor het entiteits type.
3. Typ ' City ' voor de ' entiteits naam '.
4. Klik op de knop maken.

### <a name="create-the-first-programmatic-entity"></a>De eerste programmatische entiteit maken

1. Klik op de knop nieuwe entiteit.
2. Selecteer programmatisch voor het entiteits type.
3. Typ "CityUnknown" voor "entiteits naam".
4. Klik op de knop maken.

### <a name="create-the-first-programmatic-entity"></a>De eerste programmatische entiteit maken

1. Klik op de knop nieuwe entiteit.
2. Selecteer programmatisch voor het entiteits type.
3. Typ "CityResolved" voor "entiteits naam".
4. Klik op de knop maken.

Maak nu drie acties.

### <a name="action-creation"></a>Actie maken

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. In het antwoord van de bot... typt u ' welke stad wilt u? '
3. In de ' verwachte entiteit in antwoord van de gebruiker... ' typt u ' City. '
4. Typ ' City ' in het veld ' entiteiten kwalificeren '.
5. Klik op de knop maken.
6. Klik op de knop nieuwe actie.
7. In het antwoord van de bot... typt u ' welke stad wilt u? '
8. In de ' verwachte entiteit in antwoord van de gebruiker... ' typt u ' Ik weet deze stad niet. '
9. Klik op de knop maken.
10. Klik op de knop nieuwe actie.
11. In het antwoord van de bot... het veld ' $CityResolved is zeer goed. '
12. Klik op de knop maken.

Dit is de Terugbel code:

![](../media/tutorial10_callbackcode.PNG)

### <a name="train-the-model"></a>Het model trainen

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Hallo '
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie "welke plaats wilt u?"
5. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' Big Apple '
6. Klik op de knop ' Score acties '.
    - Als u op de knop klikt, wordt de retour aanroep voor entiteits detectie geactiveerd
    - Met de call back code wordt de waarde van de CityResolved-entiteit correct ingesteld op ' New York '
7. Selecteer het antwoord ' New York is zeer goed. '

Dit patroon is doorgaans van veel bot-scenario's. De uitingen en geëxtraheerde entiteiten van de gebruiker worden verstrekt aan uw bedrijfs logica en die logica transformeert de utterance in canonieke vorm, die vervolgens wordt opgeslagen in een programma-entiteit, voor de volgende keer dat het dialoog venster wordt weer gegeven.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sessie retour aanroepen](./13-session-callbacks.md)
