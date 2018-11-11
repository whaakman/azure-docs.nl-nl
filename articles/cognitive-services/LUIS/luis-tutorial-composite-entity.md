---
title: 'Zelfstudie 6: Samengestelde gegevens met LUIS samengestelde entiteit ophalen'
titleSuffix: Azure Cognitive Services
description: Voeg een samengestelde entiteit die u wilt de opgehaalde gegevens van verschillende typen te bundelen in een enkele containerentiteit. De clienttoepassing kan door de gegevens bundeling, gerelateerde gegevens in verschillende gegevenstypen eenvoudig extraheren.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 1521bba188fcd7d8bade88196fe687929f414f93
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283509"
---
# <a name="tutorial-6-group-and-extract-related-data"></a>Zelfstudie 6: Groeperen en gerelateerde gegevens ophalen
In deze zelfstudie voegt u een samengestelde entiteit die u wilt de opgehaalde gegevens van verschillende typen in een enkele containerentiteit bundelen. De clienttoepassing kan door de gegevens bundeling, gerelateerde gegevens in verschillende gegevenstypen eenvoudig extraheren.

Het doel van de samengestelde entiteit is het groeperen van gerelateerde entiteiten in een entiteit van bovenliggende categorie. De informatie bestaat als afzonderlijke entiteiten voordat een samengestelde wordt gemaakt. Het is vergelijkbaar met hiërarchische entiteit, maar u kunt verschillende typen entiteiten bevatten. 

De samengestelde entiteit is geschikt voor dit type gegevens omdat de gegevens:

* Aan elkaar zijn gerelateerd. 
* Verschillende Entiteitstypen gebruiken.
* Moet als eenheid informatie worden gegroepeerd en verwerkt door de client-app.

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * Een samengestelde entiteit toevoegen 
> * Trainen
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken
Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-hier-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `composite`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL.


## <a name="composite-entity"></a>Samengestelde entiteit
Maak een samengestelde entiteit als afzonderlijke entiteiten kunnen logisch worden gegroepeerd en deze logische groepering is het handig om de clienttoepassing. 

In deze app, de naam van de werknemer is gedefinieerd in de **werknemer** lijst entiteit en omvat de synoniemen van de naam, e-mailadres, bedrijf toestelnummer, mobiele telefoonnummer en VS Federale btw-nummer. 

De **MoveEmployee** bedoeling uitingen om aan te vragen van een werknemer van een gebouw en office worden verplaatst naar een ander voorbeeld heeft. Building namen zijn alfabetisch: "A", "B", enzovoort kantoren numeriek zijn: '1234', '13245'. 

Voorbeeld-uitingen in de **MoveEmployee** bedoeling opnemen:

|Voorbeelden van utterances|
|--|
|John W verplaatsen. Smith naar a-2345|
|verplaats x12345 naar h-1234 morgen|
 
De verplaatsingsaanvraag moet bevatten van de medewerkers (met behulp van een synoniem) en de locatie van de laatste gebouw en office. De aanvraag kan ook de oorspronkelijke office, evenals een datum die het verplaatsen moet gebeuren. 

De opgehaalde gegevens van het eindpunt moet deze gegevens bevatten en terug in de `RequestEmployeeMove` samengestelde entiteit:

```JSON
"compositeEntities": [
  {
    "parentType": "RequestEmployeeMove",
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
]
```

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Op de **Intents** weergeeft, schakelt **MoveEmployee** intentie. 

3. Selecteer het pictogram met Vergrootglas op de werkbalk om de lijst uitingen te filteren. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Schermafbeelding van LUIS op 'MoveEmployee' doel met de knop Vergrootglas gemarkeerd")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Voer `tomorrow` in het filtertekstvak om te vinden van de utterance `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Schermafbeelding van LUIS op het doel van 'MoveEmployee' filter 'morgen' is gemarkeerd")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Een andere methode is om te filteren van de entiteit op datetimeV2, door te selecteren **entiteit filters** Selecteer **datetimeV2** in de lijst. 

