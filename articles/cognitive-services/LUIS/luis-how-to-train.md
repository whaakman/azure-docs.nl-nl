---
title: Uw LUIS-versie trainen
titleSuffix: Azure Cognitive Services
description: Training is het proces van uw app-versie van de Language Understanding (LUIS) voor het verbeteren van de natuurlijke taal begrijpen lessen. Uw LUIS-app na updates voor het model zoals toevoegen, bewerken, labels of verwijderen van entiteiten, intents of uitingen trainen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: f27716cc416b162a5b2df5542d709058f3b3e903
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182028"
---
# <a name="train-your-luis-app-version"></a>Uw LUIS-app-versie trainen

Training is het proces van uw app Language Understanding (LUIS) voor het verbeteren van de natuurlijke taal begrijpen lessen. Uw LUIS-app na updates voor het model zoals toevoegen, bewerken, labels of verwijderen van entiteiten, intents of uitingen trainen. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Training en [testen](luis-concept-test.md) een app is een iteratief proces. Nadat u uw LUIS-app trainen, kunt u deze testen met voorbeeldgegevens uitingen om te controleren of de intenties en entiteiten correct worden herkend. Als ze niet zijn, moet u opnieuw updates voor de LUIS-app, trainen en testen. 

## <a name="how-to-train"></a>Hoe u met het trainen van
Voor het starten van de iteratief proces, moet u eerst uw LUIS-app ten minste één keer te trainen. Zorg ervoor dat elke bedoeling heeft ten minste één utterance voordat een training.

1. Toegang tot uw app door het selecteren van de naam ervan op de **mijn Apps** pagina. 

2. Selecteer in uw app **Train** in het bovenste deelvenster. 

3. Wanneer training voltooid is, wordt een groen meldingsbalk weergegeven aan de bovenkant van de browser.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Als u een of meer intents in uw app die geen voorbeeld uitingen bevatten hebt, kunt u uw app kan niet trainen. Utterances voor alle uw intents toevoegen. Zie voor meer informatie, [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md).

## <a name="train-with-all-data"></a>Met alle gegevens van de trein
Training maakt gebruik van een klein percentage van het negatieve samplen bijhouden. Als u wilt dat alle gegevens gebruiken in plaats van de kleine negatieve steekproeven, gebruikt u de [versie instellingen API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) met de `UseAllTrainingData` is ingesteld op waar deze functie uitschakelen. 

## <a name="next-steps"></a>Volgende stappen

* [Label van de voorgestelde uitingen van LUIS](luis-how-to-review-endoint-utt.md) 
* [Functies gebruiken om uw LUIS-app-prestaties te verbeteren](luis-how-to-add-features.md) 