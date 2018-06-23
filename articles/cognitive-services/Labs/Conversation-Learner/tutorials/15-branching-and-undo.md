---
title: Het gebruik van de vertakking en ongedaan worden gemaakt met de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de vertakking en ongedaan worden gemaakt met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 724a9e47267e0bd7417130efe54c609ac7a465fb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345291"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Het gebruik van de vertakking en ongedaan maken
In deze zelfstudie gaan we over ongedaan maken en vertakkende bewerkingen.

## <a name="details"></a>Details
- Ongedaan maken: kan de ontwikkelaar de keuze van een gebruiker invoer of actie ongedaan ' maken '. Achter de schermen 'ongedaan maken' daadwerkelijk wordt gemaakt van een dialoogvenster voor nieuwe en opnieuw afgespeeld naar de vorige stap.  Dit betekent dat de entiteit detectie callback en API-aanroepen in het dialoogvenster moet opnieuw worden aangeroepen.

- Vertakking: maakt een nieuwe train dialoogvenster die op dezelfde manier begint als een bestaande dialoogvenster trainen: Hiermee slaat u de moeite van het dialoogvenster handmatig opnieuw in te voeren wordt. Achter de schermen 'vertakking' maakt een nieuwe dialoogvenster en opnieuw wordt afgespeeld het bestaande train dialoogvenster tot de geselecteerde stap.  Dit betekent dat de entiteit detectie callback en API-aanroepen in het dialoogvenster moet opnieuw worden aangeroepen.


## <a name="requirements"></a>Vereisten
Deze zelfstudie is vereist dat de volgorde pizza bot wordt uitgevoerd:

    npm run demo-pizza

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op TutorialDemo Pizza volgorde. 

Zie voor meer informatie over de demo Pizza volgorde, de volgorde Pizza zelfstudie.

## <a name="undo"></a>Ongedaan maken

We ongedaan maken deel uit van het dialoogvenster en maak deze opnieuw in deze stap.

### <a name="training-dialogs"></a>Training dialoogvensters
Een trainingssessie begint. 

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer in volgorde van een pizza.
2. Klik op de actie Score.
3. Klik om te selecteren, wat wilt u dat op uw pizza?'
4. Voer 'paddestoelen en kaas'.
5. Klik op Score acties.
3. Klik op 'er $Toppings op uw pizza'.
6. Selecteer, wilt u iets anders?'
7. Voer 'paddestoelen verwijderen en pepers toevoegen'.
    - Selecteer paddestoelen en de entiteit Toppings un controleren. Maakt een actie die we wordt ongedaan gemaakt.
2. Klik op stap ongedaan maken.
    - Houd er rekening mee dat het laatste item wordt verwijderd en we weer terug bent op 'Wilt u iets anders?'  (onderstaande schermafbeelding)
2. Voer 'paddestoelen verwijderen en pepers toevoegen'.
8. Schakel 'er $Toppings op uw pizza'
    - Zorg ervoor dat beide entiteiten correct zijn geselecteerd.
2. Klik op de actie Score. U kunt nu doorgaan met de gecorrigeerde dialoogvenster.
4. Klik op het onderwijs gereed.

U hebt nu het gebruik van ongedaan maken om te verwijderen van een invoer van de gebruiker en de actie gezien.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Branche

Als voorbeeld gaan we een bestaande train dialoogvenster opent en een andere train dialoogvenster maken door vertakking.

1. Klik op Train-dialoogvensters en vervolgens nieuwe volgorde wijzigen om de bestaande dialoogvenster te openen. 
2. Klik op de laatste 'Nee' in het dialoogvenster (Zie onderstaande schermafbeelding).
3. Klik op de vertakking.
    - Houd er rekening mee dat 'no' wordt verwijderd en het gehele dialoogvenster naar dat punt wordt gekopieerd naar een nieuwe. 
    - Dit bespaart u opnieuw in te voeren de voorgaande verandert in een nieuwe 'vertakking' verkennen vanaf dit punt.
1. Voer 'yes'.
2. Klik op de actie Score.
3. Selecteer 'Er $Toppings op uw pizza'.
6. Selecteer, wilt u iets anders?'
7. Voer 'no'.
4. Klik op het onderwijs gereed.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Versiebeheer en labels](./16-versioning-and-tagging.md)
