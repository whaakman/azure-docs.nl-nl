---
title: Anker relaties en manier-zoeken in Azure ruimtelijke ankers | Microsoft Docs
description: Beschrijf het conceptuele model achter anker relaties. Het proces van het verbinden van ankers binnen een spatie en het proces van het gebruik van de API in de buurt om te voldoen aan een manier zoeken naar scenario wordt beschreven. Na de uitleg over het conceptuele model, wijst u ontwikkelaars onze voorbeeld-apps die in de buurt doen, zodat ze aan de slag kunnen in dit scenario implementeren in hun eigen apps.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 56f59a76ac3d11677d5b1f76904cf74e933fa7f6
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753084"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Anker relaties en manier-zoeken in Azure ruimtelijke ankers

Anker relaties kunnen u verbonden ankers maken in een spatie en vervolgens vraagt u vragen over deze, zoals:

* Zijn er ankers in de buurt?
* Hoe ver weg zijn ze?

## <a name="examples"></a>Voorbeelden

Enkele voorbeelden van use-cases die u met verbonden ankers inschakelen kunt zijn onder andere:

1. Een werknemer moet een procedure die betrekking heeft op verschillende locaties in een industriële factory bezoeken uit te voeren. De factory is geplaatst ruimtelijke ankers op elke site in de procedure die betrokken zijn. Een HoloLens of mobiele app helpt u bij de werkrol van de ene locatie naar de volgende handleiding. Het zou eerst vragen voor de ruimtelijke ankers die in de buurt en vervolgens de worker-handleiding voor de volgende locatie. Visuele indicatoren over de algemene richting en de afstand wordt weergegeven met de volgende locatie om de taak te voltooien.

2. Een museum maakt ruimtelijke ankers op openbare weergegeven die samen een specifieke rondleiding langs de museum zoals 'Een rondleiding één uur durende van essentiële openbare geeft' maken. Wanneer bezoekers op een openbare weergave zijn, kunnen ze openen van het museum-app voor mixed reality op hun mobiele apparaat. Ze zou vervolgens wijst u hun telefoon rond de ruimte en via een camera-feed en zien van de algemene richting en afstand tot de andere openbare wordt weergegeven op de rondleiding. Als de gebruiker begint te lopen voor een van de openbare wordt weergegeven, wordt de app geleidelijk werkt de algemene richting en de afstand om er voor gebruikers te helpen.

## <a name="way-finding"></a>Manier zoeken

Stel dat de app wordt gebruikt "van regel van zicht" richting en de afstand tussen ankers richtlijnen hints geven aan gebruikers. We noemen dit algemene scenario manier zoeken. Het is belangrijk te weten manier zoeken wijkt af van de navigatie inschakelen door inschakelen. In de navigatie inschakelen door inschakelen, worden de gebruikers begeleide rond muren, via deuren en tussen verdiepingen. Met zoeken naar manier, de gebruiker ontvangt hints geeft over de algemene richting van de bestemming. Maar Deductie of kennis van de ruimte van de gebruiker ook helpt om te navigeren door de structuur naar de bestemming.

Het bouwen van een manier vinden ervaring omvat het voorbereiden van een ruimte voor de ervaring en een app ontwikkelt die eindgebruikers interactie met. De algemene stappen voor het opnemen:

