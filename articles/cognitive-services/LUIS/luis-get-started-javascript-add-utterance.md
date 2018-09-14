---
title: 'Snelstart: Utterances toevoegen aan een LUIS-app met behulp van JavaScript - Azure Cognitive Services| Microsoft Docs'
description: In deze snelstart leert u hoe u een LUIS-app aanroept met behulp van JavaScript.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: ffc19d12c1d3fbb24c514ac87f298d1a52d23eb8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43770080"
---
# <a name="quickstart-change-model-using-javascript"></a>Snelstart: Model wijzigen met behulp van JavaScript

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/).

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-bestand met voorbeeldutterances

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


## <a name="create-quickstart-code"></a>Snelstartcode maken

Maak `add-utterances.html` en voeg de volgende code toe:

   [!code-html[Html code](~/samples-luis/documentation-samples/quickstarts/change-model/javascript/add-utterance.html "Javascript code")]

## <a name="run-code"></a>Code uitvoeren

1. Open het bestand in een browser.

2. Voeg uw authoring-id en toepassings-id van LUIS toe.

3. Wijzig de **matrix met utterances** om aan uw toepassing toe te voegen. Deze worden opgeslagen in de variabele utteranceJSON. Wijzig deze waarden voor uw eigen domein- en utterance-behoeften. 

    ```json
    // example batch utterances
    var utteranceJSON = [
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
        }
    ,
        {
            "text": "book a flight",
            "intentName": "BookFlight",
            "entityLabels": []
        }
    ];
    ```

4. Selecteer de knop `Upload utterance`. De LUIS-resultaten worden onder de knoppen weergegeven.

5. Selecteer de knop `Train model` om uw toepassing te trainen met deze nieuwe utterances.

6. Selecteer de knop `Train Status` om de trainingsstatus te bekijken. 

    ![Add-utterances.html](./media/luis-quickstart-javascript-add-utterance/add-utterance.png)

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [LUIS integreren met een bot](luis-csharp-tutorial-build-bot-framework-sample.md)
