---
title: 'Zelfstudie 2: Vooraf gedefinieerde intents en entiteiten - gebruik vooraf gedefinieerde algemene utterances - extraheer algemene gegevens in LUIS'
titleSuffix: Azure Cognitive Services
description: Voeg vooraf gemaakte intenties en entiteiten toe aan de zelfstudie-app Human Resources om snel intenties te kunnen voorspellen en gegevens te extraheren. U hoeft utterances niet te labelen met vooraf gedefinieerde entiteiten. De entiteit wordt automatisch gedetecteerd.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 3bad68d1a388a5bc8780df633313206afaadcef9
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422419"
---
# <a name="tutorial-2-identify-common-intents-and-entities"></a>Zelfstudie 2: Algemene intents en entiteiten identificeren
In deze zelfstudie wijzigt u de Human Resources-app. Voeg vooraf gemaakte intenties en entiteiten toe aan de zelfstudie-app Human Resources om snel intenties te kunnen voorspellen en gegevens te extraheren. U hoeft niet alle utterances met vooraf gedefinieerde entiteiten te labelen omdat de entiteit automatisch wordt gedetecteerd.

Met vooraf gecompileerde modellen van algemene onderwerpsdomeinen en gegevenstypen kunt u uw model snel compileren, maar ook een voorbeeld geven van hoe een model eruitziet. 

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * Vooraf gemaakte intenties toevoegen 
> * Vooraf gemaakte entiteiten toevoegen 
> * Trainen 
> * Publiceren 
> * Intenties en entiteiten ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken
Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-intent-only-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `prebuilts`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL. 

## <a name="add-prebuilt-intents"></a>Vooraf gemaakte intenties toevoegen
LUIS biedt verschillende vooraf gemaakte intenties om u te helpen met algemene gebruikersintenties.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **Add prebuilt intent**. 

3. Zoeken naar `Utilities`. 

    [ ![Schermopname van dialoogvenster met vooraf gemaakte intenties met Utilities in het zoekvak](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Selecteer de volgende intenties en selecteer **Done**: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Help


## <a name="add-prebuilt-entities"></a>Vooraf gemaakte entiteiten toevoegen
LUIS biedt verschillende vooraf gemaakte entiteiten voor het ophalen van algemene gegevens. 

1. Selecteer **Entities** in het navigatiemenu aan de linkerkant.

2. Selecteer de knop **Manage prebuilt entity**.

3. Selecteer het **nummer** en **datetimeV2** uit de lijst met vooraf gedefinieerde entiteiten en selecteer vervolgens **Done**.

    ![Schermopname van het selecteren van een nummer in het dialoogvenster met de vooraf gedefinieerde entiteiten](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in de adresbalk van de browser en voer `I want to cancel on March 3` in. De laatste parameter van de queryreeks is `q`, de utterance**query**. 

    ```JSON
    {
      "query": "I want to cancel on March 3",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.7818295
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.7818295
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0237864349
        },
        {
          "intent": "GetJobInformation",
          "score": 0.017576348
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0130122062
        },
        {
          "intent": "Utilities.Help",
          "score": 0.006731322
        },
        {
          "intent": "None",
          "score": 0.00524190161
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.004912514
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00092950504
        }
      ],
      "entities": [
        {
          "entity": "march 3",
          "type": "builtin.datetimeV2.date",
          "startIndex": 20,
          "endIndex": 26,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2018-03-03"
              },
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2019-03-03"
              }
            ]
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 26,
          "endIndex": 26,
          "resolution": {
            "value": "3"
          }
        }
      ]
    }
    ```

    Het resultaat voorspelde de intentie Utilities.Cancel en extraheerde 3 maart als datum en 3 als nummer. 

    Er zijn twee waarden voor 3 maart omdat de utterance niet vermeldt of 3 maart in het verleden of in de toekomst ligt. Het is aan de clienttoepassing om een aanname te doen of om opheldering te vragen als dat nodig is. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

Door vooraf gemaakte intents en entiteiten toe te voegen, kan de clienttoepassing algemene gebruikersintents bepalen en algemene gegevenstypen extraheren. 

> [!div class="nextstepaction"]
> [Een entiteit in de vorm van een reguliere expressie aan de app toevoegen](luis-quickstart-intents-regex-entity.md)

