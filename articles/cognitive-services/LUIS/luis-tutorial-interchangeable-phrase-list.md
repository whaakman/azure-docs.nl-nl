---
title: Zelfstudie over het gebruiken van een lijst zin voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
description: In deze zelfstudie een woordgroepenlijst toevoegen aan een LUIS-app en het verbeteren van de score te zien.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 9f12d9e8c9ee2038e7841cd05bb438421a5a8984
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345342"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Zelfstudie: Woordgroepenlijst ter verbetering van voorspellingen toevoegen
In deze zelfstudie, de nauwkeurigheid van intentie scores verbeteren en identificeren van entiteiten voor woorden die beschikken over dezelfde betekenis (synoniemen) met het toevoegen van een verwisselbaar [woordgroep lijstfunctie](./luis-concept-feature.md).

> [!div class="checklist"]
* Een nieuwe app importeren  
* Query-eindpunt met de bekende utterance 
* Query-eindpunt met _onbekende_ utterance
* Woordgroepenlijst met ter verbetering van onbekende utterance score toevoegen
* Controleer of de entiteit is gevonden bij het gebruik van de woordgroepenlijst met

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="import-a-new-app"></a>Een nieuwe app importeren
1. Download de [voorbeeld LUIS-app] [ LuisSampleApp] dat ontworpen voor deze zelfstudie. U gebruikt deze in de volgende stap. 

