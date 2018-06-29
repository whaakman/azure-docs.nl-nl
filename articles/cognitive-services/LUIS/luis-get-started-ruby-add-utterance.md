---
title: 'Zelfstudie: utterances toevoegen aan een LUIS-app met behulp van Ruby | Microsoft Docs'
description: In deze zelfstudie leert u een LUIS-app aan te roepen met behulp van Ruby.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 7a470fd551a58978e6f2be0450a2e2a6cd471fc4
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266053"
---
# <a name="tutorial-add-utterances-to-app-using-ruby"></a>Zelfstudie: utterances toevoegen aan een app met behulp van Ruby 
In deze zelfstudie schrijft u een programma om een utterance aan een intentie toe te voegen met behulp van de creatie-API's in Ruby.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Visual Studio-consoleproject maken 
> * Methode toevoegen voor het aanroepen van de LUIS-API om een utterance toe te voegen en een app te trainen
> * JSON-bestand toevoegen met voorbeeldutterances voor BookFlight-intentie
> * Console uitvoeren en trainingsstatus weergeven voor utterances

Zie voor meer informatie de technische documentatie voor de API’s voor het [toevoegen van voorbeeldutterances aan intenties](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [trainen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) en [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="prerequisites"></a>Vereisten

* [Ruby](http://rubyinstaller.org/) 
* Uw LUIS-**[creatiesleutel](luis-concept-keys.md#authoring-key)**. U vindt deze sleutel onder Accountinstellingen op de website van [LUIS](luis-reference-regions.md).
* Uw bestaande LUIS-[**toepassings-id**](./luis-get-started-create-app.md). De toepassings-id wordt weergegeven op het toepassingsdashboard. De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.rb` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten. 
* De **versie-id** in de toepassing die de utterances ontvangt. De standaard-id is '0.1'
* Maak een nieuw bestand met de naam `add-utterances.rb`-project in VSCode.

> [!NOTE] 
> Het volledige `add-utterances.cs`-bestand en een voorbeeld-`utterances.json`-bestand zijn beschikbaar in de Github-opslagplaats [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/ruby/).


## <a name="write-the-ruby-code"></a>De Ruby code schrijven

Voeg de afhankelijkheden toe aan het bestand.

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=1-28 "Ruby and LUIS Dependencies")]

Voeg de GET-aanvraag toe die wordt gebruikt voor de trainingsstatus.

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=30-40 "SendGet")]

Voeg de POST-aanvraag toe die wordt gebruikt om utterances te maken of om te beginnen met trainen. 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=42-54 "SendPost")]

Voeg de `AddUtterances`-functie toe.

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=56-61 "AddUtterances method")]


Voeg de `Train`-functie toe. 

   [!code-ruby[Train](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=63-69 "Train")]

Voeg de `Status`-functie toe.

   [!code-ruby[Status](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=71-75 "Status")]

Voeg de hoofdcode toe om argumenten te beheren.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=77-93 "Main code")]

## <a name="specify-utterances-to-add"></a>Toe te voegen utterances opgeven
Maak en bewerk het bestand `utterances.json` om de **matrix met utterances** op te geven die u aan de LUIS-app wilt toevoegen. De intentie en entiteiten **moeten** al aanwezig zijn in de LUIS-app.

> [!NOTE]
> De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.rb` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten.

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

Voer de toepassing vanaf een opdrachtregel uit met Ruby.

Het aanroepen van `add-utterances.rb` met alleen de utterance.json als argument voegt de nieuwe utterances toe, maar traint LUIS er niet mee.
````
> ruby add-utterances.rb ./utterances.json
````

Dit resultaat toont de resultaten van het aanroepen van de API voor het toevoegen van utterances. Het veld `response` heeft deze notatie voor utterances die zijn toegevoegd. `hasError` is onwaar, wat aangeeft dat de utterance is toegevoegd.  

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
Roep add-utterance aan ​​met het argument `-train` om een aanvraag om te trainen te verzenden.

````
> ruby add-utterances.rb ./utterances.json -train
````

> [!NOTE]
> Dubbele utterances worden niet opnieuw toegevoegd, maar veroorzaken geen fout. De `response` bevat de id van de oorspronkelijke utterance.

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
Roep het voorbeeld aan met het argument `-status` om de trainingsstatus te controleren.

````
> ruby add-utterances.rb ./utterances.json -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met de zelfstudie, verwijdert Visual Studio en de console-toepassing als u deze niet meer nodig hebt.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
