---
title: 'Zelfstudie: utterances toevoegen aan een LUIS-app met behulp van JavaScript | Microsoft Docs'
description: In deze zelfstudie leert u een LUIS-app aan te roepen met behulp van JavaScript.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: v-geberr
ms.openlocfilehash: b6d021dcfdddb5449aa989c6aa06d7faf326befb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265456"
---
# <a name="tutorial-add-utterances-to-app-using-javascript"></a>Zelfstudie: utterances toevoegen aan een app met behulp van JavaScript
In deze zelfstudie schrijft u een programma om een utterance aan een intentie toe te voegen met behulp van de creatie-API's in JavaScript.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Visual Studio-consoleproject maken 
> * Methode toevoegen voor het aanroepen van de LUIS-API om een utterance toe te voegen en een app te trainen
> * JSON-bestand toevoegen met voorbeeldutterances voor BookFlight-intentie
> * Console uitvoeren en trainingsstatus weergeven voor utterances

Zie voor meer informatie de technische documentatie voor de API’s voor het [toevoegen van voorbeeldutterances aan intenties](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [trainen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) en [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="prerequisites"></a>Vereisten
* Uw LUIS-[**creatiesleutel**](luis-concept-keys.md#authoring-key). 
* Uw bestaande LUIS-**toepassings-id** en **-versie-id**. 
* Een nieuw bestand met de naam `add-utterances.html`-project in VSCode.

> [!NOTE] 
> Het volledige `add-utterances.html`-bestand is beschikbaar in de Github-opslagplaats [**LUIS Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/javascript/add-utterance.html).


## <a name="write-the-code"></a>De code schrijven
Maak `add-utterances.html` en voeg de volgende code toe:

   [!code-javascript[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/javascript/add-utterance.html "Java Dependencies")]

## <a name="view-in-browser"></a>Bekijken in browser
1. Open het bestand in een browser.

2. Voeg uw LUIS-creatie-id en uw LUIS-toepassings-id toe, en wijzig de versie als deze niet `0.1` is

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
Wanneer u klaar bent met de zelfstudie, verwijdert Visual Studio en de console-toepassing als u deze niet meer nodig hebt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [LUIS integreren met een bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website