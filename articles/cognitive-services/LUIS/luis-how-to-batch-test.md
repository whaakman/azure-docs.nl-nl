---
title: Uw LUIS voor app - Azure batch testen | Microsoft Docs
description: Gebruik Language Understanding (LUIS) batch testen utterances met onjuiste intents en entiteiten te vinden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 822fb1e2d5b13941527d242e8501b423bd6b81cb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265510"
---
# <a name="batch-testing"></a>Batch testen
 Testen van de batch is een uitgebreide test van uw huidige getraind model voor het meten van de prestaties in LUIS. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Een gegevensset-bestand voor het testen van batch importeren

1. Selecteer **Test** in de bovenste opdrachtbalk en selecteer vervolgens **Batch testen Configuratiescherm**.

    ![Testen van batch-koppeling](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Selecteer **importeren gegevensset**. De **importeren nieuwe gegevensset** dialoogvenster wordt weergegeven. Selecteer **bestand kiezen** en zoek de [JSON](luis-concept-batch-test.md#batch-file-format) -bestand met *niet meer dan 1000* utterances om te testen.

    ![De Dataset-bestand importeren](./media/luis-how-to-batch-test/batchtest-importset.png)

    Fouten worden gerapporteerd in een rode meldingsbalk aan de bovenkant van de browser. Wanneer een import fouten bevat, wordt er geen gegevensset gemaakt. Zie voor meer informatie [veelvoorkomende fouten](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. In de **gegevenssetnaam** en voer een naam voor uw gegevensset-bestand. Het bestand gegevensset bevat een **matrix van utterances** met inbegrip van de *bedoeling gelabeld* en *entiteiten*. Controleer de [voorbeeldbatchbestand](luis-concept-batch-test.md#batch-file-format) syntaxis. 

4. Selecteer **gedaan**. De dataset-bestand wordt toegevoegd.

## <a name="run-rename-export-or-delete-dataset"></a>Uitvoeren, wijzig de naam, exporteren of verwijder gegevensset
Als u wilt uitvoeren, wijzig de naam, exporteren of de gegevensset te verwijderen, gebruikt u de drie puntjes (**...** ) aan het einde van de rij van de gegevensset.

![DataSet acties](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Een batch-test uitvoeren op uw getraind app

Als u wilt de test uitvoert, selecteert u de gegevenssetnaam. Wanneer de test is voltooid, wordt deze rij het testresultaat van de gegevensset.

![Testresultaat van batch](./media/luis-how-to-batch-test/run-test.png)

De downloadbare gegevensset is hetzelfde bestand dat is geüpload voor het testen van de batch.

|Status|Betekenis|
|--|--|
|![Geslaagde test groene cirkel-pictogram](./media/luis-how-to-batch-test/batch-test-result-green.png)|Alle utterances zijn geslaagd.|
|![Beschadigde test rode x pictogram](./media/luis-how-to-batch-test/batch-test-result-red.png)|Ten minste één utterance doel komt niet overeen met de voorspelling.|
|![Pictogram testen](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Test is gereed om uit te voeren.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Testresultaten van weergave-batch 
Selecteer om te controleren van de testresultaten batch, **resultaten**.

![Batch-testresultaten](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a> ## Resultaten van de grafiek filteren

Wilt filteren door een specifiek doel of de entiteit van de grafiek, selecteert u de bedoeling of entiteit in de rechtertabel filteren Configuratiescherm. De gegevenspunten en de verdeling bijwerken in de grafiek volgens uw selectie. 
 
![Testresultaat gevisualiseerde Batch](./media/luis-how-to-batch-test/filter-by-entity.png) 

<!--
## Investigate false sections
Data points on the **[False Positive][false-positive]** and **[False Negative][false-negative]** sections indicate errors, which should be investigated. If all data points are on the **[True Positive][true-positive]** and **[True Negative][true-negative]** sections, then your application's performance is perfect on this dataset.


The graph indicates [F-measure][f-measure], [recall][recall], and [precision][precision].  
-->
## <a name="view-single-point-utterance-data"></a>Gegevens van één punt utterance weergeven
In de grafiek, de muisaanwijzer op een gegevenspunt om te zien van de score van de zekerheid van de voorspelling. Selecteer een gegevenspunt om op te halen van de bijbehorende utterance in de lijst utterances aan de onderkant van de pagina. 

![Geselecteerde utterance](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Gegevens van de sectie
Selecteer in het diagram van vier cijfers, de naam van de sectie zoals **onjuiste positieve** in de rechterbovenhoek van de grafiek. Onder de grafiek worden alle utterances in die sectie onder de grafiek in een lijst weergegeven. 

![Geselecteerde utterances door de sectie](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

In deze voorgaande afbeelding geeft de utterance `switch on` is gelabeld met de bedoeling TurnAllOn, maar het ontvangen van de voorspelling van geen opzet. Hiermee wordt aangegeven dat de bedoeling TurnAllOn meer voorbeeld utterances moet als doel het maken van de verwachte voorspelling. 

De twee secties van de grafiek in rood utterances die komt niet overeen met de verwachte voorspelling aangeven. Deze utterances geven aan welke LUIS moet meer training. 

De twee secties van de grafiek in het groen komt overeen met de verwachte voorspelling.

## <a name="next-steps"></a>Volgende stappen

Als testen wordt aangegeven dat uw app LUIS niet wordt herkend door de juiste intents en entiteiten, kunt u uw LUIS-app om prestaties te verbeteren door meer utterances labels of toe te voegen functies werken. 

* [Voorgestelde utterances met LUIS label](Label-Suggested-Utterances.md) 
* [Functies gebruiken om de prestaties van uw app LUIS te verbeteren](luis-how-to-add-features.md) 
* [Batch testen met deze zelfstudie begrijpen](luis-tutorial-batch-testing.md)
* [Meer informatie over het testen van concepten batch](luis-concept-batch-test.md).

[true-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-positive
[true-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-negative
[false-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-positive
[false-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-negative
[f-measure]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#f-measure
[recall]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#recall
[precision]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#precision

