---
title: Voorkomen dat inhoud vertaling - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Voorkomen dat de vertaling van inhoud met de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 70bc4a50240abd8e4b67ff572f8c472f7519569a
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53556846"
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
