---
title: Zelfstudie over het beoordelen van eindpuntuitingen in Language Understanding (LUIS) - Azure | Microsoft Docs
description: In deze zelfstudie leert u eindpuntuitingen in het domein Human Resources (HR) te beoordelen voor LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: diberry
ms.openlocfilehash: a04c8987bc2e16a41196286e3260b4b7ec11f3c4
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39357838"
---
# <a name="tutorial-review-endpoint-utterances"></a>Zelfstudie: Eindpuntuitingen beoordelen
In deze zelfstudie leert u de voorspellingen van de app te verbeteren door de uitingen die worden ontvangen via het LUIS-HTTP-eindpunt te controleren of corrigeren. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Inzicht krijgen in de beoordeling van eindpuntuitingen 
> * De LUIS-app gebruiken voor het domein Human Resources (HR) 
> * Eindpuntuitingen controleren
> * App trainen en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u nog niet beschikt uit de Human Resources-app uit de zelfstudie over [sentiment](luis-quickstart-intent-and-sentiment-analysis.md), kunt u de app importeren uit de Github-opslagplaats [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-sentiment-HumanResources.json). Als u deze zelfstudie doet met een nieuwe, geïmporteerde app, moet u die ook trainen en publiceren en vervolgens de uitingen toevoegen aan het eindpunt met een [script](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) of via het eindpunt in een browser. U dient deze uitingen toe te voegen:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `review`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

Als u alle versies van de app hebt, omdat u de hele reeks zelfstudies hebt gevolgd, zal het u wellicht verrassen dat de lijst **Eindpuntuitingen beoordelen** niet per versie verschilt. Er is één groep uitingen om te beoordelen, ongeacht welke versie van de uiting u actief bewerkt of welke versie van de app wordt gepubliceerd op het eindpunt. 

## <a name="purpose-of-reviewing-endpoint-utterances"></a>Het doel van de beoordeling van eindpuntuitingen
Dit beoordelingsproces is een andere manier waarop LUIS meer over uw app-domein kan leren. De uitingen zijn door LUIS geselecteerd in de beoordelingslijst. Deze lijst:

* is specifiek voor deze app;
* is bedoeld om de nauwkeurigheid van de app te verbeteren; 
* dient op periodieke basis te worden gecontroleerd. 

Door de eindpuntuitingen te boordelen, kunt u de voorspelde intenties controleren of corrigeren. U kunt ook aangepaste entiteiten die niet zijn voorspeld van een label voorzien. 

## <a name="review-endpoint-utterances"></a>Eindpuntuitingen controleren

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

