---
title: Knowledge base publiceren, REST, Node.js
titleSuffix: QnA Maker - Azure Cognitive Services
description: In deze Node.js-snelstartgids wordt beschreven hoe u programmatisch uw knowledge base (KB) kunt publiceren. Door te publiceren wordt de nieuwste versie van de knowledge base verstuurd naar een specifieke Azure Search-index en wordt er een eindpunt gemaakt dat kan worden aangeroepen in uw toepassing of chatbot.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 187bfcac5127afe2c48441ae993acadbf417e70d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214395"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-nodejs"></a>Snelstartgids: Een knowledge base publiceren in QnA Maker met behulp van Node.js

In deze REST-quickstart wordt beschreven hoe u programmatisch uw KB (knowledge base ) kunt publiceren. Door te publiceren wordt de nieuwste versie van de knowledge base verstuurd naar een specifieke Azure Search-index en wordt er een eindpunt gemaakt dat kan worden aangeroepen in uw toepassing of chatbot.

In deze snelstart worden QnA Maker-API's aangeroepen:
* [Publiceren](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): voor deze API zijn er geen gegevens in de hoofdtekst van de aanvraag nodig.

## <a name="prerequisites"></a>Vereisten

* [Node.js 6+](https://nodejs.org/en/download/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer onder **Resourcebeheer** de optie **Sleutel** om uw sleutel op te halen. 
* Id voor knowledge base (KB) in QnA Maker gevonden in de URL in de parameter voor de kbid-queryreeks, zoals hieronder wordt weergegeven.

    ![Id voor knowledge base in QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Als u nog geen knowledge base hebt, kunt u een voorbeeldexemplaar maken om te gebruiken met deze snelstartgids: [Een nieuwe knowledge base maken](create-new-kb-nodejs.md).


> [!NOTE] 
> Het bestand of de bestanden van de volledige oplossing zijn beschikbaar in de GitHub-opslagplaats [**Azure-Samples/cognitive-services-qnamaker-nodejs**](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/publish-knowledge-base-short).

## <a name="create-a-knowledge-base-nodejs-file"></a>Een Node.js-bestand voor de knowledge base maken

Maak een bestand met de naam `publish-knowledge-base.js`.

## <a name="add-required-dependencies"></a>Vereiste afhankelijkheden toevoegen

Voeg bovenaan `publish-knowledge-base.js` de volgende regels toe om de nodige afhankelijkheden aan het project toe te voegen:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=1-3 "Add the dependencies")]

## <a name="add-required-constants"></a>Vereiste constanten toevoegen

Voeg na de bovenstaande vereiste afhankelijkheden de vereiste constanten toe voor toegang tot QnA Maker. Vervang de waarden door uw eigen waarden.

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=11-14 "Add required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>POST-aanvraag toevoegen om knowledge base te publiceren

Voeg na de vereiste constanten de volgende code toe, waarmee een HTTP-aanvraag bij de QnA Maker-API wordt gedaan voor het publiceren van een knowledge base en het volgende antwoord wordt ontvangen:

[!code-nodejs[Add a POST request to publish knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base-short/publish-knowledge-base.js?range=16-47 "Add a POST request to publish knowledge base")]

De API-aanroep retourneert een 204-status voor een geslaagde publicatie zonder enige inhoud in de hoofdtekst van het antwoord. De code voegt inhoud toe voor 204-antwoorden.

Voor alle andere antwoorden wordt het antwoord ongewijzigd geretourneerd.

## <a name="run-the-program"></a>Het programma uitvoeren

Bouw het programma en voer het uit. De aanvraag wordt automatisch verzonden naar de QnA Maker-API om de knowledge base te publiceren en vervolgens wordt het antwoord weergegeven in het consolevenster.

Zodra uw knowledge base is gepubliceerd, kunt u deze opvragen van het eindpunt met behulp van een clienttoepassing of chatbot. 

```bash
node publish-knowledge-base.js
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Volgende stappen

Nadat de knowledge base is gepubliceerd, moet u ervoor zorgen dat via de [eindpunt-URL een antwoord wordt gegenereerd](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
