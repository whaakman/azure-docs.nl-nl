---
title: Gegevens conversie-LUIS
titleSuffix: Azure Cognitive Services
description: Informatie over hoe uitingen kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: aeb9094db83b14af988f70485788934a7854200c
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619806"
---
# <a name="convert-data-format-of-utterances"></a>De gegevensindeling van uitingen converteren
LUIS biedt de volgende conversies van een gebruiker utterance vóór voor spelling '

* Spraak naar tekst met behulp van [Cognitive Services speech](../Speech-Service/overview.md) service. 

## <a name="speech-to-text"></a>Spraak-naar-tekst

Spraak naar tekst wordt verschaft als een integratie met LUIS. 

### <a name="intent-conversion-concepts"></a>Concepten van intentie conversie
Conversie van spraak naar tekst in LUIS kunt u een eindpunt gesproken uitingen verzenden en ontvangen van een reactie van LUIS voorspelling. Het proces is een integratie van de [spraak](https://docs.microsoft.com/azure/cognitive-services/Speech) service met LUIS. Meer informatie over spraak naar opzet vindt u in een [zelf studie](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Belangrijke vereisten
U hoeft niet te maken een **Bing Speech-API** voor deze integratie. Een **Language Understanding** sleutel hebt gemaakt in Azure portal werkt voor deze integratie. Gebruik niet de LUIS-start sleutel.

### <a name="pricing-tier"></a>Prijscategorie
Deze integratie maakt gebruik van een ander [prijs](luis-boundaries.md#key-limits) model dan gebruikelijk language Understanding prijs categorieën. 

### <a name="quota-usage"></a>Gebruik van netwerkquota 's
Zie [sleutel limieten](luis-boundaries.md#key-limits) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gegevens extra heren](luis-concept-data-extraction.md)