5. Selecteer de eerste entiteit `Employee`en selecteer vervolgens **verpakken in samengestelde entiteit** in de lijst met het pop-upmenu. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Schermafbeelding van LUIS op 'MoveEmployee' bedoeling eerste entiteit selecteren in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Selecteer vervolgens de laatste entiteit onmiddellijk `datetimeV2` in de utterance. Een groene menubalk wordt onder de geselecteerde woorden die wijzen op een samengestelde entiteit getekend. Voer in het pop-upmenu, de naam van de samengestelde `RequestEmployeeMove` en selecteer enter. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Schermafbeelding van LUIS op 'MoveEmployee' doel selecteren van de laatste entiteit in samengestelde en het maken van het entiteit is gemarkeerd")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. In **welk type entiteit wilt u maken?**, bijna alle velden die vereist zijn in de lijst. Alleen de oorspronkelijke locatie ontbreekt. Selecteer **toevoegen van een onderliggende entiteit**, selecteer **Locations::Origin** uit de lijst met bestaande entiteiten, selecteert u vervolgens **gedaan**. 

    U ziet dat de vooraf gedefinieerde entiteit, een getal is toegevoegd aan de samengestelde entiteit. Als u een vooraf gedefinieerde entiteit weergegeven tussen het begin en einde van de tokens van een samengestelde entiteit hebt kan, moet de samengestelde entiteit die vooraf gemaakte entiteiten bevatten. Als de vooraf gemaakte entiteiten niet opgenomen zijn, de samengestelde entiteit niet correct wordt voorspeld, maar elk afzonderlijk element is.

    ![Schermafbeelding van LUIS op een andere entiteit in het pop-upvenster toe te voegen 'MoveEmployee'-doel](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Selecteer het Vergrootglas in de werkbalk om het filter te verwijderen. 

9. Verwijder het woord `tomorrow` van het filter, zodat u alle uitingen van het voorbeeld opnieuw kunt zien. 

## <a name="label-example-utterances-with-composite-entity"></a>Label voorbeeld uitingen met samengestelde entiteit


1. Selecteer de meest linkse-entiteit die in de samengestelde worden moet in elke utterance voorbeeld. Selecteer vervolgens **verpakken in samengestelde entiteit**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Schermafbeelding van LUIS op 'MoveEmployee' bedoeling eerste entiteit selecteren in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Selecteer het laatste woord in de samengestelde entiteit en selecteer vervolgens **RequestEmployeeMove** in het pop-upmenu. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Schermafbeelding van LUIS op 'MoveEmployee' doel selecteren van de laatste entiteit in samengestelde gemarkeerd")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Controleer of dat alle uitingen in het doel zijn gelabeld met de samengestelde entiteit. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Schermafbeelding van LUIS op 'MoveEmployee' met alle uitingen met het label")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

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
          "type": "RequestEmployeeMove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "RequestEmployeeMove",
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
      ]
    }
    ```

  Deze utterance retourneert een matrix samengestelde entiteiten. Elke entiteit is een type en de waarde opgegeven. Ga voor meer precisie voor elke onderliggende entiteit, gebruiken de combinatie van het type en de waarde van het matrixitem samengestelde aan het bijbehorende item niet vinden in de matrix entiteiten.  

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt gemaakt van een samengestelde entiteit om in te kapselen bestaande entiteiten. Hiermee wordt de clienttoepassing een reeks gerelateerde gegevens zoeken in verschillende gegevenstypen om door te gaan van de conversatie. Een clienttoepassing voor deze app Human Resources vragen welke dag en tijd het verplaatsen moet beginnen en eindigen. Het kan ook over andere logistiek van de movesuch stellen als een fysieke telefoon. 

> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een enkele entiteit met een woordgroepenlijst met](luis-quickstart-primary-and-secondary-data.md)  