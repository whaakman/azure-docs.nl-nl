---
title: Metagegevens met GenerateAnswer API - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker kunt u metagegevens in de vorm van sleutel/waarde-paren, toevoegen aan uw vraag/antwoord-sets. U kunt query's van gebruikers filteren, en opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/30/2019
ms.author: tulasim
ms.openlocfilehash: b18d47b4b09c6fa9c4d5f0ef87d7ebe73f151c60
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693245"
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

U de API GenerateAnswer in uw bot of toepassing gebruiken om op te vragen uw knowledge base met de vraag van een gebruiker, wordt voor de beste overeenkomst van de vraag en antwoord.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>Publiceren GenerateAnswer-eindpunt ophalen 

Nadat u uw knowledge base, via de [QnA Maker portal](https://www.qnamaker.ai), of met behulp van de [API](https://go.microsoft.com/fwlink/?linkid=2092179), krijgt u de details van uw eindpunt GenerateAnswer.

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

De **aanvraag-URL** heeft de volgende indeling: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

|De eigenschap van de HTTP-aanvraag|Name|Type|Doel|
|--|--|--|--|
|URL-parameter voor route|Knowledge base-ID|string|De GUID voor uw knowledge base.|
|URL-parameter voor route|Host van het eindpunt QnAMaker|string|De hostnaam van het eindpunt dat is geïmplementeerd in uw Azure-abonnement. Deze optie is beschikbaar op de **instellingen** pagina nadat u de knowledge base hebt gepubliceerd. |
|Header|Content-Type|string|Het mediatype van de hoofdtekst van het verzonden naar de API. Standaardwaarde: ''|
|Header|Autorisatie|string|De eindpuntsleutel van uw (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Hoofdtekst van bericht|JSON-object|JSON|De vraag met instellingen.|


De hoofdtekst van de JSON bevat verschillende instellingen:

|JSON-hoofdtekst van de eigenschap|Vereist|Type|Doel|
|--|--|--|--|
|`question`|Vereist|string|Een vraag van de gebruiker moet worden verzonden naar uw knowledge base.|
|`top`|Optioneel|geheel getal|Het aantal gerangschikte resultaten om op te nemen in de uitvoer. De standaardwaarde is 1.|
|`userId`|Optioneel|string|Een unieke ID voor het identificeren van de gebruiker. Deze ID wordt geregistreerd in de chat-Logboeken.|
|`scoreThreshold`|Optioneel|geheel getal|Alleen antwoorden met een betrouwbaarheidsscore boven deze drempelwaarde worden geretourneerd. De standaardwaarde is 0.|
|`isTest`|Optioneel|Boolean|Indien ingesteld op true, retourneert resultaten van `testkb` Search-index in plaats van gepubliceerde index.|
|`strictFilters`|Optioneel|string|Indien opgegeven, wordt aan de QnA Maker om terug te keren alleen de antwoorden die de opgegeven metagegevens zijn. Gebruik `none` om aan te geven antwoord moet er geen filters voor metagegevens. |
|`RankerType`|Optioneel|string|Als u opgeeft als `QuestionOnly`, QnA Maker om te zoeken naar vragen alleen aangeeft. Indien niet opgegeven, zoekt de QnA Maker vragen en antwoorden.

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
|metadata.value|De waarde van de metagegevens. (string, maximumlengte: 100, vereist)|


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
