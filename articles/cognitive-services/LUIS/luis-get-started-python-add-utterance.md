---
title: 'Zelfstudie: utterances toevoegen aan een LUIS-app met behulp van Python | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een LUIS-app aanroept met behulp van Python.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 3e1ca2ea874b6b39ac8a1b1eaa94fe9ff1894ea3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264298"
---
# <a name="tutorial-add-utterances-to-app-using-python"></a>Zelfstudie: utterances toevoegen aan een app met behulp van Python
In deze zelfstudie schrijft u een programma om een utterance aan een intentie toe te voegen met behulp van de creatie-API's in Python.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Visual Studio-consoleproject maken 
> * Methode toevoegen voor het aanroepen van de LUIS-API om een utterance toe te voegen en een app te trainen
> * JSON-bestand toevoegen met voorbeeldutterances voor BookFlight-intentie
> * Console uitvoeren en trainingsstatus weergeven voor utterances

Raadpleeg voor meer informatie de technische documentatie voor de API’s voor het [toevoegen van voorbeeldutterances aan intenties](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45), [trainen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) en [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="prerequisites"></a>Vereisten

* [Python 3.6](https://www.python.org/downloads/) of later.
* **[Aanbevolen]** [Visual Studio Code](https://code.visualstudio.com/) voor IntelliSense en foutopsporing.
* Uw LUIS-**[creatiesleutel](luis-concept-keys.md#authoring-key)**. U vindt deze sleutel onder Accountinstellingen op de website van [LUIS](luis-reference-regions.md).
* Uw bestaande LUIS-[**toepassings-id**](./luis-get-started-create-app.md). De toepassings-id wordt weergegeven op het toepassingsdashboard. De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.js` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten. 
* De **versie-id** in de toepassing die de utterances ontvangt. De standaard-id is '0.1'
* Maak een nieuw bestand met de naam `add-utterances-3-6.py`-project in VSCode.

> [!NOTE] 
> Het volledige `add-utterances-3-6.py`-bestand is beschikbaar in de Github-opslagplaats [**LUIS Samples**](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/python).


## <a name="write-the-python-code"></a>De Python-code schrijven

1. Kopieer de volgende codefragmenten:

   [!code-python[Console app code that adds an utterance Python 3.6](~/samples-luis/documentation-samples/authoring-api-samples/python/add-utterances-3-6.py)]

## <a name="specify-utterances-to-add"></a>Toe te voegen utterances opgeven
Maak en bewerk het bestand `utterances.json` om de **matrix met utterances** op te geven die u aan de LUIS-app wilt toevoegen. De intentie en entiteiten **moeten** al aanwezig zijn in de LUIS-app.

> [!NOTE]
> De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.js` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten.

Het veld `text` bevat de tekst van de utterance. Het veld `intentName` moet overeenkomen met de naam van een intentie in de LUIS-app. Het veld `entityLabels` is vereist. Als u geen entiteiten wilt labelen, geeft u een lege lijst op, zoals in het volgende voorbeeld:

Als de lijst met entityLabels niet leeg is, moeten `startCharIndex` en `endCharIndex` de entiteit markeren waarnaar wordt verwezen in het veld `entityName`. Beide indices worden vanaf 0 geteld, wat betekent dat 6 in het bovenste voorbeeld verwijst naar de 'S' van Seattle en niet naar de spatie vóór de hoofdletter S.

```json
[
    {
        "text": "go to Seattle",
        "intentName": "BookFlight",
        "entityLabels": [
            {
                "entityName": "Location::LocationTo",
                "startCharIndex": 6,
                "endCharIndex": 12
            }
        ]
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Toevoegen van een utterance vanaf de opdrachtregel

Voer de toepassing vanaf een opdrachtregel uit met Python 3.6.

Het aanroepen van add-utterance zonder argumenten voegt een utterance aan de app toe, zonder deze te trainen.

````
> python add-utterances-3-6.py
````

In dit voorbeeld wordt een bestand gemaakt met de `results.json` die de resultaten bevat van het aanroepen van de API voor het toevoegen van utterances. Het veld `response` heeft deze notatie voor utterances die zijn toegevoegd. `hasError` is onwaar, wat aangeeft dat de utterance is toegevoegd.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Een utterance toevoegen en trainen vanaf de opdrachtregel
Roep add-utterance aan met het argument `-train` om een aanvraag om te trainen te verzenden, en vraag vervolgens om een ​​trainingsstatus. De status wordt onmiddellijk nadat de training begint in de wachtrij gezet. Statusdetails worden naar een bestand geschreven.

````
> python add-utterances-3-6.py -train
````

> [!NOTE]
> Dubbele utterances worden niet opnieuw toegevoegd, maar veroorzaken geen fout. De `response` bevat de id van de oorspronkelijke utterance.

Wanneer u het voorbeeld aanroept met het argument `-train`, wordt er een `training-results.json`-bestand gemaakt dat aangeeft dat de aanvraag voor het trainen van de LUIS-app met succes in de wachtrij is geplaatst. 

Het volgende toont het resultaat van een succesvolle trainingsaanvraag:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Nadat de trainingsaanvraag in de wachtrij is geplaatst, kan het even duren voordat de training is voltooid.

## <a name="get-training-status-from-the-command-line"></a>Trainingsstatus ophalen vanaf de opdrachtregel
Roep het voorbeeld aan met het argument `-status` om de trainingsstatus te controleren en statusdetails naar een bestand te schrijven.

````
> python add-utterances-3-6.py -status
````
## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met de zelfstudie, verwijdert Visual Studio en de console-toepassing als u deze niet meer nodig hebt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website

