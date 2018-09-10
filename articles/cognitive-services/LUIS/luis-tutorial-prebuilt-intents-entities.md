---
title: Vooraf gemaakte intenties en entiteiten toevoegen om algemene gegevens te extraheren uit Language Understanding - Azure | Microsoft Docs
description: Leer hoe u verschillende soorten entiteitsgegevens kunt extraheren met behulp van vooraf gemaakte intenties en entiteiten.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: 0e45b659508c71a9f1220ef5e76b9a95438fa1e6
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162237"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Zelfstudie: 2. Vooraf gemaakte intenties en entiteiten toevoegen
Voeg vooraf gemaakte intenties en entiteiten toe aan de zelfstudie-app Human Resources om snel intenties te kunnen voorspellen en gegevens te extraheren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Vooraf gemaakte intenties toevoegen 
* Vooraf gemaakte entiteiten van datetimeV2 en nummer toevoegen
* Trainen en publiceren
* Een query uitvoeren met LUIS en een voorspelling als antwoord ontvangen

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Voordat u begint
Als u niet beschikt over de [Human Resources](luis-quickstart-intents-only.md)-app uit de vorige zelfstudie, [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de website van [LUIS](luis-reference-regions.md#luis-website). Dit kan vanuit de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `prebuilts`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="add-prebuilt-intents"></a>Vooraf gemaakte intenties toevoegen
LUIS biedt verschillende vooraf gemaakte intenties om u te helpen met algemene gebruikersintenties.  

1. Zorg ervoor dat uw app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

2. Selecteer **Add prebuilt domain intent**. 

    [ ![Schermopname van pagina Intents met de knop Add prebuilt domain intent gemarkeerd](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

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

    [ ![Schermopname van de lijst met intenties met Entities gemarkeerd in het linkernavigatiegedeelte](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Selecteer de knop **Manage prebuilt entities**.

    [ ![Schermopname van de entiteitenlijst met Manage prebuilt entities gemarkeerd](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Selecteer het **nummer** en **datetimeV2** uit de lijst met vooraf gedefinieerde entiteiten en selecteer vervolgens **Done**.

    ![Schermopname van het selecteren van een nummer in het dialoogvenster met de vooraf gedefinieerde entiteiten](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>App publiceren naar eindpunt

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-endpoint-with-an-utterance"></a>Eindpunt opvragen met een utterance

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Ga naar het einde van de URL in het adres en voer `I want to cancel on March 3` in. De laatste parameter van de queryreeks is `q`, de utterance**query**. 

    Het resultaat voorspelde de intentie Utilities.Cancel en extraheerde 3 maart als datum en 3 als nummer. 

    ```
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

    Er zijn twee waarden voor 3 maart omdat de utterance niet vermeldt of 3 maart in het verleden of in de toekomst ligt. Het is aan de toepassing die LUIS aanroept, om een aanname te doen of om opheldering te vragen als dat nodig is. 

    Door snel en eenvoudig vooraf gemaakte intenties en entiteiten toe te voegen, kan de clienttoepassing conversatiebeheer toevoegen en algemene gegevenstypen extraheren. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een entiteit in de vorm van een reguliere expressie aan de app toevoegen](luis-quickstart-intents-regex-entity.md)

