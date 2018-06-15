---
title: Met behulp van een lijst van de wachtwoordzin op te geven voor het verbeteren van LUIS voorspellingen - Azure zelfstudie | Microsoft Docs
description: In deze zelfstudie maakt een woordgroepenlijst toevoegen aan een app LUIS en het verbeteren van de score zien.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 4ced7bcec87a9edde2e3ded8c8c61abe96003572
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345962"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Zelfstudie: Woordgroepenlijst ter verbetering van de voorspellingen toevoegen
In deze zelfstudie, de nauwkeurigheid van opzet scores verbeteren en identificeren van entiteiten voor de woorden die dezelfde betekenis (synoniemen hebben) door het toevoegen van een verwisselbaar [woordgroep lijstfunctie](./luis-concept-feature.md).

> [!div class="checklist"]
* Een nieuwe app importeren  
* Query-eindpunt met bekende utterance 
* Query-eindpunt met _onbekende_ utterance
* Lijst van de wachtwoordzin op te geven ter verbetering van de onbekende utterance score toevoegen
* Controleer of de entiteit wordt gevonden wanneer met behulp van woordgroepenlijst

Voor dit artikel, moet u een gratis [LUIS] [ LUIS] account om uw toepassing LUIS ontwerpen.

## <a name="import-a-new-app"></a>Een nieuwe app importeren
1. Download de [voorbeeld LUIS app] [ LuisSampleApp] die ontworpen voor deze zelfstudie. U gebruikt deze in de volgende stap. 

