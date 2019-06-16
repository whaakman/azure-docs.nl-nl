---
title: Voorkomen dat inhoud vertaling - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Voorkomen dat de vertaling van inhoud met de Translator Text-API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: v-pawal
ms.openlocfilehash: 32e5ddecc336175443fc5e8743b19b480de71c6b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66515263"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Om te voorkomen dat de vertaling van inhoud met de Translator Text-API

De Translator Text-API kunt u inhoud labelen zodat deze niet is vertaald. Bijvoorbeeld, kunt u voor het taggen van code, een handelsmerk of een woord/bepaalde woordgroep die niet geschikt wanneer gelokaliseerd.

## <a name="methods-for-preventing-translation"></a>Methoden voor het voorkomen van vertaling
1. Als u op een Twitter-tag @somethingtopassthrough of #somethingtopassthrough. Ongedaan maken-escape na de vertaling.

2. Taggen van uw inhoud met `notranslate`.

   Voorbeeld:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Gebruik de [dynamische woordenlijst](dynamic-dictionary.md) te schrijven voor een specifieke vertaling.

4. De tekenreeks niet worden doorgegeven aan de Translator Text-API voor de vertaling.

5. Aangepaste Translator: Gebruik een [-woordenlijst in aangepaste Translator](custom-translator/what-is-dictionary.md) te schrijven van de vertaling van een zin met de kans op 100%.


## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vertalingen in uw Translator-API-aanroep voorkomen](reference/v3-0-translate.md)
