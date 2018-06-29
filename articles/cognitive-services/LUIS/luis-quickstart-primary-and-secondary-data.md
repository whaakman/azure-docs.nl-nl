---
title: Zelfstudie - LUIS-app maken voor het extraheren van gegevens - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intents' en de entiteit Simple om via machine learning verkregen gegevens te extraheren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: v-geberr
ms.openlocfilehash: 1e8647e34da3d34946a4f6ac298017f6d4c99de6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265357"
---
# <a name="tutorial-create-app-that-uses-simple-entity"></a>Zelfstudie: App maken die de entiteit Simple gebruikt
In deze zelfstudie maakt u een app die laat zien hoe u met behulp van de entiteit **Simple** via machine learning verkregen gegevens extraheert uit een 'utterance'.

<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over entiteiten van het type Simple 
> * Nieuwe LUIS-app maken voor het domein Communication met de intent SendMessage
> * Intent _None_ toevoegen en voorbeelden van utterances
> * Entiteit Simple toevoegen om berichtinhoud te extraheren uit utterance
> * App inleren en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="purpose-of-the-app"></a>Doel van de app
Deze app laat zien hoe u gegevens extraheert uit een utterance. Laten we de volgende utterance uit een chatbot als voorbeeld nemen:

```JSON
Send a message telling them to stop
```

De bedoeling (intent) is om een bericht te verzenden. De belangrijke gegevens van de utterance bestaan uit het bericht zelf,  `telling them to stop`.  

## <a name="purpose-of-the-simple-entity"></a>Doel van de entiteit Simple
Het doel van de entiteit Simple is om LUIS te leren wat een bericht is en waar dit kan worden gevonden in een utterance. Het deel van de utterance dat het bericht vormt, kan per utterance verschillen afhankelijk van woordkeuze en lengte van de utterance. LUIS heeft voorbeelden nodig van berichten in utterances voor alle intents.  

Voor deze eenvoudige app staat het bericht aan het einde van de utterance. 

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS][LUIS]. Doe dit bij de regio waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS][LUIS] de optie **Create new app**.  

    ![Lijst met apps van LUIS](./media/luis-quickstart-primary-and-secondary-data/app-list.png)

3. Typ in het pop-upvenster de naam `MyCommunicator`. 

    ![Lijst met apps van LUIS](./media/luis-quickstart-primary-and-secondary-data/create-new-app-dialog.png)

