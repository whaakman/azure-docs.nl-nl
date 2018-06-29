---
title: Zelfstudie - LUIS-app maken voor opvragen van locatiegegevens - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intents' en een hiërarchische entiteit om gegevens te extraheren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: v-geberr
ms.openlocfilehash: 2547407126943161ba604fa2f5e80b9186cae57e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266495"
---
# <a name="tutorial-create-app-that-uses-hierarchical-entity"></a>Zelfstudie: App maken die de entiteit Hierarchical gebruikt
In deze zelfstudie maakt u een app die laat zien hoe u gerelateerde soorten gegevens kunt vinden op basis van context. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiërarchische entiteiten en contextueel geleerde onderliggende elementen 
> * Nieuwe LUIS-app maken voor het domein Travel met de intent Bookflight
> * Intent _None_ toevoegen en voorbeelden van utterances
> * Entiteit van het type Hierarchical toevoegen met onderliggende elementen Origin en Destination
> * App inleren en publiceren
> * Eindpunt van de app opvragen om LUIS JSON-antwoord te zien inclusief hiërarchische onderliggende elementen 

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="purpose-of-the-app-with-this-entity"></a>Doel van de app met deze entiteit
Deze app bepaalt of een gebruiker een vlucht wil boeken. De entiteit Hierarchical wordt gebruikt om binnen de tekst van de gebruiker de locaties, plaats van herkomst en plaats van bestemming te bepalen. 

De entiteit Hierarchical is geschikt voor dit type gegevens omdat het volgende geldt voor de twee soorten gegevens:

* Het zijn beide locaties, meestal uitgedrukt als steden of luchthavencodes.
* Ze hebben beide meestal een unieke woordkeuze rond de woorden om te kunnen bepalen wat de plaats van herkomst is en wat de plaats van bestemming. Voorbeelden van deze woorden zijn: to, headed toward, from, leaving.
* Beide locaties komen vaak in dezelfde utterance voor. 

Het doel van de entiteit **Hierarchical** is het vinden van gerelateerde gegevens binnen de utterance op basis van context. Kijk eens naar de volgende utterance:

```JSON
1 ticket from Seattle to Cairo`
```

In de utterance zijn twee locaties opgegeven. Een is de plaats van herkomst, Seattle, en de andere de plaats van bestemming, Caïro. Deze steden zijn beide belangrijk voor het boeken van een vlucht. Hoewel ze met een entiteit van het type Simple kunnen worden gevonden, zijn ze gerelateerd en zullen ze vaak in dezelfde utterance worden gevonden. Daarom is het zinvol dat ze beide gegroepeerd worden als onderliggende elementen van een entiteit van het type Hierarchical met de naam **Location**. 

Aangezien het entiteiten zijn die via machine learning zijn verkregen, heeft de app voorbeelden van utterances nodig waarin de plaatsen van herkomst en bestemming zijn gelabeld. Hierdoor weet LUIS waar zich de entiteiten in utterances bevinden, hoe lang ze zijn en wat de omringende woorden zijn. 

## <a name="app-intents"></a>Intents van app
De intents zijn categorieën van wat de gebruiker wil. Deze app heeft twee intents: Bookflight en None. De intent [None](luis-concept-intent.md#none-intent-is-fallback-for-app) is doelgericht, om iets aan te geven dat buiten het bereik van de app ligt.  

## <a name="hierarchical-entity-is-contextually-learned"></a>Entiteit van het type Hierarchical wordt contextueel geleerd 
Het doel van de entiteit is het vinden en categoriseren van delen van de tekst in de utterance. Een entiteit van het type [Hierarchical](luis-concept-entity-types.md) is een bovenliggende/onderliggende entiteit op basis van de context van het gebruik. Een persoon kan de steden van herkomst en bestemming in een utterance bepalen op basis van het gebruik van `to` en `from`. Dit is een voorbeeld van contextueel gebruik.  

Voor deze reizen-app extraheert LUIS de locaties van herkomst en bestemming op zo'n manier dat er een standaardboeking kan worden gemaakt en ingevuld. LUIS ondersteunt variaties, afkortingen en spreektaal in utterances. 

Enkele voorbeelden van eenvoudige utterances van gebruikers:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Researve a seat from New York to Paris on the first of April
```

Voorbeelden van afgekorte of spreektaalversies van utterances:

