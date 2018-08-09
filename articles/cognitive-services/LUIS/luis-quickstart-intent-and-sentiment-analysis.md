---
title: Zelfstudie voor het maken van een LUIS-app die een sentimentanalyse als resultaat geeft - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u sentimentanalyse toevoegt aan uw LUIS-app om utterances te analyseren op positieve, negatieve en neutrale gevoelens.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: baa449bb9e78a5c6437b0a9528e5d1f10dfa519f
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520449"
---
# <a name="tutorial-9--add-sentiment-analysis"></a>Zelfstudie: 9.  Sentimentanalyse toevoegen
In deze zelfstudie maakt u een app die laat zien hoe u positieve, negatieve en neutrale gevoelens kunt extraheren uit utterances.

<!-- green checkmark -->
> [!div class="checklist"]
> * Sentimentanalyse begrijpen
> * De LUIS-app in HR-domein (Human Resources) gebruiken 
> * Sentimentanalyse toevoegen
> * App trainen en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien 

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u geen Human Resources-app uit de zelfstudie over vooraf gemaakte [keyPhrase-entiteit](luis-quickstart-intent-and-key-phrase.md) hebt, [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de [LUIS](luis-reference-regions.md#luis-website)-website. De app die kan worden geïmporteerd bevindt zich in de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `sentiment`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="sentiment-analysis"></a>Sentimentanalyse
Sentimentanalyse is de mogelijkheid om te bepalen of de uiting (utterance) van een gebruiker positief, negatief of neutraal is. 

De volgende utterances zijn voorbeelden van gevoelens:

|Sentiment|Score|Utterance|
|:--|:--|:--|
|positief|0,91 |John W. Smith heeft een geweldige presentatie gegeven in Parijs.|
|positief|0,84 |jill-jones@mycompany.com heeft het fantastisch gedaan om Parker als klant binnen te halen.|

Sentimentanalyse is een app-instelling die voor elke utterance geldt. U hoeft niet op zoek te gaan naar woorden om in de utterance een sentiment aan te duiden en deze van een label te voorzien, omdat sentimentanalyse voor de hele utterance geldt. 

## <a name="add-employeefeedback-intent"></a>Intent EmployeeFeedback toevoegen 
Voeg een nieuwe intentie toe om feedback van werknemers die tot het bedrijf behoren, vast te leggen. 

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

    [ ![Schermopname van LUIS-app met Build gemarkeerd in de navigatiebalk rechtsboven](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. Selecteer **Create new intent**.

    [ ![Schermopname van LUIS-app met Build gemarkeerd in de navigatiebalk rechtsboven](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

3. Geef de nieuwe intentie de naam  `EmployeeFeedback`.

    ![Een nieuw dialoogvenster maken met de naam EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Voeg een aantal utterances toe die aanduiden op welk terrein een werknemer het goed doet of waar er ruimte is voor verbetering:

    Vergeet niet dat in deze Human Resources-app, werknemers in de lijstentiteit `Employee` worden gedefinieerd met behulp van hun naam, e-mailadres, toestelnummer, mobiele-telefoonnummer en Amerikaans sociaal-fiscaal nummer. 

    |Utterances|
    |--|
    |425-555-1212 heeft het goed gedaan toen ze een collega welkom heette die terugkwam van zwangerschapsverlof.|
    |234-56-7891 heeft het geweldig gedaan toen ze een collega tijdens een emotioneel moeilijke tijd troost bood.|
    |jill-jones@mycompany.com had niet alle facturen die nodig waren om de administratieve procedures goed uit te voeren.|
    |john.w.smith@mycompany.com leverde de benodigde formulieren een maand te laat en zonder handtekeningen aan.|
    |x23456 was niet aanwezig op de belangrijke marketingbijeenkomst buiten de deur.|
    |x12345 was niet aanwezig op het beoordelingsgesprek in juni.|
    |Jill Jones deed het fantastisch tijdens haar verkooppraatje voor Harvard.|
    |John W. Smith heeft een geweldige presentatie gegeven voor Stanford.|

    [ ![Schermopname van de LUIS-app met voorbeelden van utterances in de intentie EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>App configureren voor gebruik van sentimentanalyse
Configureer sentimentanalyse op de pagina **Publish**. 

1. Selecteer **Publish** in de navigatiebalk rechtsboven.

    ![Schermopname van de pagina Intent met de knop Publish uitgevouwen ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. Selecteer **Enable Sentiment Analysis**. 

## <a name="publish-app-to-endpoint"></a>App publiceren naar eindpunt

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `Jill Jones work with the media team on the public portal was amazing` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intentie `EmployeeFeedback` als resultaat moet geven met de sentimentanalyse geëxtraheerd.

```
{
  "query": "Jill Jones work with the media team on the public portal was amazing",
  "topScoringIntent": {
    "intent": "EmployeeFeedback",
    "score": 0.4983256
  },
  "intents": [
    {
      "intent": "EmployeeFeedback",
      "score": 0.4983256
    },
    {
      "intent": "MoveEmployee",
      "score": 0.06617523
    },
    {
      "intent": "GetJobInformation",
      "score": 0.04631853
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0103248553
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.007531875
    },
    {
      "intent": "FindForm",
      "score": 0.00344597152
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00337914471
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0026357458
    },
    {
      "intent": "None",
      "score": 0.00214573368
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00157622492
    },
    {
      "intent": "Utilities.Confirm",
      "score": 7.379545E-05
    }
  ],
  "entities": [
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "media team",
      "type": "builtin.keyPhrase",
      "startIndex": 25,
      "endIndex": 34
    },
    {
      "entity": "public portal",
      "type": "builtin.keyPhrase",
      "startIndex": 43,
      "endIndex": 55
    },
    {
      "entity": "jill jones",
      "type": "builtin.keyPhrase",
      "startIndex": 0,
      "endIndex": 9
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8694164
  }
}
```

SentimentAnalysis is met een score van 0,86 positief. 

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, met sentimentanalyse ingeschakeld, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de overeenkomende geëxtraheerde gegevens geretourneerd, met inbegrip van het algehele gevoel uitgedrukt als een score. 

Uw chatbot heeft nu voldoende gegevens om de volgende stap in het gesprek te bepalen. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de sentimentgegevens uit de utterance om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Eindpuntuitingen beoordelen in de HR-app](luis-tutorial-review-endpoint-utterances.md) 

