---
title: Vertakkingen en bewerkingen voor ongedaan maken gebruiken met een Conversation Learner model-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van vertakkingen en het ongedaan maken van bewerkingen met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f4f3024451696dbd0244d9da39cba67b49447af1
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703626"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Vertakkingen en bewerkingen voor ongedaan maken gebruiken
In deze zelf studie gaan we verder met het ongedaan maken en vertakkings bewerkingen.


## <a name="details"></a>Details
### <a name="undo"></a>Ongedaan maken
Hiermee kan de ontwikkelaar de laatste invoer of actie van de gebruiker ongedaan maken. Achter de schermen maakt ' ongedaan maken ' een nieuw dialoog venster en wordt het opnieuw afgespeeld met de vorige stap.  Dit betekent dat de retour aanroep van de entiteit en API-aanroepen in het dialoog venster opnieuw worden aangeroepen.

### <a name="branch"></a>Branche
Hiermee maakt u een nieuw trein dialoog venster dat op dezelfde manier als een bestaand trein venster wordt weer geven. Hiermee bespaart u de moeite om het dialoog venster hand matig opnieuw in te voeren. Achter de schermen wordt met ' Branch ' een nieuw dialoog venster gemaakt en wordt het bestaande Train-dialoog venster met de geselecteerde stap opnieuw afgespeeld.  Dit betekent dat de retour aanroep van de entiteit en API-aanroepen in het dialoog venster opnieuw worden aangeroepen.


## <a name="requirements"></a>Vereisten
Deze zelf studie vereist dat de bot die pizza-orders gebruikt, wordt uitgevoerd:

    npm run demo-pizza

### <a name="open-or-import-the-demo"></a>De demo openen of importeren

Als u de zelf studie voor pizza best Ellen al hebt door lopen, opent u dat model eenvoudigweg vanuit de lijst in de webgebruikersinterface. Als dat niet het geval is, klikt u op zelf studies importeren en selecteert u het model ' demo-PizzaOrder '.

## <a name="undo"></a>Ongedaan maken

Hier volgt een voor beeld van het weer geven `Undo` van de functie in actie:

### <a name="training-dialogs"></a>Trainings dialoogvensters
1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en klik vervolgens op `New Train Dialog` de knop.
2. Typ ' order a Pizza '.
3. Klik op de knop `Score Actions`.
4. Klik om ' wat wilt u op uw pizza? ' selecteren?
5. Typ ' overal '.
6. Klik op de knop `Undo`.
    - De laatste vermelding wordt verwijderd, waardoor het laatste bot-antwoord van ' wat wilt u op uw pizza? ' verlaat.

## <a name="branch"></a>Branche

Voor deze demo wordt een bestaand Train-dialoog venster geopend en wordt er een nieuw dialoog venster voor de trein gemaakt door vertakking te maken.

1. Klik in het linkerdeel venster op ' dialoog vensters trainen '.
2. U ziet het raster, maar er wordt slechts één training weer gegeven die begint met ' nieuwe bestelling '.
3. Klik in het raster op nieuwe order om het dialoog venster bestaande trein te openen.
4. Klik op het laatste nummer in het dialoog venster.
5. Klik op het pictogram vertakking, het wordt in een rood omcirkeld in deze afbeelding:
    - ![](../media/tutorial15_branch.PNG)
    - Het hele trein venster vóór de "nee" wordt gekopieerd naar een nieuw trein venster.
    - Op deze manier kunt u de voor gaande beurten opnieuw invoeren om een nieuwe conversatie ' Branch ' vanaf dit punt te verkennen.
6. Typ Ja, druk op ENTER.
7. Klik op de knop `Score Actions`.
    - Op dit moment wordt door de bot automatisch een antwoord gekozen, maar dit is niet het antwoord, dus we zullen het niet wijzigen.
8. Klik op het laatste bot-antwoord.
    - Hiermee kunnen we een ander antwoord selecteren.
9. Selecteer ' UseLastToppings '.
10. Klik op de knop `Score Actions`.
    - De bot kiest automatisch een antwoord. Het zou zeggen, "u hebt worst, kaas en champignons op uw pizza.". 
    - Deze tijd is het antwoord zodat we het zullen blijven gebruiken.
11. Klik op de knop `Score Actions`.
    - De bot kiest automatisch een reactie, het zou zeggen, "wil je graag iets anders?"
12. Typ Nee.
13. Klik op de knop `Save Branch`.
14. U ziet dat het raster nu twee trainingen heeft die beginnen met ' nieuwe bestelling '.
    - Een van deze is de versie die u hebt gebruikt voor vertakking van.
    - En de andere is de gevertakkinge versie die u zojuist hebt opgeslagen.
    - Klik op elk ervan om deze verwachtingen te controleren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Versie beheer en Tags Toep assen](./18-version-tag.md)
