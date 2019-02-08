---
title: Uw LUIS-app testen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Testen, is het proces van LUIS voorbeeld uitingen die en het ophalen van een reactie van LUIS herkend intenties en entiteiten.
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: bfb6d2461caf7b5c2c03ea99ad3b4c6ffa06fa0a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55870840"
---
# <a name="testing-example-utterances-in-luis"></a>Voorbeeld-uitingen testen in LUIS

Testen, is het proces van LUIS voorbeeld uitingen die en het ophalen van een reactie van LUIS herkend intenties en entiteiten. 

U kunt [testen](luis-interactive-test.md) LUIS interactief, één utterance tegelijkertijd, of geef een [batch](luis-concept-batch-test.md) van uitingen. Met testen, vergelijkt u de huidige [active](luis-concept-version.md#active-version) modellen met het gepubliceerde model. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>

## <a name="what-is-a-score-in-testing"></a>Wat is een score in de testfase?
Zie [voorspelling score](luis-concept-prediction-score.md) concepten voor meer informatie over de scores voorspelling.

## <a name="interactive-testing"></a>Interactief testen
Interactieve testen wordt gedaan via de **Test** deelvenster van de website. U kunt een utterance om te zien hoe intenties en entiteiten worden geïdentificeerd en gewaardeerd invoeren. Als LUIS is niet voorspellen de intenties en entiteiten zoals op een utterance in het deelvenster met testen verwacht, kopieert u deze naar de **bedoeling** pagina als een nieuwe utterance. Vervolgens de onderdelen van deze utterance labelen en LUIS trainen. 

## <a name="batch-testing"></a>Batchgewijs testen
Zie [batch testen](luis-concept-batch-test.md) als u meer dan één utterance tegelijk wilt testen.

## <a name="endpoint-testing"></a>Eindpunt testen
U kunt testen met behulp van de [eindpunt](luis-glossary.md#endpoint) met een maximum van twee versies van uw app. Met uw belangrijkste of live-versie van uw app instellen als de **productie** eindpunt, Voeg een tweede versie op de **staging** eindpunt. Deze aanpak biedt u drie versies van een utterance: het huidige model in het deelvenster van de [LUIS](luis-reference-regions.md) website en de twee versies op twee verschillende eindpunten. 

Alle eindpunt testen telt mee voor uw quotum voor het gebruik. 

## <a name="do-not-log-tests"></a>Meld u tests niet
Als u op basis van een eindpunt testen en niet dat de utterance geregistreerd wilt, moet u gebruiken de `logging=false` configuratie query-tekenreeks.

## <a name="where-to-find-utterances"></a>Waar vind ik uitingen
LUIS slaat alle geregistreerde uitingen in het querylogboek, beschikbaar voor downloaden op de [LUIS](luis-reference-regions.md) website **Apps** lijstpagina, evenals de LUIS [API's ontwerpen](https://aka.ms/luis-authoring-apis). 

Alle uitingen LUIS zeker is worden weergegeven in de **[bekijken eindpunt uitingen](luis-how-to-review-endoint-utt.md)** pagina van de [LUIS](luis-reference-regions.md) website. 

![Eindpuntuitingen controleren](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Houd er rekening mee te trainen
Houd er rekening mee te [trainen](luis-how-to-train.md) LUIS nadat u wijzigingen in het model aanbrengt. Wijzigingen in de LUIS-app zijn niet zichtbaar in de testfase totdat de app wordt getraind. 

## <a name="best-practices"></a>Aanbevolen procedures
Informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [testen](luis-interactive-test.md) uw uitingen.
