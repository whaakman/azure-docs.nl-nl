---
title: Controle-eindpunt uitingen actief leren gebruiken in Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Actief leren is een van drie strategieën voor het verbeteren van nauwkeurigheid en het gemakkelijkst te implementeren. Met actief leren, uw uitingen controle-eindpunt voor de juiste intentie en entiteit. LUIS kiest eindpunt uitingen is het niet zeker weet.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 78cc2a8a2b9295654d0c6264cbf4a4d634b16544
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038167"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Actief leren aan de hand van de eindpunt-uitingen inschakelen
Actief leren is een van drie strategieën voor het verbeteren van nauwkeurigheid en het gemakkelijkst te implementeren. Met actief leren, uw uitingen controle-eindpunt voor de juiste intentie en entiteit. LUIS kiest eindpunt uitingen is het niet zeker weet.

## <a name="what-is-active-learning"></a>Wat is actief leren
Actief leren is een proces in twee stappen. Eerst selecteert LUIS uitingen die wordt ontvangen op van de app-eindpunt waarvoor validatie. De tweede stap wordt uitgevoerd door de eigenaar van de app of medewerker voor het valideren van de geselecteerde uitingen voor [bekijken](luis-how-to-review-endoint-utt.md), met inbegrip van de juiste intentie en alle entiteiten in het doel. Bekijk de uitingen en trainen en publiceer de app opnieuw. 

## <a name="which-utterances-are-on-the-review-list"></a>Welke uitingen zijn in de lijst met controleren
Als de bovenkant activeren kunt u lezen wat een lage score heeft of de bovenste twee intenties scores te sluiten zijn, LUIS uitingen toegevoegd aan de beoordeling-lijst. 

## <a name="single-pool-for-utterances-per-app"></a>Één groep voor uitingen per app
De **bekijken eindpunt uitingen** lijst niet op basis van de versie verandert. Er is één groep uitingen om te beoordelen, ongeacht welke versie van de uiting u actief bewerkt of welke versie van de app wordt gepubliceerd op het eindpunt. 

## <a name="where-are-the-utterances-from"></a>Waar zijn de uitingen van
Eindpunt-uitingen zijn afkomstig uit de query's door eindgebruikers op HTTP-eindpunt van de toepassing. Als uw app is niet gepubliceerd of nog niet treffers ontvangen is, hoeft u niet alle uitingen om te controleren. Als er geen eindpunt treffers worden ontvangen voor een bepaald doel of de entiteit, hoeft u niet uitingen om te controleren die ze bevatten. 

## <a name="schedule-review-periodically"></a>Controleer regelmatig plannen
Controleren van de voorgestelde uitingen hoeft niet elke dag worden uitgevoerd, maar moet deel uitmaken van uw regelmatig onderhoud van LUIS. 

## <a name="delete-review-items-programmatically"></a>Objecten bekijken via een programma verwijderen
Als uw app groot is, besluiten kunt u om te controleren van sommige uitingen en de rest van de lijst via een programma verwijderen. Dit wordt gedaan door de eerste [aan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) de lijst en vervolgens [verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) de uitingen van-ID.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [bekijken](luis-how-to-review-endoint-utt.md) uitingen van eindpunt
