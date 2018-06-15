---
title: Bekijk eindpunt utterances active leren gebruiken in Language Understanding (LUIS) - Azure | Microsoft Docs
description: De actieve learning-functie met de naam 'Eindpunt utterances bekijken' gebruiken voor het verbeteren van prestaties voorspellingen sneller.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: b9672e8e63fb601d4411a342b7f3c00e30f9e002
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350085"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Actieve learning aan de hand van eindpunt utterances inschakelen
Actieve learning is een van drie strategieën om nauwkeurigheid en het eenvoudigst te verbeteren. 

## <a name="what-is-active-learning"></a>Wat is active learning
Actieve learning is een proces in twee stappen. Eerst in LUIS utterances die wordt ontvangen op de app-eindpunt dat validatie moet worden geselecteerd. De tweede stap wordt uitgevoerd door de eigenaar van de app of medewerker voor het valideren van de geselecteerde utterances voor [bekijken](label-suggested-utterances.md), waaronder de juiste bedoeling en alle entiteiten in het doel. Bekijk de utterances trainen en publiceer de app opnieuw. 

## <a name="which-utterances-are-on-the-review-list"></a>Welke utterances zijn in de lijst controleren
LUIS wordt utterances toegevoegd aan de lijst controleren wanneer de bovenkant wordt intentie dan een lage score heeft of de eerste twee intents scores te sluiten zijn. 

## <a name="where-are-the-utterances-from"></a>Waar worden de utterances uit
Eindpunt utterances zijn afkomstig uit de eindgebruiker query's in de HTTP-eindpunt van de toepassing. Als uw app niet is gepubliceerd of nog niet treffers ontvangen is, hoeft u niet alle utterances om te controleren. Als geen treffers endpoint voor een specifiek doel of de entiteit worden ontvangen, hoeft u geen utterances om te controleren die ze bevatten. 

## <a name="schedule-review-periodically"></a>Controleer regelmatig plannen
Voorgestelde utterances beoordelen, hoeft niet elke dag worden uitgevoerd, maar moet deel uitmaken van uw regelmatig onderhoud van LUIS. 

## <a name="delete-review-items-programmatically"></a>Objecten bekijken via een programma verwijderen
Als uw app te lang is, kunt u sommige utterances controleren en de rest van de lijst via een programma verwijderen. Dit wordt gedaan door de eerste [ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) de lijst en vervolgens [verwijderen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) de utterances op-ID.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [bekijken](Label-Suggested-Utterances.md) eindpunt utterances