4. Als dat proces is voltooid, ziet u de pagina **Intents** met de intent **None**. 

    [![](media/luis-quickstart-primary-and-secondary-data/intents-list.png "Schermopname van de pagina Intents van LUIS met de intent None")](media/luis-quickstart-primary-and-secondary-data/intents-list.png#lightbox)

## <a name="create-a-new-intent"></a>Een nieuwe intent maken

1. Selecteer **Create new intent** op de pagina **Intents**. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png "Schermopname van LUIS met de knop 'Create new intent' gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png#lightbox)

2. Voer de naam `SendMessage` in voor de nieuwe intent. Deze intent moet steeds worden geselecteerd als een gebruiker een bericht wil versturen.

    U maakt een intent om de primaire categorie gegevens te bepalen die u wilt identificeren. Door de categorie een naam te geven, kunnen andere toepassingen die de queryresultaten van LUIS gebruiken aan de hand van die naam een geschikt antwoord vinden of passende maatregelen nemen. LUIS geeft geen antwoord op deze vragen, maar identificeert alleen om wat voor soort informatie er wordt gevraagd in natuurlijke taal. 

    ![SendMessage opgeven als naam voor intent](./media/luis-quickstart-primary-and-secondary-data/create-new-intent-popup-dialog.png)

3. Voeg zeven utterances toe aan de intent `SendMessage` waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

    | Voorbeelden van utterances|
    |--|
    |Reply with I got your message, I will have the answer tomorrow|
    |Send message of When will you be home?|
    |Text that I am busy|
    |Tell them that it needs to be done today|
    |IM that I am driving and will respond later|
    |Compose message to David that says When was that?|
    |say greg hello|

    [![](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png "Schermopname van LUIS met ingevoerde utterances")](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Utterances toevoegen aan de intent None

De LUIS-app heeft momenteel geen utterances voor de intent **None**. Er zijn utterances nodig die de app niet hoeft te beantwoorden, dus moeten er utterances worden toegevoegd aan de intent **None**. U mag deze intent niet leeglaten. 
    
1. Selecteer **Intents** in het linkerpaneel. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png "Schermopname van LUIS met de knop Intents gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png#lightbox)

2. Selecteer de intent **None**. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png "Schermopname van het selecteren van de intent None")](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png#lightbox)

3. Voeg drie utterances toe die een gebruiker misschien kan invoeren, maar die niet relevant zijn voor uw app. Enkele voorbeelden van goede utterances voor **None**:

    | Voorbeelden van utterances|
    |--|
    |Cancel!|
    |Good bye|
    |What is going on?|
    
    Als LUIS de intent **None** retourneert voor een toepassing die LUIS aanroept, zoals een chatbot, kan de bot hierop reageren door te vragen of de gebruiker het gesprek wil beëindigen. De bot kan ook meer aanwijzingen geven voor het vervolgen van het gesprek als de gebruiker dit niet wil beëindigen. 

    [![](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png "Schermopname van LUIS met utterances voor de intent None")](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png#lightbox)

## <a name="create-a-simple-entity-to-extract-message"></a>Een entiteit van het type Simple maken om een bericht te extraheren 
1. Selecteer **Intents** in het linkermenu.

    ![Selecteer de koppeling Intents](./media/luis-quickstart-primary-and-secondary-data/select-intents-from-none-intent.png)

2. Selecteer `SendMessage` in de lijst met intents.

    ![Selecteer de intent SendMessage](./media/luis-quickstart-primary-and-secondary-data/select-sendmessage-intent.png)

3. Selecteer in de utterance `Reply with I got your message, I will have the answer tomorrow` het eerste woord van de berichttekst, `I`, en het laatste woord van de berichttekst, `tomorrow`. Deze woorden worden geselecteerd voor het bericht en er verschijnt een vervolgkeuzelijst met bovenaan een tekstvak.

    [![](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png "Schermopname van geselecteerde woorden in utterance voor bericht")](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png#lightbox)

4. Typ `Message` als naam van de entiteit in het tekstvak.

    [![](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png "Schermopname van invoeren van entiteitnaam in vak")](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png#lightbox)

5. Selecteer **Create new entity** in de vervolgkeuzelijst. Het doel van de entiteit is het extraheren van de tekst die de hoofdtekst van het bericht vormt. In deze LUIS-app staat het tekstbericht aan het einde van de utterance, maar de utterance kan elke lengte hebben, evenals het bericht. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png "Schermopname van het maken van een nieuwe entiteit van utterance")](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png#lightbox)

6. In het pop-upvenster is het standaardtype voor de entiteit **Simple** en de naam van de entiteit is `Message`. Neem deze over en selecteer **Done**.

    ![Type entiteit controleren](./media/luis-quickstart-primary-and-secondary-data/entity-type.png)

7. Nu de entiteit is gemaakt en één utterance een label heeft, kunt u de rest van de utterances labelen met die entiteit. Selecteer een utterance, en selecteer vervolgens het eerste en laatste woord van een bericht. Selecteer in de vervolgkeuzelijst de entiteit `Message`. Het bericht is nu gelabeld in de entiteit. Label alle berichtzinnen in de resterende utterances.

    [![](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png "Schermopname van labelen van alle bericht-utterances")](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png#lightbox)

    De standaardweergave van de utterances is **Entities View**. Selecteer het besturingselement **Entities View** boven de utterances. In de **Tokens View** wordt de tekst van de utterances weergegeven. 

    [![](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png "Schermopname van utterances in de Tokens View")](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS niet weet dat de intents en entiteiten (het model) zijn gewijzigd, totdat u de app hebt ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![De knop Train selecteren](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Melding dat inleren is gelukt](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Schermopname van de pagina Publish app met de knop Publish gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Schermopname van de pagina Publish met eindpunt gemarkeerd")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. Ga naar het einde van de URL in het adres en voer `text I'm driving and will be 30 minutes late to the meeting` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de utterances van `SendMessage` als resultaat moet geven.

```
{
  "query": "text I'm driving and will be 30 minutes late to the meeting",
  "topScoringIntent": {
    "intent": "SendMessage",
    "score": 0.987501
  },
  "intents": [
    {
      "intent": "SendMessage",
      "score": 0.987501
    },
    {
      "intent": "None",
      "score": 0.111048922
    }
  ],
  "entities": [
    {
      "entity": "i ' m driving and will be 30 minutes late to the meeting",
      "type": "Message",
      "startIndex": 5,
      "endIndex": 58,
      "score": 0.162995353
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, bestaande uit slechts twee intents en één entiteit, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de berichtgegevens geretourneerd. 

Het JSON-resultaat identificeert de hoogst scorende intent `SendMessage` met een score van 0.987501. Alle scores liggen tussen 1 en 0, met de betere score dichterbij 1. De score van de intent `None` is 0.111048922, veel dichter bij nul. 

De berichtgegevens heeft een type `Message`, evenals een waarde, `i ' m driving and will be 30 minutes late to the meeting`. 

Uw chatbot heeft nu voldoende gegevens om de primaire actie te bepalen, `SendMessage`, en een parameter van deze actie, de tekst van het bericht. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om het bericht via de API van een derde partij te verzenden. Als er andere programmatische opties bestaan voor de bot of de aanroepende toepassing, wordt die taak niet uitgevoerd door LUIS. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het toevoegen van een hiërarchische entiteit](luis-quickstart-intent-and-hier-entity.md)


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
