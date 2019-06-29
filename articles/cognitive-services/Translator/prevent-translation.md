---
title: Voorkomen dat inhoud vertaling - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Voorkomen dat de vertaling van inhoud met de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 02c4e90879b81e29f3972d262f36d5c6b6d8e841
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448273"
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
