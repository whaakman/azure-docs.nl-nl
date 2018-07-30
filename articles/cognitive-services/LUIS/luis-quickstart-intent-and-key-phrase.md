---
title: Zelfstudie voor het maken van een LUIS-app die sleuteltermen retourneert - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u de entiteit keyPhrase toevoegt aan een LUIS-app en hieruit opvraagt om utterances te analyseren op belangrijke informatie.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/27/2018
ms.author: v-geberr
ms.openlocfilehash: ccefb4c2890d74978f340778cfab7cad979c9802
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929545"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Zelfstudie: 8. KeyPhrase-entiteit toevoegen 
In deze zelfstudie gebruikt u een app die laat zien hoe u belangrijke informatie kunt extraheren uit utterances.

<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over keyPhrase 
> * De LUIS-app in HR-domein (Human Resources) gebruiken 
> * Entiteit keyPhrase toevoegen om inhoud te extraheren uit utterance
> * App trainen en publiceren
> * Eindpunt van de app opvragen om LUIS JSON-antwoord te zien inclusief sleuteltermen

Voor dit artikel kunt u het gratis [LUIS](luis-reference-regions.md#publishing-regions)-account gebruiken om de LUIS-toepassing te maken.

## <a name="before-you-begin"></a>Voordat u begint
Als u geen Human Resources-app uit de zelfstudie over de [entiteit Simple](luis-quickstart-primary-and-secondary-data.md) hebt, [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de [LUIS](luis-reference-regions.md#luis-website)-website. De app die kan worden geïmporteerd bevindt zich in de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `keyphrase`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="keyphrase-entity-extraction"></a>Belangrijke gegevens extraheren met keyPhrase
Belangrijke informatie wordt aangeleverd door de vooraf gedefinieerde entiteit **keyPhrase**. Deze entiteit retourneert sleuteltermen in de utterance.

De volgende utterances zijn voorbeelden van belangrijke informatie:

|Utterance|Waarden voor KeyPhrase|
|--|--|
|Is there a new medical plan with a lower deductible offered next year?|"lower deductible"<br>"new medical plan"<br>"year"|
|Is vision therapy covered in the high deductible medical plan?|"high deductible medical plan"<br>"vision therapy"|

Uw clienttoepassing kan deze waarden gebruiken, samen met andere geëxtraheerde entiteiten, om zo te bepalen wat de volgende stap in de conversatie is.

## <a name="add-keyphrase-entity"></a>KeyPhrase-entiteit toevoegen 
Voeg de vooraf gedefinieerde entiteit keyPhrase toe om belangrijke informatie op te halen uit utterances.

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

    [ ![Schermopname van LUIS-app met Build gemarkeerd in de navigatiebalk rechtsboven](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png)](./media/luis-quickstart-intent-and-key-phrase/hr-first-image.png#lightbox)

2. Selecteer **Entities** in het menu aan de linkerkant.

    [ ![Schermopname van de optie Entities gemarkeerd in het linkernavigatievenster van de sectie Build](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. Selecteer **Manage prebuilt entities**.

    [ ![Schermopname van het pop-updialoogvenster Entities](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. Selecteer **keyPhrase** in het pop-updialoogvenster en selecteer vervolgens **Done**. 

    [ ![Schermopname van het pop-updialoogvenster Entities](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. Selecteer **Intents** in het menu links, selecteer vervolgens de intentie **Utilities.Confirm**. De entiteit keyPhrase is in verschillende utterances gelabeld. 

    [ ![Schermopname van de intentie Utilities.Confirm met gelabelde keyPhrases in utterances](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app trainen
De nieuwe versie `keyphrase` van de app moet worden getraind.  

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![De app trainen](./media/luis-quickstart-intent-and-key-phrase/train-button.png)

2. Het trainen is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het trainen is gelukt.

    ![Trainen is voltooid](./media/luis-quickstart-intent-and-key-phrase/trained.png)

## <a name="publish-app-to-endpoint"></a>App publiceren naar eindpunt

1. Selecteer **Publish** in de navigatiebalk rechtsboven.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png "Schermopname van de pagina Publish met de knop Publish to production slot gemarkeerd")](media/luis-quickstart-intent-and-key-phrase/hr-publish-button-top-nav.png#lightbox)

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    [![](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png "Schermopname van de pagina Publish met de knop Publish to production slot gemarkeerd")](media/luis-quickstart-intent-and-key-phrase/hr-publish-to-production-expanded.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Schermopname van de pagina Publish met eindpunt-URL gemarkeerd](media/luis-quickstart-intent-and-key-phrase/hr-endpoint-url-inline.png )

2. Ga naar het einde van de URL in het adres en voer `does form hrf-123456 cover the new dental benefits and medical plan` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

```
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

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, met detectie van sleuteltermen ingeschakeld, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de overeenkomende geëxtraheerde gegevens geretourneerd, met inbegrip van de belangrijke informatie. 

Uw chatbot heeft nu voldoende gegevens om de volgende stap in het gesprek te bepalen. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de sleuteltermen uit de utterance om de volgende stap te nemen. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer **My apps** in het menu linksboven. Selecteer de drie puntjes (***...***) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sentimentanalyse aan app toevoegen](luis-quickstart-intent-and-sentiment-analysis.md)