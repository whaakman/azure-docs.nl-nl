---
title: Zelfstudie - LUIS-app maken voor het ophalen van tekstgegevens met exacte overeenkomst - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intents' en een lijstentiteit om gegevens te extraheren.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 33394dff1091f27c79c74d8648a90724ba8d6698
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264824"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Zelfstudie: Een app maken met een lijstentiteit
In deze zelfstudie gaat u een app maken die laat zien hoe u gegevens ophaalt die overeenkomen met een vooraf gedefinieerde lijst. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Algemene informatie over lijstentiteiten 
> * Nieuwe LUIS-app maken voor het domein Beverage met de intent OrderDrinks
> * Intent _None_ toevoegen en voorbeelden van utterances
> * Lijstentiteit toevoegen om drankjes op te halen uit utterance
> * App inleren en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="purpose-of-the-list-entity"></a>Doel van de entiteit List
Deze app accepteert bestellingen van drankjes zoals `1 coke and 1 milk please` en retourneert de gegevens zoals het soort drank. Een entiteit van het type **List** zoekt naar exacte tekstovereenkomsten en retourneert deze overeenkomsten. 

Een lijstentiteit is een goede keuze voor dit type gegevens wanneer de gegevenswaarden deel uitmaken van een bekende set. De namen van drankjes kunnen variëren, inclusief spreektaal en afkortingen, maar de namen zullen niet vaak veranderen. 