2. Zoals beschreven in [maken van een app](Create-new-app.md#import-new-app), importeert u het bestand dat u hebt gedownload in de [LUIS] [ LUIS] website als een nieuwe app. Naam van de app is 'Mijn woordgroepenlijst zelfstudie'. Intenties en entiteiten uitingen heeft. 

3. [Train](luis-how-to-train.md) uw app. Totdat deze is getraind, u kunt geen [interactief testen](interactive-test.md#interactive-testing) in de [LUIS] [ LUIS] website. 

4. Op de [publiceren](luis-how-to-publish-app.md) weergeeft, schakelt de **inclusief alle voorspelde scores intentie** selectievakje. Als het selectievakje is ingeschakeld, worden alle intents worden geretourneerd. Wanneer het selectievakje is uitgeschakeld, wordt alleen het belangrijkste doel geretourneerd. 

5. [Publiceren](luis-how-to-publish-app.md) de app. De app publiceert, kunt u om dit te testen met behulp van het HTTPS-eindpunt. 

## <a name="test-a-trained-utterance"></a>Testen van een getraind utterance
Het gepubliceerde-eindpunt gebruiken om op te vragen een utterance die de app al kent. Omdat LUIS al de utterance kent, de score is hoog en de entiteit wordt gedetecteerd.

1. Op de [Language Understanding (LUIS)] [ LUIS] website op de **publiceren** pagina voor de nieuwe app, selecteert u de eindpunt-URL in de **Resources en sleutels**sectie. 

    ![Publiceren van de eindpunt-URL](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. In de browser aan het einde van de URL toevoegen de volgende query uit na het `q=`.

    `I want a computer replacement`

    Het eindpunt reageert met de volgende JSON:
    
    ```JSON
    {
      "query": "I want a computer replacement",
      "topScoringIntent": {
        "intent": "GetHardware",
        "score": 0.9735458
      },
      "intents": [
        {
          "intent": "GetHardware",
          "score": 0.9735458
        },
        {
          "intent": "None",
          "score": 0.07053033
        },
        {
          "intent": "Whois",
          "score": 0.03760778
        },
        {
          "intent": "CheckCalendar",
          "score": 0.02285902
        },
        {
          "intent": "CheckInventory",
          "score": 0.0110072717
        }
      ],
      "entities": [
        {
          "entity": "computer",
          "type": "Hardware",
          "startIndex": 9,
          "endIndex": 16,
          "score": 0.8465915
        }
      ]
    }
    ```

    De intentie score van 0.973 en de score van de detectie van entiteit van 0.846 zijn hoge omdat de app is getraind met deze utterance. De utterance is in de LUIS-app op de intentie-pagina voor **GetHardware**. Tekst van de utterance `computer`, wordt aangeduid als de **Hardware** entiteit. 
    
    |Status|Word| Intentie score | Entiteit score |
    |--|--|--|--|
    |Getraind| gewenste | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Een ongetrainde utterance testen
Gebruik hetzelfde gepubliceerde eindpunt aan query met een utterance waarmee de app niet al in de browser:

`I require a computer replacement`

Deze utterance maakt gebruik van een synoniem van de vorige utterance:

| Getrainde word | Ongetrainde synoniem |
|--|--|
| gewenste | vereisen |

De eindpunt-antwoord is:

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.840912163
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.840912163
    },
    {
      "intent": "None",
      "score": 0.0972757638
    },
    {
      "intent": "Whois",
      "score": 0.0448251367
    },
    {
      "intent": "CheckCalendar",
      "score": 0.0291390456
    },
    {
      "intent": "CheckInventory",
      "score": 0.0137837809
    }
  ],
  "entities": []
}
```

| Status | Word | Intentie score | Entiteit score |
|--|--|--|--|
| Getraind| gewenste | 0.973 | 0.846 |
| Ongetrainde| vereisen | 0.840 | - |

De intentie score ongetrainde utterance is lager dan die van de gelabelde utterance omdat LUIS weet dat de zin grammaticaal hetzelfde is. Maar LUIS niet weet dat de uitingen dezelfde betekenis hebben. Ook, zonder de woordgroepenlijst de **Hardware** entiteit is niet gevonden.

U moet LUIS die leren *wilt* en *vereisen* dezelfde betekenis in het domein van deze app omdat een woord kan meer dan één betekenis hebben. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>De score van ongetrainde utterance woordgroep lijst verbeteren 
1. Voeg een [woordgroepenlijst](luis-how-to-add-features.md) functie met de naam **wilt** met de waarde van `want`, en selecteer vervolgens **Enter**.

    > [!TIP]
    > Elke woord of woordgroep, selecteer de **Enter** sleutel. Het woord of woordgroep wordt toegevoegd aan de **woordgroep lijstwaarden** vak terwijl de cursor blijft de **waarde** vak. U kunt veel waarden snel met deze functie kunt invoeren.

2. Als u de woorden die LUIS wordt aanbevolen, selecteer **raden**. 

    ![Aanbevolen waarden](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Alle woorden toevoegen. Als `require` is niet in de lijst met aanbevolen, dit toevoegen als een vereiste waarde. 

4. Omdat deze woorden synoniemen zijn, behouden de *uitwisselbaar* instellen en selecteer vervolgens **opslaan**.

    ![Woordgroep lijstwaarden](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Selecteer in de bovenste navigatiebalk **trainen** met het trainen van de app, maar deze niet publiceren. U hebt nu twee modellen. U kunt waarden in de twee modellen kunt vergelijken.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Het model woordgroep lijst naar de gepubliceerde model vergelijken
In deze app, het gepubliceerde model niet is getraind met de synoniemen. Alleen het momenteel wordt bewerkt model bevat de woordgroepenlijst van synoniemen. Als u wilt vergelijken de modellen, gebruikt u [interactieve testen](interactive-test.md#interactive-testing). 

1. Open de **Test** in het deelvenster en voer de volgende utterance:

    `I require a computer replacement`

2. Selecteer om te openen in het deelvenster controle, **inspecteren**. 

    ![Selecteer controleren](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Als u wilt vergelijken van het gepubliceerde model naar het nieuwe model van de woordgroep-lijst, selecteer **vergelijken met gepubliceerd**.

    ![Controleer ten opzichte van de huidige gepubliceerd](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Nadat u de woordgroepenlijst, de betere nauwkeurigheid van de utterance hebt toegevoegd en de **Hardware** entiteit is gevonden. 

|Status | Woordgroepenlijst met| Intentie score | Entiteit score |
|--|--|--|--|
| Gepubliceerd | - | 0,84 | - |
| Wordt bewerkt |✔| 0.92 | Hardware-entiteit geïdentificeerd |

> [!TIP]
> * Met behulp van [interactieve testen](interactive-test.md#interactive-testing), kunt u vergelijken met het gepubliceerde model getrainde wijzigingen die zijn aangebracht nadat u hebt gepubliceerd. 
> * Met behulp van [eindpunt testen](luis-how-to-publish-app.md#test-your-published-endpoint-in-a-browser), kunt u de daadwerkelijke reactie van LUIS JSON bekijken. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>De entiteit-score ophalen met de eindpunt-test
Om de score entiteit weer te geven [publiceer het model](luis-how-to-publish-app.md) en het eindpunt op te vragen. 

`I require a computer replacement`

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.916503668
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.916503668
    },
    {
      "intent": "None",
      "score": 0.136505231
    },
    {
      "intent": "Whois",
      "score": 0.02778677
    },
    {
      "intent": "CheckInventory",
      "score": 0.0144592477
    },
    {
      "intent": "CheckCalendar",
      "score": 0.01401332
    }
  ],
  "entities": [
    {
      "entity": "computer",
      "type": "Hardware",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.5959917
    }
  ]
}
```

De **Hardware** entiteit bevat een score van 0.595 met de woordgroepenlijst met. Voordat u de woordgroepenlijst met bestond, wordt de entiteit is niet gevonden. 

|Status | Woordgroepenlijst met| Intentie score | Entiteit score |
|--|--|--|--|
| Gepubliceerd | - | 0,84 | - |
| Wordt bewerkt |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voorspelling van utterance met eindpunt query ophalen](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
