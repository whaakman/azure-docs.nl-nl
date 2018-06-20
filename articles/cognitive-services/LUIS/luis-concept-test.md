---
title: Uw app LUIS - Azure testen | Microsoft Docs
description: Language Understanding (LUIS) continu werken op uw toepassing deze verfijnen en het verbeteren van de kennis van de taal gebruiken.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8c702d2adbadd2736eed05c7580e8aabf69affbf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266325"
---
# <a name="testing-in-luis"></a>Testen in LUIS

Testen, is het proces van voorbeeld utterances bieden aan LUIS en een reactie ontvangen van LUIS herkend intents en entiteiten. 

U kunt [testen](interactive-test.md) LUIS interactief, één utterance op een tijdstip, of geef een [batch](luis-concept-batch-test.md) van utterances. Test, u de huidige vergelijken [active](luis-concept-version.md#active-version) model in het gepubliceerde model. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>
## <a name="what-is-a-score-in-testing"></a>Wat is er een score tests?
Zie [voorspelling score](luis-concept-prediction-score.md) concepten voor meer informatie over de scores voorspelling.

## <a name="interactive-testing"></a>Interactieve testen
Interactieve tests worden uitgevoerd vanuit de **Test** deelvenster van de website. U kunt een utterance om te zien hoe intents en entiteiten worden geïdentificeerd en gewaardeerd invoeren. Als LUIS is niet intents en entiteiten van de voorspellen, zoals u op een utterance in het deelvenster met testen verwacht, kopieert u deze naar de **bedoeling** pagina als een nieuwe utterance. Vervolgens de onderdelen van die utterance labelen en LUIS trainen. 

## <a name="batch-testing"></a>Batch testen
Zie [tests in batch](luis-concept-batch-test.md) als u meer dan één utterance tegelijk wilt testen.

## <a name="endpoint-testing"></a>Eindpunt testen
U kunt testen met behulp van de [eindpunt](luis-glossary.md#endpoint) met een maximum van twee versies van uw app. Met uw belangrijkste of live-versie van uw app instellen als de **productie** eindpunt, Voeg een tweede versie naar de **fasering** eindpunt. Deze aanpak geeft u drie versies van een utterance: het huidige model in het deelvenster van de [LUIS] [ LUIS] website en de twee versies op twee verschillende eindpunten. 

Alle eindpunt testen aantallen naar uw gebruiksquotum. 

## <a name="do-not-log-tests"></a>Meld u niet tests
Als u voor een eindpunt testen en niet dat de utterance geregistreerd wilt, moet u gebruiken de `logging=false` configuratie van de query-tekenreeks.

## <a name="where-to-find-utterances"></a>Waar vind ik utterances
LUIS slaat alle geregistreerde utterances in het querylogboek, beschikbaar voor downloaden op de [LUIS] [ LUIS] website **Apps** pagina weergeven, evenals de LUIS [authoring API's ](https://aka.ms/luis-authoring-apis). 

Alle utterances LUIS niet zeker weet wat is worden vermeld in de **[bekijken eindpunt utterances](label-suggested-utterances.md)** pagina van de [LUIS] [ LUIS] website. 

![Eindpunt utterances controleren](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Houd er rekening mee te trainen
Vergeet niet te [trainen](luis-how-to-train.md) LUIS nadat u wijzigingen in het model aanbrengt. Wijzigingen in de app LUIS zijn niet zichtbaar in de testfase totdat de app wordt getraind. 

## <a name="best-practices"></a>Aanbevolen procedures
Meer informatie over [aanbevolen procedures](luis-concept-best-practices.md).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [testen](interactive-test.md) uw utterances.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions