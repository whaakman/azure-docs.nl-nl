---
title: Training van uw app LUIS - Azure | Microsoft Docs
description: Language Understanding (LUIS) gebruiken voor het trainen van het model.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 4593954e9e0a60beaa5ee86df848f908b23c6b20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345757"
---
# <a name="train-your-luis-app"></a>Uw app LUIS trainen

Training is het proces van uw app Language Understanding (LUIS) voor het verbeteren van natuurlijke taal inzicht te lichten. Training van uw app LUIS na updates voor het model zoals toevoegen, bewerken, labels of entiteiten, intents of utterances verwijderen. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Training en [testen](luis-concept-test.md) een app is een iteratief proces. Nadat u uw app LUIS training, kunt u deze testen met voorbeeld utterances om te controleren of de intents en entiteiten correct worden herkend. Als ze niet, updates aanbrengen in de app LUIS, train en test opnieuw. 

## <a name="train-your-app"></a>Training van uw app
Voor het starten van de iteratief proces, moet u eerst uw app LUIS ten minste één keer te trainen. Zorg ervoor dat elke bedoeling ten minste één utterance training.

1. Toegang tot uw app door de naam ervan selecteren op de **mijn Apps** pagina. 

2. Selecteer in uw app **Train** in het bovenste deelvenster. 

    ![Knop Train](./media/luis-how-to-train/train-button.png)

3. Wanneer training voltooid is, verschijnt een groen meldingsbalk aan de bovenkant van de browser.

    ![Test-App en Train-pagina](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Als u een of meer intents in uw app die geen voorbeeld utterances bevatten hebt, kan niet een training van uw app. Utterances voor alle uw intents toevoegen. Zie voor meer informatie [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Volgende stappen

* [Voorgestelde utterances met LUIS label](Label-Suggested-Utterances.md) 
* [Functies gebruiken om de prestaties van uw app LUIS te verbeteren](luis-how-to-add-features.md) 