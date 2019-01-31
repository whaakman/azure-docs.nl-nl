---
title: Hiërarchische entiteit
titleSuffix: Azure Cognitive Services
description: Zoek gerelateerde gegevens op basis van de context. Een bestemmings- en doellocatie voor de fysieke verhuizing tussen twee gebouwen zijn bijvoorbeeld aan elkaar gerelateerd.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: dc8b9831b89aade7ca5dfd3e1f53b9dccc15e66b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55217030"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Zelfstudie: Contextueel gerelateerde gegevens ophalen uit een utterance

In deze zelfstudie zoekt u gerelateerde gegevens op basis van context. Denk bijvoorbeeld aan de locaties van oorsprong en bestemming voor een overplaatsing tussen vestigingen. Hiervoor zijn mogelijk beide gegevenselementen vereist en ze zijn aan elkaar gerelateerd.  

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Nieuwe app maken
> * Intentie toevoegen 
> * Entiteit van het type Hierarchical toevoegen met onderliggende elementen Origin en Destination
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="hierarchical-data"></a>Hiërarchische gegevens

Deze app bepaalt waar een werknemer moet worden verplaatst van de plaats van oorsprong naar de plaats van bestemming. Hierbij wordt de entiteit Hierarchical gebruikt om vast te stellen wat de locaties binnen de utterance zijn. 

De entiteit Hierarchical is geschikt voor dit type gegevens omdat het volgende geldt voor de twee soorten gegevens, wat onderliggende locaties zijn:

* Het zijn entiteiten van het type Simple.
* Ze zijn aan elkaar gerelateerd in de context van de utterance.
* Ze hebben een specifieke woordkeuze ter aanduiding van een entiteit. Voorbeelden van deze woorden zijn: van/naar verlaten/gaan naar, weg van/naar.
* Beide onderliggende elementen komen vaak in dezelfde utterance voor. 
* Moet als eenheid informatie worden gegroepeerd en verwerkt door de client-app.

## <a name="create-a-new-app"></a>Een nieuwe app maken

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Een intentie maken om werknemers te verplaatsen tussen steden

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Selecteer **Create new intent**. 

1. Voer in het pop-updialoogvenster `MoveEmployeeToCity` in en selecteer vervolgens **Done**. 

    ![Schermopname van het pop-updialoogvenster voor het maken van een nieuwe intentie met](./media/luis-quickstart-intent-and-hier-entity/create-new-intent-move-employee-to-city.png)

1. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |move John W. Smith leaving Seattle headed to Dallas|
    |transfer Jill Jones from Seattle to Cairo|
    |Place John Jackson away from Tampa, coming to Atlanta |
    |move Debra Doughtery to Tulsa from Dallas|
    |mv Jill Jones leaving Cairo headed to Tampa|
    |Shift Alice Anderson to Oakland from Redmond|
    |Carl Chamerlin from San Francisco to Redmond|
    |Transfer Steve Standish from San Diego toward Bellevue |
    |lift Tanner Thompson from Kansas city and shift to Chicago|

    [ ![Schermopname van LUIS met nieuwe utterances in de intentie MoveEmployee](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

## <a name="create-a-location-entity"></a>Een entiteit Location maken
LUIS moet kunnen vaststellen wat een locatie is door de herkomst en bestemming in de utterances van labels te voorzien. Als u de utterance in de tokenweergave (onbewerkte weergave) wilt zien, selecteert u de wisselknop op de balk boven de utterances met het label **Entities View**. Door de wisselknop te selecteren, kunt u ervoor zorgen dat voor het besturingselement het label **Entities View** wordt weergegeven.

Kijk eens naar de volgende utterance:

```json
move John W. Smith leaving Seattle headed to Dallas
```

In de utterance zijn twee locaties opgegeven `Seattle` en `Dallas`. Beide locaties zijn gegroepeerd als onderliggende elementen van een hiërarchische entiteit, `Location`, omdat beide gegevenselementen moeten worden opgehaald uit de utterance om de aanvraag in de clienttoepassing te kunnen voltooien. De twee elementen zijn aan elkaar gerelateerd. 
 
Als slechts één onderliggend element (herkomst of bestemming) van een hiërarchische entiteit aanwezig is, wordt dit nog steeds geëxtraheerd. Niet alle onderliggende elementen hoeven te worden gevonden om er maar één, of enkele, te extraheren. 

1. Selecteer het woord `Seattle` in de utterance, `move John W. Smith leaving Seattle headed to Dallas`. Er wordt een vervolgkeuzelijst weergegeven met bovenaan een tekstvak. Typ de entiteitsnaam `Location` in het tekstvak en selecteer vervolgens **Create new entity** in de vervolgkeuzelijst. 

    [![Schermopname van het maken van een nieuwe entiteit op de intentiepagina](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png "Schermopname van het maken van een nieuwe entiteit op de intentiepagina")](media/luis-quickstart-intent-and-hier-entity/create-location-hierarchical-entity-from-example-utterance.png#lightbox)

1. Selecteer in het pop-upvenster het entiteittype **Hierarchical** met `Origin` en `Destination` als de onderliggende entiteiten. Selecteer **Done**.

    ![Schermopname van het pop-upvenster voor het maken van een entiteit voor een nieuwe entiteit Location](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "Schermopname van het pop-upvenster voor het maken van een entiteit voor een nieuwe entiteit Location")

1. Het label voor `Seattle` is gemarkeerd als `Location` omdat LUIS niet weet of de term verwijst naar de plaats van herkomst of van bestemming, of geen van beide. Selecteer `Seattle`, selecteer vervolgens **Location**, vouw het menu naar rechts uit en selecteer `Origin`.

    [![Schermopname van het pop-upvenster voor het labelen van entiteiten om de onderliggende entiteit Locations te wijzigen](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png "Schermopname van het pop-upvenster voor het labelen van entiteiten om de onderliggende entiteit Locations te wijzigen")](media/luis-quickstart-intent-and-hier-entity/choose-hierarchical-child-entity-from-example-utterance.png#lightbox)

1. Geef de overige locaties in alle andere utterances een label. Wanneer alle locaties een label hebben, gaan de utterances eruitzien als een patroon. 

    [![Schermopname van de entiteit Locations die is gelabeld in utterances](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png "Schermopname van de entiteit Locations die is gelabeld in utterances")](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png#lightbox)

    De rode onderstreping geeft aan dat LUIS niet zeker is over de entiteit. Dit kan worden opgelost met training. 

## <a name="add-example-utterances-to-the-none-intent"></a>Voorbeelduitingen toevoegen aan de intentie None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>De app trainen zodat de wijzigingen aan de intentie kunnen worden getest 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>De app publiceren zodat op het getrainde model query's kunnen worden uitgevoerd vanaf het eindpunt

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Voorspelling van intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Ga naar het einde van de URL in de adresbalk en voer `Please move Carl Chamerlin from Tampa to Portland` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `MoveEmployee` als resultaat moet geven met de entiteit Hierarchical geëxtraheerd.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "portland",
          "type": "Location::Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "tampa",
          "type": "Location::Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    De juiste intentie wordt verspeld en de matrix met entiteiten bevat waarden voor zowel de oorsprong als de bestemming in de bijbehorende eigenschap **entities**.
    
## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Gerelateerde informatie

* [Entiteit Hierarchical](luis-concept-entity-types.md) - conceptuele informatie
* [Trainen](luis-how-to-train.md)
* [Hoe u kunt publiceren](luis-how-to-publish-app.md)
* [Testen in de LUIS-portal](luis-interactive-test.md)
* [Rollen versus hiërarchische entiteiten](luis-concept-roles.md#roles-versus-hierarchical-entities)
* [Voorspellingen verbeteren met patronen](luis-concept-patterns.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een nieuwe intentie gemaakt en voorbeelden van utterances toegevoegd voor de contextueel geleerde gegevens van locaties van oorsprong en bestemming. Zodra de app is getraind en gepubliceerd, kan een clienttoepassing die informatie gebruiken om een verplaatsingsticket te maken met de relevante informatie.

> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een samengestelde entiteit](luis-tutorial-composite-entity.md) 
