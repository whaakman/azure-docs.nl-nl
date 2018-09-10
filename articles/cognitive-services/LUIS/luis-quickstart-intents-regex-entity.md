---
title: Zelfstudie - LUIS-app maken voor het ophalen van gegevens die voldoen aan gewone expressie - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intenties' en een entiteit in de vorm van een gewone expressie om gegevens te extraheren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 9672215c8cc5f95775e3b7fba74b27379a58ff49
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162916"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Zelfstudie: 3. Een entiteit in de vorm van een reguliere expressie toevoegen
In deze zelfstudie maakt u een app die laat zien hoe u consistent opgemaakte gegevens kunt extraheren uit een utterance met behulp van de entiteit **Regular Expression (regex)**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over entiteit in de vorm van een gewone expressie 
> * LUIS-app gebruiken voor een HR-domein (Human Resources) met de intentie FindForm
> * Entiteit in de vorm van een gewone expressie toevoegen om formuliernummer te extraheren uit utterance
> * App trainen en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u niet beschikt over de Human Resources-app uit de zelfstudie over de [vooraf gedefinieerde entiteit](luis-tutorial-prebuilt-intents-entities.md), [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de website van [LUIS](luis-reference-regions.md#luis-website). Dit kan vanuit de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `regex`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 


## <a name="purpose-of-the-regular-expression-entity"></a>Doel van een entiteit in de vorm van een gewone expressie
Het doel van een entiteit is het extraheren van belangrijke gegevens die zijn opgenomen in de utterance. De app maakt gebruik van een entiteit in de vorm van een reguliere expressie om opgemaakte nummers van HR-formulieren (Human Resources) op te halen uit een utterance. De gegevens worden niet via machine learning verkregen. 

Enkele voorbeelden van eenvoudige utterances:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Voorbeelden van afgekorte of spreektaalversies van utterances:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
De entiteit in de vorm van een gewone expressie die moet overeenkomen met het formuliernummer is `hrf-[0-9]{6}`. Deze gewone expressie zoekt overeenkomsten voor de letterlijke tekens `hrf -`, maar negeert varianten in hoofdlettergebruik en cultuur. De cijfers 0-9 moeten tot 6 cijfers exact overeenkomen.

HRF staat voor HR-formulier.

### <a name="tokenization-with-hyphens"></a>Tokeniseren met afbreekstreepjes
LUIS tokeniseert de utterance wanneer deze wordt toegevoegd aan een intentie. Dit houdt in dat er in deze utterances spaties worden toegevoegd vóór en na het afbreekstreepje, `Where is HRF - 123456?`. De gewone expressie wordt toegepast op de utterance in de onbewerkte vorm, dus voordat deze is getokeniseerd. Omdat de expressie wordt toegepast op de _onbewerkte_ vorm, hoeft er geen rekening te worden gehouden met woordgrenzen. 


## <a name="add-findform-intent"></a>Intent FindForm toevoegen

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

2. Selecteer **Create new intent**. 

3. Voer in het pop-updialoogvenster `FindForm` in en selecteer vervolgens **Done**. 

    ![Schermopname van dialoogvenster voor het maken van een nieuwe intentie met Utilities in het zoekvak](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |What is the URL for hrf-123456?|
    |Where is hrf-345678?|
    |When was hrf-456098 updated?|
    |Did John Smith update hrf-234639 last week?|
    |How many version of hrf-345123 are there?|
    |Who needs to authorize form hrf-123456?|
    |How many people need to sign off on hrf-345678?|
    |hrf-234123 date?|
    |author of hrf-546234?|
    |title of hrf-456234?|

    [ ![Schermopname van de pagina Intent met de nieuwe utterances gemarkeerd](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    De toepassing is uitgebreid met de vooraf gedefinieerde entiteit Number (uit de vorige zelfstudie), waardoor elk formuliernummer een tag heeft. Dit is mogelijk niet voldoende voor uw clienttoepassing, maar het nummer wordt niet gelabeld met het type nummer. Door een nieuwe entiteit met een geschikte naam te maken, kan de clienttoepassing de entiteit op de juiste manier verwerken wanneer deze wordt geretourneerd door LUIS.

## <a name="create-an-hrf-number-regular-expression-entity"></a>Een entiteit in de vorm van een gewone expressie voor een HRF-nummer maken 
Voer de volgende stappen uit om een entiteit in de vorm van een gewone expressie te maken om aan LUIS door te geven wat de indeling van een HRF-nummer is:

1. Selecteer **Entities** in het linkerpaneel.

2. Selecteer **Create new entity** op de pagina Entities. 

3. Geef in het pop-updialoogvenster de naam `HRF-number` op voor de nieuwe entiteit, selecteer **RegEx** als het type entiteit, voer `hrf-[0-9]{6}` in als de gewone expressie en selecteer ten slotte **Done**.

    ![Schermopname van pop-updialoogvenster met eigenschappen voor nieuwe entiteit](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecteer **Intents** en selecteer vervolgens de intentie **FindForm** om de gewone expressie te zien die is gelabeld in de utterances. 

    [![Schermopname van het labelen van een utterance met een bestaande entiteit en regex-patroon](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Omdat de entiteit niet wordt verkregen via machine learning, wordt het label toegepast op de utterances en weergegeven op de website van LUIS zodra het label is gemaakt.

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `When were HRF-123456 and hrf-234567 published in the last year?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intentie `FindForm` als resultaat moet geven met de twee formuliernummers `HRF-123456` en `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    De nummers in de utterance worden tweemaal geretourneerd; eenmaal als de nieuwe entiteit `hrf-number` en eenmaal als een vooraf gedefinieerde entiteit, `number`. Een utterance kan meer dan één entiteit hebben, en meer dan één entiteit van hetzelfde type, zoals dit voorbeeld aantoont. Met behulp van een entiteit in de vorm van een gewone expressie kan LUIS benoemde gegevens extraheren, wat vanuit programmatisch oogpunt nuttiger is voor de clienttoepassing dan het ontvangen van het JSON-antwoord.

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app heeft de intentie geïdentificeerd en de geëxtraheerde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om de primaire actie te bepalen, `FindForm`, en welke formuliernummers aanwezig zijn in de zoekopdracht. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de formuliernummers om te zoeken via een API van een derde partij. LUIS hoeft die taak niet uit te voeren. LUIS alleen bepaalt wat de intentie van de gebruiker is en extraheert gegevens over die intentie. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de entiteit lijst](luis-quickstart-intent-and-list-entity.md)

