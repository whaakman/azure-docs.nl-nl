---
title: Zelfstudie - LUIS-app maken voor opvragen van locatiegegevens - Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een eenvoudige LUIS-app maakt met behulp van 'intents' en een hiërarchische entiteit om gegevens te extraheren.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 65c7aabb984ad0a6b3e77d0f98003803821e06cc
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158616"
---
# <a name="tutorial-5-add-hierarchical-entity"></a>Zelfstudie: 5. Een hiërarchische entiteit toevoegen
In deze zelfstudie maakt u een app die laat zien hoe u gerelateerde soorten gegevens kunt vinden op basis van context. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiërarchische entiteiten en contextueel geleerde onderliggende elementen 
> * De LUIS-app in HR-domein (Human Resources) gebruiken 
> * Entiteit van het type Hierarchical toevoegen met onderliggende elementen Origin en Destination
> * App inleren en publiceren
> * Eindpunt van de app opvragen om LUIS JSON-antwoord te zien inclusief hiërarchische onderliggende elementen 

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u geen Human Resources-app uit de zelfstudie over [List-entiteiten](luis-quickstart-intent-and-list-entity.md) hebt, [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de [LUIS](luis-reference-regions.md#luis-website)-website. De app die kan worden geïmporteerd bevindt zich in de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-list-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `hier`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="purpose-of-the-app-with-this-entity"></a>Doel van de app met deze entiteit
Met deze app wordt bepaald waar een werknemer naartoe wordt verplaatst van de locatie van herkomst (gebouw en kantoor) naar de bestemmingslocatie (gebouw en kantoor). Hierbij wordt de entiteit Hierarchical gebruikt om vast te stellen wat de locaties binnen de utterance zijn. 

De entiteit Hierarchical is geschikt voor dit type gegevens omdat het volgende geldt voor de twee soorten gegevens:

* Ze zijn aan elkaar gerelateerd in de context van de utterance.
* Ze hebben een specifieke woordkeuze ter aanduiding van een locatie. Voorbeelden van deze woorden zijn: van/naar verlaten/gaan naar, weg van/naar.
* Beide locaties komen vaak in dezelfde utterance voor. 

Het doel van de entiteit **Hierarchical** is het vinden van gerelateerde gegevens binnen de utterance op basis van context. Kijk eens naar de volgende utterance:

```JSON
mv Jill Jones from a-2349 to b-1298
```
In de utterance zijn twee locaties opgegeven `a-2349` en `b-1298`. Stel dat de letter naar een naam van een gebouw verwijst en het nummer het kantoor in dat gebouw aanduidt. Het is zinvol dat ze beide zijn gegroepeerd als onderliggende elementen van een entiteit Hierarchical, `Locations` omdat beide gegevenselementen moeten worden opgehaald uit de utterance en ze aan elkaar zijn gerelateerd. 
 
Als slechts één onderliggend element (herkomst of bestemming) van een hiërarchische entiteit aanwezig is, wordt dit nog steeds geëxtraheerd. Niet alle onderliggende elementen hoeven te worden gevonden om er maar één, of enkele, te extraheren. 

## <a name="remove-prebuilt-number-entity-from-app"></a>Vooraf gedefinieerde cijferentiteit uit app verwijderen
Om de volledige utterance te zien en de onderliggende entiteiten Hierarchical te kunnen markeren, moet u de vooraf gedefinieerde cijferentiteit tijdelijk verwijderen.

1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

2. Selecteer **Entities** in het menu aan de linkerkant.

3. Selecteer de drie puntjes (***...***) rechts van de cijferentiteit in de lijst. Selecteer **Verwijderen**. 

    [ ![Schermopname van LUIS-app op de lijstpagina met entiteiten met de verwijderknop gemarkeerd voor de vooraf gedefinieerde cijferentiteit](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png)](./media/luis-quickstart-intent-and-hier-entity/hr-delete-number-prebuilt.png#lightbox)


## <a name="add-utterances-to-moveemployee-intent"></a>Uitingen toevoegen aan de intentie MoveEmployee

1. Selecteer **Intents** in het linkermenu.

2. Selecteer **MoveEmployee** in de lijst met intenties.

    [ ![Schermopname van LUIS-app met de intentie MoveEmployee gemarkeerd in het linkermenu](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png)](./media/luis-quickstart-intent-and-hier-entity/hr-intents-list-moveemployee.png#lightbox)

3. De volgende voorbeelden van utterances toevoegen:

    |Voorbeelden van utterances|
    |--|
    |John W. Smith **naar** a-2345 verplaatsen|
    |Jill Jones **naar** b-3499 verplaatsen|
    |De verplaatsing van x23456 **van** hh-2345 **naar** e-0234 organiseren|
    |Beginnen met administratieve taken omdat x12345 a-3459 **verlaat** om **naar** f-34567 te gaan|
    |425-555-0000 **uit** g-2323 **naar** hh- 2345 verplaatsen|

    [ ![Schermopname van LUIS met nieuwe utterances in de intentie MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

    In de zelfstudie [entiteit List](luis-quickstart-intent-and-list-entity.md) kon een werknemer worden aangeduid met zijn of haar naam, e-mailadres, toestelnummer, mobiele-telefoonnummer of het Amerikaans sociaal-fiscaal nummer. Deze werknemernummers worden in de utterances gebruikt. In de voorgaande utterances worden andere manieren gebruikt om de locatie van herkomst en de bestemmingslocatie aan te duiden: vet gemarkeerd. Er zijn slechts een klein aantal utterances die opzettelijk bestemmingen bevatten. Dit helpt LUIS om te begrijpen hoe deze locaties in de utterance worden geplaatst als de locatie van herkomst niet wordt opgegeven.     

## <a name="create-a-location-entity"></a>Een entiteit Location maken
LUIS moet kunnen vaststellen wat een locatie is door de herkomst en bestemming in de utterances van labels te voorzien. Als u de utterance in de tokenweergave (onbewerkte weergave) wilt zien, selecteert u de wisselknop op de balk boven de utterances met het label **Entities View**. Door de wisselknop te selecteren, kunt u ervoor zorgen dat voor het besturingselement het label **Entities View** wordt weergegeven.

1. Selecteer het woord `g-2323` in de utterance, `Displace 425-555-0000 away from g-2323 toward hh-2345`. Er wordt een vervolgkeuzelijst weergegeven met bovenaan een tekstvak. Typ de entiteitsnaam `Locations` in het tekstvak en selecteer vervolgens **Create new entity** in de vervolgkeuzelijst. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png "Schermopname van het maken van een nieuwe entiteit op de intentiepagina")](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-1.png#lightbox)

2. Selecteer in het pop-upvenster het entiteittype **Hierarchical** met `Origin` en `Destination` als de onderliggende entiteiten. Selecteer **Done**.

    ![](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Schermopname van het pop-upvenster voor het maken van een nieuwe entiteit van het type Location")

3. Het label voor `g-2323` is gemarkeerd als `Locations` omdat LUIS niet weet of de term verwijst naar de plaats van herkomst of van bestemming, of geen van beide. Selecteer `g-2323`, vervolgens **Locations** en selecteer `Origin` in het menu aan de rechterkant.

    [![](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png "Schermopname van pop-updialoogvenster om met behulp van labels in een onderliggend entiteit de locaties te wijzigen")](media/luis-quickstart-intent-and-hier-entity/hr-label-entity.png#lightbox)

5. Voorzie de andere locaties in alle andere utterances van labels door het gebouw en kantoor in de utterance te selecteren, vervolgens Locations de selecteren en `Origin` of `Destination` in het rechtermenu te selecteren. Wanneer alle locaties van een label zijn voorzien, beginnen de utterances in **Tokens View** er als een patroon uit te zien. 

    [![](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png "Schermopname van de gelabelde entiteit Locations in utterances")](media/luis-quickstart-intent-and-hier-entity/hr-entities-labeled.png#lightbox)

## <a name="add-prebuilt-number-entity-to-app"></a>Vooraf gedefinieerde cijferentiteit aan app toevoegen
De vooraf gedefinieerde cijferentiteit opnieuw toevoegen aan de toepassing.

1. Selecteer **Entities** in het navigatiemenu aan de linkerkant.

2. Selecteer de knop **Manage prebuilt entities**.

    [ ![Schermopname van de entiteitenlijst met Manage prebuilt entities gemarkeerd](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png)](./media/luis-quickstart-intent-and-hier-entity/hr-manage-prebuilt-button.png#lightbox)

3. Selecteer het **cijfer** uit de lijst met vooraf gedefinieerde entiteiten en selecteer vervolgens **Done**.

    ![Schermopname van het selecteren van een nummer in het dialoogvenster met de vooraf gedefinieerde entiteiten](./media/luis-quickstart-intent-and-hier-entity/hr-add-number-back-ddl.png)

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


2. Ga naar het einde van de URL in de adresbalk en voer `Please relocation jill-jones@mycompany.com from x-2345 to g-23456` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `MoveEmployee` als resultaat moet geven met de entiteit Hierarchical geëxtraheerd.

  ```JSON
  {
    "query": "Please relocation jill-jones@mycompany.com from x-2345 to g-23456",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9966052
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9966052
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0325253047
      },
      {
        "intent": "FindForm",
        "score": 0.006137873
      },
      {
        "intent": "GetJobInformation",
        "score": 0.00462633232
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00415637763
      },
      {
        "intent": "ApplyForJob",
        "score": 0.00382325822
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00249120337
      },
      {
        "intent": "None",
        "score": 0.00130756292
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00119622645
      },
      {
        "intent": "Utilities.Confirm",
        "score": 1.26910036E-05
      }
    ],
    "entities": [
      {
        "entity": "jill - jones @ mycompany . com",
        "type": "Employee",
        "startIndex": 18,
        "endIndex": 41,
        "resolution": {
          "values": [
            "Employee-45612"
          ]
        }
      },
      {
        "entity": "x - 2345",
        "type": "Locations::Origin",
        "startIndex": 48,
        "endIndex": 53,
        "score": 0.8520272
      },
      {
        "entity": "g - 23456",
        "type": "Locations::Destination",
        "startIndex": 58,
        "endIndex": 64,
        "score": 0.974032
      },
      {
        "entity": "-2345",
        "type": "builtin.number",
        "startIndex": 49,
        "endIndex": 53,
        "resolution": {
          "value": "-2345"
        }
      },
      {
        "entity": "-23456",
        "type": "builtin.number",
        "startIndex": 59,
        "endIndex": 64,
        "resolution": {
          "value": "-23456"
        }
      }
    ]
  }
  ```

## <a name="could-you-have-used-a-regular-expression-for-each-location"></a>Kan het zijn dat u een reguliere expressie voor elke locatie hebt gebruikt?
Ja, maak de reguliere expressie met de rollen herkomst en bestemming en gebruik deze in een patroon.

De locaties in dit voorbeeld zoals `a-1234` volgen een specifieke indeling met één of twee letters, dan een streepje en vervolgens een reeks van 4 of 5 cijfers. Deze gegevens kunnen worden aangeduid als een entiteit van een reguliere expressie met een rol voor elke locatie. Rollen zijn beschikbaar voor patronen. U kunt op basis van deze utterances patronen maken en vervolgens een reguliere expressie voor de locatie-indeling maken en deze toevoegen aan de patronen. <!-- Go to this tutorial to see how that is done -->

## <a name="patterns-with-roles"></a>Patronen met rollen

[!INCLUDE [LUIS Compare hierarchical entities to patterns with roles](../../../includes/cognitive-services-luis-hier-roles.md)]

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, bestaande uit slechts een paar intenties en één entiteit Hierarchical, heeft de intentie van een query in natuurlijke taal geïdentificeerd en de geëxtraheerde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om de primaire actie te bepalen, `MoveEmployee`, en de locatiegegevens die aanwezig zijn in de utterance. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een samengestelde entiteit](luis-tutorial-composite-entity.md) 