---
title: Dialoog vensters in een Conversation Learner model vastleggen-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het vastleggen van dialoog vensters in een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 388942e8195048d7528a54e5a290f1724c8e876b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703973"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Dialoog vensters in een Conversation Learner model vastleggen

In deze zelf studie ziet u hoe logboeken worden gebruikt om Conversation Learner modellen beter te trainen van vastgelegde interacties met gebruikers van de echte wereld.

## <a name="video"></a>Video

[![Preview van zelf studie voor logboeken](https://aka.ms/cl_Tutorial_v3_LogDialogs_Preview)](https://aka.ms/cl_Tutorial_v3_LogDialogs)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

en het weer model dat u in de vorige zelf studies hebt gemaakt.

## <a name="details"></a>Details
Dialoog vensters voor logboeken worden vastgelegd in Logboeken van de interactie van uw bot met eind gebruikers. Door deze logboeken te benutten, kunt u entiteits labels en actie selecties oplossen om de prestaties van het model en de algehele systeem prestaties te verbeteren.

## <a name="steps"></a>Stappen

Klik in de webgebruikersinterface op zelf studies importeren en selecteer het model met de naam ' zelf studie-11-LogDialogs '.

Dit model bevat één entiteit met de naam City en acties die zijn ontworpen om te reageren op vragen over de weers omstandigheden in die stad. Er zijn twee trein dialoogvensters gebruikt om het model te trainen, waardoor prestatie verwachtingen enigszins laag zijn. Het model wordt verbeterd met extra training en bloot stelling aan echte gebruikers interacties van de wereld.

### <a name="create-a-new-conversation"></a>Een nieuw gesprek maken

1. Klik in het linkerdeel venster op dialoog vensters logboeken en vervolgens op de knop nieuw logboek.
2. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' Austin weer prognose '
3. Klik op de knop testen is voltooid.
4. Klik in de lijst op het dialoog venster "Austin weer geven".
5. Klik in het chat venster op de utterance ' Austin weer Forecast '.
6. Klik op Austin en klik vervolgens op plaats in de lijst met entiteiten.
7. Klik op de knop Wijzigingen verzenden.
    - Deze wijziging in de entiteits waarde veroorzaakt downstream wijzigingen aan de conversatie, omdat er nieuwe entiteits waarden in het geheugen zijn. Latere acties zijn waarschijnlijk ongeldig geworden met betrekking tot de ' City '-entiteit.
8. Klik op de plaats ' welke stad? ' utterance in het chat venster.
9. Selecteer de reactie: ' het weer in Austin is waarschijnlijk zonnig. '
10. Klik op de knop Opslaan als trein venster.
    - Training wordt onmiddellijk gestart

![](../media/T11_logdialog.png)

Een laatste opmerking. Afhankelijk van de behoeften van uw bedrijf, kunt u de functie voor het registreren van gesp rekken uitschakelen door naar instellingen te gaan en conversaties vastleggen uit te scha kelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Retour aanroep van entiteits detectie](./12-entity-detection-callback.md)
