---
title: Zelfstudie - LUIS-app maken voor het ophalen van gegevens die voldoen aan gewone expressie - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intents' en een entiteit in de vorm van een gewone expressie om gegevens te extraheren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/18/2018
ms.author: v-geberr
ms.openlocfilehash: c31e7d130d02ab6b0fad7577026e557692c2b60e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285947"
---
# <a name="tutorial-use-regular-expression-entity"></a>Zelfstudie: Entiteit in de vorm van een gewone expressie gebruiken
In deze zelfstudie maakt u een app die laat zien hoe u consistent opgemaakte gegevens kunt extraheren uit een utterance met behulp van de entiteit **Regular Expression (regex)**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over entiteit in de vorm van een gewone expressie 
> * LUIS-app gebruiken voor een HR-domein (Human Resources) met de intent FindForm
> * Entiteit in de vorm van een gewone expressie toevoegen om formuliernummer te extraheren uit utterance
> * App inleren en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="before-you-begin"></a>Voordat u begint
Als u niet beschikt over de Human Resources-app uit de snelstartgids over de vooraf gedefinieerde entiteit [custom domain](luis-tutorial-prebuilt-intents-entities.md), [importeert](create-new-app.md#import-new-app) u de JSON in een nieuwe app op de website van [LUIS][LUIS]. Dit kan vanuit de Github-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `regex`. Klonen is een uitstekende manier om te experimenten met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 


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
LUIS tokeniseert de utterance wanneer deze wordt toegevoegd aan een intent. Dit houdt in dat er in deze utterances spaties worden toegevoegd vóór en na het afbreekstreepje, `Where is HRF - 123456?`  De gewone expressie wordt toegepast op de utterance in de onbewerkte vorm, dus voordat deze is getokeniseerd. Omdat de expressie wordt toegepast op de _onbewerkte_ vorm, hoeft er geen rekening te worden gehouden met woordgrenzen. 


## <a name="add-findform-intent"></a>Intent FindForm toevoegen

1. Zorg ervoor dat uw Human Resources-app zich bevindt in de sectie **Build** van LUIS. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

    [ ![Schermopname van LUIS-app met Build gemarkeerd in de navigatiebalk rechtsboven](./media/luis-quickstart-intents-regex-entity/first-image.png)](./media/luis-quickstart-intents-regex-entity/first-image.png#lightbox)

2. Selecteer **Create new intent**. 

    [![Schermopname van pagina Intents met de knop 'Create new intent' gemarkeerd](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png) ](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png#lightbox)

3. Voer in het pop-updialoogvenster `FindForm` in en selecteer vervolgens **Done**. 

    ![Schermopname van dialoogvenster voor het maken van een nieuwe intent met Utilities in het zoekvak](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Voeg voorbeelden van utterances toe aan de intent.

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

    [ ![Schermopname van de intent met de nieuwe utterances gemarkeerd](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    De toepassing is uitgebreid met de vooraf gedefinieerde entiteit Number (uit de vorige zelfstudie), waardoor elk formuliernummer een tag heeft. Dit is mogelijk niet voldoende voor uw clienttoepassing, maar het nummer wordt niet gelabeld met het type nummer. Door een nieuwe entiteit met een geschikte naam te maken, kan de clienttoepassing de entiteit op de juiste manier verwerken wanneer deze wordt geretourneerd door LUIS.

## <a name="create-a-hrf-number-regular-expression-entity"></a>Een entiteit in de vorm van een gewone expressie voor een HRF nummer maken 
Voer de volgende stappen uit om een entiteit in de vorm van een gewone expressie te maken om aan LUIS door te geven wat de indeling van een HRF-nummer is:

1. Selecteer **Entities** in het linkerpaneel.

2. Selecteer **Create new entity** op de pagina Entities. 

    [![Schermopname van pagina Entities met de knop 'Create new entity' gemarkeerd](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png#lightbox)

3. Geef in het pop-updialoogvenster de naam `HRF-number` op voor de nieuwe entiteit, selecteer **RegEx** als het type entiteit, voer `hrf-[0-9]{6}` in als de gewone expressie en selecteer ten slotte **Done**.

    ![Schermopname van pop-updialoogvenster met eigenschappen voor nieuwe entiteit](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecteer **Intents** en selecteer vervolgens de intent **FindForm** om de gewone expressie te zien die is gelabeld in de utterances. 

    [![Schermopname van het labelen van een utterance met een bestaande entiteit en regex-patroon](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Omdat de entiteit niet wordt verkregen via machine learning, wordt het label toegepast op de utterances en weergegeven op de website van LUIS zodra het label is gemaakt.

## <a name="train-the-luis-app"></a>LUIS-app inleren
Het is niet nodig om een entiteit in de vorm van een gewone expressie in te leren. Dit geldt wel voor de nieuwe intent en de utterances. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![Afbeelding van de knop Train](./media/luis-quickstart-intents-regex-entity/train-button.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Afbeelding van melding dat het inleren is voltooid](./media/luis-quickstart-intents-regex-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

    ![Schermopname van FindKnowledgeBase met de knop Publish gemarkeerd in de bovenste navigatiebalk](./media/luis-quickstart-intents-regex-entity/publish-button.png)

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    ![Schermopname van de pagina Publish app met de knop Publish gemarkeerd](./media/luis-quickstart-intents-regex-entity/publish-to-production.png)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Schermopname van de pagina Publish met eindpunt-URL gemarkeerd](./media/luis-quickstart-intents-regex-entity/publish-select-endpoint.png)

2. Ga naar het einde van de URL in het adres en voer `When were HRF-123456 and hrf-234567 published?` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `FindForm` als resultaat moet geven met de twee formuliernummers `HRF-123456` en `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.970179737
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.970179737
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0131893409
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00364777143
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0024568392
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00173760345
        },
        {
          "intent": "None",
          "score": 0.00173070864
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00130692765
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00130328839
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0006671795
        }
      ],
      "entities": [
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
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de entiteit KeyPhrase](luis-quickstart-intent-and-key-phrase.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
