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
ms.openlocfilehash: 988a2433f098f41bca4796299825293efd4de44b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171131"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Het gebruik van kaarten (deel 1 van 2)

Deze zelfstudie laat zien hoe u toevoegt en een eenvoudige kaart gebruiken in uw bot.

> [!NOTE]
> Conversatiecursist momenteel wordt verwacht dat de kaart definitiebestanden zich bevinden in een map met de naam 'kaarten' die aanwezig is in de map waar de bot wordt gestart. We doen dit kunnen worden geconfigureerd in de toekomst.

## <a name="video"></a>Video

[![Zelfstudie 13-Preview](http://aka.ms/cl-tutorial-13-preview)](http://aka.ms/blis-tutorial-13)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie te selecteren in de conversatie. 

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op zelfstudie-13-kaarten-1. 

### <a name="the-card"></a>De kaart

De definitie van de kaart is op de volgende locatie: C:\<installedpath\>\src\cards\prompt.json.

Het systeem wordt verwacht dat de definities van de kaart in deze map kaarten vinden.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> U ziet het type instantie `TextBlock` en de `{{question}}` tijdelijke aanduiding in het tekstveld.
> Er zijn twee knoppen en de tekst die wordt ingediend voor elk indienen.

### <a name="actions"></a>Acties

We hebben drie acties gemaakt. Zoals u hieronder ziet, is de eerste actie in een kaart.

![](../media/tutorial13_actions.PNG)

We gaan nu kijken hoe het actietype kaart wordt gemaakt op:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> De invoer van de vraag en de knoppen 1 en 2. Dit zijn verwijzingen naar de sjabloon in de kaart waar u de vraag en de bijbehorende antwoorden invoeren. U kunt ook verwijzen naar en entiteiten of een combinatie van tekst en entiteiten gebruiken.

Het oogpictogram ziet u hoe de kaart eruit ziet.

### <a name="train-dialog"></a>Dialoogvenster van de trein

We nemen een dialoogvenster voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'Hallo'.
2. Klik op Score actie.
3. Klik op 'Vragen Ga naar links of rechts'.
    - Te klikken op 'links' of 'juiste' is gelijk aan de gebruiker te typen in "links" of "rechts" respectievelijk. 
4. Klik op Score acties.
4. Klik op selecteren links. Dit is een niet-wait-actie.
6. Klik op 'Vragen Ga naar links of rechts'.
4. Klik op 'rechts'.
5. Klik op Score acties.
3. Klik om te selecteren 'Chterkant'
6. Klik op 'Vragen Ga naar links of rechts'.
4. Klik op gereed testen.

U hebt nu gezien hoe kaarten werken. Ze zijn gedefinieerd in de map kaarten als json-sjablonen. De sjablonen verschijnt in de gebruikersinterface die u kunt vullen met behulp van een tekenreeks of een entiteit of een combinatie van beide.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Kaarten deel 2](./14-cards-2.md)
