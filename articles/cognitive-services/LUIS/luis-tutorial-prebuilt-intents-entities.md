---
title: Vooraf gedefinieerde intents en entiteiten om op te halen van algemene gegevens in het Language Understanding - Azure toevoegen | Microsoft Docs
description: Informatie over het uitpakken van verschillende soorten entiteitsgegevens met vooraf gedefinieerde intents en entiteiten.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 06/11/2018
ms.author: v-geberr
ms.openlocfilehash: 20950ced66497fb0dc96365975b37f244f677ce3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266376"
---
# <a name="use-prebuilt-intents-and-entities-to-handle-common-intents-and-data"></a>Gebruik van vooraf gedefinieerde intents en entiteiten voor de verwerking van algemene intents en gegevens
Vooraf gedefinieerde intents en entiteiten toevoegen aan de Human Resources Quick Start-app snel krijgen opzet voorspelling en gegevens uitpakken. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Vooraf gedefinieerde intents toevoegen 
* Vooraf gedefinieerde entiteiten datetimeV2 en nummer toevoegen
* Trainen en publiceren
* LUIS opvragen en voorspelling reactie ontvangen

## <a name="before-you-begin"></a>Voordat u begint
Als u nog geen Human Resources-app uit de [aangepast domein](luis-quickstart-intents-only.md) Quick Start, [importeren](create-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS] [ LUIS] website , uit de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json) Github-opslagplaats.

Als u behouden de oorspronkelijke Human Resources-app wilt, het klonen van de versie op de [instellingen](luis-how-to-manage-versions.md#clone-a-version) pagina en noem deze `prebuilts`. Het klonen is een uitstekende manier om af te spelen met verschillende LUIS functies zonder dat de oorspronkelijke versie. 

## <a name="add-prebuilt-intents"></a>Vooraf gedefinieerde intents toevoegen
LUIS biedt verschillende vooraf gedefinieerde intents om te helpen bij algemene bedoelingen van de gebruiker.  

1. Zorg ervoor dat uw app wordt de **bouwen** sectie van LUIS. U kunt wijzigen in deze sectie door te selecteren **bouwen** rechtermuisknop op de bovenste menubalk. 

    [ ![Schermopname van LUIS app met Build hightlighted in de bovenste, rechts navigatiebalk](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

2. Selecteer **intentie van de vooraf gedefinieerde domein toevoegen**. 

    [ ![Schermopname van Intents pagina met vooraf gedefinieerde domein opzet knop toevoegen gemarkeerd](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

3. Zoeken naar `Utilities`. 

    [ ![Schermafbeelding van dialoogvenster vooraf gedefinieerde intents met hulpprogramma's in het zoekvak](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Selecteer de volgende intents en selecteer **gedaan**: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.Stop
    * Utilities.StartOver

## <a name="add-prebuilt-entities"></a>Vooraf gedefinieerde entiteiten toevoegen
LUIS biedt verschillende vooraf gedefinieerde entiteiten voor het ophalen van algemene gegevens. 

1. Selecteer **entiteiten** in het linkernavigatievenster-menu.

    [ ![Schermopname van Intents lijst met entiteiten die zijn gemarkeerd in het linkernavigatievenster](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Selecteer **vooraf gemaakte entiteiten beheren** knop.

    [ ![Schermopname van entiteiten lijst met vooraf gedefinieerde entiteiten gemarkeerd beheren](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Selecteer **getal** en **datetimeV2** uit de lijst met vooraf gedefinieerde entiteiten Selecteer **gedaan**.

    ![Schermopname van nummer is geselecteerd in het dialoogvenster van de vooraf gedefinieerde entiteiten](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Trainen en publiceer de app
1. Selecteer in het bovenste aan de rechterkant van de website LUIS, de **Train** knop. 

    ![Knop Train](./media/luis-quickstart-intents-only/train-button.png)

    Training is voltooid wanneer u de groene statusbalk aan de bovenkant van de website bevestiging ziet.

    ![Getraind statusbalk](./media/luis-quickstart-intents-only/trained.png)

2. Selecteer in de rechterbovenhoek, rechts van de website LUIS de **publiceren** te openen van de pagina publiceren. De productiesite is standaard geselecteerd. Selecteer de **publiceren** knop door de keuze van de sleuf productie. Het publiceren is voltooid wanneer u de groene statusbalk aan de bovenkant van de website bevestiging ziet.

    U beschikt niet over een sleutel LUIS in de Azure portal maken voordat u publiceren of de eindpunt-URL te testen. Elke app LUIS heeft een gratis starter-sleutel voor het ontwerpen. Dit biedt u onbeperkte ontwerpen en een [enkele eindpunt treffers](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-with-an-utterance"></a>Query-eindpunt met een utterance
Op de **publiceren** pagina de **eindpunt** koppeling aan de onderkant van de pagina. Deze actie wordt een ander browservenster geopend met de eindpunt-URL in de adresbalk. Ga naar het einde van de URL in het adres en voer `I want to cancel on March 3`. De laatste parameter van de query-tekenreeks is `q`, de utterance **query**. 

Het resultaat de bedoeling Utilities.Cancel voorspeld en de datum van 3 maart en het getal 3 hebt uitgepakt. 

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

Door eenvoudig en snel toe te voegen vooraf gedefinieerde intents en entiteiten, de clienttoepassing conversatie management toevoegen en uitpakken van veelvoorkomende gegevenstypen. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over entiteiten](luis-concept-entity-types.md). 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
