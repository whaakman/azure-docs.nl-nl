---
title: 'Zelfstudie 9: gevoel analyseren (inclusief positief, negatief en neutraal) in LUIS'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie maakt u een app die laat zien hoe u positieve, negatieve en neutrale gevoelens kunt extraheren uit utterances. Het gevoel wordt bepaald op basis van de gehele uiting.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 95d1c4ffe76cf4c652f347014a838f1250c0ca15
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277474"
---
# <a name="tutorial-9--extract-sentiment-of-overall-utterance"></a>Zelfstudie 9: het gevoel van een volledige uiting herkennen
In deze zelfstudie maakt u een app die laat zien hoe u positieve, negatieve en neutrale gevoelens kunt extraheren uit utterances. Het gevoel wordt bepaald op basis van de gehele uiting.

Sentimentanalyse is de mogelijkheid om te bepalen of de uiting (utterance) van een gebruiker positief, negatief of neutraal is. 

De volgende utterances zijn voorbeelden van gevoelens:

|Sentiment|Score|Utterance|
|:--|:--|:--|
|positief|0,91 |John W. Smith heeft een geweldige presentatie gegeven in Parijs.|
|positief|0,84 |jill-jones@mycompany.com heeft het fantastisch gedaan om Parker als klant binnen te halen.|

Sentimentanalyse is een publicatie-instelling die voor elke uiting geldt. U hoeft niet op zoek te gaan naar woorden om in de utterance een sentiment aan te duiden en deze van een label te voorzien, omdat sentimentanalyse voor de hele utterance geldt. 

Omdat dit een publicatie-instelling is, wordt deze niet weergegeven op de intentie- of entiteitenpagina. De instelling is te zien in het deelvenster [Interactieve test](luis-interactive-test.md#view-sentiment-results) of bij het uitvoeren van tests bij de eindpunt-URL. 

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken 
> * Sentimentanalyse toevoegen als publicatie-instelling
> * Trainen
> * Publiceren
> * Het gevoel achter een uiting ophalen van een eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken

Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-keyphrase-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `sentiment`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.

## <a name="employeefeedback-intent"></a>De intentie EmployeeFeedback 
Voeg een nieuwe intentie toe om feedback van werknemers die tot het bedrijf behoren, vast te leggen. 

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **Create new intent**.

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

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>App configureren voor gebruik van sentimentanalyse
1. Selecteer **Beheren** in het navigatievenster rechtsboven en selecteer in het menu links de optie **Publicatie-instellingen**.

2. Klik op de knop bij **Sentimentanalyse** om deze instelling in te schakelen. 

    ![](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-sentiment-of-utterance-from-endpoint"></a>Het gevoel achter een uiting ophalen van een eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `Jill Jones work with the media team on the public portal was amazing` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intentie `EmployeeFeedback` als resultaat moet geven met de sentimentanalyse geëxtraheerd.
    
    ```JSON
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

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie voegt u Sentimentanalyse toe als publicatie-instelling om de gevoelswaarden uit gehele uitingen op te halen.

> [!div class="nextstepaction"] 
> [Eindpuntuitingen beoordelen in de HR-app](luis-tutorial-review-endpoint-utterances.md) 

