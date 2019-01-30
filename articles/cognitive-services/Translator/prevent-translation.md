---
title: Voorkomen dat inhoud vertaling - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Voorkomen dat de vertaling van inhoud met de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: 0b227803379ed0a64d17c7a7d19b187250b9f845
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226907"
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
