---
title: Metagegevens met GenerateAnswer API - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kunt u metagegevens in de vorm van sleutel/waarde-paren, toevoegen aan uw vraag/antwoord-sets. Deze informatie kan worden gebruikt voor het filteren van resultaten voor query's van gebruikers en voor het opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/21/2019
ms.author: tulasim88
ms.openlocfilehash: 9cb16842e0bc80a1fcbd066bea44c5b9701bb6d5
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56651206"
---
# <a name="get-a-knowledge-answer-with-the-generateanswer-api-and-metadata"></a>Een antwoord kennis met de GenerateAnswer API en metagegevens

Als u het voorspelde antwoord op de vraag van een gebruiker, gebruikt u de GenerateAnswer-API. Wanneer u een kennisdatabase publiceert, wordt deze informatie over het gebruik van deze API wordt weergegeven op de pagina publiceren. U kunt ook de API om te antwoorden op basis van metagegevenstags filteren configureren en testen van de knowledge base van het eindpunt met de test queryreeks-parameter.

QnA Maker kunt u metagegevens in de vorm van sleutel / waarde-paren, toevoegen aan uw vraag/antwoord-sets. Deze informatie kan worden gebruikt voor het filteren van resultaten voor query's van gebruikers en voor het opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties. Zie voor meer informatie, [Knowledge base](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="storing-questions-and-answers-with-a-qna-entity"></a>Vragen en antwoorden met een entiteit QnA opslaan

Eerst is het belangrijk om te begrijpen hoe QnA Maker de vraag/antwoord-gegevens opslaat. De volgende afbeelding ziet u een entiteit QnA:

![QnA-entiteit](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Elke entiteit QnA heeft een unieke en permanente-ID. De ID kan worden gebruikt om updates voor een bepaalde QnA-entiteit maken.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>De voorspellingen antwoord met de API GenerateAnswer ophalen

In dat geval gebruikt u de API GenerateAnswer in uw Bot of toepassing in om op te vragen uw knowledge base met een vraag gebruikers om op te halen van de beste overeenkomst van de vraag/antwoord-sets.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>Publiceren GenerateAnswer-eindpunt ophalen 

Nadat u uw knowledge base, via de [QnA Maker portal](https://www.qnamaker.ai), of met behulp van de [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), krijgt u de details van uw eindpunt GenerateAnswer.

Uw eindpunt om informatie te krijgen:
1. Meld u aan bij [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
1. In **mijn knowledge bases**, klikt u op **Code weergeven** voor uw knowledge base.
    ![Mijn knowledge bases](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. De details van uw GenerateAnswer-eindpunt ophalen.

    ![details van eindpunt](../media/qnamaker-how-to-metadata-usage/view-code.png)

U krijgt ook de details van het eindpunt van de **instellingen** tabblad van de knowledge base.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>Configuratie van de aanvraag GenerateAnswer

U aanroepen GenerateAnswer met een HTTP POST-aanvraag. Voorbeeld van code die laat zien hoe u aan te roepen GenerateAnswer, Zie de [snelstartgidsen](../quickstarts/csharp.md).

De **aanvraag-URL** heeft de volgende indeling: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer?isTest=true
```

|De eigenschap van de HTTP-aanvraag|Name|Type|Doel|
|--|--|--|--|
|URL-parameter voor route|Knowledge base-ID|string|De GUID voor uw knowledge base.|
|URL-parameter voor route|Host van het eindpunt QnAMaker|string|De hostnaam van het eindpunt dat is geïmplementeerd in uw Azure-abonnement. Dit is beschikbaar op de pagina instellingen nadat u de knowledge base hebt gepubliceerd. |
|Header|Content-Type|string|Het mediatype van de hoofdtekst van het verzonden naar de API. Standaardwaarde: ''|
|Header|Autorisatie|string|De eindpuntsleutel van uw (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Hoofdtekst van bericht|JSON-object|JSON|De vraag met instellingen|
|Queryreeks-parameter (optioneel)|`isTest`|booleaans|Indien ingesteld op true, retourneert resultaten van `testkb` Search-index in plaats van gepubliceerde index.|

De hoofdtekst van de JSON bevat verschillende instellingen:

|JSON-hoofdtekst van de eigenschap|Vereist|Type|Doel|
|--|--|--|--|
|`question`|vereist|string|Een vraag van de gebruiker moet worden verzonden naar uw knowledge base.|
|`top`|optioneel|geheel getal|Het aantal gerangschikte resultaten om op te nemen in de uitvoer. De standaardwaarde is 1.|
|`userId`|optioneel|string|Een unieke ID voor het identificeren van de gebruiker. Deze ID wordt geregistreerd in de chat-Logboeken.|
|`strictFilters`|optioneel|string|Indien opgegeven, wordt aan de QnA Maker om terug te keren alleen de antwoorden die de opgegeven metagegevens zijn.|

Een voorbeeld-JSON-hoofdtekst ziet eruit zoals:

```json
{
    "question": "qna maker and luis",
    "top": 6,
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

Een geslaagde respons retourneert een status van 200 en een JSON-antwoord. 

|Antwoorden eigenschap (gesorteerd op score)|Doel|
|--|--|
|score|Een rangorde tussen 0 en 100 liggen.|
|Id|Een unieke ID die is toegewezen aan het antwoord.|
|Vragen|De vragen die is opgegeven door de gebruiker.|
|Antwoord|Het antwoord op de vraag.|
|source|De naam van de bron van waaruit het antwoord is uitgepakt of opgeslagen in het knowledge base.|
|metagegevens|De metagegevens die zijn gekoppeld aan het antwoord.|
|metadata.name|Naam voor de metagegevens. (string, maximumlengte: 100, vereist)|
|metadata.value: De waarde van de metagegevens. (string, maximumlengte: 100, vereist)|


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

<a name="metadata-example"></a>

## <a name="using-metadata-allows-you-to-filter-answers-by-custom-metadata-tags"></a>Met behulp van metagegevens kunt u antwoorden op aangepaste metagegevenstags filteren

Metagegevens toe te voegen, kunt u de antwoorden op deze metagegevenstags filteren. Houd rekening met de onderstaande veelgestelde vragen over gegevens. Metagegevens toevoegen aan uw knowledge base door te klikken op het pictogram van de metagegevens.

![metagegevens toevoegen](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Resultaten met strictFilters voor metagegevenstags filteren

Houd rekening met de vraag van de gebruiker "Wanneer wordt deze hotel sluiten?" Wanneer de bedoeling is impliciet voor het restaurant "Paradise."

Omdat de resultaten zijn alleen vereist voor het restaurant "Paradise", kunt u een filter in de aanroep van GenerateAnswer over de metagegevens "Restaurant naam", als volgt instellen.

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

<name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Vraag en antwoord resultaten te houden van de conversatie-context gebruiken

Het antwoord op de GenerateAnswer bevat de bijbehorende metagegevens van de set overeenkomende vraag/antwoord. Deze informatie kan worden gebruikt in uw clienttoepassing voor het opslaan van de context van het vorige gesprek voor gebruik in latere conversaties. 

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

## <a name="next-steps"></a>Volgende stappen

De pagina publiceren bevat ook informatie voor het genereren van een antwoord met [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) en [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](./create-knowledge-base.md)
