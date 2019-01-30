---
title: Gebruik vertakkingen en ongedaan worden gemaakt met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik vertakkingen en bewerkingen uit met een model Conversatiecursist ongedaan maken.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: cca7b42c83be3ca428509ea48c387a1c35ddb35a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206727"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Het gebruik van vertakkings- en Operations ongedaan maken
In deze zelfstudie gaan we over ongedaan maken en vertakkingen bewerkingen.


## <a name="details"></a>Details
### <a name="undo"></a>Ongedaan maken
Kan de ontwikkelaar "ongedaan maken' de laatste gebruiker invoer of actie keuze. Achter de schermen ' ongedaan maken"daadwerkelijk maakt u een nieuw dialoogvenster en deze opnieuw weergegeven tot de vorige stap.  Dit betekent dat de entiteit detectie callback en de API-aanroepen in het dialoogvenster opnieuw aangeroepen.

### <a name="branch"></a>Branch
Hiermee maakt u een nieuw dialoogvenster train, die op dezelfde manier begint als een bestaande dialoogvenster trainen: Hiermee slaat u de moeite van het handmatig opnieuw in te voeren dialoogvenster inschakelt. Achter de schermen, 'vertakking' maakt u een nieuw dialoogvenster en de bestaande dialoogvenster van de trein tot de geselecteerde stap opnieuw weergegeven.  Dit betekent dat de entiteit detectie callback en de API-aanroepen in het dialoogvenster opnieuw aangeroepen.


## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de Bot waarmee pizza orders wordt uitgevoerd:

    npm run demo-pizza

### <a name="open-or-import-the-demo"></a>Open of importeren van de Demo

Als u al de pizza bestellen zelfstudie gewerkt hebt, klikt u vervolgens opent u eenvoudig dat Model in de lijst in de web-UI. Anders moet u op "Zelfstudies importeren" en selecteer het model met de naam 'Demo-PizzaOrder'.

## <a name="undo"></a>Ongedaan maken

Hier volgt een voorbeeld van hoe u kunt de `Undo` functie in actie:

### <a name="training-dialogs"></a>Dialoogvensters voor training
1. In het linkerdeelvenster klikt u op 'Train-dialoogvensters' en klik vervolgens op de `New Train Dialog` knop.
2. Typ 'Order een pizza'.
3. Klik op de knop `Score Actions`.
4. Klik om te selecteren "Wat wilt u dat op uw pizza?"
5. Typ 'Alles'.
6. Klik op de knop `Undo`.
    - Het laatste item wordt verwijderd, waardoor de laatste Bot-reactie van het "Wat wilt u dat op uw pizza?"

## <a name="branch"></a>Branch

Voor deze demo we een bestaande Train dialoogvenster openen en maken van een nieuw dialoogvenster van de trein uit door vertakking.

1. Klik op 'Train-dialoogvensters' in het linkerdeelvenster.
2. U ziet het raster, ziet u slechts één training die begint met 'nieuwe volgorde'.
3. In het raster, klikt u op 'nieuwe volgorde' om de bestaande Train-dialoogvenster te openen.
4. Klik op de laatste "Nee" in het dialoogvenster.
5. Klik op het pictogram 'Vertakking', dit is met een rode cirkel in deze afbeelding:
    - ![](../media/tutorial15_branch.PNG)
    - Het gehele dialoogvenster Train voorafgaand aan de "Nee" is gekopieerd naar een nieuw dialoogvenster van de trein.
    - Dit bespaart u opnieuw in te voeren de voorgaande verandert in een nieuwe discussie 'vertakking' verkennen vanaf dit punt.
6. Typ 'Ja', bereikt invoeren.
7. Klik op de knop `Score Actions`.
    - De Bot kiest op dit moment automatisch een antwoord, maar we het antwoord niet tevreden we gaan om deze te wijzigen.
8. Klik op de laatste Bot-reactie.
    - Dit selecteert laat het ons een andere reactie.
9. Selecteer 'UseLastToppings'.
10. Klik op de knop `Score Actions`.
    - De Bot kiest opnieuw automatisch een antwoord. Het moet bijvoorbeeld 'U hebt worst, kaas en paddestoelen op uw pizza.'. 
    - Nu we het antwoord zoals, zodat we het blijft.
11. Klik op de knop `Score Actions`.
    - Opnieuw kiest de Bot automatisch een antwoord met deze melding, "Wilt u iets anders?"
12. Typ 'Nee'.
13. Klik op de knop `Save Branch`.
14. U ziet het raster heeft nu twee trainingen die begint met 'nieuwe volgorde'.
    - Een van beide is degene die u gebruikt voor het uitschakelen van de vertakking.
    - En een andere is de vertakkingen versie die u zojuist hebt opgeslagen.
    - Klik op elk van deze om te controleren of deze verwachtingen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Versiebeheer en labels](./18-version-tag.md)
