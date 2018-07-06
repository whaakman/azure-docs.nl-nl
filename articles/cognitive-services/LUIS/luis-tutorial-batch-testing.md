---
title: Testen van batch gebruiken voor het verbeteren van LUIS voorspellingen | Microsoft Docs
titleSuffix: Azure
description: Een belastingtest uit batch, bekijk de resultaten en verbeteren van LUIS voorspellingen te wijzigen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 4a5ace10c171d17235051c5bd666526318829fd7
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867338"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Gebruik van batch testen om te zoeken voorspelling nauwkeurigheid problemen

Deze zelfstudie wordt gedemonstreerd hoe u met batch testen utterance voorspelling problemen vinden.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Maak een batchbestand testen 
* Een batch-test uitvoeren
* Bekijk de resultaten
* Corrigeer de fouten voor intents
* Testen van de batch

## <a name="prerequisites"></a>Vereisten

> [!div class="checklist"]
> * Voor dit artikel, moet u ook een [LUIS][LUIS] account om te kunnen maken van uw LUIS-toepassing.

> [!Tip]
> Als u nog geen een abonnement, kunt u zich registreren voor een [gratis account](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Nieuwe app maken
In dit artikel wordt het vooraf gedefinieerde HomeAutomation-domein. De vooraf gedefinieerde domein heeft intenties en entiteiten uitingen voor het beheren van apparaten zoals verlichting HomeAutomation. De app maken, toevoegen van het domein, trainen en publiceren.

1. In de [LUIS] website, een nieuwe app maken door te selecteren **nieuwe app maken** op de **MyApps** pagina. 

    ![Nieuwe app maken](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Voer de naam `Batchtest-HomeAutomation` in het dialoogvenster.

    ![Voer de naam van app](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Selecteer **vooraf gemaakte domeinen** in de linkerbenedenhoek. 

    ![Vooraf gedefinieerde domein selecteren](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Selecteer **domein toevoegen** voor HomeAutomation.

    ![HomeAutomation domein toevoegen](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Selecteer **Train** in de navigatiebalk bovenaan rechts.

    ![Selecteer de knop van de trein](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Criteria voor batch-test
Batch testen kunt tot 1000 uitingen testen op een tijdstip. De batch mag geen dubbele waarden hebben. [Exporteren](create-new-app.md#export-app) de app om te zien van de lijst met huidige uitingen.  

De test-strategie voor LUIS maakt gebruik van drie verschillende sets van gegevens: uitingen batch test uitingen en eindpunt uitingen model. Zorg ervoor dat u niet de uitingen van een model-uitingen (toegevoegd aan een doel) of een eindpunt uitingen voor deze zelfstudie. 

Gebruik niet een van de uitingen al in de app voor de batch-test:

```
'breezeway on please',
'change temperature to seventy two degrees',
'coffee bar on please',
'decrease temperature for me please',
'dim kitchen lights to 25 .',
'fish pond off please',
'fish pond on please',
'illuminate please',
'living room lamp on please',
'living room lamps off please',
'lock the doors for me please',
'lower your volume',
'make camera 1 off please',
'make some coffee',
'play dvd',
'set lights bright',
'set lights concentrate',
'set lights out bedroom',
'shut down my work computer',
'silence the phone',
'snap switch fan fifty percent',
'start master bedroom light .',
'theater on please',
'turn dimmer off',
'turn off ac please',
'turn off foyer lights',
'turn off living room light',
'turn off staircase',
'turn off venice lamp',
'turn on bathroom heater',
'turn on external speaker',
'turn on my bedroom lights .',
'turn on the furnace room lights',
'turn on the internet in my bedroom please',
'turn on thermostat please',
'turn the fan to high',
'turn thermostat on 70 .' 
```

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Maak een batch als u wilt testen van intentie nauwkeurigheid
1. Maak `homeauto-batch-1.json` in een teksteditor zoals [VSCode](https://code.visualstudio.com/). 

2. Utterances met toevoegen de **bedoeling** gewenste voorspelde in de test. Voor deze zelfstudie om het eenvoudig maken, uitingen nemen de `HomeAutomation.TurnOn` en `HomeAutomation.TurnOff` en schakelt u over de `on` en `off` tekst in de uitingen. Voor de `None` doel toevoegen van een aantal uitingen die geen deel uitmaken van de [domein](luis-glossary.md#domain) (onderwerp). 

    Om te begrijpen hoe de resultaten van de batch correleert met de JSON van de batch, moet u alleen zes intents toevoegen.

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>Voer de batch
1. Selecteer **Test** in de bovenste navigatiebalk. 

    ![Selecteer de Test in de navigatiebalk](./media/luis-tutorial-batch-testing/test-1.png)

2. Selecteer **Batch testen deelvenster** in het deelvenster aan de rechterkant. 

    ![Selecteer Batch test deelvenster](./media/luis-tutorial-batch-testing/test-2.png)

3. Selecteer **importeren gegevensset**.

    ![Selecteer de gegevensset importeren](./media/luis-tutorial-batch-testing/test-3.png)

4. Kies de locatie van het bestand system van de `homeauto-batch-1.json` bestand.

5. Naam van de gegevensset `set 1`.

    ![Bestand selecteren](./media/luis-tutorial-batch-testing/test-4.png)

6. Selecteer de knop **Run**. Wacht totdat de test is voltooid.

    ![Selecteer uitvoeren](./media/luis-tutorial-batch-testing/test-5.png)

7. Selecteer **resultaten**.

    ![Resultaten weergeven](./media/luis-tutorial-batch-testing/test-6.png)

8. Bekijk de resultaten in de grafiek en legenda.

    ![Batch-resultaten](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Bekijk de resultaten van batch
De batch-resultaten worden in twee secties. Het bovenste gedeelte bevat de grafiek en de legenda. In het onderste gedeelte vindt u uitingen als u de naam van een gebied van de grafiek.

Eventuele fouten worden aangeduid met de kleur rood. De grafiek wordt in vier secties met twee van de secties in het rood weergegeven. **Dit zijn de secties zich richten op**. 

De rechterbovenhoek sectie onjuist geeft aan dat de test voorspelde sprake is van een doel of de entiteit. Het linkergedeelte onder geeft aan dat de test voorspeld ten onrechte de afwezigheid van een doel of de entiteit.

### <a name="homeautomationturnoff-test-results"></a>De resultaten van HomeAutomation.TurnOff
Selecteer in de legenda de `HomeAutomation.TurnOff` intentie. Heeft een pictogram groen succes aan de linkerkant van de naam in de legenda. Er zijn geen fouten voor dit doel. 

![Batch-resultaten](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn en geen intents bevatten fouten
De andere twee intenties bevatten fouten, wat betekent dat de test voorspellingen komen niet overeen met de verwachtingen van batch-bestand. Selecteer de `None` intentie in de legenda om te controleren van de eerste fout. 

![Geen intentie](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Fouten worden weergegeven in de grafiek in de rode secties: **ONWAAR positief** en **False negatieve**. Selecteer de **False negatieve** sectienaam in de grafiek om te zien van de mislukte uitingen onder de grafiek. 

![De waarde False negatieve fouten](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

De utterance mislukt `help` verwacht als een `None` doel, maar de test voorspeld `HomeAutomation.TurnOn` intentie.  

Er zijn twee fouten, één in HomeAutomation.TurnOn en één in None. Beide zijn veroorzaakt door de utterance `help` omdat het niet voldeed aan de verwachting geen en er een onverwachte overeenkomst voor het doel HomeAutomation.TurnOn is. 

Om te bepalen waarom de `None` uitingen mislukken, controleert u de uitingen die momenteel in `None`. 

## <a name="review-none-intents-utterances"></a>Controleer geen intentie uitingen de

1. Sluit de **Test** panel door het selecteren van de **Test** knop op de bovenste navigatiebalk. 

2. Selecteer **bouwen** van het bovenste navigatievenster. 

3. Selecteer **geen** intentie van de lijst met intents.

4. Besturingselement + E om een token weergave van de uitingen selecteren 
    
    |Geen intentie's uitingen|Voorspellingsscore|
    |--|--|
    |"verkleinen temperatuur voor mij."|0.44|
    |"dimensie keuken verlichting op 25."|0,43|
    |"het volume lager"|0.46|
    |"inschakelen op het internet in mijn Neem slaapkamers"|0,28|

## <a name="fix-none-intents-utterances"></a>Los geen intentie uitingen de
    
Alle uitingen in `None` zijn buiten de app-domein. Deze uitingen zijn ten opzichte van HomeAutomation, zodat ze zich in de verkeerde bedoelingen. 

LUIS biedt ook de uitingen op die kleiner is dan 50% (<.50) voorspelling score. Als u de uitingen in de andere twee intenties bekijkt, ziet u veel hoger voorspelling scores. Als LUIS lage scores voor de voorbeeld-uitingen die een goede indicatie zijn de uitingen lastig te LUIS tussen de huidige intentie en andere intents. 

Om op te lossen van de app, de uitingen die momenteel in de `None` doel moet worden verplaatst naar het juiste doel en de `None` bedoeling moet nieuwe, juiste intents. 

Drie van de uitingen in de `None` doel zijn bedoeld om u te verlagen van de instellingen van het automation-apparaat. Ze gebruiken woorden, zoals `dim`, `lower`, of `decrease`. De vierde utterance wordt gevraagd om in te schakelen op het internet. Aangezien alle vier uitingen over het inschakelen of wijzigen van de mate van kracht op een apparaat, moeten u ze verplaatst naar de `HomeAutomation.TurnOn` intentie. 

Dit is slechts één oplossing. U kunt ook een nieuw doel van maken `ChangeSetting` en verplaatsen van de uitingen met behulp van dimensie, verlagen en verlagen naar die nieuwe intentie. 

## <a name="fix-the-app-based-on-batch-results"></a>Herstellen van de app op basis van batch-resultaten
Verplaatsen van de vier uitingen naar de `HomeAutomation.TurnOn` intentie. 

1. Selecteer het selectievakje boven de lijst met utterance zodat alle uitingen zijn geselecteerd. 

2. In de **opnieuw toewijzen van intentie** vervolgkeuzelijst, selecteer `HomeAutomation.TurnOn`. 

    ![Uitingen verplaatsen](./media/luis-tutorial-batch-testing/move-utterances.png)

    Nadat de vier uitingen zijn toegewezen, de utterance lijst voor de `None` bedoeling is leeg.

3. Vier nieuwe intents toevoegen voor de intentie geen:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Deze uitingen zijn absoluut buiten het domein van HomeAutomation. Als u elke utterance invoert, bekijk de score voor. De score is mogelijk met lage of zelfs zeer laag (met een rood kader rond het). Nadat u in stap 8, de app trainen zal de score is veel hoger zijn. 

7. Verwijderen van de labels die door het blauwe label selecteren in de utterance en selecteer **label verwijderen**.

8. Selecteer **Train** in de navigatiebalk bovenaan rechts. De score van elke utterance is veel hoger. Alle scores voor de `None` nu kunt u lezen wat hierboven.80 moet zijn. 

## <a name="verify-the-fix-worked"></a>Controleren of de oplossing heeft gewerkt
Om te controleren dat de uitingen in de batch-test correct worden voorspeld voor de **geen** doel, voer de batch-test opnieuw uit.

1. Selecteer **Test** in de bovenste navigatiebalk. 

2. Selecteer **Batch testen deelvenster** in het deelvenster aan de rechterkant. 

3. Selecteer het weglatingsteken (***...*** ) aan de rechterkant van de batchnaam en selecteer **gegevensset uitvoeren**. Wacht totdat de batch-test is voltooid.

    ![Gegevensset uitvoeren](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Selecteer **resultaten**. De intenties moeten alle groen pictogrammen aan de linkerkant van de intentie namen hebben. Met het juiste filter ingesteld op de `HomeAutomation.Turnoff` doel, selecteer het groene stip in het bovenste deelvenster aan de rechterkant die het dichtst bij het midden van de grafiek. De naam van de utterance wordt weergegeven in de tabel onder de grafiek. De score van `breezeway off please` zeer laag. Een optionele activiteit is meer uitingen toevoegen aan het doel te verhogen van deze score. 

    ![Gegevensset uitvoeren](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

<!--
    The Entities section of the legend may have errors. That is the next thing to fix.

## Create a batch to test entity detection
1. Create `homeauto-batch-2.json` in a text editor such as [VSCode](https://code.visualstudio.com/). 

2. Utterances have entities identified with `startPos` and `endPost`. These two elements identify the entity before [tokenization](luis-glossary.md#token), which happens in some [cultures](luis-supported-languages.md#tokenization) in LUIS. If you plan to batch test in a tokenized culture, learn how to [extract](luis-concept-data-extraction.md#tokenized-entity-returned) the non-tokenized entities.

    Copy the following JSON into the file:

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 4
            }
          ]
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Operation",
              "startPos": 7,
              "endPos": 17
            }
          ]
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 9
            }
          ]
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Device",
              "startPos": 0,
              "endPos": 10
            }
          ]
        }
      ]
    ```

3. Import the batch file, following the [same instructions](#run-the-batch) as the first import, and name the dataset `set 2`. Run the test.

## Possible entity errors
Since the intents in the right-side filter of the test panel still pass the test, this section focuses on correct entity identification. 

Entity testing is diferrent than intents. An utterance will have only one top scoring intent, but it may have several entities. An utterance's entity may be correctly identified, may be incorrectly identified as an entity other than the one in the batch test, may overlap with other entities, or not identified at all. 

## Review entity errors
1. Select `HomeAutomation.Device` in the filter panel. The chart changes to show a single false positive and several true negatives. 

2. Select the False positive section name. The utterance for this chart point is displayed below the chart. The labeled intent and the predicted intent are the same, which is consistent with the test -- the intent prediction is correct. 

    The issue is that the HomeAutomation.Device was detected but the batch expected HomeAutomation.Room for the utterance "coffee bar off please". `Coffee bar` could be a room or a device, depending on the environment and context. As the model designer, you can either enforce the selection as `HomeAutomation.Room` or change the batch file to use `HomeAutomation.Device`. 

    If you want to reinforce that coffee bar is a room, you nee to add an utterances to LUIS that help LUIS decide a coffee bar is a room. 

    The most direct route is to add the utterance to the intent but that to add the utterance for every entity detection error is not the machine-learned solution. Another fix would be to add an utterance with `coffee bar`.

## Add utterance to help extract entity
1. Select the **Test** button on the top navigation to close the batch test panel.

2. On the `HomeAutomation.TurnOn` intent, add the utterance, `turn coffee bar on please`. The uttterance should have all three entities detected after you select enter. 

3. Select **Train** on the top navigation panel. Wait until training completes successfully.

3. Select **Test** on the top navigation panel to open the Batch testing pane again. 

4. If the list of datasets is not visible, select **Back to list**. Select the ellipsis (***...***) button at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de voorbeeld-uitingen](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions