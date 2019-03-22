---
title: 'Quickstart: Antwoord uit knowledge base ophalen - REST, Node.js - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Deze Node.js REST-quickstart begeleidt u bij het programmatisch ophalen van een antwoord uit een knowledge base.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: f055ecdcc157a9ecf1a564913b0a4f889364b427
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309822"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-nodejs"></a>Antwoorden vinden op vragen met behulp van een knowledge base met Node.js

In deze quickstart wordt beschreven hoe u programmatisch een antwoord uit een gepubliceerde QnA Maker-knowledge base kunt ophalen. De knowledge base bevat vragen en antwoorden uit [gegevensbronnen](../Concepts/data-sources-supported.md) zoals veelgestelde vragen over. De [vraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) wordt verzonden naar de QnA Maker-service. De [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) bevat het antwoord boven voorspeld. 

## <a name="prerequisites"></a>Vereisten

* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/)
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer **Sleutels** onder **Resourcebeheer** in het Azure-dashboard voor de QnA Maker-resource om uw sleutel op te halen. 
* Pagina Instellingen voor **Publiceren**. Als u nog geen knowledge base hebt gepubliceerd, moet u een lege knowledge base maken, een knowledge base importeren op de pagina **Instellingen** en de knowledge base publiceren. U kunt [deze eenvoudige knowledge base](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv) downloaden en gebruiken. 

    De pagina Instellingen voor Publiceren bevat de waarde voor POST-route, Host en EndpointKey. 

    ![Instellingen voor Publiceren](../media/qnamaker-quickstart-get-answer/publish-settings.png)

De code voor deze quickstart bevindt zich in de opslagplaats [https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-nodejs-file"></a>Een Node.js-bestand maken

Open VSCode en maak een nieuw bestand met de naam `get-answer.js`. 

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Voeg bovenaan het bestand `get-answer.js` de vereiste afhankelijkheden toe aan het project:

[!code-nodejs[Add the required dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=1-4 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>De vereiste constanten toevoegen

Voeg vervolgens de vereiste constanten toe voor toegang tot QnA Maker. Deze waarden vindt u op de pagina **Publiceren** nadat u de knowledge base hebt gepubliceerd. 

[!code-nodejs[Add the required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=6-22 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Een POST-aanvraag toevoegen om vragen te verzenden en antwoorden op te halen

Met de volgende code wordt een HTTPS-aanvraag naar de QnA Maker-API verzonden om de vraag naar de knowledge base te verzenden en het antwoord te ontvangen:

[!code-nodejs[Add a POST request to send question to knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=24-49 "Add a POST request to send question to knowledge base")]

De waarde van de header van `Authorization` bevat de tekenreeks `EndpointKey `. 

## <a name="install-the-dependencies"></a>De afhankelijkheden installeren

Installeer als volgt de afhankelijkheden vanaf de opdrachtregel:

```bash
npm install request request-promise
```

## <a name="run-the-program"></a>Het programma uitvoeren

Voer het programma uit vanaf de opdrachtregel. De aanvraag wordt automatisch verzonden naar de QnA Maker-API, waarna het antwoord wordt weergegeven in het consolevenster.

Het bestand uitvoeren:

```bash
node get-answer.js
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

Meer informatie over de [aanvraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request) en [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response).

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
