---
title: 'Snelstart: Knowledge base publiceren - REST, Node.js - QnA Maker'
titleSuffix: Azure Cognitive Services
description: In deze snelstart wordt beschreven hoe u programmatisch uw knowledge base (KB) kunt publiceren. Door te publiceren wordt de nieuwste versie van de knowledge base verstuurd naar een specifieke Azure Search-index en wordt er een eindpunt gemaakt dat kan worden aangeroepen in uw toepassing of chatbot.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: diberry
ms.openlocfilehash: c70b90a6e465c72193f63afd7ab9106579e2c634
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886607"
---
# <a name="quickstart-publish-a-qna-maker-knowledge-base-in-nodejs"></a>Snelstart: Een QnA Maker-knowledge base publiceren in Node.js

In deze snelstart wordt beschreven hoe u programmatisch uw knowledge base (KB) kunt publiceren. Door te publiceren wordt de nieuwste versie van de knowledge base verstuurd naar een specifieke Azure Search-index en wordt er een eindpunt gemaakt dat kan worden aangeroepen in uw toepassing of chatbot.

In deze snelstart worden QnA Maker-API's aangeroepen:
* [Publiceren](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): voor deze API zijn er geen gegevens in de hoofdtekst van de aanvraag nodig.

## <a name="prerequisites"></a>Vereisten

* [Node.js 6+](https://nodejs.org/en/download/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer onder **Resourcebeheer** de optie **Sleutel** om uw sleutel op te halen. 
* Id voor knowledge base (KB) in QnA Maker gevonden in de URL in de parameter voor de kbid-queryreeks, zoals hieronder wordt weergegeven.

    ![Id voor knowledge base in QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Als u nog geen knowledge base hebt, kunt u een voorbeeldexemplaar maken om te gebruiken met deze snelstart: [Een nieuwe knowledge base maken](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Een Node.js-bestand voor de knowledge base maken

Maak een bestand met de naam `publish-knowledge-base.js`.

## <a name="add-required-dependencies"></a>Vereiste afhankelijkheden toevoegen

Voeg bovenaan `publish-knowledge-base.js` de volgende regels toe om de nodige afhankelijkheden aan het project toe te voegen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Vereiste constanten toevoegen

Voeg na de bovenstaande vereiste afhankelijkheden de vereiste constanten toe voor toegang tot QnA Maker. Vervang de waarde van de variabele `subscriptionKey` door uw eigen QnA Maker-sleutel. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Knowledge Base-id toevoegen

Voeg na de vorige constanten de knowledge base-id toe en koppel deze aan het pad:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-supporting-functions"></a>Ondersteunende functies toevoegen

Voeg daarna de volgende ondersteunende functies toe.

1. Voeg de volgende functie toe om JSON in een leesbare indeling weer te geven:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=25-28 "Add supporting functions, step 1")]

2. Voeg de volgende functies voor het beheren van het HTTP-antwoord toe om de bewerkingsstatus voor het maken op te halen:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=30-52 "Add supporting functions, step 2")]

## <a name="add-the-publishkb-function-and-main-function"></a>De functie publish_kb en hoofdfunctie toevoegen

De volgende code maakt een HTTPS-aanvraag naar de QnA Maker-API om een KB te publiceren en ontvangt het antwoord:

[!code-nodejs[Add POST request to publish KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=54-71 "Add POST request to publish KB")]

[!code-nodejs[Add the publish_kb function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=73-91 "Add the publish_kb function and main function")]

## <a name="add-the-main-function"></a>De hoofdfunctie toevoegen

Voeg de volgende functie toe om de aanvraag en het antwoord te beheren:

[!code-nodejs[Add the main function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=94-97 "Add the main function")]

## <a name="run-the-program"></a>Het programma uitvoeren

Bouw het programma en voer het uit. De aanvraag wordt automatisch verzonden naar de QnA Maker-API om de KB te publiceren en vervolgens wordt het antwoord weergegeven in het consolevenster.

Zodra uw knowledge base is gepubliceerd, kunt u deze opvragen van het eindpunt met behulp van een clienttoepassing of chatbot. 

```bash
node publish-knowledge-base.js
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)