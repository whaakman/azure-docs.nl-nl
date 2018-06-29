---
title: Zelfstudie voor het maken van een LUIS-app die sleuteltermen retourneert - Azure geeft | Microsoft Docs
description: In deze zelfstudie leert u hoe u de entiteit keyPhrase toevoegt aan een LUIS-app en hieruit opvraagt om utterances te analyseren op belangrijke informatie.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12c306b5199da5862302c28d1690b81c6e1edb0e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264612"
---
# <a name="tutorial-create-app-that-returns-keyphrases-entity-data-found-in-utterances"></a>Zelfstudie: app maken die sleuteltermen retourneert uit utterances
In deze zelfstudie maakt u een app die laat zien hoe u belangrijke informatie kunt extraheren uit utterances.

<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over keyPhrase 
> * Nieuwe LUIS-app maken voor het domein Human Resources
> * Intent _None_ toevoegen en voorbeelden van utterances
> * Entiteit keyPhrase toevoegen om inhoud te extraheren uit utterance
> * App inleren en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

Voor dit artikel kunt u de gratis [LUIS][LUIS]-account gebruiken om de LUIS-toepassing te maken.

## <a name="keyphrase-entity-extraction"></a>Belangrijke gegevens extraheren met keyPhrase
Belangrijke informatie wordt aangeleverd door de vooraf gedefinieerde entiteit **keyPhrase**. Deze entiteit retourneert sleuteltermen uit in de utterance.

De volgende utterances zijn voorbeelden van belangrijke informatie:

|Utterance|Waarden voor KeyPhrase|
|--|--|
|Is there a new medical plan with a lower deductible offered next year?|"lower deductible"<br>"new medical plan"<br>"year"|
|Is vision therapy covered in the high deductible medical plan?|"high deductible medical plan"<br>"vision therapy"|

Uw chatbot kan rekening houden met deze waarden, naast alle andere geëxtraheerde entiteiten, om de volgende stap in het gesprek te bepalen.

## <a name="download-sample-app"></a>Voorbeeld-app downloaden
Download de [Human Resources](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json)-app en sla deze op in een bestand met de extensie *.json. Deze voorbeeld-app herkent utterances die relevant zijn voor secundaire arbeidsvoorwaarden, organigrammen en fysieke activa.

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS][LUIS]. Doe dit bij de [regio][LUIS-regions] waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS][LUIS] de optie **Import new app** om de Human Resources-app te importeren die u in de vorige sectie hebt gedownload. 

    [![](media/luis-quickstart-intent-and-key-phrase/app-list.png "Schermopname van de pagina met de lijst met apps")](media/luis-quickstart-intent-and-key-phrase/app-list.png#lightbox)

3. Voer in het dialoogvenster **Import new app** de naam `Human Resources with Key Phrase entity` in voor de app. 

    ![Afbeelding van het dialoogvenster voor het maken van een nieuwe app](./media/luis-quickstart-intent-and-key-phrase/import-new-app-inline.png)

    Wanneer het maken van de app is voltooid, ziet u in LUIS een lijst met intents.

    [![](media/luis-quickstart-intent-and-key-phrase/intents-list.png "Schermopname van de lijst met intents")](media/luis-quickstart-intent-and-key-phrase/intents-list.png#lightbox)

## <a name="add-keyphrase-entity"></a>KeyPhrase-entiteit toevoegen 
Voeg de vooraf gedefinieerde entiteit keyPhrase toe om belangrijke informatie op te halen uit utterances.

1. Selecteer **Entities** in het menu aan de linkerkant.

    [ ![Schermopname van de optie Entities gemarkeerd in het linkernavigatievenster van de sectie Build](./media/luis-quickstart-intent-and-key-phrase/select-entities.png)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png#lightbox)

2. Selecteer **Manage prebuilt entities**.

    [ ![Schermopname van het pop-upvenster Entities](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png#lightbox)

3. Selecteer **keyPhrase** in het pop-upvenster en selecteer vervolgens **Done**. 

    [ ![Schermopname van het pop-upvenster Entities](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS kan niet weten dat het model is gewijzigd, en dit moet daarom worden ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![Schermopname met knop Train gemarkeerd](./media/luis-quickstart-intent-and-key-phrase/train-button-expanded.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Schermopname van melding dat het inleren is gelukt ](./media/luis-quickstart-intent-and-key-phrase/trained-inline.png)

## <a name="publish-app-to-endpoint"></a>App publiceren naar eindpunt

1. Selecteer **Publish** in de navigatiebalk rechtsboven.

    ![Schermopname van de pagina Entity met de knop Publish uitgevouwen ](./media/luis-quickstart-intent-and-key-phrase/publish-expanded.png)

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    [![](media/luis-quickstart-intent-and-key-phrase/publish-to-production-inline.png "Schermopname van de pagina Publish app met de knop Publish gemarkeerd")](media/luis-quickstart-intent-and-key-phrase/publish-to-production-expanded.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Schermopname van de pagina Publish met eindpunt-URL gemarkeerd](media/luis-quickstart-intent-and-key-phrase/endpoint-url-inline.png )

2. Ga naar het einde van de URL in het adres en voer `Is there a new medical plan with a lower deductible offered next year?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. 

```
{
  "query": "Is there a new medical plan with a lower deductible offered next year?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.216838628
  },
  "entities": [
    {
      "entity": "lower deductible",
      "type": "builtin.keyPhrase",
      "startIndex": 35,
      "endIndex": 50
    },
    {
      "entity": "new medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 26
    },
    {
      "entity": "year",
      "type": "builtin.keyPhrase",
      "startIndex": 65,
      "endIndex": 68
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, met detectie van sleuteltermen ingeschakeld, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de overeenkomende geëxtraheerde gegevens geretourneerd, met inbegrip van de belangrijke informatie. 

Uw chatbot heeft nu voldoende gegevens om de volgende stap in het gesprek te bepalen. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de sleuteltermen uit de utterance om de volgende stap te nemen. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [App maken die gevoel retourneert samen met voorspelling van intentie](luis-quickstart-intent-and-sentiment-analysis.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
