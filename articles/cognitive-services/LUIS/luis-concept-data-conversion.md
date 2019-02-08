---
title: Gegevensconversie
titleSuffix: Language Understanding - Azure Cognitive Services
description: Informatie over hoe uitingen kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: bdf2d87e558726937fa4221a0d95179216c66051
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859197"
---
# <a name="convert-data-format-of-utterances"></a>De gegevensindeling van uitingen converteren
LUIS maakt gebruik van Cognitive Services Speech service uitingen van gesproken uitingen converteren naar tekst uitingen voordat voorspelling. 

## <a name="speech-to-intent-conversion-concepts"></a>Spraak naar intentie conversie-concepten
Conversie van spraak naar tekst in LUIS kunt u een eindpunt gesproken uitingen verzenden en ontvangen van een reactie van LUIS voorspelling. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met LUIS. Meer informatie over spraak naar intentie met een [zelfstudie](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft niet te maken een **Bing Speech-API** voor deze integratie. Een **Language Understanding** sleutel hebt gemaakt in Azure portal werkt voor deze integratie. Gebruik niet de LUIS starter-sleutel.

### <a name="pricing-tier"></a>Prijscategorie
Deze integratie maakt gebruik van een andere [prijzen](luis-boundaries.md#key-limits) model dan de gebruikelijke Language Understanding Prijscategorieën. 

### <a name="quota-usage"></a>Gebruik van netwerkquota 's
Zie [sleutel limieten](luis-boundaries.md#key-limits) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gebruik spraak naar tekst](luis-tutorial-speech-to-intent.md)