2. Zoals beschreven in [maken van een app](Create-new-app.md#import-new-app), importeer het bestand dat u gedownload naar de [LUIS] [ LUIS] website als een nieuwe app. De appnaam van de is 'Mijn woordgroepenlijst zelfstudie'. Intents en entiteiten utterances heeft. 

3. [Train](luis-how-to-train.md) uw app. Totdat deze is getraind, u kunt geen [interactief testen](Train-Test.md#interactive-testing) in de [LUIS] [ LUIS] website. 

4. Op de [publiceren](PublishApp.md) pagina de **opnemen alle voorspelde scores opzet** selectievakje. Wanneer het selectievakje is ingeschakeld, worden alle intents geretourneerd. Wanneer het selectievakje is uitgeschakeld, wordt alleen de bovenste bedoeling geretourneerd. 

5. [Publiceren](PublishApp.md) de app. De app publiceert, kunt u testen met behulp van het HTTPS-eindpunt. 

## <a name="test-a-trained-utterance"></a>Een getraind utterance testen
Gebruik het gepubliceerde eindpunt query uitvoeren op een utterance dat de app al kent. Omdat LUIS al de utterance kent, de score is hoog en de entiteit wordt gedetecteerd.

1. Op de [Language Understanding (LUIS)] [ LUIS] website op de **publiceren** pagina voor de nieuwe app, selecteert u de eindpunt-URL in de **Resources en sleutels**sectie. 

    ![Publiceren van de eindpunt-URL](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. Toevoegen aan het einde van de URL in de browser de volgende query na de `q=`.

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

    De opzet score van 0.973 en de score van de detectie van entiteit van 0.846 zijn hoog omdat de app is getraind met deze utterance. De utterance is in de app LUIS op de pagina opzet voor **GetHardware**. Tekst van de utterance, `computer`, wordt aangeduid als de **Hardware** entiteit. 
    
    |Status|Word| Opzet score | Entiteit score |
    |--|--|--|--|
    |Getraind| gewenste | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Een ongetrainde utterance testen
In de browser, gebruikt u hetzelfde gepubliceerde eindpunt aan query met een utterance die de app niet al bekend is:

`I require a computer replacement`

Deze utterance maakt gebruik van een synoniem van de vorige utterance:

| Getraind word | Ongetrainde synoniem |
|--|--|
| gewenste | Vereisen |

Het antwoord van het eindpunt is:

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

| Status | Word | Opzet score | Entiteit score |
|--|--|--|--|
| Getraind| gewenste | 0.973 | 0.846 |
| Ongetrainde| Vereisen | 0.840 | - |

De opzet score ongetrainde utterance is lager dan die van de gelabelde utterance omdat LUIS weet dat de zin grammaticaal hetzelfde is. Maar LUIS niet weet dat de utterances dezelfde betekenis hebben. Ook zonder de woordgroepenlijst de **Hardware** entiteit is niet gevonden.

U moet leren gebruiken LUIS die *wilt* en *vereisen* dezelfde betekenis in dit app domain omdat een woord kan meer dan één betekenis hebben. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>De score van ongetrainde utterance woordgroep lijst verbeteren 
1. Voeg een [woordgroepenlijst](luis-how-to-add-features.md) functie **wilt** met de waarde van `want`, en selecteer vervolgens **Enter**.

    > [!TIP]
    > Elk woord of woordgroep, selecteer de **Enter** sleutel. Het woord of zinsdeel wordt toegevoegd aan de **woorden lijstwaarden** vak terwijl de cursor blijft de **waarde** vak. U kunt meerdere waarden snel met deze functie kunt invoeren.

2. Als u wilt de woorden die LUIS aanbeveelt weergeven, selecteert u **raden**. 

    ![Aanbevolen waarden](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. De woorden toevoegen. Als `require` is niet in de aanbevolen lijst toevoegen als een vereiste waarde. 

4. Omdat deze woorden synoniemen, blijven de *uitwisselbaar* instelling en selecteer vervolgens **opslaan**.

    ![De lijstwaarden woordgroep](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Selecteer in de bovenste navigatiebalk **trainen** voor het trainen van de app, maar deze niet publiceren. U hebt nu twee modellen. U kunt waarden in de twee modellen vergelijken.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Het model woordgroep lijst naar de gepubliceerde model vergelijken
In deze app is niet het gepubliceerde model getraind met de synoniemen. Alleen het momenteel bewerkte model bevat de woordgroep synoniemen. Met kunt u de modellen vergelijken [interactieve testen](Train-Test.md#interactive-testing). 

1. Open de **Test** deelvenster en voer de volgende utterance:

    `I require a computer replacement`

2. De controle om Configuratiescherm te openen, selecteer **inspecteren**. 

    ![Selecteer controleren](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Als u wilt vergelijken in het gepubliceerde model naar het nieuwe model van de woordgroep-lijst, selecteert u **vergelijken met gepubliceerd**.

    ![Controleer ten opzichte van de huidige gepubliceerd](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Nadat u de woordgroepenlijst, de toegenomen nauwkeurigheid van de utterance toevoegen en de **Hardware** entiteit is gevonden. 

|Status | Lijst van de wachtwoordzin op te geven| Opzet score | Entiteit score |
|--|--|--|--|
| Gepubliceerd | - | 0,84 | - |
| Momenteel bewerken |✔| 0.92 | Hardware-entiteit geïdentificeerd |

> [!TIP]
> * Met behulp van [interactieve testen](Train-Test.md#interactive-testing), u kunt vergelijken met het gepubliceerde model om te trained wijzigingen die zijn aangebracht nadat u hebt gepubliceerd. 
> * Met behulp van [eindpunt testen](PublishApp.md#test-your-published-endpoint-in-a-browser), kunt u de daadwerkelijke LUIS reactie JSON bekijken. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>De score van de entiteit ophalen met de eindpunt-test
Om de score entiteit weer te geven [publiceren van het model](PublishApp.md) en het eindpunt van een query. 

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

De **Hardware** entiteit toont een score van 0.595 met de lijst wachtwoordzin op te geven. Voordat u de woordgroepenlijst bestond, wordt de entiteit is niet gedetecteerd. 

|Status | Lijst van de wachtwoordzin op te geven| Opzet score | Entiteit score |
|--|--|--|--|
| Gepubliceerd | - | 0,84 | - |
| Momenteel bewerken |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet langer nodig is, verwijdert u de app LUIS. Om dit te doen, selecteert u het menu drie punt (...) rechts van de naam van de app in de lijst met Apps, selecteer **verwijderen**. In het pop-updialoogvenster **app verwijderen?**, selecteer **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Utterance voorspelling met eindpunt query ophalen](luis-get-started-cs-get-intent.md)

[LUIS]: luis-reference-regions.md

  [LUIS]:luis-reference-regions.md
  [LuisFeatures]: luis-concept-feature.md
  [LuisSampleApp]:https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
