---
title: Zelfstudie voor het maken van een LUIS-app die een sentimentanalyse als resultaat geeft - Azure geeft | Microsoft Docs
description: In deze zelfstudie leert u hoe u sentimentanalyse toevoegt aan uw LUIS-app om utterances te analyseren op positieve, negatieve en neutrale gevoelens.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d000637312619fc493e2f7bad8e8edf0d8d0d94b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265331"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Zelfstudie: app maken die gevoel retourneert samen met voorspelling van intentie
In deze zelfstudie maakt u een app die laat zien hoe u positieve, negatieve en neutrale gevoelens kunt extraheren uit utterances.

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiërarchische entiteiten en contextueel geleerde onderliggende elementen 
> * Nieuwe LUIS-app maken voor het domein Travel met de intent Bookflight
> * Intent _None_ toevoegen en voorbeelden van utterances
> * Entiteit van het type Hierarchical toevoegen met onderliggende elementen Origin en Destination
> * App inleren en publiceren
> * Eindpunt van de app opvragen om LUIS JSON-antwoord te zien inclusief hiërarchische onderliggende elementen 

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="sentiment-analysis"></a>Sentimentanalyse
Sentimentanalyse is de mogelijkheid om te bepalen of de uiting (utterance) van een gebruiker positief, negatief of neutraal is. 

De volgende utterances zijn voorbeelden van gevoelens:

|Gevoel en score|Utterance|
|:--|--|
|positief - 0.89 |De combinatie van soep en salade was geweldig.|
|negatief - 0.07 |Ik vond het voorgerecht niet lekker.|

Sentimentanalyse is een app-instelling die voor elke utterance geldt. U hoeft niet de woorden te vinden die gevoel aangeven in een utterance en deze te labelen. LUIS doet dat voor u.

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS][LUIS]. Doe dit bij de [regio][LUIS-regions] waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS][LUIS] de optie **Create new app**. 

    [![](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png "Schermopname van de pagina met de lijst met apps")](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png#lightbox)

3. Geef op de pagina **Create new app** de app de naam `Restaurant Reservations With Sentiment` en selecteer **Done**. 

    ![Afbeelding van het dialoogvenster voor het maken van een nieuwe app](./media/luis-quickstart-intent-and-sentiment-analysis/create-app-ddl.png)

    Wanneer het maken van de app is voltooid, ziet u in LUIS een lijst met intents met daarin de intent None.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png "Schermopname van de lijst met intents")](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png#lightbox)

## <a name="add-a-prebuilt-domain"></a>Een vooraf gedefinieerd domein toevoegen
Voeg een vooraf gedefinieerd domein toe om snel intents, entiteiten en gelabelde utterances toe te voegen.

1. Selecteer **Prebuilt Domains** in het menu aan de linkerkant.

    [ ![Schermopname van de knop Prebuilt Domains](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-expanded.png#lightbox)

2. Selecteer **Add domain** voor het vooraf gedefinieerde domein **RestaurantReservation**. Wacht totdat het domein is toegevoegd.

    [ ![Schermopname van de lijst met vooraf gedefinieerde domeinen](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-expanded.png#lightbox)

3. Selecteer **Intents** in de linkernavigatiebalk. Dit vooraf gedefinieerde domein heeft één intent.

    [ ![Schermopname van de lijst met vooraf gedefinieerde domeinen met Intents gemarkeerd in het linkernavigatievenster](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png#lightbox)

4.  Selecteer de intent **RestaurantReservation.Reserve**. 

    [ ![Schermopname van lijst met intents met RestaurantReservation.Reserve gemarkeerd](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png#lightbox)

5. Schakel **Entities View** in en uit om de verschillende utterances te zien, met labels voor domeinspecifieke entiteiten.

    [ ![Schermopname van de intent RestaurantReservation.Reserve met Entities View uitgeschakeld en Token View gemarkeerd](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-expanded.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS niet weet dat de intents en entiteiten (het model) zijn gewijzigd, totdat u de app hebt ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![Schermopname met knop Train gemarkeerd](./media/luis-quickstart-intent-and-sentiment-analysis/train-button-expanded.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Schermopname van melding dat het inleren is gelukt ](./media/luis-quickstart-intent-and-sentiment-analysis/trained-expanded.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>App configureren voor gebruik van sentimentanalyse
U kunt sentimentanalyse inschakelen op de pagina **Publish**. 

1. Selecteer **Publish** in de navigatiebalk rechtsboven.

    ![Schermopname van de pagina Intent met de knop Publish uitgevouwen ](./media/luis-quickstart-intent-and-sentiment-analysis/publish-expanded.png)

2. Selecteer **Enable Sentiment Analysis**.

    ![Schermopname van de pagina Publish met Enable Sentiment Analysis gemarkeerd ](./media/luis-quickstart-intent-and-sentiment-analysis/enable-sentiment-expanded.png)

3. Selecteer de slot Production en vervolgens de knop **Publish**.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-inline.png "Schermopname van de pagina Publish app met de knop Publish gemarkeerd")](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-expanded.png#lightbox)

4. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Schermopname van de pagina Publish met eindpunt-URL gemarkeerd](media/luis-quickstart-intent-and-sentiment-analysis/endpoint-url-inline.png)

2. Ga naar het einde van de URL in het adres en voer `Reserve table for  10 on upper level away from kitchen` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `RestaurantReservation.Reserve` als resultaat moet geven met de sentimentanalyse geëxtraheerd.

```
{
  "query": "Reserve table for 10 on upper level away from kitchen",
  "topScoringIntent": {
    "intent": "RestaurantReservation.Reserve",
    "score": 0.9926384
  },
  "intents": [
    {
      "intent": "RestaurantReservation.Reserve",
      "score": 0.9926384
    },
    {
      "intent": "None",
      "score": 0.00961109251
    }
  ],
  "entities": [],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, met sentimentanalyse ingeschakeld, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de overeenkomende geëxtraheerde gegevens geretourneerd, met inbegrip van het algehele gevoel uitgedrukt als een score. 

Uw chatbot heeft nu voldoende gegevens om de volgende stap in het gesprek te bepalen. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de sentimentgegevens uit de utterance om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [API op LUIS-eindpunt aanroepen met C#](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
