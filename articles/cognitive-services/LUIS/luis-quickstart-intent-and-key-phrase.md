---
title: 'Zelfstudie 8: Sleuteltermen extraheren in LUIS'
titleSuffix: Azure Cognitive Services
description: Leer hoe u met de vooraf gedefinieerde entiteit keyPhrase belangrijke informatie kunt extraheren uit utterances. U hoeft utterances niet te labelen met vooraf gedefinieerde entiteiten. De entiteit wordt automatisch gedetecteerd.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: fafb6a6cb986b5bcd795c9bac0fadfce2827c667
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424809"
---
# <a name="tutorial-8-extract-key-phrases-of-utterance"></a>Zelfstudie 8: Sleuteltermen extraheren uit een utterance
In deze zelfstudie gebruikt u de vooraf gedefinieerde entiteit keyPhrase om belangrijke informatie te extraheren uit utterances. U hoeft utterances niet te labelen met vooraf gedefinieerde entiteiten. De entiteit wordt automatisch gedetecteerd.

De volgende utterances zijn voorbeelden van belangrijke informatie:

|Utterance|Waarden voor KeyPhrase|
|--|--|
|Is er een nieuwe medische abonnement met lagere zelf moet betalen volgend jaar aangeboden?|"lagere zelf moet betalen"<br>"nieuw medische plan"<br>“year“|
|Vision behandeling valt het hoge aftrek medische plan?|"hoge aftrek ziektekostenverzekering"<br>"vision behandeling"|

Uw clienttoepassing kan deze waarden gebruiken, samen met andere geëxtraheerde entiteiten, om zo te bepalen wat de volgende stap in de conversatie is.

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * KeyPhrase-entiteit toevoegen 
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken

Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-simple-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `keyphrase`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.

## <a name="add-keyphrase-entity"></a>KeyPhrase-entiteit toevoegen 
Voeg de vooraf gedefinieerde entiteit keyPhrase toe om belangrijke informatie op te halen uit utterances.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **Entities** in het menu aan de linkerkant.

3. Selecteer **Add prebuilt entity**.

4. Selecteer **keyPhrase** in het pop-updialoogvenster en selecteer vervolgens **Done**. 

    [ ![Schermopname van het pop-updialoogvenster Entities](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

5. Selecteer **Intents** in het menu links, selecteer vervolgens de intentie **Utilities.Confirm**. De entiteit keyPhrase is in verschillende utterances gelabeld. 

    [ ![Schermopname van de intentie Utilities.Confirm met gelabelde keyPhrases in utterances](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `does form hrf-123456 cover the new dental benefits and medical plan` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 
    
    ```JSON
    {
      "query": "does form hrf-123456 cover the new dental benefits and medical plan",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9300641
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9300641
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0359598845
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0141798034
        },
        {
          "intent": "MoveEmployee",
          "score": 0.0112197418
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00507669244
        },
        {
          "intent": "None",
          "score": 0.00238501839
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00202810857
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00102957746
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0008688423
        },
        {
          "intent": "Utilities.Confirm",
          "score": 3.557994E-05
        }
      ],
      "entities": [
        {
          "entity": "hrf-123456",
          "type": "HRF-number",git 
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "new dental benefits",
          "type": "builtin.keyPhrase",
          "startIndex": 31,
          "endIndex": 49
        },
        {
          "entity": "medical plan",
          "type": "builtin.keyPhrase",
          "startIndex": 55,
          "endIndex": 66
        },
        {
          "entity": "hrf",
          "type": "builtin.keyPhrase",
          "startIndex": 10,
          "endIndex": 12
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        }
      ]
    }
    ```

    De gebruiker heeft bij zijn zoekactie naar een formulier meer gegevens verstrekt dan nodig is om het formulier te zoeken. De extra gegevens worden geretourneerd als **builtin.keyPhrase**. De clienttoepassing kan deze extra gegevens gebruiken om een vervolgvraag te formuleren, zoals 'Wilt u met een HR-medewerker praten over het nieuwe tandheelkundige programma?', of ervoor zorg dat een menu met meer opties wordt weergegeven, met onder andere 'Meer informatie over het nieuwe tandheelkundige programma of de nieuwe zorgverzekering'.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de vooraf gedefinieerde entiteit keyPhrase toegevoegd om snel sleuteltermen te detecteren in utterances zonder dat u deze daar voor hoeft te labelen. 

> [!div class="nextstepaction"]
> [Sentimentanalyse aan app toevoegen](luis-quickstart-intent-and-sentiment-analysis.md)
