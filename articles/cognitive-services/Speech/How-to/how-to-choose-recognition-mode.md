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
ms.openlocfilehash: ed270ed19959240bc1b90ba6171792cf4369e273
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221588"
---
# <a name="bing-speech-recognition-modes"></a>Bing Speech-opname-modi

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

De Bing Speech to Text-API's ondersteunen meerdere modi van spraakherkenning. Kies de modus waarin de resultaten van de beste erkenning voor uw toepassing.

| Modus | Beschrijving |
|---|---|
| *interactief* | 'Command and control' erkenning van toepassingsscenario's interactieve gebruiker. Gebruikers spreken woordgroepen bedoeld als opdrachten naar een toepassing. |
| *Dicteren* | Continue erkenning voor dicteren scenario's. Gebruikers spreken langer zinnen die worden weergegeven als tekst. Gebruikers vast een meer formele spreekstijl. |
| *Gesprek* | Continue erkenning van gesprekken tussen mensen te transcriberen. Gebruikers een minder formele spreekstijl vast en kunnen schakelen tussen langer zinnen en kortere zinnen.

> [!NOTE]
> Deze modi van toepassing zijn wanneer u de [REST-API's](../GetStarted/GetStartedREST.md). De [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven van opname-modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

Zie voor meer informatie de [erkenning modi](../concepts.md#recognition-modes) pagina.