```
LHR tomorrow
SEA to NYC next Monday
LA to MCO spring break
```
 
De entiteit Hierarchical matches de locatie van de plaats van herkomst en van bestemming. Als slechts één onderliggend element (herkomst of bestemming) van een hiërarchische entiteit aanwezig is, wordt dit nog steeds geëxtraheerd. Niet alle onderliggende elementen hoeven te worden gevonden om er maar één, of enkele, te extraheren. 

## <a name="what-luis-does"></a>Wat LUIS doet
Als de intent en entiteiten van de utterance zijn geïdentificeerd, [geëxtraheerd](luis-concept-data-extraction.md#list-entity-data) en geretourneerd in JSON vanaf het [eindpunt](https://aka.ms/luis-endpoint-apis), zit de taak van LUIS erop. De aanroepende toepassing of chatbot verwerkt dit JSON-antwoord en reageert vervolgens op de aanvraag, op de manier waarop de app of chatbot is ontworpen om dit te doen. 

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS][LUIS]. Doe dit bij de [regio][LUIS-regions] waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS][LUIS] de optie **Create new app**.  

    [![](media/luis-quickstart-intent-and-hier-entity/app-list.png "Schermopname van de pagina met de lijst met apps")](media/luis-quickstart-intent-and-hier-entity/app-list.png#lightbox)

3. Typ in het pop-upvenster de naam `MyTravelApp`. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-app.png "Schermopname van het pop-upvenster voor de naam van een nieuwe app")](media/luis-quickstart-intent-and-hier-entity/create-new-app.png#lightbox)

4. Als dat proces is voltooid, ziet u de pagina **Intents** met de intent **None**. 

    [![](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png "Schermopname van de lijst met intents met alleen de intent None")](media/luis-quickstart-intent-and-hier-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Een nieuwe intent maken

1. Selecteer **Create new intent** op de pagina **Intents**. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png "Schermopname van lijst met intents met de knop 'Create new intent' gemarkeerd")](media/luis-quickstart-intent-and-hier-entity/create-new-intent-button.png#lightbox)

2. Voer de naam `BookFlight` in voor de nieuwe intent. Deze intent moet steeds worden geselecteerd als een gebruiker een vlucht wil boeken.

    U maakt een intent om de primaire categorie gegevens te bepalen die u wilt identificeren. Door de categorie een naam te geven, kunnen andere toepassingen die de queryresultaten van LUIS gebruiken aan de hand van die naam een geschikt antwoord vinden of passende maatregelen nemen. LUIS geeft geen antwoord op deze vragen, maar identificeert alleen om wat voor soort informatie er wordt gevraagd in natuurlijke taal. 

    [![](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png "Schermopname van het pop-upvenster voor het maken van een nieuwe intent")](media/luis-quickstart-intent-and-hier-entity/create-new-intent.png#lightbox)

3. Voeg verschillende utterances toe aan de intent `BookFlight` waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

    | Voorbeelden van utterances|
    |--|
    |Book 2 flights from Seattle to Cairo next Monday|
    |Reserve a ticket to London tomorrow|
    |Schedule 4 seats from Paris to London for April 1|

    [![](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png "Schermopname van het invoeren van utterances voor de intent Bookflight")](media/luis-quickstart-intent-and-hier-entity/enter-utterances-on-intent.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Utterances toevoegen aan de intent None

De LUIS-app heeft momenteel geen utterances voor de intent **None**. Er zijn utterances nodig die de app niet hoeft te beantwoorden, dus moeten er utterances worden toegevoegd aan de intent **None**. U mag deze intent niet leeglaten. 

1. Selecteer **Intents** in het linkerpaneel. 

    [![](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png "Schermopname van de intent Bookflight met de knop Intents gemarkeerd")](media/luis-quickstart-intent-and-hier-entity/select-intents-from-bookflight-intent.png#lightbox)

2. Selecteer de intent **None**. Voeg drie utterances toe die een gebruiker misschien kan invoeren, maar die niet relevant zijn voor uw app:

    | Voorbeelden van utterances|
    |--|
    |Cancel!|
    |Good bye|
    |What is going on?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Wanneer de utterance wordt voorspeld voor de intent None
Als LUIS de intent **None** retourneert voor een toepassing die LUIS aanroept (zoals een chatbot), kan de bot hierop reageren door te vragen of de gebruiker het gesprek wil beëindigen. De bot kan ook meer aanwijzingen geven voor het vervolgen van het gesprek als de gebruiker dit niet wil beëindigen. 

Entiteiten werken in de intent **None**. Als de hoogst scorende intent **None** is maar er een entiteit wordt geëxtraheerd die zinvol is voor uw chatbot, kan de chatbot met een vraag komen die is gericht op de intentie van de klant. 

## <a name="create-a-location-entity-from-the-intent-page"></a>Een entiteit van het type Location maken op de pagina Intent
Nu dat de twee intents utterances hebben, moet LUIS begrijpen wat een locatie is. Ga terug naar de intent `BookFlight` en label (markeer) de plaatsnaam in een utterance door deze stappen te volgen:

1. Ga terug naar de intent `BookFlight` door **Intents** te selecteren in het linkerdeelvenster.

2. Selecteer `BookFlight` in de lijst met intents.

3. Selecteer het woord `Seattle` in de utterance, `Book 2 flights from Seattle to Cairo next Monday`. Er wordt een vervolgkeuzelijst weergegeven met bovenaan een tekstvak voor het maken van de nieuwe entiteit. Typ de entiteitsnaam `Location` in het tekstvak en selecteer vervolgens **Create new entity** in de vervolgkeuzelijst. 

    [![](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png "Schermopname van de pagina met de intent Bookflight, voor het maken van een nieuwe entiteit van de geselecteerde tekst")](media/luis-quickstart-intent-and-hier-entity/label-seattle-in-utterance.png#lightbox)

4. Selecteer in het pop-upvenster het entiteittype **Hierarchical** met `Origin` en `Destination` als de onderliggende entiteiten. Selecteer **Done**.

    [![](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png "Schermopname van het pop-upvenster voor het maken van een nieuwe entiteit van het type Location")](media/luis-quickstart-intent-and-hier-entity/hier-entity-ddl.png#lightbox)

    Het label voor `Seattle` is gemarkeerd als `Location` omdat LUIS niet weet of de term verwijst naar de plaats van herkomst of van bestemming, of geen van beide. Selecteer `Seattle`, vervolgens Location en selecteer `Origin` in het menu aan de rechterkant.

5. Nu dat de entiteit is gemaakt, en één utterance een label heeft, geeft u de andere steden een label door de plaatsnaam te selecteren, vervolgens Location en ten slotte `Origin` of `Destination` in het menu aan de rechterkant.

    [![](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png "Schermopname van de entiteit Bookflight met utterancetekst geselecteerd entiteitsselectie")](media/luis-quickstart-intent-and-hier-entity/label-destination-in-utterance.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS niet weet dat de intents en entiteiten (het model) zijn gewijzigd, totdat u de app hebt ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![App inleren](./media/luis-quickstart-intent-and-hier-entity/train-button.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Inleren is voltooid](./media/luis-quickstart-intent-and-hier-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish.png "Schermopname van de intent Bookflight met de knop Publish gemarkeerd")](media/luis-quickstart-intent-and-hier-entity/publish.png#lightbox)

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    [![](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png "Schermopname van de pagina Publish app met de knop Publish gemarkeerd")](media/luis-quickstart-intent-and-hier-entity/publish-to-production.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    [![](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png "Schermopname van de pagina Publish met eindpunt-URL gemarkeerd")](media/luis-quickstart-intent-and-hier-entity/publish-select-endpoint.png#lightbox)

2. Ga naar het einde van de URL in het adres en voer `1 ticket to Portland on Friday` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `BookFlight` als resultaat moet geven met de entiteit Hierarchical geëxtraheerd.

```
{
  "query": "1 ticket to Portland on Friday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9998226
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.9998226
    },
    {
      "intent": "None",
      "score": 0.221926212
    }
  ],
  "entities": [
    {
      "entity": "portland",
      "type": "Location::Destination",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.564448953
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, bestaande uit slechts twee intents en één hiërarchische entiteit, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de geëxtraheerde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om de primaire actie te bepalen, `BookFlight`, en de locatiegegevens die aanwezig zijn in de utterance. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een entiteit van het type List](luis-quickstart-intent-and-list-entity.md) 

Voeg de [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-prebuilt-entity) **number** toe om het aantal te extraheren. 

Voeg de [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-prebuilt-entity) **datetimeV2** toe om de datumgegevens te extraheren.


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
