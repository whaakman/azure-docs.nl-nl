---
title: Tests in batch te gebruiken voor het verbeteren van LUIS voorspellingen | Microsoft Docs
titleSuffix: Azure
description: Laden van de batch-test, resultaten bekijken en LUIS voorspellingen met wijzigingen te verbeteren.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 5788f17f2724a0354a1db506971c2343c1800f01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266393"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Gebruik alleen voor testdoeleinden batch voorspelling nauwkeurigheid vinden

Deze zelfstudie laat zien hoe gebruikt u batch testen om te zoeken utterance voorspelling problemen.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Maak een batchbestand test 
* Voer een batch-test
* Test-resultaten bekijken
* Los de fouten voor intents
* De batch testen

## <a name="prerequisites"></a>Vereisten

> [!div class="checklist"]
> * Voor dit artikel, moet u ook een [LUIS][LUIS] account om uw toepassing LUIS ontwerpen.

> [!Tip]
> Als u nog geen een abonnement, kunt u registreren voor een [gratis account](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Nieuwe app maken
Dit artikel wordt gebruikt voor het vooraf gedefinieerde HomeAutomation-domein. Het vooraf gedefinieerde domein heeft intents en entiteiten utterances voor het beheren van HomeAutomation apparaten zoals lichten. Maken van de app, het toevoegen van het domein, te trainen en te publiceren.

1. In de [LUIS] website, een nieuwe app maken door het selecteren van **nieuwe app maken** op de **MyApps** pagina. 

    ![Nieuwe app maken](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Voer de naam `Batchtest-HomeAutomation` in het dialoogvenster.

    ![Voer de naam van app](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Selecteer **vooraf gedefinieerde domeinen** in de linkerbenedenhoek. 

    ![Vooraf gedefinieerde domein selecteren](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Selecteer **domein toevoegen** voor HomeAutomation.

    ![HomeAutomation domein toevoegen](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Selecteer **Train** in de juiste navigatiebalk bovenaan.

    ![Selecteer de knop Train](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Criteria voor batch-test
Testen van batch kunt maximaal 1000 utterances testen op een tijdstip. De batch mag geen dubbele waarden. [Exporteren](create-new-app.md#export-app) de app om te zien van de lijst met huidige utterances.  

De test-strategie voor LUIS maakt gebruik van drie afzonderlijke gegevenssets: model utterances, batch test utterances en utterances eindpunt. Zorg ervoor dat u niet de utterances van beide model utterances (toegevoegd aan de opzet), of een eindpunt utterances voor deze zelfstudie. 

Gebruik niet een van de utterances al in de app voor de batch-test:

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

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Maak een batch opzet nauwkeurigheid testen
1. Maak `homeauto-batch-1.json` in een teksteditor zoals [VSCode](https://code.visualstudio.com/). 

2. Toevoegen van utterances met de **bedoeling** gewenste voorspelde in de test. Voor deze zelfstudie, u kunt het eenvoudig door utterances duren in de `HomeAutomation.TurnOn` en `HomeAutomation.TurnOff` en schakel de `on` en `off` tekst in de utterances. Voor de `None` hetzelfde doel toevoegen van een aantal utterances die geen deel uit van de [domein](luis-glossary.md#domain) (onderwerp). 

    Toevoegen om te begrijpen hoe de testresultaten batch correleert met de batch JSON, alleen zes intents.

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

## <a name="run-the-batch"></a>De batch uitvoeren
1. Selecteer **Test** in de bovenste navigatiebalk. 

    ![Selecteer de Test in de navigatiebalk](./media/luis-tutorial-batch-testing/test-1.png)

2. Selecteer **Batch testen Configuratiescherm** in het paneel aan de rechterkant. 

    ![Selecteer Batch test deelvenster](./media/luis-tutorial-batch-testing/test-2.png)

3. Selecteer **importeren gegevensset**.

    ![Selecteer de gegevensset importeren](./media/luis-tutorial-batch-testing/test-3.png)

4. Kies de locatie van de `homeauto-batch-1.json` bestand.

5. Naam van de gegevensset `set 1`.

    ![Bestand selecteren](./media/luis-tutorial-batch-testing/test-4.png)

6. Selecteer de knop **Run**. Wacht totdat de test is voltooid.

    ![Selecteer uitvoeren](./media/luis-tutorial-batch-testing/test-5.png)

7. Selecteer **resultaten**.

    ![Resultaten te zien](./media/luis-tutorial-batch-testing/test-6.png)

8. Bekijk de resultaten in de grafiek en de legenda.

    ![Batch-resultaten](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Batch-resultaten bekijken
De resultaten van de batch zijn in twee secties. Het bovenste gedeelte bevat de grafiek en de legenda. Het onderste gedeelte wordt utterances wanneer u een naam van de grafiek selecteert.

Eventuele fouten worden aangeduid met de kleur rood. De grafiek wordt in vier secties, met twee van de secties in rood weergegeven. **Dit zijn de secties zich te concentreren op**. 

De rechterbovenhoek sectie geeft aan de test onjuist voorspelde het bestaan van een doel of de entiteit. Het linkergedeelte onder geeft aan dat de test voorspeld onjuist het ontbreken van een doel of de entiteit.

### <a name="homeautomationturnoff-test-results"></a>HomeAutomation.TurnOff testresultaten
Selecteer in de legenda de `HomeAutomation.TurnOff` bedoeld. Heeft een pictogram groen succes aan de linkerkant van de naam in de legenda. Er zijn geen fouten voor dit doel. 

![Batch-resultaten](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn en er is geen intents bevatten fouten
De twee intents hebt fouten, wat betekent dat de voorspellingen test niet overeenkomen met de batch-bestand. Selecteer de `None` opzet in de legenda om te controleren van de eerste fout. 

![Geen opzet](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Fouten worden weergegeven in de grafiek in de secties rode: **onjuiste positieve** en **False negatieve**. Selecteer de **False negatieve** sectienaam in de grafiek zien van de mislukte utterances onder de grafiek. 

![ONWAAR negatieve fouten](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

De utterance mislukt `help` werd verwacht als een `None` bedoelingen, maar de test voorspeld `HomeAutomation.TurnOn` bedoeld.  

Er zijn twee fouten, één in HomeAutomation.TurnOn en één in None. Beide zijn veroorzaakt door de utterance `help` omdat het niet voldeed aan de verwachting geen en dit een onverwachte overeenkomst voor de bedoeling HomeAutomation.TurnOn is. 

Om te bepalen waarom de `None` utterances mislukken, controleert u de utterances in `None`. 

## <a name="review-none-intents-utterances"></a>Bekijk geen opzet utterances de

1. Sluit de **Test** Configuratiescherm door het selecteren van de **Test** knop op de bovenste navigatiebalk. 

2. Selecteer **bouwen** van het bovenste navigatievenster. 

3. Selecteer **geen** opzet uit de lijst met intents.

4. Besturingselement + E om te zien van de token weergave van de utterances selecteren 
    
    |Geen opzet's utterances|Voorspelling score|
    |--|--|
    |'verkleinen temperatuur voor mij Neem'|0.44|
    |"dim keuken lichten 25."|0,43|
    |'het volume verlagen'|0.46|
    |"internet in mijn Raadpleeg kamer uw inschakelen"|0,28|

## <a name="fix-none-intents-utterances"></a>Herstel geen opzet utterances de
    
Alle utterances in `None` zijn buiten de app-domein. Deze utterances zijn ten opzichte van HomeAutomation, zodat ze in de verkeerde bedoelingen. 

LUIS biedt ook de utterances op die kleiner is dan 50% (<.50) voorspelling score. Als u de utterances in de andere twee intents bekijkt, ziet u veel hoger voorspelling scores. Als LUIS lage scores voor voorbeeld utterances, die een goede indicatie zijn de utterances verwarring opleveren voor LUIS tussen het huidige doel en andere intents. 

Oplossen van de app, de utterances geopend in de `None` bedoeling moeten worden verplaatst naar het juiste doel en de `None` doel moet een nieuwe, juiste intents. 

Drie van de utterances in de `None` bedoeld zijn bedoeld om het verlagen van de instellingen van het automation-apparaat. Ze gebruiken woorden zoals `dim`, `lower`, of `decrease`. De vierde utterance wordt u gevraagd om in te schakelen op het internet. Aangezien alle vier utterances over het inschakelen van of de mate van power wijzigen op een apparaat, moeten u deze verplaatst naar de `HomeAutomation.TurnOn` bedoeld. 

Dit is slechts één oplossing. U kunt ook een nieuwe bedoeling van maken `ChangeSetting` en verplaatsen van de utterances met dim, verlagen en verlagen naar die nieuwe intentie. 

## <a name="fix-the-app-based-on-batch-results"></a>Los van de app op basis van batch-resultaten
Verplaatsen van de vier utterances naar de `HomeAutomation.TurnOn` bedoeld. 

1. Schakel het selectievakje boven de lijst utterance zodat alle utterances zijn geselecteerd. 

2. In de **opnieuw toewijzen bedoeling** vervolgkeuzelijst, selecteer `HomeAutomation.TurnOn`. 

    ![Utterances verplaatsen](./media/luis-tutorial-batch-testing/move-utterances.png)

    Nadat de vier utterances opnieuw zijn toegewezen, de utterance lijst voor de `None` bedoeling is leeg.

3. Vier nieuwe intents voor de opzet geen toevoegen:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Deze utterances zijn zeker buiten het domein van HomeAutomation. Terwijl u elke utterance invoert, bekijkt u de score voor. De score zijn lage of zelfs zeer lage (met een rood kader omheen). Nadat het trainen van de app, klikt u in stap 8, wordt de score veel hoger zijn. 

7. Labels verwijderen door de blauwe label in het utterance en selecteer **label verwijderen**.

8. Selecteer **Train** in de juiste navigatiebalk bovenaan. De score van elke utterance is veel hoger. Alle scores voor de `None` bedoeling nu hoger.80 zijn. 

## <a name="verify-the-fix-worked"></a>Controleren of het probleem heeft gewerkt
Om te controleren dat de utterances in de batch-test correct worden voorspeld voor de **geen** hetzelfde doel de test van de batch opnieuw uitvoeren.

1. Selecteer **Test** in de bovenste navigatiebalk. 

2. Selecteer **Batch testen Configuratiescherm** in het paneel aan de rechterkant. 

3. Selecteer de drie puntjes (...) rechts van de batchnaam van de en selecteer **gegevensset uitgevoerd**. Wacht totdat de batch-test is voltooid.

    ![Dataset uitvoeren](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Selecteer **resultaten**. De intents moeten alle groen pictogrammen aan de linkerkant van de opzet namen hebben. Met het juiste filter ingesteld op de `HomeAutomation.Turnoff` bedoelingen, selecteer de groene punt in het bovenste rechterpaneel die het dichtst bij het midden van de grafiek. De naam van de utterance wordt weergegeven in de tabel onder de grafiek. De score van `breezeway off please` zeer laag. Een optionele activiteit is meer utterances toevoegen aan de bedoeling deze score verhogen. 

    ![Dataset uitvoeren](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

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

4. If the list of datasets is not visible, select **Back to list**. Select the three dots (...) at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over voorbeeld utterances](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions