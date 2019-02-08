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
ms.date: 12/18/2018
ms.author: tulasim88
ms.openlocfilehash: 1294b714c217178d53ed69cc886cd89f23620274
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859484"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Met behulp van metagegevens en de GenerateAnswer-API

QnA Maker kunt u metagegevens in de vorm van sleutel / waarde-paren, toevoegen aan uw vraag/antwoord-sets. Deze informatie kan worden gebruikt voor het filteren van resultaten voor query's van gebruikers en voor het opslaan van aanvullende informatie die kan worden gebruikt in follow-up conversaties. Zie voor meer informatie, [Knowledge base](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>QnA-entiteit

Eerst is het belangrijk om te begrijpen hoe QnA Maker de vraag/antwoord-gegevens opslaat. De volgende afbeelding ziet u een entiteit QnA:

![QnA-entiteit](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Elke entiteit QnA heeft een unieke en permanente-ID. De ID kan worden gebruikt om updates voor een bepaalde QnA-entiteit maken.

## <a name="generateanswer-api"></a>GenerateAnswer API

In dat geval gebruikt u de API GenerateAnswer in uw Bot of toepassing in om op te vragen uw knowledge base met een vraag gebruikers om op te halen van de beste overeenkomst van de vraag/antwoord-sets.

### <a name="generateanswer-endpoint"></a>GenerateAnswer eindpunt

Nadat u uw knowledge base, via de [QnA Maker portal](https://www.qnamaker.ai), of met behulp van de [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), krijgt u de details van uw eindpunt GenerateAnswer.

Uw eindpunt om informatie te krijgen:
1. Meld u aan bij [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. In **mijn knowledge bases**, klikt u op **Code weergeven** voor uw knowledge base.
![Mijn knowledge bases](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. De details van uw GenerateAnswer-eindpunt ophalen.

![details van eindpunt](../media/qnamaker-how-to-metadata-usage/view-code.png)

U krijgt ook de details van het eindpunt van de **instellingen** tabblad van de knowledge base.

### <a name="generateanswer-request"></a>GenerateAnswer aanvraag

U aanroepen GenerateAnswer met een HTTP POST-aanvraag. Voorbeeld van code die laat zien hoe u aan te roepen GenerateAnswer, Zie de [snelstartgidsen](../quickstarts/csharp.md).

- **Aanvraag-URL**: https://{QnA Maker eindpunt} /knowledgebases/ {knowledge base-ID} / generateAnswer

- **Aanvraagparameters**: 
    - **Knowledge base-ID** (tekenreeks): De GUID voor uw knowledge base.
    - **QnAMaker eindpunt** (tekenreeks): De hostnaam van het eindpunt dat is geïmplementeerd in uw Azure-abonnement.
- **Aanvraagheaders**
    - **Content-Type** (tekenreeks): Het mediatype van de hoofdtekst van het verzonden naar de API.
    - **Autorisatie** (tekenreeks): De eindpuntsleutel van uw (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Aanvraagtekst**
    - **vraag** (tekenreeks): Een vraag van de gebruiker moet worden gezocht op basis van uw knowledge base.
    - **Top** (optioneel, geheel getal): Het aantal gerangschikte resultaten om op te nemen in de uitvoer. De standaardwaarde is 1.
    - **gebruikers-id** (optioneel, string): Een unieke ID voor het identificeren van de gebruiker. Deze ID wordt geregistreerd in de chat-Logboeken.
    - **strictFilters** (optioneel, string): Indien opgegeven, wordt aan de QnA Maker om terug te keren alleen de antwoorden die de opgegeven metagegevens zijn. Zie hieronder voor meer informatie.
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

### <a name="generateanswer-response"></a>GenerateAnswer antwoord

- **Antwoord 200** -een geslaagde aanroep retourneert het resultaat van de vraag. Het antwoord bevat de volgende velden:
    - **antwoorden** -een lijst met antwoorden voor de gebruikersquery, gesorteerd in aflopende volgorde van de volgorde van score.
        - **score**: Een rangorde tussen 0 en 100 liggen.
        - **vragen over**: De vragen die is opgegeven door de gebruiker.
        - **antwoord**: Het antwoord op de vraag.
        - **source**: De naam van de bron van waaruit het antwoord is uitgepakt of opgeslagen in het knowledge base.
        - **metadata**: De metagegevens die zijn gekoppeld aan het antwoord.
            - Naam: Naam voor de metagegevens. (string, maximumlengte: 100, vereist)
            - Waarde: De waarde van de metagegevens. (string, maximumlengte: 100, vereist)
        - **Id**: Een unieke ID die is toegewezen aan het antwoord.
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

## <a name="metadata-example"></a>Voorbeeld van de metagegevens

Houd rekening met de onderstaande veelgestelde vragen over gegevens. Metagegevens toevoegen aan uw knowledge base door te klikken op het pictogram van de metagegevens.

![metagegevens toevoegen](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Resultaten met strictFilters voor metagegevenstags filteren

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

### <a name="keep-context"></a>Context behouden
Het antwoord op de GenerateAnswer bevat de bijbehorende metagegevens van de overeenkomende vraag/antwoord is ingesteld, als volgt.

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

Deze informatie kan worden gebruikt om vast te leggen van de context van het vorige gesprek voor gebruik in latere conversaties. 

## <a name="next-steps"></a>Volgende stappen

De pagina publiceren bevat ook informatie voor het genereren van een antwoord met [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) en [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](./create-knowledge-base.md)
