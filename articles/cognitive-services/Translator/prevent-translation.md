---
title: Vertaling van inhoud voor komen-Translator Text-API
titleSuffix: Azure Cognitive Services
description: Omzetting van inhoud met de Translator Text-API voor komen.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: f9b48b094713f6ee141c5c15d5636ca965ad61b9
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595220"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Omzetting van inhoud met de Translator Text-API voor komen

Met de Translator Text-API kunt u inhoud labelen zodat deze niet wordt vertaald. Stel dat u code, een merk naam of een woord/zin wilt labelen die niet zinvol is wanneer deze wordt gelokaliseerd.

## <a name="methods-for-preventing-translation"></a>Methoden voor het voor komen van vertalingen
1. Escape naar een Twitter- @somethingtopassthrough tag of #somethingtopassthrough. Escape na omzetting ongedaan maken.

2. Voorzie uw inhoud van `notranslate`met.

   Voorbeeld:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Gebruik de [dynamische woorden lijst](dynamic-dictionary.md) om een specifieke vertaling te bepalen.

4. Geef de teken reeks niet door aan de Translator Text-API voor vertaling.

5. Aangepaste vertaler: Gebruik een [woorden lijst in het aangepaste conversie programma](custom-translator/what-is-dictionary.md) om de vertaling van een woord groep met een waarschijnlijkheid van 100% te bepalen.


## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vertaling in de API-aanroep van Translator vermijden](reference/v3-0-translate.md)
