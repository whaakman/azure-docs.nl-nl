---
title: Snelstart Model wijzigen en LUIS-app trainen met behulp van Go - Azure Cognitive Services | Microsoft Docs
description: Voeg in deze Go-snelstart voorbeeldutterances toe aan een Home Automation-app en train de app. Voorbeeldutterances zijn teksten uit gesprekken met gebruikers die worden toegewezen aan een intentie. Door voorbeeldutterances op te geven voor intenties leert u LUIS welke soorten door de gebruiker geleverde tekst bij welke intentie horen.
titleSuffix: Microsoft Cognitive Services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: da57a7e46cccbf0a9b34b3961a831e7982160e6b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157664"
---
# <a name="quickstart-change-model-using-go"></a>Snelstart: Model wijzigen met behulp van Go

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Programmeertaal [Go](https://golang.org/) moet geïnstalleerd zijn.
* [VSCode](https://code.visualstudio.com) 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-bestand met voorbeeldutterances

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Snelstartcode maken 

1. Maak `add-utterances.go` met VSCode. 

2. Voeg afhankelijkheden toe. 

   [!code-go[Add dependencies.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=2-10 "Add dependencies.")]

3. Voeg een generieke HTTP-aanvraagfunctie toe waarin de authoring-code wordt doorgegeven in de koptekst. 

   [!code-go[Add HTTP request function which includes passing authoring key in header. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=12-36 "Add HTTP request function, which includes passing authoring key in header. ")]

4. Voeg voorbeeldutterances toe uit een JSON-bestand.

   [!code-go[Add example utterances from JSON file.](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=62-76 "Add example utterances from JSON file.")]

5. Vraag training aan. Gebruikt een hulpfunctie om de bewerking (VERB) in te stellen voor dezelfde route als de trainingsstatus. 

   [!code-go[Request training. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=77-86 "Request training. ")]

6. Vraag trainingsstatus aan. Gebruikt een hulpfunctie om de bewerking (VERB) in te stellen voor dezelfde route als de aanvraagtraining. 

   [!code-go[Request training status. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=87-90 "Request training status. ")]

7. Voeg main-functie toe voor het parseren van de opdrachtregel.

   [!code-go[Add main function to handle command line parsing. ](~/samples-luis/documentation-samples/quickstarts/change-model/go/add-utterances.go?range=38-60 "Add main function to handle command-line parsing.")]

## <a name="add-an-utterance-from-the-command-line-train-and-get-status"></a>Een utterance toevoegen vanaf de opdrachtregel, trainen en status ophalen

1. Voer vanuit een opdrachtprompt in de map waarin u het Go-bestand hebt gemaakt `go build add-utterances.go` in om het Go-bestand te compileren. De opdrachtprompt retourneert geen informatie voor een geslaagde build.

2. Voer de Go-toepassing uit vanaf de opdrachtregel door de volgende tekst in te voeren in de opdrachtprompt: 

    ```CMD
    add-utterances -appID <your-app-id> -authoringKey <add-your-authoring-key> -version <your-version-id> -region westus -utteranceFile utterances.json

    ```

    Vervang `<add-your-authoring-key>` door de waarde van uw authoring-code (ook wel de startsleutel genoemd). Vervang `<your-app-id>` door de waarde van uw app-id. Vervang `<your-version-id>` door de waarde van uw versie. De standaardversie is `0.1`.

    De resultaten worden weergegeven met deze opdrachtprompt:

    ```CMD
    add example utterances requested
    [
        {
            "text": "go lang 1",
            "intentName": "None",
            "entityLabels": []
        }
    ,
        {
            "text": "go lang 2",
            "intentName": "None",
            "entityLabels": []
        }
    ]
    201
    [
        {
            "value": {
                "ExampleId": 77783998,
                "UtteranceText": "go lang 1"
            },
            "hasError": false
        },
        {
            "value": {
                "ExampleId": 77783999,
                "UtteranceText": "go lang 2"
            },
            "hasError": false
        }
    ]
    training selected
    202
    {"statusId":9,"status":"Queued"}
    training status selected
    200
    [
        {
            "modelId": "c52d6509-9261-459e-90bc-b3c872ee4a4b",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "5119cbe8-97a1-4c1f-85e6-6449f3a38d77",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "01e6b6bc-9872-47f9-8a52-da510cddfafe",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "33b409b2-32b0-4b0c-9e91-31c6cfaf93fb",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "1fb210be-2a19-496d-bb72-e0c2dd35cbc1",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "3d098beb-a1aa-423f-a0ae-ce08ced216d6",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "cce854f8-8f8f-4ed9-a7df-44dfea562f62",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        },
        {
            "modelId": "4d97bf0d-5213-4502-9712-2d6e77c96045",
            "details": {
                "statusId": 3,
                "status": "InProgress",
                "exampleCount": 24
            }
        }
    ]
    ```

    Dit antwoord bevat de HTTP-statuscode voor elk van de drie HTTP-aanroepen, evenals elk JSON-antwoord dat in de hoofdtekst van het antwoord wordt geretourneerd. 

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Web-app met aangepast domein maken](luis-quickstart-intents-only.md) 