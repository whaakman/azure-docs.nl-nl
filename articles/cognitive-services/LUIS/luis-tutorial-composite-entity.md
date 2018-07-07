---
title: Maken van een samengestelde entiteit om uit te pakken van complexe gegevens - Azure | Microsoft Docs
description: Informatie over het maken van een samengestelde entiteit in uw LUIS-app om op te halen van verschillende typen entiteitsgegevens.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: 375b52f9206f55e620d5e664844b8fa1d7249a07
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888742"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Samengestelde entiteit gebruiken om complexe gegevens te extraheren
Deze eenvoudige app heeft twee [intents](luis-concept-intent.md) en over verschillende entiteiten. Het doel is het boek van vluchten, zoals '1 ticket van Seattle naar Cairo op vrijdag' en de details van de reservering als een los stukje van gegevens retourneren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Vooraf gemaakte entiteiten datetimeV2 en nummer toevoegen
* Een samengestelde entiteit maken
* LUIS opvragen en ontvangen van samengestelde entiteitsgegevens

## <a name="before-you-begin"></a>Voordat u begint
* Uw LUIS-app uit de  **[hiërarchische snelstartgids](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Als u nog geen een abonnement, kunt u zich registreren voor een [gratis account](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Samengestelde entiteit is een logische groepering 
Het doel van de entiteit is het vinden en categoriseren van delen van de tekst in de utterance. Een [samengestelde](luis-concept-entity-types.md) entiteit bestaat uit andere Entiteitstypen geleerd van context. Voor deze reis-app waarmee de Vluchtreserveringen, zijn er verschillende soorten gegevens, zoals datums, locaties en het aantal seats. 

De informatie bestaat als afzonderlijke entiteiten voordat een samengestelde wordt gemaakt. Maak een samengestelde entiteit als afzonderlijke entiteiten kunnen logisch worden gegroepeerd en deze logische groepering is het handig om de chatbot of andere LUIS verbruikende toepassing. 

Enkele voorbeelden van eenvoudige utterances van gebruikers:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
De samengestelde entiteit komt overeen met het aantal seats, verzendingslocatie doellocatie en datum. 

## <a name="what-luis-does"></a>Wat LUIS doet
Als de intent en entiteiten van de utterance zijn geïdentificeerd, [geëxtraheerd](luis-concept-data-extraction.md#list-entity-data) en geretourneerd in JSON vanaf het [eindpunt](https://aka.ms/luis-endpoint-apis), zit de taak van LUIS erop. De aanroepende toepassing of chatbot verwerkt dit JSON-antwoord en reageert vervolgens op de aanvraag, op de manier waarop de app of chatbot is ontworpen om dit te doen. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Nummer van de vooraf gemaakte entiteiten en datetimeV2 toevoegen
1. Selecteer de `MyTravelApp` app uit de lijst met apps op de [LUIS](luis-reference-regions.md#luis-website) website.

2. Wanneer de app wordt geopend, selecteert u de **entiteiten** koppeling van de navigatiebalk aan de linkerkant.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Selecteer **Manage prebuilt entities**.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Selecteer in het pop- **getal** en **datetimeV2**.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Selecteer in de volgorde voor de nieuwe entiteiten moet worden opgehaald wordt geplaatst, **Train** in de bovenste navigatiebalk.

    ![De knop Train selecteren](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Gebruik bestaande bedoeling om samengestelde entiteit te maken
1. Selecteer **Intents** in de linkernavigatiebalk. 

    ![Intents pagina selecteren](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Selecteer `BookFlight` uit de **Intents** lijst.  

    ![BookFlight doel selecteren in lijst](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Het aantal en de datetimeV2 vooraf gemaakte entiteiten gelabeld zijn op de uitingen.

3. Voor de utterance `book 2 flights from seattle to cairo next monday`, selecteert u de blauwe `number` entiteit, selecteer vervolgens **verpakken in samengestelde entiteit** in de lijst. Een groene lijn, onder de woorden, volgt de cursor als wordt verplaatst naar de rechterkant, die wijzen op een samengestelde entiteit. Schuif naar rechts om te selecteren van de laatste entiteit van de vooraf gedefinieerde `datetimeV2`, voert u `FlightReservation` in het tekstvak van het pop-upvenster, schakelt u vervolgens **maken nieuwe samengestelde**. 

    ![Samengestelde entiteit maken op de pagina intents](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Een pop-upvenster wordt weergegeven zodat u kunt controleren of de onderliggende samengestelde entiteit. Selecteer **Done**.

    ![Samengestelde entiteit maken op de pagina intents](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>De entiteiten in de samengestelde entiteit verpakken
Zodra de samengestelde entiteit is gemaakt, de resterende uitingen in de samengestelde entiteit van label. Als u wilt dat een zin als een samengestelde entiteit wordt weergegeven, moet u het meest linkse woord selecteert en vervolgens **verpakken in samengestelde entiteit** in de lijst die wordt weergegeven, klikt u vervolgens selecteert u het meest rechtse woord en vervolgens de benoemde samengestelde entiteit `FlightReservation`. Dit is een snelle en vloeiende stap van de selecties, onderverdeeld in de volgende stappen uit:

1. In de utterance `schedule 4 seats from paris to london for april 1`, selecteert u de 4 als aantal vooraf gedefinieerde entiteit.

    ![Selecteer het meest linkse woord](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Selecteer **verpakken in samengestelde entiteit** in de lijst die wordt weergegeven.

    ![Selecteer terugloop in de lijst](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Selecteer het meest rechtse woord. Een groene lijn weergegeven onder de woordgroep die wijzen op een samengestelde entiteit.

    ![Meest rechtse woord selecteren](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Selecteer samengestelde naam `FlightReservation` in de lijst die wordt weergegeven.

    ![Benoemde samengestelde entiteit selecteren](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Voor de laatste utterance verpakken `London` en `tomorrow` met behulp van de instructies in de samengestelde entiteit. 

## <a name="train-the-luis-app"></a>LUIS-app inleren
LUIS niet weet dat de intents en entiteiten (het model) zijn gewijzigd, totdat u de app hebt ingeleerd. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![App inleren](./media/luis-tutorial-composite-entity/train-button.png)

2. Het inleren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het inleren is gelukt.

    ![Inleren is voltooid](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    ![app publiceren](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Eindpunt opvragen met een andere utterance
1. Selecteer onderaan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Eindpunt-URL selecteren](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Ga naar het einde van de URL in het adres en voer `reserve 3 seats from London to Cairo on Sunday` in. De laatste querystring-parameter is `q`, de query utterance. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intent `BookFlight` als resultaat moet geven met de entiteit Hierarchical geëxtraheerd.

```
{
  "query": "reserve 3 seats from London to Cairo on Sunday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.227036044
    }
  ],
  "entities": [
    {
      "entity": "sunday",
      "type": "builtin.datetimeV2.date",
      "startIndex": 40,
      "endIndex": 45,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-03-25"
          },
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-04-01"
          }
        ]
      }
    },
    {
      "entity": "3 seats from london to cairo on sunday",
      "type": "flightreservation",
      "startIndex": 8,
      "endIndex": 45,
      "score": 0.6892485
    },
    {
      "entity": "cairo",
      "type": "Location::Destination",
      "startIndex": 31,
      "endIndex": 35,
      "score": 0.557570755
    },
    {
      "entity": "london",
      "type": "Location::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.8933808
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "3"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "flightreservation",
      "value": "3 seats from london to cairo on sunday",
      "children": [
        {
          "type": "builtin.datetimeV2.date",
          "value": "sunday"
        },
        {
          "type": "Location::Destination",
          "value": "cairo"
        },
        {
          "type": "builtin.number",
          "value": "3"
        },
        {
          "type": "Location::Origin",
          "value": "london"
        }
      ]
    }
  ]
}
```

Deze utterance retourneert een samengestelde entiteiten matrix met inbegrip van de **flightreservation** object met gegevens die worden geëxtraheerd.  

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app, met slechts twee intents en een samengestelde entiteit een natuurlijke taal query voornemen geïdentificeerd en de opgehaalde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om te bepalen van de primaire actie `BookFlight`, en de reserveringsinformatie gevonden in de utterance. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over entiteiten](luis-concept-entity-types.md). 
