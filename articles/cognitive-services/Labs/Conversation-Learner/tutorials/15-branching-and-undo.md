---
title: Gebruik vertakkingen en ongedaan worden gemaakt met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik vertakkingen en bewerkingen uit met een model Conversatiecursist ongedaan maken.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 05140693026e21a73b756ed0ea7bc9936bef067e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173295"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Het gebruik van vertakkingen en ongedaan maken
In deze zelfstudie gaan we over ongedaan maken en vertakkingen bewerkingen.


## <a name="details"></a>Details
- Ongedaan maken: kan de ontwikkelaar "ongedaan maken" de keuze van een gebruiker invoer of actie. Achter de schermen ' ongedaan maken"daadwerkelijk maakt u een nieuw dialoogvenster en deze opnieuw wordt afgespeeld tot de vorige stap.  Dit betekent dat de entiteit detectie callback en de API-aanroepen in het dialoogvenster opnieuw aangeroepen.

- Vertakking: Hiermee maakt u een dialoogvenster voor nieuwe train die op dezelfde manier begint als een bestaande dialoogvenster trainen: Hiermee slaat u de moeite van het handmatig opnieuw in te voeren dialoogvenster inschakelt. Achter de schermen, 'vertakking' maakt u een nieuw dialoogvenster en opnieuw wordt afgespeeld het bestaande dialoogvenster van de trein tot de geselecteerde stap.  Dit betekent dat de entiteit detectie callback en de API-aanroepen in het dialoogvenster opnieuw aangeroepen.


## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de volgorde pizza bot wordt uitgevoerd:

    npm run demo-pizza

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op TutorialDemo Pizza volgorde. 

Zie voor meer informatie over de demo Pizza volgorde, de volgorde Pizza zelfstudie.

## <a name="undo"></a>Ongedaan maken

We ongedaan maken deel uit van het dialoogvenster en opnieuw maken van deze stap.

### <a name="training-dialogs"></a>Dialoogvensters voor training
Laten we beginnen een trainingssessie. 

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'order een pizza'.
2. Klik op Score actie.
3. Klik om te selecteren 'Wat wilt u dat op uw pizza?'
4. Voer 'paddestoelen en kaas'.
5. Klik op Score acties.
3. Klik om te selecteren, hebt u $Toppings uw pizza'.
6. Selecteer 'Wilt u iets anders?'
7. Voer 'paddestoelen verwijderen en peppers toevoegen'.
    - Selecteer paddestoelen en schakel de Toppings-entiteit. Maakt een actie die we wordt ongedaan gemaakt.
2. Klik op stap ongedaan maken.
    - Het laatste item wordt verwijderd, en we weer terug bent op 'Wilt u iets anders?'  (onderstaande schermafbeelding)
2. Voer 'paddestoelen verwijderen en peppers toevoegen'.
8. Klik om te selecteren, hebt u $Toppings uw pizza'
    - Zorg ervoor dat beide entiteiten correct zijn geselecteerd.
2. Klik op Score actie. U kunt nu doorgaan met de gecorrigeerde dialoogvenster.
4. Klik op het onderwijs gereed.

U hebt nu gezien hoe u kunt gebruiken voor het verwijderen van een invoer van de gebruiker ongedaan maken en de actie.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Branche

Als voorbeeld gaan we een bestaande train dialoogvenster opent en maken van een ander dialoogvenster van de trein door vertakking.

1. Klik op Train-dialoogvensters en vervolgens 'nieuwe order' om de bestaande dialoogvenster te openen. 
2. Klik op de laatste 'Nee' in het dialoogvenster (Zie de onderstaande schermafbeelding).
3. Klik op de vertakking.
    - 'Nee' wordt verwijderd en het gehele dialoogvenster tot dat moment is gekopieerd naar een nieuwe. 
    - Dit bespaart u opnieuw in te voeren de voorgaande verandert in een nieuwe "vertakking' verkennen vanaf dit punt.
1. Voer 'Ja'.
2. Klik op Score actie.
3. Selecteer 'U hebt $Toppings op uw pizza'.
6. Selecteer 'Wilt u iets anders?'
7. Voer 'Nee'.
4. Klik op het onderwijs gereed.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Versiebeheer en labels](./16-versioning-and-tagging.md)
