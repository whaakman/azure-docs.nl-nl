---
title: Anker relaties en manier-zoeken in Azure ruimtelijke ankers | Microsoft Docs
description: Meer informatie over het conceptuele model achter anker relaties. Meer informatie over verbinden ankers binnen een spatie en voor het gebruik van de API in de buurt om te voldoen aan een manier zoeken naar scenario.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 619cd051eccce3434469ae909f69496a254d0d9a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57863335"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Anker relaties en manier-zoeken in Azure ruimtelijke ankers

U kunt met behulp van anker relaties, verbonden ankers maken in een spatie en vervolgens vragen stellen zoals deze:

* Zijn er ankers in de buurt?
* Hoe ver weg zijn ze?

## <a name="examples"></a>Voorbeelden

U kunt verbonden ankers gebruiken in gevallen zoals deze:

* Een werknemer moet een taak uitvoeren die betrekking heeft op verschillende locaties in een industriële factory bezoeken. De factory heeft ruimtelijke ankers op elke locatie. Een HoloLens of mobiele app helpt u bij de werkrol van de ene locatie naar de volgende handleiding. De app eerst vraagt om de in de buurt ruimtelijke ankers en helpt de werkrol met de volgende locatie. De app toont visueel de algemene richting en de afstand naar de volgende locatie.

* Een museum maakt ruimtelijke ankers op openbare weergegeven. Samen vormen deze ankers een essentiële openbare geeft van het museum rondleiding door één uur. Bij een openbare weergeven kunnen bezoekers van het museum-app voor mixed reality op hun mobiele apparaat openen. Vervolgens wijst ze hun telefoon camera rond de ruimte voor de algemene richting en de afstand tot de andere openbare wordt weergegeven op de rondleiding te volgen. Als een gebruiker naar een openbare weer te geven leidt, wordt de app werkt de algemene richting en de afstand om te helpen de gebruiker.

## <a name="set-up-way-finding"></a>Instellen van manier zoeken

Het met behulp van een app die gebruikmaakt van de regel van zicht richting en de afstand tussen ankers begeleiding te leveren *manier zoeken*. Manier zoeken wijkt af van de navigatie inschakelen door inschakelen. In de navigatie inschakelen door inschakelen, worden gebruikers begeleide rond muren, via deuren en tussen verdiepingen. De gebruiker krijgt met zoeken naar manier, tips over het algemeen richting van de bestemming. Maar Deductie of kennis van de ruimte helpt ook de gebruiker navigeert u door de structuur naar de bestemming.

Voor het bouwen van een manier zoeken-ervaring, eerst een spatie voorbereiden voor de ervaring en ontwikkelen van een app die gebruikers gaat communiceren. Dit zijn de algemene stappen:

1. **De ruimte van plan bent**: Bepalen welke locaties binnen de ruimte die deel uitmaken van de ervaring manier zoeken. In ons scenario's besluiten de supervisor factory of de rondleiding museum coördinator welke locaties om op te nemen in de ervaring van de manier waarop zoeken.
2. **Verbinding maken met ankers**: Ga naar de gekozen locaties voor het maken van ruimtelijke ankers. U kunt dit doen in een modus door Administrator van de eindgebruiker-app of in een andere app volledig. U kunt verbinding maken of elk anker betrekking op de andere. De service houdt deze relaties.
3. **Starten van ervaring voor de eindgebruiker**: Gebruikers de app uitvoeren naar een anker, die zich op een van de gekozen locaties kan vinden. Uw algehele ontwerp bepaalt de locaties waar gebruikers de ervaring kunnen invoeren.
4. **Zoeken in de buurt ankers**: Nadat de gebruiker een anker wordt gevonden, kan de app in de buurt ankers aanvragen. Deze procedure geeft als resultaat een houding tussen het apparaat en deze ankers.
5. **De gebruiker**: De app kan de houding aan elk van deze ankers geven informatie over het algemeen richting van de gebruiker en de afstand gebruiken. De camera van het kanaal in de app kan bijvoorbeeld worden weergegeven een pictogram en een pijl voor elk mogelijk doel, zoals in de volgende afbeelding wordt weergegeven.
6. **De richtlijnen verfijnen**: Als de gebruiker leidt, kan de app een nieuwe houding tussen het apparaat en het doel anker periodiek berekenen. De app blijft gewoon de hints richtlijnen waarmee de gebruiker die binnenkomen op de bestemming te verfijnen.

    ![Een voorbeeld van hoe een app manier vinden hulp kunt weergeven](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Verbinding maken met ankers

Voor het bouwen van een manier zoeken-ervaring, moet u eerst de ankers in de gekozen locaties plaatsen. In deze sectie zult gaan we ervan uit dat de beheerder van de app al dit werk is voltooid.

### <a name="connect-anchors-in-a-single-session"></a>Verbinding maken met ankers tijdens één sessie

Verbinding maken ankers:

1. Op de eerste locatie lopen en anker een maken met behulp van een CloudSpatialAnchorSession.
2. Stappen voor op de tweede locatie. Het verkeer, het onderliggende MR/AR-platform worden bijgehouden.
3. Anker B maken met behulp van de dezelfde CloudSpatialAnchorSession. Ankers A en B zijn nu verbonden. De ruimtelijke ankers-service houdt deze relatie.
4. De procedure voor de resterende ankers voortzetten.

### <a name="connect-anchors-in-multiple-sessions"></a>Verbinding maken met ankers in meerdere sessies

U kunt ruimtelijke ankers verbinden via meerdere sessies. Met deze methode, kunt u maken en sommige ankers in één keer verbinding maken en vervolgens later maken en verbinding maken met meer ankers. 

Verbinding maken met ankers via meerdere sessies:

1. De app maakt sommige ankers in één CloudSpatialAnchorSession. 
2. Een ander tijdstip, de app wordt gezocht naar een van deze ankers (bijvoorbeeld anker A) met behulp van een nieuwe CloudSpatialAnchorSession.
3. Stappen voor naar een nieuwe locatie. Het onderliggende platform voor mixed-reality of uitgebreide realiteit houdt bij of het verkeer.
4. Anker C maken met behulp van de dezelfde CloudSpatialAnchorSession. Ankers A, B en C zijn nu verbonden. De ruimtelijke ankers-service houdt deze relatie.

U kunt deze procedure voor meer ankers en meer sessies blijven na verloop van tijd.

### <a name="verify-anchor-connections"></a>Anker verbindingen controleren

De app kunt controleren of er twee ankers door uitgifte van een query voor in de buurt ankers zijn verbonden. Wanneer het resultaat van de query bevat het anker doel, wordt gecontroleerd of de anker-verbinding. Als de ankers niet zijn verbonden, wordt de app kunt proberen om ze opnieuw verbinding te maken. 

Hier volgen enkele redenen waarom ankers mislukken om verbinding te maken:

* Het onderliggende mixed-reality of uitgebreide realiteit platform bijhouden tijdens het proces van het verbinden van ankers verloren gegaan.
* Vanwege een netwerkfout is opgetreden tijdens de communicatie met de service ruimtelijke ankers of kan niet de anker-verbinding worden vastgehouden.

### <a name="find-sample-code"></a>Voorbeeld van code zoeken

Voorbeeld van code die laat zien hoe u verbinding maken met ankers en in de buurt van query's, Zie [ruimtelijke ankers voorbeeld-apps](https://github.com/Azure/azure-spatial-anchors-samples).
