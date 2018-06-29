---
title: 'Zelfstudie: utterances toevoegen aan een LUIS-app met behulp van Java | Microsoft Docs'
description: In deze zelfstudie leert u een LUIS-app aan te roepen met behulp van Java.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 5b9c7b90ca96b23fbabfeb1e1d06e4124e65a0dc
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266036"
---
# <a name="tutorial-add-utterances-to-app-using-java"></a>Zelfstudie: utterances toevoegen aan een app met behulp van Java 
In deze zelfstudie schrijft u een programma om een utterance aan een intentie toe te voegen met behulp van de creatie-API's in Java.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Visual Studio-consoleproject maken 
> * Methode toevoegen voor het aanroepen van de LUIS-API om een utterance toe te voegen en een app te trainen
> * JSON-bestand toevoegen met voorbeeldutterances voor BookFlight-intentie
> * Console uitvoeren en trainingsstatus weergeven voor utterances

Zie voor meer informatie de technische documentatie voor de API’s voor het [toevoegen van voorbeeldutterances aan intenties](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [trainen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) en [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="prerequisites"></a>Vereisten

* Meest recente Oracle [**Java/JDK**](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [De GSON JSON-bibliotheek van Google](https://github.com/google/gson).
* Uw LUIS-**[creatiesleutel](luis-concept-keys.md#authoring-key)**. U vindt deze sleutel onder Accountinstellingen op de website van [LUIS](luis-reference-regions.md).
* Uw bestaande LUIS-[**toepassings-id**](./luis-get-started-create-app.md). De toepassings-id wordt weergegeven op het toepassingsdashboard. De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `AddUtterances.java` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten. 
* De **versie-id** in de toepassing die de utterances ontvangt. De standaard-id is '0.1'
* Maak een nieuw tekstbestand met de naam `AddUtterances.java`.

> [!NOTE] 
> Het volledige `add-utterances.cs`-bestand en een voorbeeld-`utterances.json`-bestand zijn beschikbaar in de Github-opslagplaats [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/java).


## <a name="write-the-java-code"></a>De Java-code schrijven

Voeg de Java-afhankelijkheden toe aan het bestand.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=31-34 "Java Dependencies")]

De klasse `AddUtterances` maken

```Java
public class AddUtterances {
    // Insert code here
}
```

Deze klasse bevat alle codefragmenten die volgen.

Voeg de LUIS-constanten toe aan de klasse. Kopieer de volgende code en wijzig deze met uw creatiecode, toepassings-id en versie-id.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=41-53 "LUIS-based IDs")]

Voeg de methode toe om de LUIS-API aan te roepen. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=55-178 "HTTP request to LUIS")]

Voeg de methode toe voor het HTTP-antwoord van de LUIS-API.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=180-226 "HTTP response from LUIS")]


Voeg uitzonderingsafhandeling toe. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=228-256 "Exception Handling")]

Voeg uitvoer-/afdrukafhandeling toe.

   [!code-java[Add output handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=258-267 "Add configuration information for adding utterance")]

Voeg de hoofdfunctie toe.

   [!code-java[Add main function](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=269-345 "Add main function")]


## <a name="specify-utterances-to-add"></a>Toe te voegen utterances opgeven
Maak en bewerk het bestand `utterances.json` om de **matrix met utterances** op te geven die u aan de LUIS-app wilt toevoegen. De intentie en entiteiten **moeten** al aanwezig zijn in de LUIS-app.

> [!NOTE]
> De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `AddUtterances.java` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten.

Het veld `text` bevat de tekst van de utterance. Het veld `intentName` moet overeenkomen met de naam van een intentie in de LUIS-app. Het veld `entityLabels` is vereist. Als u geen entiteiten wilt labelen, geeft u een lege lijst op, zoals in het volgende voorbeeld.

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

AddUtterance compileren met de afhankelijkheden

```
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

Het aanroepen van `AddUtterance` zonder argumenten voegt de LUIS-utterances aan de app toe, zonder deze te trainen.
````
> java -classpath .;gson-2.8.2.jar AddUtterances
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
Roep `add-utterance` aan ​​met het argument `-train` om een aanvraag om te trainen te verzenden. 

````
> java -classpath .;gson-2.8.2.jar AddUtterances -train
````

> [!NOTE]
> Dubbele utterances worden niet opnieuw toegevoegd, maar veroorzaken geen fout. De `response` bevat de id van de oorspronkelijke utterance.

Wanneer u de app aanroept met het argument `-train`, wordt er een `training-results.json`-bestand gemaakt dat aangeeft dat de aanvraag voor het trainen van de LUIS-app met succes in de wachtrij is geplaatst. 

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
Roep de app aan met het argument `-status` om de trainingsstatus te controleren en statusdetails naar een bestand te schrijven.

````
> java -classpath .;gson-2.8.2.jar AddUtterances -status
````

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met de zelfstudie, verwijdert Visual Studio en de console-toepassing als u deze niet meer nodig hebt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website