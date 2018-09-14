---
title: Tekst in natuurlijke taal analyseren in LUIS (Language Understanding) met Ruby - Cognitive Services - Azure Cognitive Services | Microsoft Docs
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik Ruby om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 7344d0e4d649134b7d928daec99fa79d0644a7e4
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "43769976"
---
# <a name="quickstart-analyze-text-using-ruby"></a>Snelstart: tekst analyseren met Ruby

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

* [Ruby](https://www.ruby-lang.org/)-programmeertaal
* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

<a name="create-luis-subscription-key"></a>

## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Tekst analyseren met browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-ruby"></a>Tekst analyseren met Ruby 

U kunt Ruby gebruiken voor toegang tot de resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Kopieer de volgende code en sla deze op in een bestand met de naam `endpoint-call.rb`:

   [!code-ruby[Ruby code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/ruby/endpoint-call.rb)]

2. Vervang `"YOUR-KEY"` door uw eindpuntsleutel.

3. Voer de Ruby-toepassing uit op de opdrachtregel met `ruby endpoint-call.rb`. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.

    ```
    LUIS query: turn on the left light
    
    Request URI: https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn+on+the+left+light&timezoneOffset=0&verbose=false&spellCheck=false&staging=false
    
    JSON Response:
    
    {
      "query": "turn on the left light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.933549
      },
      "entities": [
        {
          "entity": "left",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 15,
          "score": 0.540835142
        }
      ]
    }
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het Ruby-bestand.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-ruby-add-utterance.md)