---
title: 'Over het gebruiken van kaarten met een model Conversatiecursist, deel 1: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Informatie over het gebruiken van kaarten met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 16381a9529f516dcb9ffd39022a658a7dd17ff57
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796823"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Het gebruik van kaarten (deel 1 van 2)

Deze zelfstudie laat zien hoe u toevoegt en een eenvoudige kaart gebruiken in uw bot.

> [!NOTE]
> Conversatiecursist is op dit moment wordt verwacht dat de kaart definitiebestanden zich bevinden in een map met de naam 'kaarten' die aanwezig is in de map waar de Bot wordt gestart.

## <a name="video"></a>Video

[![Zelfstudie kaarten-Preview](https://aka.ms/cl_Tutorial_v3_Cards_Preview)](https://aka.ms/cl_Tutorial_v3_Cards)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie te selecteren in de conversatie. 

### <a name="open-the-demo"></a>Open de demo

In de web-UI, klikt u op "Zelfstudies importeren" en selecteer het model met de naam '-zelfstudie-15-kaarten'.

### <a name="the-card"></a>De kaart

De definitie van de kaart is op de volgende locatie: C:\<installedpath\>\src\cards\prompt.json.

Het systeem wordt verwacht dat de definities van de kaart in deze map 'kaarten' vinden.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> U ziet de hoofdtekst van het typt u 'TextBlock' en de tijdelijke aanduiding voor '{{vraag}}' in het tekstveld.
> Er zijn twee knoppen en de tekst die wordt ingediend voor elk indienen.

### <a name="actions"></a>Acties

We hebben drie acties gemaakt. Zoals u hieronder ziet, is de eerste actie in een kaart.

![](../media/tutorial13_actions.PNG)

We gaan nu kijken hoe het actietype kaart wordt gemaakt op:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> De kaart bevat drie verschillende parameters - vraag invoeren, knoppen 1 en 2. Deze elementen zijn verwijzingen naar de sjabloon in de kaart waar u de vraag en de bijbehorende antwoorden invoeren. U kunt ook verwijzen naar en entiteiten of een combinatie van tekst en entiteiten gebruiken.

Het oogpictogram ziet u hoe de kaart eruit ziet.

### <a name="practicing-creating-card-actions"></a>Het maken van het kaart acties te oefenen

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. Selecteer 'Kaart' voor het "actietype'.
3. Selecteer de optie 'vragen' in de lijst 'Sjabloon'.
4. Typ in het veld "vraag", "Gaat u naar links of rechts"
5. Typ "links" in het veld 'knop1'.
6. Typ "rechts" in het veld 'knop2'.
7. Klik op de knop 'Annuleren'.

### <a name="train-dialog-using-an-adaptive-card"></a>Dialoogvenster met een adaptieve kaart trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo'
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "prompt: vraag: Ga naar links of rechts?"
    - Het oogpictogram kan worden gebruikt om een voorbeeld van de kaart
5. Klik op de knop "Links" in de prompt weergegeven in het deelvenster chat.
6. Klik op de knop 'Acties Score'.
7. Selecteer het antwoord, "Links"
8. Klik op de knop 'Opslaan'.
9. Selecteer het antwoord, "prompt: vraag: Ga naar links of rechts?"
10. Klik op de knop 'Recht op' in de prompt weergegeven in het deelvenster chat.
11. Klik op de knop 'Acties Score'.
12. Selecteer het antwoord, "Rechts"

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hybride Bots](./16-hybrid-bots.md)
