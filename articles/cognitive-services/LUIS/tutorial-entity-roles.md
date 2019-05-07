---
title: Contextuele gegevens met de rol - Language Understanding
titleSuffix: Azure Cognitive Services
description: Gerelateerde gegevens op basis van context vinden. Een bestemmings- en doellocatie voor de fysieke verhuizing tussen twee gebouwen zijn bijvoorbeeld aan elkaar gerelateerd.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a0ab928ef3b8551e3e20ff3c4b16533c80ee4b7d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149247"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Zelfstudie: Contextueel gerelateerde gegevens ophalen uit een utterance

In deze zelfstudie zoekt u gerelateerde gegevens op basis van context. Denk bijvoorbeeld aan de locaties van oorsprong en bestemming voor een overplaatsing tussen vestigingen. Hiervoor zijn mogelijk beide gegevenselementen vereist en ze zijn aan elkaar gerelateerd.  

Een rol kan worden gebruikt met een vooraf gedefinieerde of aangepaste entiteit, en worden gebruikt in zowel voorbeeld uitingen en patronen. 

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Nieuwe app maken
> * Intentie toevoegen 
> * Bron en doel-informatie met behulp van rollen
> * Trainen
> * Publiceren
> * Intenties en entiteiten rollen ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Verwante gegevens

Deze app bepaalt waar een werknemer moet worden verplaatst van de plaats van oorsprong naar de plaats van bestemming. Hierbij een GeographyV2 vooraf gedefinieerde entiteit om de plaatsnamen en deze te identificeren gebruik van rollen om te bepalen welke locatie (bron en doel) binnen de utterance.

Een rol moet worden gebruikt wanneer de entiteitsgegevens om op te halen:

* Is gerelateerd aan elkaar in de context van de utterance.
* Maakt gebruik van specifieke word keuze om aan te geven van elke rol. Voorbeelden van deze woorden zijn: van/naar verlaten/gaan naar, weg van/naar.
* Beide rollen zijn vaak in de dezelfde utterance, zodat van LUIS om van dit regelmatig contextuele gebruik te leren.
* Moet als eenheid informatie worden gegroepeerd en verwerkt door de client-app.

## <a name="create-a-new-app"></a>Een nieuwe app maken

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Een intentie maken om werknemers te verplaatsen tussen steden

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Selecteer **Create new intent**. 

1. Voer in het pop-updialoogvenster `MoveEmployeeToCity` in en selecteer vervolgens **Done**. 

    ![Schermopname van het pop-updialoogvenster voor het maken van een nieuwe intentie met](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |John W. Smith verlaten Seattle onder leiding aan Orlando verplaatsen|
    |transfer Jill Jones from Seattle to Cairo|
    |Place John Jackson away from Tampa, coming to Atlanta |
    |Debra Doughtery verplaatsen naar Tulsa vanuit Chicago|
    |mv Jill Jones leaving Cairo headed to Tampa|
    |Shift Alice Anderson to Oakland from Redmond|
    |Carl Chamerlin from San Francisco to Redmond|
    |Transfer Steve Standish from San Diego toward Bellevue |
    |lift Tanner Thompson from Kansas city and shift to Chicago|

    [![Schermopname van LUIS met nieuwe uitingen in de intentie MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)](./media/tutorial-entity-roles/hr-enter-utterances.png#lightbox)

## <a name="add-prebuilt-entity-geographyv2"></a>Vooraf gedefinieerde entiteit geographyV2 toevoegen

De vooraf gedefinieerde entiteit, geographyV2, haalt de locatie-informatie, waaronder plaatsnamen. Aangezien de uitingen twee plaatsnamen hebben, met betrekking tot elkaar in de context, kunt u rollen gebruiken om op te halen die context.

1. Selecteer **entiteiten** in de navigatie aan de linkerkant.

1. Selecteer **vooraf gedefinieerde entiteit toevoegen**en selecteer vervolgens `geo` in de zoekbalk om te filteren van de vooraf gemaakte entiteiten. 

    ![Toevoegen van geographyV2 vooraf gedefinieerde entiteit naar app](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)
1. Schakel het selectievakje in en selecteer **gedaan**.
1. In de **entiteiten** in de lijst met de **geographyV2** openen van de nieuwe entiteit. 
1. Twee rollen toevoegen `Origin`, en `Destination`. 

    ![Rollen toevoegen aan bestaande entiteit](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)
1. Selecteer **Intents** in de navigatie aan de linkerkant, selecteert u vervolgens de **MoveEmployeeToCity** intentie. U ziet de plaatsnamen zijn gelabeld met de vooraf gedefinieerde entiteit **geogrpahyV2**.
1. Selecteer in de eerste utterance van de lijst de verzendingslocatie. Een vervolgkeuzelijst wordt weergegeven. Selecteer **geographyV2** Volg het menu over meerdere te selecteren in de lijst **oorsprong**.
1. Gebruik de methode van de vorige stap voor alle rollen van de locaties in alle uitingen markeren. 


## <a name="add-example-utterances-to-the-none-intent"></a>Voorbeelduitingen toevoegen aan de intentie None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>De app trainen zodat de wijzigingen aan de intentie kunnen worden getest 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>De app publiceren zodat op het getrainde model query's kunnen worden uitgevoerd vanaf het eindpunt

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Voorspelling van intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Ga naar het einde van de URL in de adresbalk en voer `Please move Carl Chamerlin from Tampa to Portland` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde uitingen zodat het is een goede test en moet de `MoveEmployee` intentie aan de entiteit hebt uitgepakt.

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
          "entity": "geographyV2",
          "role": "Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "geographyV2",
          "role": "Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    De juiste intentie wordt voorspeld en de entiteiten is ingesteld voor de oorsprong en de bestemming rollen in de bijbehorende **entiteiten** eigenschap.
    
## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Gerelateerde informatie

* [Concepten van entiteiten](luis-concept-entity-types.md)
* [Concepten van rollen](luis-concept-roles.md)
* [Lijst met vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md)
* [Trainen](luis-how-to-train.md)
* [Hoe u kunt publiceren](luis-how-to-publish-app.md)
* [Testen in de LUIS-portal](luis-interactive-test.md)
* [Rollen](luis-concept-roles.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een nieuwe intentie gemaakt en voorbeelden van utterances toegevoegd voor de contextueel geleerde gegevens van locaties van oorsprong en bestemming. Zodra de app is getraind en gepubliceerd, kan een clienttoepassing die informatie gebruiken om een verplaatsingsticket te maken met de relevante informatie.

> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een samengestelde entiteit](luis-tutorial-composite-entity.md) 
