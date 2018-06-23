---
title: Herkenningsmodus kiezen | Microsoft Docs
description: Klik hier voor meer informatie over het kiezen van de beste opname-modus.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 4f02b683dde16b537ae5554e6435c068f0fcb808
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344671"
---
# <a name="speech-recognition-modes"></a>Spraak erkenning modi

Microsoft *spraak naar tekst* API's ondersteunen meerdere modi van spraakherkenning. Kies in de modus die het beste herkenningsresultaten voor uw toepassing produceert.

| Modus | Beschrijving |
|---|---|
| *interactieve* | 'Opdracht en controle' herkenning van interactieve gebruiker toepassingsscenario's. Gebruikers uitspreken woordgroepen bedoeld als opdrachten naar een toepassing. |
| *dicteren* | Continue erkenning voor dicteren scenario's. Gebruikers uitspreken langer zinnen die worden weergegeven als tekst. Gebruikers vast een meer formele spreken stijl. |
| *gesprek* | Continue recognition voor uitwisselingen tussen mensen ze. Gebruikers een minder formele spreken stijl vast en kunnen schakelen tussen langer zinnen en kortere zinnen.

> [!NOTE]
> Deze modi zijn van toepassing wanneer u de [REST-API's](../GetStarted/GetStartedREST.md). De [clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verschillende parameters gebruiken om op te geven voor de modus. Zie voor meer informatie de clientbibliotheek van uw keuze.

Zie voor meer informatie de [erkenning modi](../concepts.md#recognition-modes) pagina.