2. Selecteer in de linkernavigatiebalk **Eindpuntuitingen beoordelen**. De lijst is gefilterd voor de intentie **ApplyForJob**. 

    [ ![Schermafbeelding van de knop Eindpuntuitingen beoordelen in de navigatiebalk aan de linkerkant](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png)](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png#lightbox)

3. Schakelen tussen de **Weergave Entiteiten** om de entiteiten met labels weer te geven. 
    
    [ ![Schermafbeelding van het paneel Eindpuntuitingen beoordelen met de wisselknop Entiteiten weergeven gemarkeerd](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png#lightbox)

    |Utterance|Correcte intentie|Ontbrekende entiteiten|
    |:--|:--|:--|
    |Ik ben op zoek naar een taak met natuurlijke taalverwerking|GetJobInfo|Taak - 'Natuurlijke taalverwerking'|

    Deze uiting is niet ingedeeld onder de juiste intentie en heeft een score lager dan 50%. De intentie **ApplyForJob** bevat 21 uitingen, terwijl **GetJobInformation** zeven uitingen bevat. Nu dient dus eerst de eindpuntuiting correct te worden gekoppeld, maar er moeten ook meer uitingen worden toegevoegd aan de intentie **GetJobInformation**. Deze stappen kunt u bij wijze van oefening zelf uitvoeren. Alle intenties, met uitzondering van de intentie **None**, zouden ongeveer hetzelfde aantal voorbeelduitingen moeten bevatten. De intentie **None** zou 10% van het totale aantal uitingen in de app moeten bevatten. 

    Wanneer u zich in de **weergave Tokens** bevindt, kunt u de muisaanwijzer boven blauwe tekst houden om de naam van de voorspelde entiteit weer te geven. 

4. Selecteer voor de intentie `I'm looking for a job with Natual Language Processing` de correcte intentie, **GetJobInformation**, in de kolom **Uitgelijnde intentie** kolom. 

    [ ![Schermafbeelding van het uitlijnen van uitingen met intenties in het venster Eindpuntuitingen beoordelen](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

5. In de dezelfde uiting is de entiteit voor `Natural Language Processing` keyPhrase. Dit zou eigenlijk de entiteit **Job** moeten zijn. Selecteer `Natural Language Processing` en selecteer vervolgens in de lijst de entiteit **Job**.

    [ ![Schermafbeelding van het toepassen van entiteitslabels op een uiting in het venster Eindpuntuitingen beoordelen](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

6. Selecteer op dezelfde regel het omcirkelde vinkje in de kolom **Toevoegen aan uitgelijnde intentie**. 

    [ ![Schermafbeelding van het voltooien van de uitlijning van een uiting met een intentie](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Met deze actie verplaatst u de uiting van **Eindpuntuitingen beoordelen** naar de intentie **GetJobInformation**. De eindpuntuiting is nu een voorbeelduiting voor deze intentie. 

7. Beoordeel de resterende uitingen in deze intentie, waarbij u de uitingen van een label kunt voorzien en de **Uitgelijnde intentie** kunt corrigeren als deze onjuist zijn.

8. Selecteer, wanneer alle uitingen correct zijn, het selectievakje op iedere rij en selecteer vervolgens **Selectie toevoegen** om de uitingen correct uit te lijnen. 

    [ ![Schermafbeelding van het voltooien van de resterende uitingen naar de uitgelijnde intentie](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

9. Deze uitingen zouden nu niet meer op de lijst moeten staan. Als meer uitingen worden weergegeven, dient u de lijst af te blijven gaan, de intenties waar nodig te corrigeren en eventuele ontbrekende entiteiten van een label te voorzien, tot de lijst leeg is. Selecteer de volgende intentie in de filterlijst en blijf vervolgens uitingen corrigeren en entiteiten van labels voorzien. Vergeet niet voor iedere intentie als laatste stap ofwel **Toevoegen aan uitgelijnde intentie** te selecteren op de rij van die uiting of het selectievakje te selecteren bij die intentie en boven de tabel op **Selectie toevoegen** te klikken. 

    Dit is een hele kleine app. Het beoordelingsproces duurt slechts een paar minuten.

## <a name="add-new-job-name-to-phrase-list"></a>Een nieuwe taaknaam toevoegen aan de woordgroepenlijst
Houd de woordgroepenlijst actueel door alle nieuwe gedetecteerde taaknamen toe te voegen. 

1. Selecteer **Woordgroepenlijsten** in de linkernavigatiebalk.

2. Selecteer de woordgroepenlijst **Taken**.

3. Voeg `Natural Language Processing` toe als een waarde en selecteer vervolgens **Opslaan**. 

## <a name="train-the-luis-app"></a>LUIS-app trainen

LUIS is niet op de hoogte van eventuele wijzigingen totdat deze wordt getraind. 

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen

Als u deze app hebt geïmporteerd, dient u **Sentimentanalyse** te selecteren.

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

Probeer een uiting die lijkt op de gecorrigeerde uiting. 

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `Are there any natural language processing jobs in my department right now?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

  ```JSON
  {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
  }
  }
  ```

  De juiste intentie is voorspeld met een hoge score en de entiteit **Job** is gedetecteerd als `natural language processing`. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Kan de beoordeling worden vervangen door meer uitingen toe te voegen? 
U vraagt zich misschien af waarom we niet meer voorbeelduitingen toevoegen. Wat is het doel van de beoordeling van eindpuntuitingen? In een echte LUIS-app zijn de eindpuntuitingen van gebruikers met een woordkeuze en indeling die u nog niet hebt gebruikt. Als u de dezelfde woordkeuze en rangschikking had gebruikt, zou de oorspronkelijke voorspelling een hoger percentage hebben gehad. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>Waarom staat de bovenste intentie in de lijst met uitingen? 
Sommige eindpuntuitingen hebben een hoog percentage in de beoordelingslijst. U dient deze uitingen alsnog te beoordelen en verifiëren. Ze zijn opgenomen in de lijst omdat de beoordeling van de tweede intentie een score had die te dicht lag bij de score van de hoogst scorende intentie. 

## <a name="what-has-this-tutorial-accomplished"></a>Wat is er in deze zelfstudie bereikt?
De nauwkeurigheid van de voorspellingen van deze app is verbeterd door eindpuntuitingen te beoordelen. 

## <a name="clean-up-resources"></a>Resources opschonen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het gebruik van patronen](luis-tutorial-pattern.md)
