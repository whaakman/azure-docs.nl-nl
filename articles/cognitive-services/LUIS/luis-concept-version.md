---
title: Versiebeheer
titleSuffix: Language Understanding - Azure Cognitive Services
description: Versies in LUIS, zijn vergelijkbaar met versies in traditionele programmeren. Elke versie is een momentopname in de tijd van de app. Voordat u wijzigingen in de app aanbrengt, maakt u een nieuwe versie. Het is eenvoudiger om terug te gaan naar de exacte app, klikt u vervolgens probeert te unpeel en het doel en de uitingen naar een eerdere status van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: diberry
ms.openlocfilehash: e71fc5bb0bd8ce54b471829df20336a9dea0ccda
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217081"
---
# <a name="understand-how-and-when-to-use-a-luis-version"></a>Begrijpen hoe en wanneer een LUIS-versie te gebruiken

Versies in LUIS, zijn vergelijkbaar met versies in traditionele programmeren. Elke versie is een momentopname in de tijd van de app. Voordat u wijzigingen in de app aanbrengt, maakt u een nieuwe versie. Het is eenvoudiger om terug te gaan naar de exacte versie, en vervolgens om te proberen om intents en uitingen naar een eerdere status te verwijderen.

Maken van verschillende modellen van dezelfde app met [versies](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Versie-ID
De versie-ID bestaat uit tekens, cijfers of '.' en mag niet langer zijn dan 10 tekens.

## <a name="initial-version"></a>Oorspronkelijke versie
De oorspronkelijke versie (0,1) is de standaard-active-versie. 

## <a name="active-version"></a>Actieve versie
Naar [instellen van een versie](luis-how-to-manage-versions.md#set-active-version) als de actieve betekent dat momenteel wordt bewerkt en getest in de [LUIS](luis-reference-regions.md) website. Een versie instellen als actief toegang tot de gegevens, moet ook worden bijgewerkt, te testen en publiceren.

De naam van de momenteel actieve versie wordt weergegeven in het deelvenster linkerbovenhoek na de naam van de app. 

[ ![Actieve versie wijzigen](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Versies en publicatie-sleuven
U publiceren naar de fase en product sleuven. Elke site kan een andere versie of dezelfde versie hebben. Dit is handig voor het controleren van wijzigingen tussen modelversies via het eindpunt, is beschikbaar voor bots of andere LUIS aanroepen van toepassingen. 

## <a name="clone-a-version"></a>Een versie klonen
Kloon een versie voor het maken van een kopie van een bestaande versie en sla deze op als een nieuwe versie. Kloon een versie voor het gebruik van dezelfde inhoud van de bestaande versie als een beginpunt voor de nieuwe versie. Wanneer u een versie klonen, de nieuwe versie wordt de **active** versie. 

## <a name="import-and-export-a-version"></a>Importeren en exporteren van een versie
U kunt een versie op het niveau van de app importeren. Deze versie wordt de actieve versie en gebruikt de versie-ID in de eigenschap 'versionId' van het app-bestand. U kunt ook op het versieniveau van de importeren in een bestaande app. De nieuwe versie wordt de actieve versie. 

U kunt een versie op het niveau van de app exporteren of u kunt een versie op het versieniveau van de. Het enige verschil is dat de geëxporteerde versie op app-niveau is de momenteel actieve versie terwijl op het versieniveau van de, kunt u elke versie om te exporteren op de **[instellingen](luis-how-to-manage-versions.md)** pagina. 

Het geëxporteerde bestand bevat geen informatie hebt geleerd van een machine omdat de app is retrained nadat deze zijn geïmporteerd. Het geëxporteerde bestand bevat geen deelnemers, moet u deze weer toe te voegen zodra de versie wordt geïmporteerd in de nieuwe app.

## <a name="export-each-version-as-app-backup"></a>Elke versie als de back-up app exporteren
Als u wilt back-up van uw LUIS-app, exporteert u elke versie op de **[instellingen](luis-how-to-manage-versions.md)** pagina.

## <a name="delete-a-version"></a>Een versie verwijderen
U kunt alle versies, behalve de actieve versie verwijderen uit de lijst met versies op de pagina instellingen. 

## <a name="version-availability-at-the-endpoint"></a>Beschikbaarheid van de versie van het eindpunt
Getrainde versies zijn niet automatisch beschikbaar op uw app [eindpunt](luis-glossary.md#endpoint). U moet [publiceren](luis-how-to-publish-app.md) of een versie op zodat deze beschikbaar zijn op het eindpunt van uw app publiceren. U kunt publiceren naar **fasering** en **productie**, zodat u maximaal twee versies van de app beschikbaar is op het eindpunt. Als u meer versies van de app beschikbaar is op een eindpunt, moet u de versie exporteren en opnieuw importeren in een nieuwe app. De nieuwe app heeft een andere app-ID.

## <a name="collaborators"></a>Samenwerkers
De eigenaar en alle [samenwerkers](luis-how-to-collaborate.md) hebben volledige toegang tot alle versies van de app.

## <a name="next-steps"></a>Volgende stappen

Zie how to add [versiebeheer](luis-how-to-manage-versions.md) op de pagina app-instellingen. 

Meer informatie over het ontwerpen van [intents](luis-concept-intent.md) in het model.
