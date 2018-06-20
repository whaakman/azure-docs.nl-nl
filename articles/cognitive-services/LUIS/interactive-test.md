---
title: Trainen en te testen van uw app LUIS - Azure | Microsoft Docs
description: Language Understanding (LUIS) continu werken op uw toepassing deze verfijnen en het verbeteren van de kennis van de taal gebruiken.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: fb4c3bb117d1ea60c9cc28d2b193ee3c01f6c945
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221628"
---
# <a name="test-your-luis-app"></a>Uw app LUIS testen
<a name="train-your-app"></a>
[Training](luis-how-to-train.md) en [testen](luis-concept-test.md) een app is een iteratief proces. Na het trainen van uw app LUIS is getest met voorbeeld utterances om te controleren of de intents en entiteiten correct worden herkend. Als ze niet, updates aanbrengen in de app LUIS, train en test opnieuw. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="interactive-testing"></a>
## <a name="test-an-utterance"></a>Een utterance testen

1. Toegang tot uw app door de naam ervan selecteren op de **mijn Apps** pagina. 

2. Voor toegang tot de **Test** dia-out Configuratiescherm, selecteer **Test** in het bovenpaneel van uw toepassing.

    ![Test-App en Train-pagina](./media/luis-how-to-interactive-test/test.png)

3. Voer een utterance in het tekstvak in en selecteer Enter. U kunt opgeven als veel utterances testen als u wilt in de **testen**, maar slechts één utterance tegelijk.

4. De utterance, de belangrijkste doel en score worden toegevoegd aan de lijst met utterances onder het tekstvak.

    ![Interactieve testen identificeert de verkeerde bedoelingen](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="clear-test-panel"></a>Schakel test-Configuratiescherm
Schakel alle ingevoerde test-utterances en de resultaten van de test-console, selecteer **beginnen** op de linkerbovenhoek van de **Test Configuratiescherm**. 

## <a name="close-test-panel"></a>Deelvenster sluiten test
Sluit de **Test** Configuratiescherm, selecteer de **Test** knop opnieuw.

## <a name="inspect-score"></a>Score controleren
Controleren van details van de testresultaten in het **inspecteren** Configuratiescherm. 
 
1. Met de **Test** dia-out Configuratiescherm openen, selecteer **inspecteren** voor een utterance die u wilt vergelijken. 

    ![Knop controleren](./media/luis-how-to-interactive-test/inspect.png)

2. De **inspectie** deelvenster wordt weergegeven. Het deelvenster bevat de bovenste score berekenen voor doel, evenals de geïdentificeerde entiteiten. Het paneel geeft het resultaat van de geselecteerde utterance.

    ![Knop controleren](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Juiste boven bedoeling score berekenen

1. Als boven score berekenen bedoeling onjuist is, selecteert u de **bewerken** knop.

2.  Selecteer het juiste type voor de utterance in de vervolgkeuzelijst.

    ![Juiste doel selecteren](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>De resultaten gevoel weergeven

Als **gevoel analysis** is geconfigureerd op de **[publiceren](publishapp.md#enable-sentiment-analysis)** pagina, de testresultaten bevatten de gevoel gevonden in de utterance. 

![Afbeelding van testvenster met gevoel analyses](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Overeenkomende patroon bedoeling corrigeren
Als u [patronen](luis-concept-patterns.md) en de utterance komt overeen met een patroon, maar de verkeerde bedoelingen voorspeld is, selecteert u de **bewerken** koppelen door het patroon en selecteer vervolgens de juiste bedoeling.

## <a name="compare-with-published-version"></a>Vergelijken met de gepubliceerde versie
U kunt de actieve versie van uw app met de gepubliceerde testen [eindpunt](luis-glossary.md#endpoint) versie. In de **inspecteren** Configuratiescherm, selecteer **vergelijken met gepubliceerd**. Alle tests op het gepubliceerde model wordt afgetrokken van het saldo van uw Azure-abonnement quotum. 

![Vergelijken met gepubliceerd](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Eindpunt JSON in test-deelvenster weergeven
Ziet u het eindpunt JSON geretourneerd voor de vergelijking door het selecteren van de **weergeven JSON-weergave**.

![Gepubliceerde JSON-antwoord](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Extra instellingen in Configuratiescherm van de test

### <a name="luis-endpoint"></a>LUIS eindpunt
Als u verschillende LUIS eindpunten hebt, gebruikt u de **extra instellingen** koppeling op de testmachine deelvenster voor het wijzigen van het eindpunt dat wordt gebruikt voor het testen van de gepubliceerd. Als u niet zeker welk eindpunt weet te gebruiken, selecteert u de standaard **Starter_Key**. 

![Deelvenster testen met extra instellingen koppeling gemarkeerd](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Spellingcontrole van Bing correcties in test-deelvenster weergeven
Vereisten voor de spellingcorrecties weergeven: 

* Gepubliceerde app
* Spellingcontrole van Bing [servicesleutel](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). De sleutel van de service wordt niet opgeslagen en moet opnieuw worden ingesteld voor elke browsersessie. 

Gebruik de volgende procedure om op te nemen de [spellingcontrole van Bing v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) service in het deelvenster resultaten. 

1. In de **Test** deelvenster een utterance invoeren. Wanneer de utterance wordt voorspeld, selecteert u **[inspecteren](#inspect-score)** onder de utterance die u hebt ingevoerd. 

2. Wanneer de **inspecteren** Configuratiescherm wordt geopend, selecteer  **[vergelijken met gepubliceerde](#compare-with-published-version)**. 

3. Wanneer de **gepubliceerde** Configuratiescherm wordt geopend, selecteer  **[extra instellingen](#additional-settings-in-test-panel)**.

4. Voer in het pop-updialoogvenster uw **spellingcontrole van Bing** servicesleutel. 
    ![Voer de sleutel voor spellingcontrole van Bing-service](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

5. Voer een query met een onjuiste spelling zoals `book flite to seattle` en voert u selecteert. De onjuiste spelling van het woord `flite` in de query verzonden naar LUIS wordt vervangen en de resulterende JSON zowel de oorspronkelijke query wordt weergegeven als `query`, en de gecorrigeerde spelling in de query als `alteredQuery`.

    ![Gecorrigeerd spelling JSON](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="batch-testing"></a>Batch testen
Zie batch testen [concepten](luis-concept-batch-test.md) en meer informatie over [hoe](luis-how-to-batch-test.md) een batch utterances testen.

## <a name="next-steps"></a>Volgende stappen

Als testen wordt aangegeven dat uw app LUIS niet wordt herkend door de juiste intents en entiteiten, kunt u uw LUIS-app om nauwkeurigheid te verbeteren door meer utterances labels of toe te voegen functies werken. 

* [Voorgestelde utterances met LUIS label](Label-Suggested-Utterances.md) 
* [Functies gebruiken om de prestaties van uw app LUIS te verbeteren](luis-how-to-add-features.md) 
