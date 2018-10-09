---
title: 'Zelfstudie 4: Exact tekstovereenkomst - List-entiteit van LUIS'
titleSuffix: Azure Cognitive Services
description: Leer hoe u gegevens ophaalt die overeenkomen met een vooraf gedefinieerde lijst met items. Elk item in de lijst kan synoniemen hebben die ook exact overeenkomen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: b4fdf094653a4b16dead6397fe8e1a9f1a0258b9
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162080"
---
# <a name="tutorial-4-extract-exact-text-matches"></a>Zelfstudie 4: Exacte tekstovereenkomsten extraheren
In deze zelfstudie leert u hoe u gegevens ophaalt die overeenkomen met een vooraf gedefinieerde lijst met termen. Elk item in de lijst kan een lijst met synoniemen bevatten. Voor de Human Resources-app kan een werknemer worden geïdentificeerd aan de hand van enkele belangrijke gegevens, zoals naam, e-mailadres, telefoonnummer en sociaal-fiscaal nummer. 

De Human Resources-app moet bepalen welke werknemer wordt overgeplaatst van het ene naar het andere gebouw. Voor een utterance over het verplaatsen van een werknemer bepaalt LUIS de intentie en extraheert de werknemer, zodat een standaardopdracht voor het verplaatsen van de werknemer kan worden aangemaakt door de clienttoepassing.

Deze app maakt gebruik van een List-entiteit om de werknemer te extraheren. Er kan worden verwezen naar de werknemer op naam, doorkiesnummer, mobiel nummer, e-mailadres of sociaal-fiscaal nummer. 

Een entiteit List is een goede keuze voor dit type gegevens wanneer:

* De gegevenswaarden deel uitmaken van een bekende set.
* De set maximale [begrenzingen](luis-boundaries.md) van LUIS voor dit entiteitstype niet overschrijdt.
* De tekst in de utterance is een exact overeenkomst met een synoniem of de canonieke naam. 

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Bestaande zelfstudie-app gebruiken
> * De intentie MoveEmployee toevoegen
> * Een entiteit List toevoegen 
> * Trainen 
> * Publiceren
> * Intenties en entiteiten ophalen van eindpunt

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Bestaande app gebruiken
Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Als u niet over de app Human Resources uit de vorige zelfstudie beschikt, voert u de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-regex-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `list`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL. 


## <a name="moveemployee-intent"></a>De intentie MoveEmployee

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecteer **Create new intent**. 

3. Voer in het pop-updialoogvenster `MoveEmployee` in en selecteer vervolgens **Done**. 

    ![Schermopname van het pop-updialoogvenster voor het maken van een nieuwe intentie met](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |John W. Smith van B-1234 naar H-4452 verplaatsen|
    |verplaats john.w.smith@mycompany.com van kantoor b-1234 naar kantoor h-4452|
    |verplaats x12345 naar h-1234 morgen|
    |plaats 425-555-1212 in HH 2345|
    |verplaats 123-45-6789 van A-4321 naar J-23456|
    |Verplaats Jill Jones van D-2345 naar J-23456|
    |verplaats jill-jones@mycompany.com naar M-12345|
    |x23456 naar M-12345|
    |425-555-0000 naar h-4452|
    |234 56 7891 naar hh 2345|

    [ ![Schermopname van de pagina Intent met de nieuwe utterances gemarkeerd](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

    Houd er rekening mee dat number en datetimeV2 in een vorige zelfstudie zijn toegevoegd en dat deze automatisch worden gelabeld wanneer ze worden gevonden in voorbeelden van utterances.

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="employee-list-entity"></a>De entiteit Employee List
Nu de intentie **MoveEmployee** voorbeelden van utterances bevat, moet LUIS kunnen vaststellen wat een werknemer is. 

De primaire, _canonieke_ naam voor elk item is het werknemersnummer. Voorbeelden van synoniemen voor elke canonieke naam binnen dit domein zijn: 

|Doel synoniem|Waarde synoniem|
|--|--|
|Naam|John W. Smith|
|E-mailadres|john.w.smith@mycompany.com|
|Toestelnummer|x12345|
|Mobiel nummer (privé)|425-555-1212|
|Amerikaans sociaal-fiscaal nummer|123-45-6789|


1. Selecteer **Entities** in het linkerpaneel.

2. Selecteer **Create new intent**.

3. Voer in het pop-updialoogvenster `Employee` in als naam voor de entiteit en **List** als het entiteitstype. Selecteer **Done**.  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Schermopname van het pop-updialoogvenster voor het maken van een nieuwe entiteit")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Voer op de pagina van de entiteit Employee `Employee-24612` als de nieuwe waarde in.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Schermafbeelding van het invoeren van de waarde")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Voor synoniemen voegt u de volgende waarden toe:

    |Doel synoniem|Waarde synoniem|
    |--|--|
    |Naam|John W. Smith|
    |E-mailadres|john.w.smith@mycompany.com|
    |Toestelnummer|x12345|
    |Mobiel nummer (privé)|425-555-1212|
    |Amerikaans sociaal-fiscaal nummer|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Schermafbeelding van het invoeren van synoniemen")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Voer `Employee-45612` als een nieuwe waarde in.

7. Voor synoniemen voegt u de volgende waarden toe:

    |Doel synoniem|Waarde synoniem|
    |--|--|
    |Naam|Jill Jones|
    |E-mailadres|jill-jones@mycompany.com|
    |Toestelnummer|x23456|
    |Mobiel nummer (privé)|425-555-0000|
    |Amerikaans sociaal-fiscaal nummer|234-56-7891|

## <a name="train"></a>Trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publiceren

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Ga naar het einde van de URL in het adres en voer `shift 123-45-6789 from Z-1242 to T-54672` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde utterances en dit is dus een goede test die de intentie `MoveEmployee` met `Employee` geëxtraheerd als resultaat moet geven.

  ```JSON
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  De werknemer is gevonden en is geretourneerd als type `Employee` met de resolutiewaarde `Employee-24612`.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een nieuwe intentie gemaakt, voorbeelden van utterances toegevoegd en vervolgens een List-entiteit gemaakt om exacte tekstovereenkomsten te extraheren uit utterances. Na het trainen en het publiceren van de app is met een query naar het eindpunt de intentie achterhaald. Vervolgens zijn de geëxtraheerde gegevens geretourneerd.

> [!div class="nextstepaction"]
> [Een hiërarchische entiteit aan de app toevoegen](luis-quickstart-intent-and-hier-entity.md)

