---
title: Inspecteer de kwaliteit van de gegevens voor aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Aangepaste spraak biedt hulpprogramma's waarmee u kunt de kwaliteit van de herkenning van een model visueel inspecteren door het vergelijken van audiogegevens met het bijbehorende resultaat voor de opname. U kunt vanuit de portal voor aangepaste spraak geüploade geluid af te spelen en te bepalen of het opgegeven herkenningsresultaat juist is.  Dit hulpprogramma kunt u snel kwaliteit van de Microsoft baseline spraak-naar-tekst model of model is getraind aangepaste controleren zonder dat u hoeft audiogegevens transcriberen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 682f3df362a7fbb0e95a07aa8a8f3a068367eef2
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025816"
---
# <a name="inspect-custom-speech-data"></a>Aangepaste spraak-gegevens controleren

> [!NOTE]
> Deze pagina wordt ervan uitgegaan dat u hebt gelezen [testgegevens voorbereiden voor aangepaste spraak](how-to-custom-speech-test-data.md) en een gegevensset voor inspectie hebt geüpload.

Aangepaste spraak biedt hulpprogramma's waarmee u kunt de kwaliteit van de herkenning van een model visueel inspecteren door het vergelijken van audiogegevens met het bijbehorende resultaat voor de opname. U kunt vanuit de portal voor aangepaste spraak geüploade geluid af te spelen en te bepalen of het opgegeven herkenningsresultaat juist is. Dit hulpprogramma kunt u snel kwaliteit van de Microsoft baseline spraak-naar-tekst model of model is getraind aangepaste controleren zonder dat u hoeft audiogegevens transcriberen.

In dit document leert u hoe u visueel controleren en de kwaliteit van een model met behulp van de trainingsgegevens die u eerder hebt geüpload.

Op deze pagina leert u hoe u de kwaliteit van de Microsoft baseline spraak-naar-tekst model en/of een aangepast model dat u hebt getraind, visueel te controleren. U gebruikt de gegevens die u hebt geüpload naar de **gegevens** tabblad voor het testen.

## <a name="create-a-test"></a>Een test maken

Volg deze instructies voor het maken van een test:

1. Navigeer naar **spraak-naar-tekst > aangepaste spraak > testen**.
2. Klik op **Test toevoegen**.
3. Selecteer **inspecteren kwaliteit (alleen Audio-gegevens)**. Geef de test een naam en beschrijving, en selecteer uw audio-gegevensset.
4. Selecteer maximaal twee modellen die u wilt testen.
5. Klik op **Create**.

Nadat een test is gemaakt, kunt u de modellen naast elkaar kunt vergelijken.

## <a name="side-by-side-model-comparisons"></a>Side-by-side-model vergelijkingen

Wanneer de status van de test is *geslaagd*, klikt u in de naam van het test-item om details van de test te bekijken. Deze pagina geeft een lijst van alle uitingen in uw gegevensset, die wijzen op de resultaten van de twee modellen naast de transcriptie uit de opgegeven gegevensset.

Om te controleren van de side-by-side-vergelijking, kunt u verschillende fouttypen, inclusief invoegen, verwijderen en vervangen van kunt schakelen. Door te luisteren naar de audio en vergelijken herkenningsresultaten in elke kolom (met human etiket transcriptie en de resultaten van de twee modellen met spraak naar tekst), kunt u bepalen welk model u voldoet aan uw behoeften en waar verbeteringen nodig zijn.

Inspecteren kwaliteitstests is handig om te controleren of de kwaliteit van een spraakherkenningseindpunt voldoende voor een toepassing is.  Volg de instructies in testen voor een objectieve meting van nauwkeurigheid, getranscribeerde audio vereisen: Nauwkeurigheid van de gegevens worden geëvalueerd.

## <a name="next-steps"></a>Volgende stappen

* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Testgegevens voorbereiden voor de aangepaste spraak](how-to-custom-speech-test-data.md)
