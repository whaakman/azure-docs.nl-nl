---
title: 'Quickstart: Knowledge base bijwerken - REST, Node.js - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Deze quickstart helpt u bij het programmatisch bijwerken van een bestaande QnA Maker-knowledge base (KB).  Met deze JSON kunt u een KB bijwerken door nieuwe gegevensbronnen toe te voegen, gegevensbronnen te wijzigen of gegevensbronnen te verwijderen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 1b92589fe01f171b732c04057dc290f0f32cb31a
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647813"
---
# <a name="quickstart-update-a-knowledge-base-in-qna-maker-using-nodejs"></a>Snelstart: Een knowledge base bijwerken in QnA Maker met behulp van Node.js

Deze quickstart helpt u bij het programmatisch bijwerken van een bestaande QnA Maker-knowledge base (KB).  Met deze JSON kunt u een KB bijwerken door nieuwe gegevensbronnen toe te voegen, gegevensbronnen te wijzigen of gegevensbronnen te verwijderen.

Deze API is gelijk aan bewerken, waarna u de knop **Save and train** in de QnA Maker-portal gebruikt.

In deze snelstart worden QnA Maker-API's aangeroepen:
* [Update](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): het model voor de knowledge base is gedefinieerd in de JSON die in de hoofdtekst van de API-aanvraag wordt verzonden. 
* [Bewerkingsdetails ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Vereisten

* [Node.js 6+](https://nodejs.org/en/download/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer onder **Resourcebeheer** de optie **Sleutel** om uw sleutel op te halen. 
* Id voor knowledge base (KB) in QnA Maker gevonden in de URL in de parameter voor de kbid-queryreeks, zoals hieronder wordt weergegeven.

    ![Id voor knowledge base in QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Als u nog geen knowledge base hebt, kunt u een voorbeeldexemplaar maken om te gebruiken met deze snelstart: [Een nieuwe knowledge base maken](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Een Node.js-bestand voor de knowledge base maken

Maak een bestand met de naam `update-knowledge-base.js`.

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Voeg aan het begin van `update-knowledge-base.js` de volgende regels toe om de nodige afhankelijkheden aan het project toe te voegen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Vereiste constanten toevoegen
Voeg na de bovenstaande vereiste afhankelijkheden de vereiste constanten toe voor toegang tot QnA Maker. Vervang de waarde van de variabele `subscriptionKey` door uw eigen QnA Maker-sleutel. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Knowledge Base-id toevoegen

Voeg na de vorige constanten de knowledge base-id toe en koppel deze aan het pad:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-the-kb-update-model-definition"></a>De definitie van het KB-updatemodel toevoegen

Voeg na de constanten de volgende KB-updatedefinitie toe. De updatedefinitie heeft drie secties:

* add
* update
* delete

Elke sectie kan worden gebruikt in een en dezelfde aanvraag voor de API. 

[!code-nodejs[Add the KB update definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=25-53 "Add the KB update definition")]


## <a name="add-supporting-functions"></a>Ondersteunende functies toevoegen

Voeg daarna de volgende ondersteunende functies toe.

1. Voeg de volgende functie toe om JSON in een leesbare indeling weer te geven:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=55-58 "Add supporting functions, step 1")]

2. Voeg de volgende functies voor het beheren van het HTTP-antwoord toe om de bewerkingsstatus voor het maken op te halen:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=60-82 "Add supporting functions, step 2")]

## <a name="add-patch-request-to-update-kb"></a>PATCH-aanvraag toevoegen om KB bij te werken

Voeg de volgende functies toe voor het maken van een HTTP PATCH-aanvraag om de knowledge base bij te werken. De `Ocp-Apim-Subscription-Key` is de sleutel van de QnA Maker-service die wordt gebruikt voor verificatie.

[!code-nodejs[Add PATCH request to update KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=84-111 "Add PATCH request to update KB")]

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkings-id bevat. De bewerkings-id is nodig om de status aan te vragen als de bewerking niet voltooid is.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-10-02T01:23:00Z",
  "lastActionTimestamp": "2018-10-02T01:23:00Z",
  "userId": "335c3841df0b42cdb00f53a49d51a89c",
  "operationId": "e7be3897-88ff-44e5-a06c-01df0e05b78c"
}
```

## <a name="add-get-request-to-determine-operation-status"></a>GET-aanvraag toevoegen om de bewerkingsstatus te bepalen

Controleer de status van de bewerking.
    
[!code-nodejs[Add GET request to determine operation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=113-137 "Add GET request to determine operation status")]

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkingsstatus bevat: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

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

## <a name="add-updatekb-method"></a>update_kb-methode toevoegen

Met de volgende methode wordt de KB bijgewerkt en worden controles van de status herhaald. Omdat het maken van de KB enige tijd kan duren, moet u aanroepen herhalen om de status te controleren totdat de status geslaagd of mislukt is.

[!code-nodejs[Add update_kb method](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=139-169 "Add update_kb method")]

## <a name="run-the-program"></a>Het programma uitvoeren

Voer de volgende opdracht op een opdrachtregel in om het programma uit te voeren. Hiermee wordt de aanvraag verzonden naar de QnA Maker-API om de KB bij te werken en vervolgens worden elke 30 seconden de resultaten gecontroleerd. Elk antwoord wordt weergegeven in het consolevenster.

```bash
node update-knowledge-base.js
```

Zodra de knowledge base is bijgewerkt, kunt u deze weergeven in de QnA Maker-portal op de pagina [Mijn knowledge bases](https://www.qnamaker.ai/Home/MyServices). 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)