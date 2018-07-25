---
title: Zelfstudie voor het maken van een samengestelde entiteit om uit te pakken van complexe gegevens - Azure | Microsoft Docs
description: Informatie over het maken van een samengestelde entiteit in uw LUIS-app om op te halen van verschillende typen entiteitsgegevens.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 07/09/2018
ms.author: diberry
ms.openlocfilehash: d14041e895bdf70544f7e956c76f91992a2df991
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238094"
---
# <a name="tutorial-6-add-composite-entity"></a>Zelfstudie: 6. Samengestelde entiteit toevoegen 
In deze zelfstudie voegt u een samengestelde entiteit die u wilt de opgehaalde gegevens in een entiteit met bundelen.

In deze zelfstudie leert u het volgende:

<!-- green checkmark -->
> [!div class="checklist"]
> * Samengestelde entiteiten begrijpen 
> * Samengestelde entiteit om gegevens te extraheren toevoegen
> * App trainen en publiceren
> * Eindpunt van app opvragen om JSON-antwoord van LUIS te zien

## <a name="before-you-begin"></a>Voordat u begint
Als u geen Human Resources-app uit de zelfstudie over de [entiteit Hierarchical](luis-quickstart-intent-and-hier-entity.md) hebt, [importeert](luis-how-to-start-new-app.md#import-new-app) u de JSON in een nieuwe app op de [LUIS](luis-reference-regions.md#luis-website)-website. De app die kan worden geïmporteerd bevindt zich in de GitHub-opslagplaats met [voorbeelden van LUIS](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json).

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `composite`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd.  

## <a name="composite-entity-is-a-logical-grouping"></a>Samengestelde entiteit is een logische groepering 
Het doel van de samengestelde entiteit is het groeperen van gerelateerde entiteiten in een entiteit van bovenliggende categorie. De informatie bestaat als afzonderlijke entiteiten voordat een samengestelde wordt gemaakt. Het is vergelijkbaar met hiërarchische entiteit, maar u kunt meer typen entiteiten bevatten. 

 Maak een samengestelde entiteit als afzonderlijke entiteiten kunnen logisch worden gegroepeerd en deze logische groepering is het handig om de clienttoepassing. 

In deze app, de naam van de werknemer is gedefinieerd in de **werknemer** lijst entiteit en omvat de synoniemen van de naam, e-mailadres, bedrijf toestelnummer, mobiele telefoonnummer en VS Federale btw-nummer. 

De **MoveEmployee** bedoeling uitingen om aan te vragen van een werknemer van een gebouw en office worden verplaatst naar een ander voorbeeld heeft. Building namen zijn alfabetisch: "A", "B", enzovoort kantoren numeriek zijn: '1234', '13245'. 

Voorbeeld-uitingen in de **MoveEmployee** bedoeling opnemen:

|Voorbeelden van utterances|
|--|
|John W verplaatsen. Smith naar a-2345|
|verplaats x12345 naar h-1234 morgen|
 
De verplaatsingsaanvraag moet ten minste bevatten de werknemer (met behulp van een synoniem) en de locatie van de laatste gebouw en office. De aanvraag kan ook de oorspronkelijke office, evenals een datum die het verplaatsen moet gebeuren. 

De opgehaalde gegevens van het eindpunt moet deze gegevens bevatten en deze op in een `RequestEmployeeMove` samengestelde entiteit. 

## <a name="create-composite-entity"></a>Samengestelde entiteit maken
1. Zorg ervoor dat uw Human Resources-app zich in de sectie **Build** van LUIS bevindt. U kunt naar deze sectie gaan door **Build** te selecteren in de menubalk rechtsboven. 

    [ ![Schermopname van LUIS-app met Build gemarkeerd in de navigatiebalk rechtsboven](./media/luis-tutorial-composite-entity/hr-first-image.png)](./media/luis-tutorial-composite-entity/hr-first-image.png#lightbox)

2. Op de **Intents** weergeeft, schakelt **MoveEmployee** intentie. 

    [![](media/luis-tutorial-composite-entity/hr-intents-moveemployee.png "Schermafbeelding van LUIS met 'MoveEmployee' doel is gemarkeerd")](media/luis-tutorial-composite-entity/hr-intents-moveemployee.png#lightbox)

3. Selecteer het pictogram met Vergrootglas op de werkbalk om de lijst uitingen te filteren. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Schermafbeelding van LUIS op 'MoveEmployee' doel met de knop Vergrootglas gemarkeerd")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Voer `tomorrow` in het filtertekstvak om te vinden van de utterance `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Schermafbeelding van LUIS op het doel van 'MoveEmployee' filter 'morgen' is gemarkeerd")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Een andere methode is om te filteren van de entiteit op datetimeV2, door te selecteren **entiteit filters** Selecteer **datetimeV2** in de lijst. 

5. Selecteer de eerste entiteit `Employee`en selecteer vervolgens **verpakken in samengestelde entiteit** in de lijst met het pop-upmenu. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Schermafbeelding van LUIS op 'MoveEmployee' bedoeling eerste entiteit selecteren in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Selecteer vervolgens de laatste entiteit onmiddellijk `datetimeV2` in de utterance. Een groene menubalk wordt onder de geselecteerde woorden die wijzen op een samengestelde entiteit getekend. Voer in het pop-upmenu, de naam van de samengestelde `RequestEmployeeMove` Selecteer **maken nieuwe samengestelde** op in het pop-upmenu. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Schermafbeelding van LUIS op 'MoveEmployee' doel selecteren van de laatste entiteit in samengestelde en het maken van het entiteit is gemarkeerd")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. In **welk type entiteit wilt u maken?**, bijna alle velden die vereist zijn in de lijst. Alleen de oorspronkelijke locatie ontbreekt. Selecteer **toevoegen van een onderliggende entiteit**, selecteer **Locations::Origin** uit de lijst met bestaande entiteiten, selecteert u vervolgens **gedaan**. 

  ![Schermafbeelding van LUIS op een andere entiteit in het pop-upvenster toe te voegen 'MoveEmployee'-doel](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Selecteer het Vergrootglas in de werkbalk om het filter te verwijderen. 

## <a name="label-example-utterances-with-composite-entity"></a>Label voorbeeld uitingen met samengestelde entiteit
1. Selecteer de meest linkse-entiteit die in de samengestelde worden moet in elke utterance voorbeeld. Selecteer vervolgens **verpakken in samengestelde entiteit**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Schermafbeelding van LUIS op 'MoveEmployee' bedoeling eerste entiteit selecteren in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Selecteer het laatste woord in de samengestelde entiteit en selecteer vervolgens **RequestEmployeeMove** in het pop-upmenu. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Schermafbeelding van LUIS op 'MoveEmployee' doel selecteren van de laatste entiteit in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Controleer of dat alle uitingen in het doel zijn gelabeld met de samengestelde entiteit. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Schermafbeelding van LUIS op 'MoveEmployee' met alle uitingen met het label")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS-app trainen
LUIS weten niet over de nieuwe samengestelde entiteit totdat de app wordt getraind. 

1. Selecteer rechtsboven op de website van LUIS de knop **Train**.

    ![De app trainen](./media/luis-tutorial-composite-entity/hr-train-button.png)

2. Het trainen is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het trainen is gelukt.

    ![Trainen is voltooid](./media/luis-tutorial-composite-entity/hr-trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>App publiceren om eindpunt-URL op te vragen
Om LUIS een voorspelling te laten geven in een chatbot of een andere toepassing, moet u de app publiceren. 

1. Selecteer rechtsboven op de website van LUIS de knop **Publish**. 

2. Selecteer de slot Production en vervolgens de knop **Publish**.

    ![app publiceren](./media/luis-tutorial-composite-entity/hr-publish-to-production.png)

3. Het publiceren is voltooid wanneer u een groene statusbalk bovenaan aan de website ziet met de melding dat het publiceren is gelukt.

## <a name="query-the-endpoint"></a>Query uitvoeren op het eindpunt 
1. Selecteer onder aan de pagina **Publish** de koppeling **endpoint**. Er wordt nu een nieuw browservenster geopend, met de eindpunt-URL in de adresbalk. 

    ![Eindpunt-URL selecteren](./media/luis-tutorial-composite-entity/hr-publish-select-endpoint.png)

2. Ga naar het einde van de URL in het adres en voer `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.` in. De laatste querystring-parameter is `q`, de query utterance. 

    Omdat deze test om te controleren of dat de samengestelde correct wordt opgehaald, kan een test ofwel een bestaande voorbeeld-utterance of een nieuwe utterance bevatten. Er is een goede test om op te nemen van de onderliggende entiteiten in de samengestelde entiteit.

```JSON
{
  "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
  "topScoringIntent": {
    "intent": "MoveEmployee",
    "score": 0.9959525
  },
  "intents": [
    {
      "intent": "MoveEmployee",
      "score": 0.9959525
    },
    {
      "intent": "GetJobInformation",
      "score": 0.009858314
    },
    {
      "intent": "ApplyForJob",
      "score": 0.00728598563
    },
    {
      "intent": "FindForm",
      "score": 0.0058053555
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.005371796
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00266987388
    },
    {
      "intent": "None",
      "score": 0.00123299169
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.00116407464
    },
    {
      "intent": "Utilities.Confirm",
      "score": 0.00102653319
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0006628214
    }
  ],
  "entities": [
    {
      "entity": "march 3 2 p.m",
      "type": "builtin.datetimeV2.datetime",
      "startIndex": 41,
      "endIndex": 54,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-03-03T14",
            "type": "datetime",
            "value": "2018-03-03 14:00:00"
          },
          {
            "timex": "XXXX-03-03T14",
            "type": "datetime",
            "value": "2019-03-03 14:00:00"
          }
        ]
      }
    },
    {
      "entity": "jill jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 14,
      "resolution": {
        "values": [
          "Employee-45612"
        ]
      }
    },
    {
      "entity": "z - 2345",
      "type": "Locations::Destination",
      "startIndex": 31,
      "endIndex": 36,
      "score": 0.9690751
    },
    {
      "entity": "a - 1234",
      "type": "Locations::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.9713137
    },
    {
      "entity": "-1234",
      "type": "builtin.number",
      "startIndex": 22,
      "endIndex": 26,
      "resolution": {
        "value": "-1234"
      }
    },
    {
      "entity": "-2345",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 36,
      "resolution": {
        "value": "-2345"
      }
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 47,
      "endIndex": 47,
      "resolution": {
        "value": "3"
      }
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 50,
      "endIndex": 50,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
      "type": "requestemployeemove",
      "startIndex": 5,
      "endIndex": 54,
      "score": 0.4027723
    }
  ],
  "compositeEntities": [
    {
      "parentType": "requestemployeemove",
      "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
      "children": [
        {
          "type": "builtin.datetimeV2.datetime",
          "value": "march 3 2 p.m"
        },
        {
          "type": "Locations::Destination",
          "value": "z - 2345"
        },
        {
          "type": "Employee",
          "value": "jill jones"
        },
        {
          "type": "Locations::Origin",
          "value": "a - 1234"
        }
      ]
    }
  ],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

Deze utterance retourneert een matrix samengestelde entiteiten. Elke entiteit is een type en de waarde opgegeven. Ga voor meer precisie voor elke onderliggende entiteit, gebruiken de combinatie van het type en de waarde van het matrixitem samengestelde aan het bijbehorende item niet vinden in de matrix entiteiten.  

## <a name="what-has-this-luis-app-accomplished"></a>Wat is er met deze LUIS-app bereikt?
Deze app een natuurlijke taal query voornemen geïdentificeerd en de opgehaalde gegevens als een benoemde groep geretourneerd. 

Uw chatbot heeft nu voldoende gegevens om te bepalen van de primaire actie en de bijbehorende details in de utterance. 

## <a name="where-is-this-luis-data-used"></a>Waar worden deze gegevens van LUIS gebruikt? 
LUIS hoeft niets meer te doen met deze aanvraag. De aanroepende toepassing, zoals een chatbot, kan het resultaat topScoringIntent nemen plus de gegevens van de entiteit om de volgende stap uit te voeren. LUIS is niet verantwoordelijk voor die programmatische werken voor de bot of aanroepende toepassing. LUIS bepaalt alleen wat de bedoeling van de gebruiker is. 

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Selecteer **My apps** in het menu linksboven. Selecteer het weglatingsteken (***...*** ) knop aan de rechterkant van de naam van de app in de applijst, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een enkele entiteit met een woordgroepenlijst met](luis-quickstart-primary-and-secondary-data.md)  