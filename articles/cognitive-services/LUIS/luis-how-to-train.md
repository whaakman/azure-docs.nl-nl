---
title: App trainen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Training is het proces van uw app-versie van de Language Understanding (LUIS) voor het verbeteren van de natuurlijke taal begrijpen lessen. Uw LUIS-app na updates voor het model zoals toevoegen, bewerken, labels of verwijderen van entiteiten, intents of uitingen trainen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: ff1239093f7562bd314305ae3ea8a580fddb8326
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862272"
---
# <a name="train-your-active-version-of-the-luis-app"></a>Trainen van uw actieve versie van de LUIS-app 

Training is het proces van uw app Language Understanding (LUIS) voor het verbeteren van de natuurlijke taal begrijpen lessen. Uw LUIS-app na updates voor het model zoals toevoegen, bewerken, labels of verwijderen van entiteiten, intents of uitingen trainen. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Training en [testen](luis-concept-test.md) een app is een iteratief proces. Nadat u uw LUIS-app trainen, kunt u deze testen met voorbeeldgegevens uitingen om te controleren of de intenties en entiteiten correct worden herkend. Als ze niet zijn, moet u opnieuw updates voor de LUIS-app, trainen en testen. 

Training wordt toegepast op de actieve versie in de LUIS-portal. 

## <a name="how-to-train-interactively"></a>Hoe u met het trainen van interactief

De iteratief proces gestart in de [LUIS portal](https://www.luis.ai), moet u eerst uw LUIS-app ten minste één keer te trainen. Zorg ervoor dat elke bedoeling heeft ten minste één utterance voordat een training.

1. Toegang tot uw app door het selecteren van de naam ervan op de **mijn Apps** pagina. 

2. Selecteer in uw app **Train** in het bovenste deelvenster. 

3. Wanneer training voltooid is, wordt een groen meldingsbalk weergegeven aan de bovenkant van de browser.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Als u een of meer intents in uw app die geen voorbeeld uitingen bevatten hebt, kunt u uw app kan niet trainen. Utterances voor alle uw intents toevoegen. Zie voor meer informatie, [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md).

## <a name="train-with-all-data"></a>Met alle gegevens van de trein

Training maakt gebruik van een klein percentage van het negatieve samplen bijhouden. Als u wilt dat alle gegevens gebruiken in plaats van de kleine negatieve steekproeven, gebruikt u de [versie instellingen API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) met de `UseAllTrainingData` ingesteld op waar deze functie uitschakelen. 

## <a name="unnecessary-training"></a>Onnodige training

U hoeft niet te trainen na elke één wijziging. Training moet worden uitgevoerd nadat een groep van de wijzigingen worden toegepast op het model en de volgende stap die u wilt doen, is te testen of te publiceren. Als u niet hoeft te testen of te publiceren, training niet nodig. 

## <a name="training-with-the-rest-apis"></a>Training met de REST API 's

Training in de portal LUIS bestaat uit één stap van de te drukken de **Train** knop. Training met de REST API's is een proces in twee stappen. De eerste [aanvragen training](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) met HTTP POST. Vraag de [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46) met HTTP Get. 

Als u wilt weten wanneer de training is voltooid, moet u de status te peilen totdat alle modellen met succes zijn getraind. 

## <a name="next-steps"></a>Volgende stappen

* [Label van de voorgestelde uitingen van LUIS](luis-how-to-review-endoint-utt.md) 
* [Functies gebruiken om uw LUIS-app-prestaties te verbeteren](luis-how-to-add-features.md) 
