---
title: 'Quickstart: Een knowledge base maken - REST, Go - QnA Maker'
titlesuffix: Azure Cognitive Services
description: In deze op REST gebaseerde snelstart wordt stapsgewijs uitgelegd hoe u, met behulp van een programma, een voorbeeldexemplaar van een knowledge base in QnA Maker kunt maken, dat wordt weergegeven op het Azure-dashboard van uw account voor de Cognitive Services-API.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/04/2019
ms.author: diberry
ms.openlocfilehash: 4831fca8a72f62d64c1977b510f039065f6cd3c5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788570"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-go"></a>Quickstart: Een knowledge base maken in QnA Maker met behulp van Go

In deze snelstart wordt beschreven hoe u programmatisch een voorbeeld van een QnA Maker-knowledge base kunt maken. Met QnA Maker worden automatisch vragen en antwoorden opgehaald uit semi-gestructureerde inhoud, zoals veelgestelde vragen, vanuit [gegevensbronnen](../Concepts/data-sources-supported.md). Het model voor de knowledge base wordt gedefinieerd in de JSON die in de hoofdtekst van de API-aanvraag wordt verzonden. 

In deze snelstart worden QnA Maker-API's aangeroepen:
* [KB maken](https://go.microsoft.com/fwlink/?linkid=2092179)
* [Bewerkingsdetails ophalen](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

## <a name="prerequisites"></a>Vereisten

* [Go 1.10.1](https://golang.org/dl/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer onder **Resourcebeheer** de optie **Sleutel** om uw sleutel op te halen. 

## <a name="create-a-knowledge-base-go-file"></a>Een Go-bestand met knowledge base maken

Maak een bestand met de naam `create-new-knowledge-base.go`.

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Voeg aan het begin van `create-new-knowledge-base.go` de volgende regels toe om de nodige afhankelijkheden aan het project toe te voegen:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=1-11 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>De vereiste constanten toevoegen
Voeg de vereiste constanten toe voor toegang tot QnA Maker na de bovenstaande vereiste afhankelijkheden. Vervang de waarde van de variabele `subscriptionKey` door uw eigen QnA Maker-sleutel.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=13-20 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>De definitie van het KB-model toevoegen
Voeg na de constanten de volgende KB-modeldefinitie toe. Het model wordt na de definitie naar een tekenreeks geconverteerd.

[!code-go[Add the KB model definition](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=22-44 "Add the KB model definition")]

## <a name="add-supporting-structures-and-functions"></a>Ondersteunende functies en structuren toevoegen

Voeg daarna de volgende ondersteunende functies toe.

1. De structuur van een HTTP-aanvraag toevoegen:

    [!code-go[Add the structure for an HTTP request](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=46-49 "Add the structure for an HTTP request")]

2. Voeg de volgende methode toe voor het afhandelen van een POST-bewerking voor de QnA Maker-API's. In deze snelstart wordt POST gebruikt voor het verzenden van de KB-definitie naar QnA Maker.

    [!code-go[Add the POST method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=51-66 "Add the POST method")]

3. Voeg de volgende methode toe voor het afhandelen van een GET-bewerking voor de QnA Maker-API's. In deze snelstart wordt GET gebruikt om de status van de maakbewerking te controleren. 

    [!code-go[Add the GET method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=68-83 "Add the GET method")]

## <a name="add-function-to-create-kb"></a>Functie toevoegen om KB te maken

Voeg de volgende functies toe voor het maken van een HTTP POST-aanvraag om de knowledge base te maken. De **bewerkings-id** voor _maken_ wordt geretourneerd in het header-veld voor het POST-antwoord **Locatie**, en vervolgens gebruikt als onderdeel van de route in de GET-aanvraag. De `Ocp-Apim-Subscription-Key` is de sleutel van de QnA Maker-service die wordt gebruikt voor verificatie. 

[!code-go[Add the create_kb method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=85-97 "Add the create_kb method")]

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkings-id bevat. Gebruik de bewerkings-id om te bepalen of de KB is gemaakt. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-get-status"></a>Een functie toevoegen om de status op te halen

Voeg de volgende functie toe om een HTTP GET-aanvraag te maken om de bewerkingsstatus te controleren. De `Ocp-Apim-Subscription-Key` is de sleutel van de QnA Maker-service die wordt gebruikt voor verificatie. 

[!code-go[Add the check_status method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=99-108 "Add the check_status method")]

Herhaal de aanroep totdat deze lukt of mislukt: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```
## <a name="add-main-function"></a>Een hoofdfunctie toevoegen

De volgende functie is de hoofdfunctie. Deze maakt de KB en voert herhalend controles uit voor de status. Omdat het maken van de KB enige tijd kan duren, moet u aanroepen herhalen om de status te controleren totdat de status geslaagd of mislukt is.

[!code-go[Add the main method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=110-140 "Add the main method")]


## <a name="compile-the-program"></a>Het programma compileren
Voer de volgende opdracht in voor het compileren van het bestand. De opdrachtprompt retourneert geen informatie voor een geslaagde build.

```bash
go build create-new-knowledge-base.go
```

## <a name="run-the-program"></a>Het programma uitvoeren

Voer de volgende opdracht op een opdrachtregel in om het programma uit te voeren. Hiermee wordt de aanvraag verzonden naar de QnA Maker-API om de KB te maken en vervolgens worden elke 30 seconden de resultaten gecontroleerd. Elk antwoord wordt weergegeven in het consolevenster.

```bash
go run create-new-knowledge-base
```

Zodra de knowledge base is gemaakt, kunt u deze weergeven in de QnA Maker-portal op de pagina [Mijn knowledge bases](https://www.qnamaker.ai/Home/MyServices). 

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
