---
title: Het gebruiken van kaarten met een toepassing conversatie cursist, deel 1 – Microsoft cognitieve Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruiken van kaarten met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e90ccd42b21eea6139c402937be7e20513d73c84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345274"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Het gebruik van kaarten (deel 1 van 2)

Deze zelfstudie laat zien hoe toevoegen en een eenvoudige kaart gebruiken in uw bot.

Houd er rekening mee dat conversatie cursist verwacht uw kaart definitiebestanden zich bevinden in een map 'kaarten' die is aanwezig in de map waar de bot wordt gestart.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie selecteren in de conversatie. 

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op de zelfstudie-13-kaarten-1. 

### <a name="the-card"></a>De kaart

De definitie van de kaart is op de volgende locatie: C:\<installedpath\>\src\cards\prompt.json.

Het systeem wordt verwacht dat de definities van de kaart niet vinden in deze map kaarten.

![](../media/tutorial13_prompt.PNG)

- Noteer de TextBlock en de vraag-sjabloon.
- Er zijn twee knoppen en de tekst die wordt ingediend voor elk verzenden.

### <a name="actions"></a>Acties

We hebt drie acties gemaakt. Zoals u hieronder ziet, is de eerste actie een kaart.

![](../media/tutorial13_actions.PNG)

Laten we zien hoe het kaarttype actie is gemaakt:

![](../media/tutorial13_cardaction.PNG)

Noteer de invoer van de vraag en knoppen 1 en 2. De zijn sjabloon-verwijzingen in de kaart waarin u de vraag en de bijbehorende antwoorden invoeren. U kunt ook verwijzen naar en entiteiten of een combinatie van tekst en entiteiten.

Het oogje ziet u hoe de kaart eruit ziet.

### <a name="train-dialog"></a>Dialoogvenster Train

We gaan een dialoogvenster onderwijs doorlopen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'Hallo'.
2. Klik op de actie Score.
3. Klik op 'Vragen gaat u naar links of rechts'.
    - Te klikken op 'links' of 'rechts' is gelijk aan de gebruiker te typen in 'links' of 'rechts, respectievelijk. 
4. Klik op Score acties.
4. Klik op om Selecteer links. Dit is een niet-wait-bewerking.
6. Klik op 'Vragen gaat u naar links of rechts'.
4. Klik op 'rechts'.
5. Klik op Score acties.
3. Schakel 'Rechts'
6. Klik op 'Vragen gaat u naar links of rechts'.
4. Klik op gereed testen.

Nu hebt u gezien hoe kaarten werken. Ze zijn gedefinieerd in de map kaarten als json-sjablonen. De sjablonen wordt surface in de gebruikersinterface die u kunt vullen met een tekenreeks of een entiteit of een combinatie van beide.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Deel 2 van kaarten](./14-cards-2.md)
