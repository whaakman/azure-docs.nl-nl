---
title: Bing Speech erkenning modus kiezen | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Klik hier voor meer informatie over het kiezen van de beste opname-modus in Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 99e69691d9904285ecff356523813b426c98fca2
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669717"
---
# <a name="bing-speech-recognition-modes"></a>Bing Speech-opname-modi

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

De Bing Speech to Text-API's ondersteunen meerdere modi van spraakherkenning. Kies de modus waarin de resultaten van de beste erkenning voor uw toepassing.

| Modus | Description |
|---|---|
| *interactief* | 'Command and control' erkenning van toepassingsscenario's interactieve gebruiker. Gebruikers spreken woordgroepen bedoeld als opdrachten naar een toepassing. |
| *Dicteren* | Continue erkenning voor dicteren scenario's. Gebruikers spreken langer zinnen die worden weergegeven als tekst. Gebruikers vast een meer formele spreekstijl. |
| *Gesprek* | Continue erkenning van gesprekken tussen mensen te transcriberen. Gebruikers een minder formele spreekstijl vast en kunnen schakelen tussen langer zinnen en kortere zinnen.

> [!NOTE]
> Deze modi van toepassing zijn wanneer u de [REST-API's](../GetStarted/GetStartedREST.md). De [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven van opname-modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

Zie voor meer informatie de [erkenning modi](../concepts.md#recognition-modes) pagina.