## <a name="app-intents"></a>Intents van app
De intents zijn categorieën van wat de gebruiker wil. Deze app heeft twee intents: OrderDrinks en None. De intent [None](luis-concept-intent.md#none-intent-is-fallback-for-app) is doelgericht, om iets aan te geven dat buiten het bereik van de app ligt.  

## <a name="list-entity-is-an-exact-text-match"></a>Lijstentiteit is een exacte overeenkomst
Het doel van de entiteit is het vinden en categoriseren van delen van de tekst in de utterance. Met een entiteit van het type [List](luis-concept-entity-types.md) kunt u zoeken naar een exacte overeenkomst van woorden of zinnen.  

Voor deze bestel-app extraheert LUIS de bestelde drankjes op zo'n manier dat er een standaardbestelling kan worden gemaakt en ingevuld. LUIS ondersteunt variaties, afkortingen en spreektaal in utterances. 

Enkele voorbeelden van eenvoudige utterances van gebruikers:

```
2 glasses of milk
3 bottles of water
2 cokes
```

Voorbeelden van afgekorte of spreektaalversies van utterances:

```
5 milk
3 h2o
1 pop
```
 
De lijstentiteit koppelt `h2o` aan water en `pop` aan fris.  

## <a name="what-luis-does"></a>Wat LUIS doet
Als de intent en entiteiten van de utterance zijn geïdentificeerd, [geëxtraheerd](luis-concept-data-extraction.md#list-entity-data) en geretourneerd in JSON vanaf het [eindpunt](https://aka.ms/luis-endpoint-apis), zit de taak van LUIS erop. De aanroepende toepassing of chatbot verwerkt dit JSON-antwoord en reageert vervolgens op de aanvraag, op de manier waarop de app of chatbot is ontworpen om dit te doen. 

## <a name="create-a-new-app"></a>Een nieuwe app maken
1. Meld u aan op de website van [LUIS][LUIS]. Doe dit bij de [regio][LUIS-regions] waarin u de LUIS-eindpunten wilt publiceren.

2. Selecteer op de website van [LUIS][LUIS] de optie **Create new app**.  

    ![Nieuwe app maken](./media/luis-quickstart-intent-and-list-entity/app-list.png)

3. Typ in het pop-upvenster de naam `MyDrinklist`. 

    ![Geef de app de naam MyDrinkList](./media/luis-quickstart-intent-and-list-entity/create-app-dialog.png)

4. Als dat proces is voltooid, ziet u de pagina **Intents** met de intent **None**. 

    [![](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png "Schermopname van pagina Intents")](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Een nieuwe intent maken

1. Selecteer **Create new intent** op de pagina **Intents**. 

    [![](media/luis-quickstart-intent-and-list-entity/create-new-intent.png "Schermopname van pagina Intents met de knop 'Create new intent' gemarkeerd")](media/luis-quickstart-intent-and-list-entity/create-new-intent.png#lightbox)

2. Voer de naam `OrderDrinks` in voor de nieuwe intent. Deze intent moet steeds worden geselecteerd als een gebruiker een drankje wil bestellen.

    U maakt een intent om de primaire categorie gegevens te bepalen die u wilt identificeren. Door de categorie een naam te geven, kunnen andere toepassingen die de queryresultaten van LUIS gebruiken aan de hand van die naam een geschikt antwoord vinden of passende maatregelen nemen. LUIS geeft geen antwoord op deze vragen, maar identificeert alleen om wat voor soort informatie er wordt gevraagd in natuurlijke taal. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png "Schermopname van het maken van de nieuwe intent OrderDrinks")](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png#lightbox)

3. Voeg verschillende utterances toe aan de intent `OrderDrinks` waarnaar een gebruiker waarschijnlijk zal vragen, zoals:

    | Voorbeelden van utterances|
    |--|
    |Please send 2 cokes and a bottle of water to my room|
    |2 perriers with a twist of lime|
    |h20|

    [![](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png "Schermopname van het invoeren van utterances voor de intent OrderDrinks")](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Utterances toevoegen aan de intent None

De LUIS-app heeft momenteel geen utterances voor de intent **None**. Er zijn utterances nodig die de app niet hoeft te beantwoorden, dus moeten er utterances worden toegevoegd aan de intent **None**. U mag deze intent niet leeglaten. 

1. Selecteer **Intents** in het linkerpaneel. 

    [![](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png "Schermopname van het selecteren van de koppeling Intents in het linkerpaneel")](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png#lightbox)

2. Selecteer de intent **None**. Voeg drie utterances toe die een gebruiker misschien kan invoeren, maar die niet relevant zijn voor uw app:

    | Voorbeelden van utterances|
    |--|
    |Cancel!|
    |Good bye|
    |What is going on?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Wanneer de utterance wordt voorspeld voor de intent None
Als LUIS de intent **None** retourneert voor een toepassing die LUIS aanroept (zoals een chatbot), kan de bot hierop reageren door te vragen of de gebruiker het gesprek wil beëindigen. De bot kan ook meer aanwijzingen geven voor het vervolgen van het gesprek als de gebruiker dit niet wil beëindigen. 

Entiteiten werken in de intent **None**. Als de hoogst scorende intent **None** is maar er een entiteit wordt geëxtraheerd die zinvol is voor uw chatbot, kan de chatbot met een vraag komen die is gericht op de intentie van de klant. 

## <a name="create-a-menu-entity-from-the-intent-page"></a>Een entiteit van het type List maken op de pagina Intent
Nu dat de twee intents utterances hebben, moet LUIS begrijpen wat een drankje is. Ga terug naar de intent `OrderDrinks` en label (markeer) de drankjes in een utterance door deze stappen te volgen:

1. Ga terug naar de intent `OrderDrinks` door **Intents** te selecteren in het linkerdeelvenster.

2. Selecteer `OrderDrinks` in de lijst met intents.

3. Selecteer het woord `water` in de utterance, `Please send 2 cokes and a bottle of water to my room`. Er wordt een vervolgkeuzelijst weergegeven met bovenaan een tekstvak voor het maken van de nieuwe entiteit. Typ de entiteitsnaam `Drink` in het tekstvak en selecteer vervolgens **Create new entity** in de vervolgkeuzelijst. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png "Schermopname van het maken van een nieuwe entiteit door een woord te selecteren in een utterance")](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png#lightbox)

4. Selecteer in het pop-upvenster het entiteitstype **list**. Voeg het synoniem `h20` toe. Selecteer de Enter-toets na elk synoniem. Voeg `perrier` niet toe aan de synoniemenlijst. Dat woord wordt als voorbeeld toegevoegd in de volgende stap. Selecteer **Done**.

    [![](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png "Schermopname van het configureren van een nieuwe entiteit")](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png#lightbox)

5. Nu de entiteit is gemaakt, kunt u de andere synoniemen voor water labelen door het synoniem voor water te selecteren en vervolgens `Drink` te selecteren in de vervolgkeuzelijst. Volg het menu naar rechts en selecteer achtereenvolgens `Set as synonym` en `water`.

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png "Schermopname van het labelen van een utterance met een bestaande entiteit")](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png#lightbox)

## <a name="modify-the-list-entity-from-the-entity-page"></a>De lijstentiteit wijzigen op de pagina Entity
De entiteit Drink is gemaakt, maar bevat onvoldoende items en synoniemen. Als u enkele van de woorden, afkortingen of spreektaal weet, is het sneller om de lijst op de pagina **Entity** in te vullen. 

1. Selecteer **Entities** in het linkerpaneel.

    [![](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png "Schermopname van het selecteren van Entities in het linkerpaneel")](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png#lightbox)

2. Selecteer `Drink` in de lijst met entiteiten.

    [![](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png "Schermopname van het selecteren van de entiteit Drink in de lijst met entiteiten")](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png#lightbox)

3. Typ `Soda pop` in het tekstvak en selecteer Enter. 'Pop' is een term die in Amerika algemeen wordt gebruikt voor frisdrank. Elke cultuur heeft een andere naam of aanduiding voor dit type drankje.

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png "Schermafbeelding van het invoeren van een canonieke naam")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png#lightbox)

4. Voer op dezelfde rij als `Soda pop` synoniemen in zoals: 

    ```
    coke
    cokes
    coca-cola
    coca-colas
    ```

    De synoniemen kunnen zinsdelen, leestekens, bezittelijke voornaamwoorden en meervouden bevatten. Aangezien de entiteit List een exacte tekstovereenkomst is (met uitzondering van hoofdlettergebruik), moeten de synoniemen elke variatie hebben. U kunt de lijst uitbreiden als er meer variaties bekend worden vanuit de querylogboeken of door het bekijken van hits op eindpunten. 

    Dit artikel bevat slechts enkele synoniemen om het voorbeeld kort te houden. Een LUIS-app op productieniveau zou veel meer synoniemen bevatten en regelmatig worden gecontroleerd en uitgebreid. 

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png "Schermopname van het toevoegen van synoniemen")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS niet weet dat de intents en entiteiten (het model) zijn gewijzigd, totdat u de app hebt ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![App inleren](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Inleren is voltooid](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Schermopname van het selecteren van de knop Publish")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Selecteer de slot Production en vervolgens de knop **Publish**. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Schermopname van het selecteren van de slot Production en de knop Publish")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Schermopname van eindpunt-URL op pagina Publish")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Ga naar het einde van de URL in het adres en voer `2 cokes and 3 waters` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `OrderDrinks` als resultaat moet geven met de twee drankjes `cokes` en `waters`.

```
{
  "query": "2 cokes and 3 waters",
  "topScoringIntent": {
    "intent": "OrderDrinks",
    "score": 0.999998569
  },
  "intents": [
    {
      "intent": "OrderDrinks",
      "score": 0.999998569
    },
    {
      "intent": "None",
      "score": 0.23884207
    }
  ],
  "entities": [
    {
      "entity": "cokes",
      "type": "Drink",
      "startIndex": 2,
      "endIndex": 6,
      "resolution": {
        "values": [
          "Soda pop"
        ]
      }
    },
    {
      "entity": "waters",
      "type": "Drink",
      "startIndex": 14,
      "endIndex": 19,
      "resolution": {
        "values": [
          "h20"
        ]
      }
    }
  ]
}
```

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Waar vindt de verwerking van natuurlijke taal plaats in de entiteit List? 
Omdat de lijstentiteit een exacte tekstovereenkomst is, is er geen verwerking van natuurlijke taal (of machine learning) nodig. LUIS maakt gebruik van verwerking van natuurlijke taal (of machine learning) om de juiste best scorende intent te selecteren. Daarnaast kan een utterance een combinatie zijn van meer dan één entiteit of zelfs meer dan één type entiteit. Elke utterance wordt verwerkt voor alle entiteiten in de app, inclusief de verwerking van entiteiten in natuurlijke taal (of verkregen via machine learning), zoals de entiteit **Simple**.

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, bestaande uit slechts twee intents en één lijstentiteit, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de geëxtraheerde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om de primaire actie te bepalen, `OrderDrinks`, en welke soorten drankjes er zijn besteld uit de lijstentiteit Drink. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer hiervoor het menu met de drie punten (...) rechts van de app-naam in de lijst met apps en selecteer vervolgens **Delete**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het toevoegen van een entiteit in de vorm van een reguliere expressie](luis-quickstart-intents-regex-entity.md)

Voeg de [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-prebuilt-entity) **number** toe om het aantal te extraheren. 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
