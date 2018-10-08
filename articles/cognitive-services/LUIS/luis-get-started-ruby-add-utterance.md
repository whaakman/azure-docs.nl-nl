---
title: Ruby-snelstart - model wijzigen en de LUIS-app trainen
titleSuffix: Azure Cognitive Services
description: Voeg in deze Ruby-snelstart voorbeeldutterances toe aan een Home Automation-app en train de app. Voorbeeldutterances zijn teksten uit gesprekken met gebruikers die worden toegewezen aan een intentie. Door voorbeeldutterances op te geven voor intenties leert u LUIS welke soorten door de gebruiker geleverde tekst bij welke intentie horen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 2d4633592742bd81cc6972884a0c4f5141aa9943
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030898"
---
# <a name="quickstart-change-model-using-ruby"></a>Snelstart: Model wijzigen met behulp van Ruby

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Ruby](http://rubyinstaller.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-bestand met voorbeeldutterances

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Snelstartcode maken 

Voeg de afhankelijkheden toe aan het bestand `add-utterances.rb`.

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=1-21 "Ruby and LUIS Dependencies")]

Voeg de GET-aanvraag toe die wordt gebruikt voor de trainingsstatus.

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=23-33 "SendGet")]

Voeg de POST-aanvraag toe die wordt gebruikt om utterances te maken of om te beginnen met trainen. 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=35-47 "SendPost")]

Voeg de `AddUtterances`-functie toe.

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=49-54 "AddUtterances method")]


Voeg de `Train`-functie toe. 

   [!code-ruby[Train](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=56-62 "Train")]

Voeg de `Status`-functie toe.

   [!code-ruby[Status](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=64-68 "Status")]

Voeg de hoofdcode toe om argumenten te beheren.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/quickstarts/change-model/ruby/add-utterances.rb?range=70-72 "Main code")]

## <a name="run-code"></a>Code uitvoeren

Voer de toepassing vanaf een opdrachtregel uit met Ruby.

### <a name="add-an-utterance-from-the-command-line"></a>Toevoegen van een utterance vanaf de opdrachtregel

Door `add-utterances.rb` aan te roepen worden de utterances toegevoegd, wordt de toepassing getraind en wordt de trainingsstatus opgehaald.

```CMD
> ruby add-utterances.rb 
```

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

Het volgende antwoord toont de training in de wachtrij. Het daarna volgende antwoord laat vervolgens de status van elke intentie zien. 

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
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md)