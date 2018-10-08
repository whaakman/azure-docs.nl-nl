---
title: 'Zelfstudie 1: eindpuntuitingen controleren middels actief leren'
titleSuffix: Azure Cognitive Services
description: Verbeter de voorspellingen van de app door de uitingen die worden ontvangen via het LUIS-HTTP-eindpunt (en waar LUIS niet zeker over is) te controleren of corrigeren. Bij sommige uitingen moet mogelijk de intentie worden gecontroleerd en bij andere de entiteit. Controleer de eindpuntuitingen regelmatig als onderdeel van uw geplande LUIS-onderhoud.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 1047c117228b57f7361a1e386bc6cde7acbfdde8
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042264"
---
# <a name="tutorial-1-fix-unsure-predictions"></a>Zelfstudie 1: onzekere voorspellingen herstellen
In deze zelfstudie leert u de voorspellingen van de app te verbeteren door de uitingen die worden ontvangen via het LUIS-HTTP-eindpunt (en waar LUIS niet zeker over is) te controleren of corrigeren. Bij sommige uitingen moet mogelijk de intentie worden gecontroleerd en bij andere de entiteit. Controleer de eindpuntuitingen regelmatig als onderdeel van uw geplande LUIS-onderhoud. 

Dit beoordelingsproces is een andere manier waarop LUIS meer over uw app-domein kan leren. De uitingen die in de beoordelingslijst worden weergegeven, zijn door LUIS geselecteerd. Deze lijst:

* is specifiek voor deze app;
* is bedoeld om de nauwkeurigheid van de app te verbeteren; 
* dient op periodieke basis te worden gecontroleerd. 

Door de eindpuntuitingen te boordelen, kunt u de voorspelde intenties controleren of corrigeren. U kunt ook aangepaste entiteiten die niet of onjuist zijn voorspeld van een label voorzien. 

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * Eindpuntuitingen controleren
> * Frasenlijst bijwerken
> * App trainen
> * App publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken

Ga door met de in de laatste zelfstudie gemaakt app, **HumanResources**. 

Als u niet over de app HumanResources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `review`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.

    Als u deze zelfstudie doet met een nieuwe, geïmporteerde app, moet u die ook trainen en publiceren en vervolgens de uitingen toevoegen aan het eindpunt met een [script](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) of via het eindpunt in een browser. U dient deze uitingen toe te voegen:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    Als u alle versies van de app hebt, omdat u de hele reeks zelfstudies hebt gevolgd, zal het u wellicht verrassen dat de lijst **Eindpuntuitingen beoordelen** niet per versie verschilt. Er is één groep uitingen om te beoordelen, ongeacht welke versie u actief bewerkt of welke versie van de app wordt gepubliceerd op het eindpunt. 

## <a name="review-endpoint-utterances"></a>Eindpuntuitingen controleren

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer in de linkernavigatiebalk **Eindpuntuitingen beoordelen**. De lijst is gefilterd voor de intentie **ApplyForJob**. 

    [ ![Schermafbeelding van de knop Eindpuntuitingen beoordelen in de navigatiebalk aan de linkerkant](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

3. Schakelen tussen de **Weergave Entiteiten** om de entiteiten met labels weer te geven. 
    
    [ ![Schermafbeelding van het paneel Eindpuntuitingen beoordelen met de wisselknop Entiteiten weergeven gemarkeerd](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)

    |Utterance|Correcte intentie|Ontbrekende entiteiten|
    |:--|:--|:--|
    |Ik ben op zoek naar een taak met natuurlijke taalverwerking|GetJobInfo|Taak - 'Natuurlijke taalverwerking'|

    Deze uiting is niet ingedeeld onder de juiste intentie en heeft een score lager dan 50%. De intentie **ApplyForJob** bevat 21 uitingen, terwijl **GetJobInformation** zeven uitingen bevat. Nu dient dus eerst de eindpuntuiting correct te worden gekoppeld, maar er moeten ook meer uitingen worden toegevoegd aan de intentie **GetJobInformation**. Deze stappen kunt u bij wijze van oefening zelf uitvoeren. Alle intenties, met uitzondering van de intentie **None**, zouden ongeveer hetzelfde aantal voorbeelduitingen moeten bevatten. De intentie **None** zou 10% van het totale aantal uitingen in de app moeten bevatten. 

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

9. Deze uitingen zouden nu niet meer op de lijst moeten staan. Als meer uitingen worden weergegeven, dient u de lijst af te blijven gaan, de intenties waar nodig te corrigeren en eventuele ontbrekende entiteiten van een label te voorzien, tot de lijst leeg is. 

10. Selecteer de volgende intentie in de filterlijst en blijf vervolgens uitingen corrigeren en entiteiten van labels voorzien. Vergeet niet voor iedere intentie als laatste stap ofwel **Toevoegen aan uitgelijnde intentie** te selecteren op de rij van die uiting of het selectievakje te selecteren bij die intentie en boven de tabel op **Selectie toevoegen** te klikken.

    Ga door totdat alle intenties en entiteiten in de filterlijst over een lege lijst beschikken. Dit is een hele kleine app. Het beoordelingsproces duurt slechts een paar minuten. 

## <a name="update-phrase-list"></a>Frasenlijst bijwerken
Houd de woordgroepenlijst actueel door alle nieuwe gedetecteerde taaknamen toe te voegen. 

1. Selecteer **Woordgroepenlijsten** in de linkernavigatiebalk.

2. Selecteer de woordgroepenlijst **Taken**.

3. Voeg `Natural Language Processing` toe als een waarde en selecteer vervolgens **Opslaan**. 

## <a name="train"></a>Trainen

LUIS is niet op de hoogte van eventuele wijzigingen totdat deze wordt getraind. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

Als u deze app hebt geïmporteerd, dient u **Sentimentanalyse** te selecteren.

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

Probeer een uiting die lijkt op de gecorrigeerde uiting. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

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
Sommige eindpuntuitingen hebben een hoge voorspellingsscore in de beoordelingslijst. U dient deze uitingen alsnog te beoordelen en verifiëren. Ze zijn opgenomen in de lijst omdat de beoordeling van de tweede intentie een score had die te dicht lag bij de score van de hoogst scorende intentie. Er moet ongeveer 15% verschil zijn tussen de bovenste twee intenties.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u uitingen bekeken die bij het eindpunt zijn ingediend en waar LUIS niet zeker van was. Zodra deze uitingen zijn gecontroleerd en als voorbeelduitingen naar de juiste intenties zijn verplaatst, kan LUIS de voorspellingsnauwkeurigheid vergroten.

> [!div class="nextstepaction"]
> [Meer informatie over het gebruik van patronen](luis-tutorial-pattern.md)
