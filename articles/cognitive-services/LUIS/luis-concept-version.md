---
title: Versie beheer-LUIS
titleSuffix: Azure Cognitive Services
description: Versies, in LUIS, zijn vergelijkbaar met versies in traditioneel Program meren. Elke versie is een moment opname in de tijd van de app. Maak een nieuwe versie voordat u wijzigingen aanbrengt in de app. Het is eenvoudiger om terug te gaan naar de exacte app en vervolgens te proberen om de intentie van de app ongedaan te maken en te uitingen naar een eerdere status.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 2e13efa70d0344defeb306a92ac405439635e929
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619696"
---
# <a name="understand-how-and-when-to-use-a-luis-version"></a>Meer informatie over hoe en wanneer u een LUIS-versie gebruikt

Versies, in LUIS, zijn vergelijkbaar met versies in traditioneel Program meren. Elke versie is een moment opname in de tijd van de app. Maak een nieuwe versie voordat u wijzigingen aanbrengt in de app. Het is eenvoudiger om terug te gaan naar de exacte versie en vervolgens om te proberen de intenties en uitingen te verwijderen uit een eerdere status.

Maak verschillende modellen van dezelfde app met [versies](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Versie-ID
De versie-ID bestaat uit tekens, cijfers of '. ' en mag niet langer zijn dan 10 tekens.

## <a name="initial-version"></a>Eerste versie
De eerste versie (0,1) is de standaard versie die actief is. 

## <a name="active-version"></a>Actieve versie
Als u [een versie](luis-how-to-manage-versions.md#set-active-version) als actief wilt instellen, wordt deze momenteel bewerkt en getest op de [Luis](luis-reference-regions.md) -website. Een versie als actief instellen om toegang te krijgen tot de gegevens, updates te maken en deze te testen en te publiceren.

De naam van de momenteel actieve versie wordt weer gegeven in het bovenste deel venster links na de naam van de app. 

[![Actieve versie wijzigen](./media/luis-concept-version/version-in-nav-bar-inline.png)](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Versies en publicatie sleuven
U publiceert naar het werk gebied en de product sleuven. Elke sleuf kan een andere versie of dezelfde versie hebben. Dit is handig voor het controleren van wijzigingen tussen model versies via het eind punt, dat beschikbaar is voor bots of andere toepassingen voor LUIS-aanroepen. 

## <a name="clone-a-version"></a>Een versie klonen
Een versie klonen om een kopie van een bestaande versie te maken en deze op te slaan als een nieuwe versie. Een versie klonen voor het gebruik van dezelfde inhoud van de bestaande versie als uitgangs punt voor de nieuwe versie. Wanneer u een versie kloont, wordt de nieuwe versie de **actieve** versie. 

## <a name="import-and-export-a-version"></a>Een versie importeren en exporteren
U kunt een versie importeren op het niveau van de app. Deze versie wordt de actieve versie en gebruikt de versie-ID in de eigenschap ' versionId ' van het app-bestand. U kunt ook op versie niveau importeren in een bestaande app. De nieuwe versie wordt de actieve versie. 

U kunt een versie exporteren op het niveau van de app of u kunt een versie exporteren op versie niveau. Het enige verschil is dat de geëxporteerde versie op app-niveau de momenteel actieve versie is op versie niveau, u kunt een wille keurige versie kiezen die u wilt exporteren op de pagina **[instellingen](luis-how-to-manage-versions.md)** . 

Het geëxporteerde bestand bevat geen door de computer geleerde informatie omdat de app opnieuw wordt getraind nadat deze is geïmporteerd. Het geëxporteerde bestand bevat geen mede werkers--u moet deze terug toevoegen zodra de versie in de nieuwe app is geïmporteerd.

## <a name="export-each-version-as-app-backup"></a>Elke versie exporteren als app-back-up
Als u een back-up van uw LUIS-app wilt maken, exporteert u elke versie op de pagina **[instellingen](luis-how-to-manage-versions.md)** .

## <a name="delete-a-version"></a>Een versie verwijderen
U kunt alle versies, met uitzonde ring van de actieve versie, verwijderen uit de lijst met versies op de pagina instellingen. 

## <a name="version-availability-at-the-endpoint"></a>Beschik baarheid van versie op het eind punt
Getrainde versies zijn niet automatisch beschikbaar op het [eind punt](luis-glossary.md#endpoint)van uw app. U moet een versie [publiceren](luis-how-to-publish-app.md) of opnieuw publiceren zodat deze beschikbaar is op het eind punt van de app. U kunt publiceren naar **fase ring** en **productie**, zodat u Maxi maal twee versies van de app beschikbaar hebt op het eind punt. Als u meer versies van de app op een eind punt beschikbaar wilt stellen, moet u de versie exporteren en opnieuw importeren in een nieuwe app. De nieuwe app heeft een andere app-ID.

## <a name="collaborators"></a>Samenwerkers
De eigenaar en alle [](luis-how-to-collaborate.md) deel nemers hebben volledige toegang tot alle versies van de app.

## <a name="next-steps"></a>Volgende stappen

Zie [versie beheer](luis-how-to-manage-versions.md) toevoegen op de pagina app-instellingen. 

Meer informatie over het [](luis-concept-intent.md) ontwerpen van intenties in het model.
