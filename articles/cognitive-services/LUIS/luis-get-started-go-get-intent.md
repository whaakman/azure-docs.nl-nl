---
title: Snelstart voor Go - Intentie voorspellen - LUIS
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik Go om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f19c46fcc7dcff2e02786bb3a03082f831e880a2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032730"
---
# <a name="quickstart-get-intent-using-go"></a>Snelstart: intentie ophalen met Go

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

* [Go](https://golang.org/)-programmeertaal  
* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>De intentie via een browser ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>De intentie programmatisch ophalen

U kunt Go gebruiken voor toegang tot de dezelfde resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Maak een nieuw bestand met de naam `endpoint.go`. Voeg de volgende code toe:
    
   [!code-go[Go code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/go/endpoint.go?range=36-98)]

2. Voer, in dezelfde map waarin u het bestand hebt gemaakt, met een opdrachtprompt `go build endpoint.go` in om het Go-bestand te compileren. De opdrachtprompt retourneert geen informatie voor een geslaagde build.

3. Voer de Go-toepassing uit vanaf de opdrachtregel door de volgende tekst in te voeren in de opdrachtprompt: 

    ```CMD
    go run endpoint.go -appID df67dcdb-c37d-46af-88e1-8b97951ca1c2 -endpointKey <add-your-key> -region westus
    ```
    
    Vervang `<add-your-key>` door de waarde van de sleutel.  
    
    Het antwoord van de opdrachtregel is: 
    
    ```CMD
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    region has value westus
    utterance has value turn on the bedroom light
    response
    {
        "query": "turn on the bedroom light",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.809439957
        },
        "entities": [
            {
            "entity": "bedroom",
            "type": "HomeAutomation.Room",
            "startIndex": 12,
            "endIndex": 18,
            "score": 0.8065475
            }
        ]
    }
    ```
    
## <a name="luis-keys"></a>LUIS-sleutels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen
Sluit het Go-bestand en verwijder het uit het bestandssysteem. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-go-add-utterance.md)