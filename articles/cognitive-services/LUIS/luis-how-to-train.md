---
title: Uw LUIS-app - Azure trainen | Microsoft Docs
description: Language Understanding (LUIS) gebruiken voor het trainen van uw model.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8ec731ee2110b21a35d76d53fae4a1a6756fb5b8
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951811"
---
# <a name="train-your-luis-app"></a>Uw LUIS-app trainen

Training is het proces van uw app Language Understanding (LUIS) voor het verbeteren van de natuurlijke taal begrijpen lessen. Uw LUIS-app na updates voor het model zoals toevoegen, bewerken, labels of verwijderen van entiteiten, intents of uitingen trainen. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Training en [testen](luis-concept-test.md) een app is een iteratief proces. Nadat u uw LUIS-app trainen, kunt u deze testen met voorbeeldgegevens uitingen om te controleren of de intenties en entiteiten correct worden herkend. Als ze niet zijn, moet u opnieuw updates voor de LUIS-app, trainen en testen. 

## <a name="train-your-app"></a>Uw app trainen
Voor het starten van de iteratief proces, moet u eerst uw LUIS-app ten minste één keer te trainen. Zorg ervoor dat elke bedoeling heeft ten minste één utterance voordat een training.

1. Toegang tot uw app door het selecteren van de naam ervan op de **mijn Apps** pagina. 

2. Selecteer in uw app **Train** in het bovenste deelvenster. 

    ![Knop van de trein](./media/luis-how-to-train/train-button.png)

3. Wanneer training voltooid is, wordt een groen meldingsbalk weergegeven aan de bovenkant van de browser.

    ![Pagina Train & Test-App](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Als u een of meer intents in uw app die geen voorbeeld uitingen bevatten hebt, kunt u uw app kan niet trainen. Utterances voor alle uw intents toevoegen. Zie voor meer informatie, [voorbeeld utterances toevoegen](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Volgende stappen

* [Label van de voorgestelde uitingen van LUIS](luis-how-to-review-endoint-utt.md) 
* [Functies gebruiken om uw LUIS-app-prestaties te verbeteren](luis-how-to-add-features.md) 