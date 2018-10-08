---
title: 'Zelfstudie 3: met reguliere expressies overeenkomende gegevens - Goed opgemaakte gegevens extraheren'
titleSuffix: Azure Cognitive Services
description: U kunt consistent opgemaakte gegevens uit een uiting extraheren met de reguliere-expressie-entiteit.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 06e212ef756fda9224b38b41c69c7c4eccfb9796
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159853"
---
# <a name="tutorial-3-extract-well-formatted-data"></a>Zelfstudie 3: goed opgemaakte gegevens herkennen
In deze zelfstudie bewerkt u de Human Resources-app om consistent opgemaakte gegevens te extraheren uit een uiting met behulp van de entiteit **Regular Expression (regex)**.

Het doel van een entiteit is het extraheren van belangrijke gegevens uit een uiting. De app maakt gebruik van een entiteit in de vorm van een reguliere expressie om opgemaakte nummers van HR-formulieren (Human Resources) op te halen uit een uiting. Hoewel de intentie van een uiting altijd wordt bepaald aan de hand van machine learning, wordt voor dit specifieke type entiteit geen machine learning gebruikt. 

**Enkele voorbeelden van uitingen:**

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Een reguliere expressie is een goede keuze voor dit type gegevens wanneer:

* de gegevens goed zijn opgemaakt.

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * Intent FindForm toevoegen
> * Een entiteit in de vorm van een reguliere expressie toevoegen 
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken
Ga door met de in de laatste zelfstudie gemaakt app, **HumanResources**. 

Als u niet over de app HumanResources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1. Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-prebuilts-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `regex`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL. 

## <a name="findform-intent"></a>Intentie FindForm

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

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

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="regular-expression-entity"></a>Een entiteit in de vorm van een reguliere expressie 
De entiteit in de vorm van een gewone expressie die moet overeenkomen met het formuliernummer is `hrf-[0-9]{6}`. Deze gewone expressie zoekt overeenkomsten voor de letterlijke tekens `hrf-`, maar negeert varianten in hoofdlettergebruik en cultuur. De cijfers 0-9 moeten tot 6 cijfers exact overeenkomen.

HRF staat voor `human resources form`.

LUIS tokeniseert de uiting wanneer deze wordt toegevoegd aan een intentie. Dit houdt in dat er in deze utterances spaties worden toegevoegd vóór en na het afbreekstreepje, `Where is HRF - 123456?`. De gewone expressie wordt toegepast op de utterance in de onbewerkte vorm, dus voordat deze is getokeniseerd. Omdat de expressie wordt toegepast op de _onbewerkte_ vorm, hoeft er geen rekening te worden gehouden met woordgrenzen. 

Voer de volgende stappen uit om een entiteit in de vorm van een gewone expressie te maken om aan LUIS door te geven wat de indeling van een HRF-nummer is:

1. Selecteer **Entities** in het linkerpaneel.

2. Selecteer **Create new entity** op de pagina Entities. 

3. Geef in het pop-updialoogvenster de naam `HRF-number` op voor de nieuwe entiteit, selecteer **RegEx** als het type entiteit, voer `hrf-[0-9]{6}` in als **RegEx**-waarde en selecteer ten slotte **Gereed**.

    ![Schermopname van pop-updialoogvenster met eigenschappen voor nieuwe entiteit](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecteer **Intenties** in het menu links en selecteer vervolgens de intentie **FindForm** om de gewone expressie te zien die is gelabeld in de uitingen. 

    [![Schermopname van het labelen van een utterance met een bestaande entiteit en regex-patroon](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Omdat de entiteit niet wordt verkregen via machine learning, wordt het label toegepast op de utterances en weergegeven op de website van LUIS zodra het label is gemaakt.

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `When were HRF-123456 and hrf-234567 published in the last year?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intentie `FindForm` als resultaat moet geven met de twee formuliernummers `HRF-123456` en `hrf-234567`.

    ```JSON
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


## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een nieuwe intentie gemaakt, voorbeelduitingen toegevoegd en een entiteit in de vorm van een reguliere expressie gemaakt om goed opgemaakte gegevens te extraheren uit de uitingen. Na het trainen en het publiceren van de app is met een query naar het eindpunt de intentie achterhaald. Vervolgens zijn de geëxtraheerde gegevens geretourneerd.

> [!div class="nextstepaction"]
> [Meer informatie over de entiteit lijst](luis-quickstart-intent-and-list-entity.md)

