---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 08328b3c872ce9e8a25db6075b00e0cee2fdf4fd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55480522"
---
De clienttoepassing moet weten of een uiting niet zinvol of gepast is voor de toepassing. De intentie **Geen** wordt als onderdeel van het maakproces aan elke toepassing toegevoegd om te bepalen of een uiting niet kan worden beantwoord door de clienttoepassing.

Als LUIS de intentie **Geen** retourneert voor een uiting, kan de clienttoepassing vragen of de gebruiker het gesprek wil beëindigen of meer aanwijzingen voor het vervolgen van het gesprek wil geven. 

> [!CAUTION] 
> Laat niet de intentie **Geen** niet leeg. 

1. Selecteer **Intents** in het linkerpaneel.

2. Selecteer de intent **None**. Voeg drie uitingen toe die een gebruiker zou kunnen invoeren, maar die niet relevant zijn voor uw HR-app:

    | Voorbeelden van utterances|
    |--|
    |Blaffende honden zijn irritant|
    |Bestel een pizza voor me|
    |Pinguïns in de oceaan|
