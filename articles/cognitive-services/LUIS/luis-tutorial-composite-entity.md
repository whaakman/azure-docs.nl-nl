---
title: Maak een samengestelde entiteit om uit te pakken complexe gegevens - Azure | Microsoft Docs
description: Informatie over het maken van een samengestelde entiteit in uw app LUIS uitpakken van verschillende soorten entiteitsgegevens.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: cb581ee60dea2b0810332933455a03a8b68e16ea
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264382"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Samengestelde entiteit gebruiken om complexe gegevens te extraheren
Deze eenvoudige app heeft twee [intents](luis-concept-intent.md) en meerdere entiteiten. Het doel is het boek vlucht zoals '1 ticket van Seattle Caïro op vrijdag' en de details van de reservering als een los stukje van gegevens retourneren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* Vooraf gedefinieerde entiteiten datetimeV2 en nummer toevoegen
* Maak een samengestelde entiteit
* LUIS opvragen en ontvangen van samengestelde entiteitsgegevens

## <a name="before-you-begin"></a>Voordat u begint
* Uw app LUIS uit de  **[hiërarchische Quick Start](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Als u nog geen een abonnement, kunt u registreren voor een [gratis account](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Samengestelde entiteit is een logische groepering 
Het doel van de entiteit is om te zoeken en delen van de tekst in de utterance categoriseren. Een [samengestelde](luis-concept-entity-types.md) entiteit bestaat uit andere Entiteitstypen geleerd uit context. Er zijn verschillende soorten informatie zoals datums, locaties en aantal seats voor deze app reizen waarmee Vluchtreserveringen. 

De informatie bestaat als afzonderlijke entiteiten voordat een samengestelde wordt gemaakt. Maak een samengestelde entiteit als afzonderlijke entiteiten kunnen logisch worden gegroepeerd en deze logische groepering nuttig zijn voor de chatbot of andere LUIS verbruikende toepassing is. 

Eenvoudige voorbeeld utterances van gebruikers zijn onder andere:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
De samengestelde entiteit komt overeen met seat aantal, de locatie van de oorsprong, doellocatie en datum. 

## <a name="what-luis-does"></a>Wat LUIS doet
Als de bedoeling en entiteiten van de utterance zijn geïdentificeerd, [uitgepakt](luis-concept-data-extraction.md#list-entity-data), en geretourneerd in de JSON van de [eindpunt](https://aka.ms/luis-endpoint-apis), LUIS wordt uitgevoerd. De aanroepende toepassing of chatbot neemt deze JSON-antwoord en voldoet aan de aanvraag--op welke manier de app of chatbot is ontworpen om u te doen. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Aantal vooraf gedefinieerde entiteiten en datetimeV2 toevoegen
1. Selecteer de `MyTravelApp` -app uit de lijst met apps op de [LUIS] [ LUIS] website.

2. Wanneer de app wordt geopend, selecteert u de **entiteiten** linkernavigatievenster koppeling.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Selecteer **vooraf gemaakte entiteiten beheren**.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Selecteer in het pop- **getal** en **datetimeV2**.

    ![Selecteer de knop entiteiten](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Selecteer in de volgorde voor de nieuwe entiteiten moet worden geëxtraheerd, **Train** in de bovenste navigatiebalk.

    ![Selecteer train-knop](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Bestaande bedoeling gebruiken voor het maken van samengestelde entiteit
1. Selecteer **Intents** van de linkernavigatiebalk. 

    ![Intents pagina selecteren](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Selecteer `BookFlight` van de **Intents** lijst.  

    ![BookFlight doel selecteren in lijst](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Het aantal en de datetimeV2 vooraf gemaakte entiteiten worden aangeduid op de utterances.

3. Voor de utterance `book 2 flights from seattle to cairo next monday`, selecteer de blauw `number` entiteit, selecteer vervolgens **verpakken in samengestelde entiteit** uit de lijst. Een groene lijn, onder de woorden, volgt de cursor wordt aan de rechterkant, die wijzen op een samengestelde entiteit. Ga vervolgens naar het recht om te selecteren van de laatste vooraf gedefinieerde entiteit `datetimeV2`, voer dan `FlightReservation` in het tekstvak van het pop-upvenster, schakelt u **maken nieuwe samengestelde**. 

    ![Samengestelde entiteit op de pagina intents maken](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Een pop-upvenster wordt weergegeven zodat u kunt controleren of de onderliggende samengestelde entiteit. Selecteer **gedaan**.

    ![Samengestelde entiteit op de pagina intents maken](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Het verpakken van de entiteiten in de samengestelde entiteit
Zodra de samengestelde entiteit is gemaakt, label de resterende utterances in de samengestelde entiteit. Als u wilt een wachtwoordzin als een samengesteld entiteit doorloopt, moet u het meest linkse woord selecteert en vervolgens **verpakken in samengestelde entiteit** uit de lijst die wordt weergegeven, klikt u vervolgens selecteert u het woord meest rechtse en selecteer vervolgens de benoemde samengestelde entiteit `FlightReservation`. Dit is een snelle, smooth stap van selecties, onderverdeeld in de volgende stappen uit:

1. In de utterance `schedule 4 seats from paris to london for april 1`, selecteer de 4 als aantal vooraf gedefinieerde entiteit.

    ![Selecteer de meest linkse word](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Selecteer **verpakken in samengestelde entiteit** uit de lijst die wordt weergegeven.

    ![Selecteer tekstterugloop uit de lijst](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Selecteer de meest rechtse woord. Er verschijnt een groen lijn onder de wachtwoordzin op te geven, die wijzen op een samengestelde entiteit.

    ![Selecteer de meest rechtse word](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Selecteer samengestelde naam `FlightReservation` uit de lijst die wordt weergegeven.

    ![Benoemde samengestelde entiteit selecteren](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Voor de laatste utterance teruglopen `London` en `tomorrow` met behulp van de instructies in de samengestelde entiteit. 

## <a name="train-the-luis-app"></a>De app LUIS trainen
LUIS niet weet over de wijzigingen aan de intents en entiteiten (model), totdat deze is getraind. 

1. Selecteer in het bovenste aan de rechterkant van de website LUIS, de **Train** knop.

    ![De app trainen](./media/luis-tutorial-composite-entity/train-button.png)

2. Training is voltooid wanneer u de groene statusbalk aan de bovenkant van de website bevestiging ziet.

    ![Training is voltooid](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publiceer de app als u de eindpunt-URL
Om een voorspelling LUIS in een chatbot of een andere toepassing, moet u de app te publiceren. 

1. Selecteer in het bovenste aan de rechterkant van de website LUIS, de **publiceren** knop. 

2. Selecteer de productiesite en de **publiceren** knop.

    ![App publiceren](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. Het publiceren is voltooid wanneer u de groene statusbalk aan de bovenkant van de website bevestiging ziet.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Het eindpunt met een andere utterance query
1. Op de **publiceren** pagina de **eindpunt** koppeling aan de onderkant van de pagina. Deze actie wordt een ander browservenster geopend met de eindpunt-URL in de adresbalk. 

    ![Selecteer de eindpunt-URL](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Ga naar het einde van de URL in het adres en voer `reserve 3 seats from London to Cairo on Sunday`. De laatste parameter van de querystring is `q`, de utterance-query. Deze utterance is niet hetzelfde zijn als een van de gelabelde utterances zodat het is een goede test en als resultaat moet de `BookFlight` opzet met de hiërarchische entiteit hebt uitgepakt.

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

Deze utterance retourneert een samengestelde entiteiten matrix met inbegrip van de **flightreservation** object met de gegevens hebt uitgepakt.  

## <a name="what-has-this-luis-app-accomplished"></a>Wat is deze app LUIS bereikt?
Deze app, met slechts twee intents en een samengestelde entiteit een natuurlijke taal query voornemen geïdentificeerd en de geëxtraheerde gegevens geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om te bepalen van de primaire actie `BookFlight`, en de reserveringsinformatie gevonden in de utterance. 

## <a name="where-is-this-luis-data-used"></a>Waar wordt deze gegevens LUIS gebruikt? 
LUIS wordt aan deze aanvraag gedaan. De aanroepende toepassing, zoals een chatbot kan duren voordat het resultaat topScoringIntent en de gegevens van de entiteit op te nemen van de volgende stap. LUIS doen niet die programmatische werken voor de bot of de aanroepende toepassing. LUIS alleen bepaalt wat de bedoeling van de gebruiker. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over entiteiten](luis-concept-entity-types.md). 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
