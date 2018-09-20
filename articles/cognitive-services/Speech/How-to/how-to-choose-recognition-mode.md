---
title: Bing Speech erkenning modus kiezen | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Klik hier voor meer informatie over het kiezen van de beste opname-modus in Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 6ea11054e3062c0a265701f6d7fdd136cb692213
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366400"
---
# <a name="bing-speech-recognition-modes"></a>Bing Speech-opname-modi

De Bing Speech to Text-API's ondersteunen meerdere modi van spraakherkenning. Kies de modus waarin de resultaten van de beste erkenning voor uw toepassing.

| Modus | Beschrijving |
|---|---|
| *Interactieve* | 'Command and control' erkenning van toepassingsscenario's interactieve gebruiker. Gebruikers spreken woordgroepen bedoeld als opdrachten naar een toepassing. |
| *Dicteren* | Continue erkenning voor dicteren scenario's. Gebruikers spreken langer zinnen die worden weergegeven als tekst. Gebruikers vast een meer formele spreekstijl. |
| *Gesprek* | Continue erkenning van gesprekken tussen mensen te transcriberen. Gebruikers een minder formele spreekstijl vast en kunnen schakelen tussen langer zinnen en kortere zinnen.

> [!NOTE]
> Deze modi van toepassing zijn wanneer u de [REST-API's](../GetStarted/GetStartedREST.md). De [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven van opname-modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

Zie voor meer informatie de [erkenning modi](../concepts.md#recognition-modes) pagina.