1. Het plannen van de ruimte: De locaties in de ruimte die deel uitmaken van de ervaring van de manier te bepalen. In de eerdere voorbeelden, kan deze activiteit worden uitgevoerd door de supervisor factory of museum rondleiding coordinator.
2. Ankers verbinden: Iemand de gekozen locaties bezoeken en maakt u er ruimtelijke ankers. Deze taak kan worden gedaan met een beheermodus van de eindgebruiker-app of een andere app volledig. Door dit proces is elk anker verbonden of met betrekking tot de andere. Deze relaties worden bijgehouden in de service.
3. Starten van ervaring voor de eindgebruiker: De eerste stap voor eindgebruikers is het lokaliseren van een van de ankers die gebruikmaken van de app, die kan zich in een van de gekozen locaties. Bepalen van de locaties waar eindgebruikers de ervaring kunt invoeren, maakt deel uit van het ontwerpen van de algehele ervaring.
4. Zoeken in de buurt ankers: Zodra de gebruiker zich één anker, kan de app in de buurt ankers aanvragen. Deze procedure geeft als resultaat een houding tussen het apparaat en deze ankers.
5. Leidt de gebruiker: De app kan profiteren van de houding aan elk van deze ankers om hulp hints geeft over de algemene richting en afstand weer te geven. Bijvoorbeeld, kan er een pictogram en pijl op een camera-feed in een mobiele app voor elk mogelijk doel zoals in de onderstaande afbeelding.
6. Verfijnen van de richtlijnen: Als de gebruiker leidt, kan de app een nieuwe houding tussen het apparaat en het doel anker periodiek berekenen. De app blijft gewoon de hints richtlijnen waarmee de gebruiker die binnenkomen op de bestemming te verfijnen.

![Positie van de vergadering](./media/meeting-spot.png)

## <a name="connecting-anchors"></a>Ankers verbinding te maken

Voor het bouwen van een manier zoeken-ervaring, moet u verbonden ankers in de gekozen locaties plaatsen. Hieronder we gaan ervan uit dat deze taak wordt uitgevoerd door een beheerder van de app.

### <a name="connecting-anchors-in-a-single-session"></a>Ankers op tijdens één sessie verbinding maken

De stappen die betrokken zijn bij het maken van een verbinding ankers zijn:

1. De beheerder wordt voor de eerste locatie en maakt u een anker met behulp van een CloudSpatialAnchorSession.
2. De beheerder wordt voor de tweede locatie terwijl de onderliggende MR/AR-platform voor het bijhouden van de gebruiker blijft.
3. De beheerder maakt anker B met de dezelfde CloudSpatialAnchorSession. Ankers A en B nu zijn verbonden en deze relatie wordt onderhouden door de service Azure ruimtelijke ankers.
4. Doorgaan met de procedure voor alle ankers waarmee u verbinding wilt maken.

### <a name="multiple-sessions"></a>Meerdere sessies

U kunt ook ruimtelijke ankers verbinden via meerdere sessies. Deze methode kunt u maken en bepaalde ankers in één keer verbinding maakt en later maken en verbinding maken met meer ankers. Verbinding maken ankers met meerdere sessies:

1. De app maakt sommige ankers in één CloudSpatialAnchorSession.
2. Later, bijvoorbeeld op een andere dag, de app wordt gezocht naar een van deze ankers met een nieuwe CloudSpatialAnchorSession (bijvoorbeeld anker A).
3. De gebruiker wordt voor een nieuwe locatie, terwijl de onderliggende MR/AR-platform voor het bijhouden van de gebruiker blijft.
4. De gebruiker maakt met behulp van dezelfde CloudSpatialAnchorSession, anker C. ankers A, B en C nu zijn verbonden en deze relatie wordt onderhouden door ruimtelijke ankers van Azure.
5. U kunt deze procedure voor meer ankers en meer sessies blijven na verloop van tijd.

### <a name="verifying-anchor-connections"></a>Anker verbindingen controleren

De app kunt controleren of er twee ankers door uitgifte van een query voor in de buurt ankers zijn verbonden. Wanneer het resultaat van de query het anker gewenste doel bevat, heeft de app bevestiging dat de ankers zijn verbonden. Als deze niet zijn verbonden, kan de app opnieuw de verbindingsprocedure opnieuw. Hier volgen enkele redenen waarom ankers mislukken om verbinding te maken:

1. De onderliggende MR/AR tracker bijhouden tijdens het proces van het verbinden van ankers verloren gegaan.
2. Er is een netwerkfout opgetreden tijdens de communicatie met de service Azure ruimtelijke ankers en de anker-verbinding kan niet worden vastgehouden.

### <a name="sample-code"></a>Voorbeeldcode

Hier ziet u de volgende voorbeeldcode ziet u hoe u verbinding maken met ankers en doen in de buurt van query's. Raadpleeg de voorbeeld-apps van Azure ruimtelijke ankers op GitHub.
