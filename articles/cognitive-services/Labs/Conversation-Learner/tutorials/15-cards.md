---
title: Kaarten gebruiken met een Conversation Learner model, deel 1-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van kaarten met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 426d7c8de29abeb88833e94962a7291a641702ac
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703746"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kaarten gebruiken (deel 1 van 2)

Deze zelf studie laat zien hoe u een eenvoudige kaart kunt toevoegen en gebruiken in uw bot.

> [!NOTE]
> Conversation Learner verwacht momenteel dat uw kaart definitie bestanden zich bevinden in een map met de naam "kaarten" die aanwezig is in de map waarin de bot wordt gestart.

## <a name="video"></a>Video

[![Preview-zelf studie voor kaarten](https://aka.ms/cl_Tutorial_v3_Cards_Preview)](https://aka.ms/cl_Tutorial_v3_Cards)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie in de conversatie kan selecteren. 

### <a name="open-the-demo"></a>De demo openen

Klik in de webgebruikersinterface op zelf studies importeren en selecteer het model met de naam zelf studie-15 kaarten.

### <a name="the-card"></a>De kaart

De definitie van de kaart bevindt zich op de volgende locatie: C:\<installedpath\>\src\cards\prompt.json.

Het systeem verwacht uw kaart definities in deze map "kaarten" te vinden.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Let op het type ' TextBlock ' en de tijdelijke aanduiding ' {{vraagteken}} ' in het tekst veld.
> Er zijn twee verzend knoppen en de tekst die voor elk bericht wordt verzonden.

### <a name="actions"></a>Acties

We hebben drie acties gemaakt. Zoals hieronder wordt weer gegeven, is de eerste actie een kaart.

![](../media/tutorial13_actions.PNG)

Laten we eens kijken hoe het kaart actie type is gemaakt:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> De kaart bevat drie verschillende para meters: vraag invoer, knop 1 en knop 2. Deze elementen zijn sjabloon verwijzingen in de kaart waar u de vraag en de respectieve antwoorden invoert. U kunt ook verwijzen naar en gebruik van entiteiten of een combi natie van tekst en entiteiten.

Het oogpictogram laat zien hoe de kaart eruit ziet.

### <a name="practicing-creating-card-actions"></a>Oefenen met het maken van kaart acties

1. Klik in het linkerdeel venster op acties en vervolgens op de knop nieuwe actie.
2. Selecteer ' kaart ' voor het ' actie type '.
3. Selecteer ' prompt ' in de lijst ' sjabloon '.
4. Typ in het veld vraag ' Ga naar links of rechts '
5. Typ ' left ' in het veld ' Knop1 '
6. Typ ' right ' in het veld ' knop2 toe '
7. Klik op de knop Annuleren.

### <a name="train-dialog-using-an-adaptive-card"></a>Dialoog venster trainen met een adaptieve kaart

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Hallo '
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie "vraag: vraag: Naar links of rechts? "
    - Het oogpictogram kan worden gebruikt om een voor beeld van de kaart te bekijken
5. Klik in het chat venster op de knop ' links ' in de gerenderde prompt.
6. Klik op de knop ' Score acties '.
7. De reactie ' links ' selecteren
8. Klik op de knop Opslaan.
9. Selecteer de reactie "vraag: vraag: Naar links of rechts? "
10. Klik in het chat venster op de knop rechts in de gerenderde prompt.
11. Klik op de knop ' Score acties '.
12. Selecteer de reactie ' rechts '

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hybride bots](./16-hybrid-bots.md)
