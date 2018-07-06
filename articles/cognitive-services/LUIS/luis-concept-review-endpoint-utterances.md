---
title: Bekijk eindpunt uitingen actief leren gebruiken in Language Understanding (LUIS) - Azure | Microsoft Docs
description: Gebruik de functie actief leren is met de naam 'Eindpunt uitingen bekijken' voor het verbeteren van prestaties voorspellingen sneller.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: e416eec29ff9f4ac96eabf11c87424abeba0c75b
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855714"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Actief leren aan de hand van de eindpunt-uitingen inschakelen
Actief leren is een van drie strategieën voor het verbeteren van nauwkeurigheid en het gemakkelijkst te implementeren. 

## <a name="what-is-active-learning"></a>Wat is actief leren
Actief leren is een proces in twee stappen. Eerst selecteert LUIS uitingen die wordt ontvangen op van de app-eindpunt waarvoor validatie. De tweede stap wordt uitgevoerd door de eigenaar van de app of medewerker voor het valideren van de geselecteerde uitingen voor [bekijken](label-suggested-utterances.md), met inbegrip van de juiste intentie en alle entiteiten in het doel. Bekijk de uitingen en trainen en publiceer de app opnieuw. 

## <a name="which-utterances-are-on-the-review-list"></a>Welke uitingen zijn in de lijst met controleren
Als de bovenkant activeren kunt u lezen wat een lage score heeft of de bovenste twee intenties scores te sluiten zijn, LUIS uitingen toegevoegd aan de beoordeling-lijst. 

## <a name="single-pool-for-utterances-per-app"></a>Één groep voor uitingen per app
De **bekijken eindpunt uitingen** lijst niet op basis van de versie verandert. Er is één groep uitingen om te controleren, ongeacht welke versie de utterance u actief bewerkt, of welke versie van de app is gepubliceerd op het eindpunt. 

## <a name="where-are-the-utterances-from"></a>Waar zijn de uitingen van
Eindpunt-uitingen zijn afkomstig uit de query's door eindgebruikers op HTTP-eindpunt van de toepassing. Als uw app is niet gepubliceerd of nog niet treffers ontvangen is, hoeft u niet alle uitingen om te controleren. Als er geen eindpunt treffers worden ontvangen voor een bepaald doel of de entiteit, hoeft u niet uitingen om te controleren die ze bevatten. 

## <a name="schedule-review-periodically"></a>Controleer regelmatig plannen
Controleren van de voorgestelde uitingen hoeft niet elke dag worden uitgevoerd, maar moet deel uitmaken van uw regelmatig onderhoud van LUIS. 

## <a name="delete-review-items-programmatically"></a>Objecten bekijken via een programma verwijderen
Als uw app groot is, besluiten kunt u om te controleren van sommige uitingen en de rest van de lijst via een programma verwijderen. Dit wordt gedaan door de eerste [aan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) de lijst en vervolgens [verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) de uitingen van-ID.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [bekijken](Label-Suggested-Utterances.md) uitingen van eindpunt
