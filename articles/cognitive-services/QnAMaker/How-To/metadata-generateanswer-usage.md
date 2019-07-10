---
title: Metagegevens met GenerateAnswer API - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kunt u metagegevens in de vorm van sleutel/waarde-paren, toevoegen aan uw vraag/antwoord-sets. U kunt query's van gebruikers filteren, en opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/27/2019
ms.author: diberry
ms.openlocfilehash: 6bfcb531d0e4e8073a5553f7bc84a25e4f8a92a9
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785685"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>Een antwoord met de GenerateAnswer API en metagegevens

Als u het voorspelde antwoord op de vraag van een gebruiker, gebruikt u de GenerateAnswer-API. Wanneer u een kennisdatabase publiceert, ziet u informatie over het gebruik van deze API op de **publiceren** pagina. U kunt ook de API om te antwoorden op basis van metagegevenstags filteren configureren en testen van de knowledge base van het eindpunt met de test queryreeks-parameter.

QnA Maker kunt u metagegevens in de vorm van sleutel / waarde-paren, toevoegen aan uw sets met vragen en antwoorden. U kunt deze informatie vervolgens gebruiken om resultaten op query's van gebruikers te filteren, en voor het opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties. Zie voor meer informatie, [Knowledge base](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Vragen en antwoorden met een entiteit QnA Store

Het is belangrijk om te begrijpen hoe QnA Maker de vraag en antwoord-gegevens opslaat. De volgende afbeelding ziet u een entiteit QnA:

![Afbeelding van een entiteit QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Elke entiteit QnA heeft een unieke en permanente-ID. De ID kunt u updates aanbrengen in een bepaalde QnA-entiteit.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>De voorspellingen antwoord met de API GenerateAnswer ophalen

U gebruikt de [GenerateAnswer API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) in uw bot of de toepassing om op te vragen uw knowledge base met de vraag van een gebruiker, de beste overeenkomst ophalen van de vraag en antwoord ingesteld.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>Publiceren GenerateAnswer-eindpunt ophalen 

Nadat u uw knowledge base, via de [QnA Maker portal](https://www.qnamaker.ai), of met behulp van de [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish), krijgt u de details van uw eindpunt GenerateAnswer.

Uw eindpunt om informatie te krijgen:
1. Meld u aan bij [https://www.qnamaker.ai](https://www.qnamaker.ai).
1. In **mijn knowledge bases**, selecteer **Code weergeven** voor uw knowledge base.
    ![Schermopname van mijn knowledge bases](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. De details van uw GenerateAnswer-eindpunt ophalen.

    ![Schermafbeelding van de details van eindpunt](../media/qnamaker-how-to-metadata-usage/view-code.png)

U krijgt ook de details van het eindpunt van de **instellingen** tabblad van de knowledge base.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>Configuratie van de aanvraag GenerateAnswer

U aanroepen GenerateAnswer met een HTTP POST-aanvraag. Voorbeeld van code die laat zien hoe u aan te roepen GenerateAnswer, Zie de [snelstartgidsen](../quickstarts/csharp.md). 

Maakt gebruik van de POST-aanvraag:

* Vereiste [URI-parameters](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#uri-parameters)
* Vereiste [eigenschap header](https://docs.microsoft.com/azure/cognitive-services/qnamaker/quickstarts/get-answer-from-knowledge-base-nodejs#add-a-post-request-to-send-question-and-get-an-answer), `Authorization`, voor beveiliging
* Vereiste [eigenschappen hoofdtekst](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train#feedbackrecorddto). 

De URL GenerateAnswer heeft de volgende indeling: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

Houd er rekening mee om in te stellen de eigenschap van de HTTP-header van `Authorization` met een waarde van de tekenreeks `EndpointKey` waarbij een afsluitende spatie vervolgens de eindpuntsleutel gevonden op de **instellingen** pagina.

Een voorbeeld-JSON-hoofdtekst ziet eruit zoals:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>GenerateAnswer response-eigenschappen

De [antwoord](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#successful-query) is een JSON-object met inbegrip van alle informatie die u nodig hebt om weer te geven van het antwoord en de volgende inschakelen in het gesprek op, indien beschikbaar.

```json
{
    "answers": [
        {
            "score": 28.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
            "metadata": [
                {
                    "name": "category",
                    "value": "api"
                }
            ]
        }
    ]
}
```

## <a name="use-qna-maker-with-a-bot-in-c"></a>Gebruik QnA Maker met een bot inC#

Botframework biedt toegang tot de QnA Maker-eigenschappen:

```csharp
using Microsoft.Bot.Builder.AI.QnA;
var metadata = new Microsoft.Bot.Builder.AI.QnA.Metadata();
var qnaOptions = new QnAMakerOptions();

qnaOptions.Top = Constants.DefaultTop;
qnaOptions.ScoreThreshold = 0.3F;
var response = await _services.QnAServices[QnAMakerKey].GetAnswersAsync(turnContext, qnaOptions);
```

De bot ondersteuning heeft [een voorbeeld](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-support/csharp_dotnetcore/Service/SupportBotService.cs#L418) met deze code.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Gebruik QnA Maker met een bot in Node.js

Botframework biedt toegang tot de QnA Maker-eigenschappen:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.03,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

De bot ondersteuning heeft [een voorbeeld](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs/Helpers/dialogHelper.js#L36) met deze code.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Met behulp van metagegevens voor het filteren van antwoorden door aangepaste metagegevenstags

Metagegevens toe te voegen, kunt u de antwoorden op deze metagegevenstags filteren. Toevoegen van de metagegevenskolom van de **weergaveopties** menu. Metagegevens toevoegen aan uw knowledge base door te selecteren van de metagegevens van de **+** pictogram voor het toevoegen van een combinatie van metagegevens. Deze paar bestaat uit één sleutel en één waarde.

![Schermafbeelding van het toevoegen van metagegevens](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Resultaten met strictFilters voor metagegevenstags filteren

Houd rekening met de vraag van de gebruiker 'Wanneer is deze hotel sluiten?', waarbij de bedoeling is impliciet voor het restaurant "Paradise."

Omdat de resultaten zijn alleen vereist voor het restaurant "Paradise", kunt u een filter in de aanroep van GenerateAnswer instellen op de metagegevens van de 'Naam Restaurant'. Het volgende voorbeeld laat dit zien:

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

<a name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Vraag en antwoord resultaten te houden van de conversatie-context gebruiken

Het antwoord op de GenerateAnswer bevat de bijbehorende metagegevens van de overeenkomende vraag en antwoord-set. U kunt deze informatie gebruiken in uw clienttoepassing voor het opslaan van de context van het vorige gesprek voor gebruik in latere conversaties. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="match-questions-only-by-text"></a>Alleen vragen door tekst overeenkomen

Standaard zoekt QnA Maker met vragen en antwoorden. Als u doorzoeken vragen alleen wilt, voor het genereren van een antwoord, gebruikt de `RankerType=QuestionOnly` in de hoofdtekst van bericht van de aanvraag GenerateAnswer.

U kunt zoeken via de gepubliceerde kb, met behulp van `isTest=false`, of in de test kb met `isTest=true`.

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="next-steps"></a>Volgende stappen

De **publiceren** pagina bevat ook informatie voor het genereren van een antwoord met [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) en [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](./create-knowledge-base.md)
