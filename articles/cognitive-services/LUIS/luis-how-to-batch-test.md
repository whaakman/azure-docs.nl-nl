---
title: Batch-test
titleSuffix: Language Understanding - Azure Cognitive Services
description: Met Language Understanding (LUIS) batch testsets te uitingen met onjuiste intenties en entiteiten.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 4d29602581531e9e42edbf4e00eca810757a8a99
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895729"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Testen met een set van voorbeeld-uitingen batch

 Testen van de batch is een uitgebreide test op uw huidige, getrainde model voor het meten van de prestaties van LUIS. De gegevens die wordt gebruikt voor het testen van de batch mag niet voorbeeld uitingen bevatten in de intents of uitingen ontvangen van het eindpunt van de runtime voorspelling. 

<a name="batch-testing"></a>

## <a name="import-a-dataset-file-for-batch-testing"></a>Importeren van een gegevensset-bestand voor het testen van batch

1. Selecteer **Test** in de bovenste menubalk en selecteert u vervolgens **Batch testen deelvenster**.

    ![Batch-koppeling testen](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Selecteer **importeren gegevensset**. De **nieuwe gegevensset importeren** in het dialoogvenster wordt weergegeven. Selecteer **bestand kiezen** en zoekt u een JSON-bestand met de juiste [JSON-indeling](luis-concept-batch-test.md#batch-file-format) die bevat *niet meer dan 1000* uitingen om te testen.

    Fouten worden gerapporteerd in een rode meldingsbalk aan de bovenkant van de browser. Wanneer een import fouten bevat, wordt er geen gegevensset gemaakt. Zie voor meer informatie, [veelvoorkomende fouten](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. In de **gegevenssetnaam** en voer een naam voor uw gegevensset-bestand. De gegevensset-bestand bevat een **reeks uitingen** met inbegrip van de *met het label bedoeling* en *entiteiten*. Controleer de [voorbeeld van de batch-bestand](luis-concept-batch-test.md#batch-file-format) -syntaxis. 

4. Selecteer **Done**. De gegevensset-bestand wordt toegevoegd.

## <a name="run-rename-export-or-delete-dataset"></a>Voer, wijzig de naam van, exporteren of gegevensset verwijderen

Als u wilt uitvoeren, wijzig de naam van, exporteren of verwijderen van de gegevensset, gebruikt u het beletselteken (***...*** ) knop aan het einde van de rij van de gegevensset.

![Gegevensset acties](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Een batch-test uitvoeren op uw getrainde app

Als u wilt de test uitvoert, selecteert u de naam van de gegevensset. Wanneer de test is voltooid, wordt het resultaat van de gegevensset weergegeven in deze rij.

![Testresultaat van batch](./media/luis-how-to-batch-test/run-test.png)

De downloadbare gegevensset is de hetzelfde bestand dat is geüpload voor het testen van batch.

|Status|Betekenis|
|--|--|
|![Pictogram van de groene cirkel test is geslaagd](./media/luis-how-to-batch-test/batch-test-result-green.png)|Alle uitingen zijn geslaagd.|
|![Beschadigde test rode x-pictogram](./media/luis-how-to-batch-test/batch-test-result-red.png)|Ten minste één utterance doel komt niet overeen met de voorspelling.|
|![Gereed voor het testen van pictogram](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Test is gereed om uit te voeren.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>

## <a name="view-batch-test-results"></a>Batch-testresultaten weergeven 

Als u wilt controleren van de resultaten van de batch, selecteer **resultaten**.

![De resultaten van batch](./media/luis-how-to-batch-test/run-test-results.png)

<a name="filter-chart-results-by-intent-or-entity"></a>  

## <a name="filter-chart-results"></a>Resultaten van de grafiek filteren

Om te filteren op de kaart op een bepaald doel of de entiteit, selecteert u het doel of de entiteit in het filteren aan de rechterkant-deelvenster. De gegevenspunten en de verdeling ervan bijwerken in de grafiek op basis van uw selectie. 
 
![Testresultaat gevisualiseerde Batch](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Single point utterance-gegevens weergeven

In de grafiek, Beweeg de muisaanwijzer boven een gegevenspunt om te zien van de score zekerheid van de voorspelling. Selecteer een gegevenspunt om op te halen van de bijbehorende utterance in de lijst uitingen onder aan de pagina. 

![Geselecteerde utterance](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="view-section-data"></a>Gegevens van de sectie weergeven

Selecteer in de grafiek van vier cijfers, de naam van de sectie zoals **ONWAAR positief** in de rechterbovenhoek van de grafiek. Onder de grafiek, worden alle uitingen in die sectie onder de grafiek in een lijst weergegeven. 

![Geselecteerde uitingen door sectie](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

In deze voorgaande afbeelding is de utterance `switch on` wordt aangeduid met de bedoeling TurnAllOn, maar de voorspelling van geen intentie ontvangen. Hiermee wordt aangegeven dat de bedoeling TurnAllOn behoefte heeft aan meer voorbeeld uitingen om de verwachte voorspelling. 

De twee secties van de grafiek in het rood geven uitingen die komt niet overeen met de verwachte voorspelling. Deze uitingen geven aan welke LUIS meer training nodig heeft. 

De twee secties van de grafiek in het groen komt overeen met de verwachte voorspelling.

## <a name="next-steps"></a>Volgende stappen

Als testen geeft aan dat uw LUIS-app niet wordt herkend door de juiste intenties en entiteiten, kunt u uw LUIS-app om prestaties te verbeteren door meer uitingen labels of het toevoegen van functies werken. 

* [Label van de voorgestelde uitingen van LUIS](luis-how-to-review-endpoint-utterances.md) 
* [Functies gebruiken om uw LUIS-app-prestaties te verbeteren](luis-how-to-add-features.md) 
* [Informatie over batch testen met deze zelfstudie](luis-tutorial-batch-testing.md)
* [Informatie over batch testen van concepten](luis-concept-batch-test.md).
