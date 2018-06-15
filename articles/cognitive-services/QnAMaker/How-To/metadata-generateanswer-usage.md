---
title: Met behulp van metagegevens in de kennisdatabase samen met de API GenerateAnswer | Microsoft Docs
description: Met behulp van de metagegevens met GenerateAnswer API
services: cognitive-services
author: pchoudhari
manager: rsrikan
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/18/2018
ms.author: pchoudh
ms.openlocfilehash: 94e3632884d7033971ff1c45b455afb9a09ee798
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "35345951"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Gebruik van metagegevens en de API GenerateAnswer

QnA Maker kunt u de metagegevens in de vorm van sleutel-waardeparen op uw vraag/antwoord-sets toevoegen. Deze informatie kan worden gebruikt op verschillende manieren zoals filteren van resultaten op gebruiker query's en aanvullende gegevens die kan worden gebruikt in vervolgzelfstudie conversaties opgeslagen versterking van bepaalde resultaten. Zie voor meer informatie [kennisdatabase](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>QnA entiteit

Eerst is het belangrijk te begrijpen hoe QnA Maker de vraag/antwoord-gegevens opslaat. De volgende afbeelding ziet u een entiteit QnA:

![QnA entiteit](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Elke entiteit QnA heeft een uniek en permanente-ID. De ID kan worden gebruikt om updates voor een bepaalde QnA-entiteit.

## <a name="generateanswer-api"></a>GenerateAnswer API

U de API GenerateAnswer in uw Bot of toepassing gebruiken om op te vragen uw kennisdatabase met een vraag van de gebruiker het meest geschikt is van de vraag/antwoord-sets ophalen.

### <a name="generateanswer-endpoint"></a>GenerateAnswer eindpunt

Nadat u uw kennisdatabase vanaf publiceren de [QnA Maker portal](https://www.qnamaker.ai), of met behulp van de [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), kunt u de details van uw eindpunt GenerateAnswer krijgen.

De eindpuntdetails van uw ophalen:
1. Meld u aan bij [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. In **mijn basissen knowledge**, klikt u op **Code weergeven** voor uw knowledge base.
![Mijn basissen kennis](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. Uw GenerateAnswer endpoint-details ophalen.

![eindpuntdetails van het](../media/qnamaker-how-to-metadata-usage/view-code.png)

U kunt ook ophalen met de eindpuntdetails van uw van de **instellingen** tabblad van de knowledge base.

### <a name="generateanswer-request"></a>GenerateAnswer aanvraag

U aanroepen GenerateAnswer met een HTTP POST-aanvraag. Zie voor voorbeeldcode die laat zien hoe u aan te roepen GenerateAnswer de [snelstartgidsen](../quickstarts/csharp.md).

- **Aanvraag-URL**: https://{QnA Maker eindpunt} {kB ID} /knowledgebases/ / generateAnswer

- **Aanvraagparameters**: 
    - **Knowledge base-ID** (tekenreeks): de GUID voor uw knowledge base.
    - **QnAMaker eindpunt** (tekenreeks): de hostnaam van het eindpunt dat is geïmplementeerd in uw Azure-abonnement.
- **Aanvraagheaders**
    - **Content-Type** (tekenreeks): het mediatype van de instantie die wordt verzonden naar de API.
    - **Autorisatie** (tekenreeks): de eindpuntsleutel van uw.
- **Aanvraagtekst**
    - **vraag** (tekenreeks): een vraag gebruiker moeten worden opgevraagd op basis van uw knowledge base.
    - **Top** (optioneel, geheel getal): het aantal gerangschikte resultaten moeten worden opgenomen in de uitvoer. De standaardwaarde is 1.
    - **userId** (optioneel, string): een unieke ID voor het identificeren van de gebruiker. Deze ID wordt geregistreerd in de logboeken chat.
    - **strictFilters** (optioneel, string): als u opgeeft, wordt uitgelegd QnA Maker om terug te keren alleen antwoorden die de opgegeven metagegevens zijn. Zie hieronder voor meer informatie.
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
    - **antwoorden** -een lijst met antwoorden voor de gebruikersquery gesorteerd in aflopende volgorde van de positie van de score.
        - **score**: een ranking-score tussen 0 en 100 liggen.
        - **vragen**: de vragen die is opgegeven door de gebruiker.
        - **bron**: de naam van de bron van waaruit het antwoord is uitgepakt of opgeslagen in het knowledge base.
        - **metagegevens**: de metagegevens gekoppeld aan het antwoord.
            - naam: de naam van de metagegevens. (tekenreeks, maximumlengte: 100, vereist)
            - waarde: de metagegevens. (tekenreeks, maximumlengte: 100, vereist)
        - **Id**: een unieke ID die is toegewezen aan het antwoord.
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

## <a name="metadata-example"></a>Voorbeeld van metagegevens

Houd rekening met de onderstaande veelgestelde vragen over gegevens voor restaurant in Hyderabad. Metagegevens toevoegen aan uw knowledge base door te klikken op het pictogram tandwielpictogram.

![metagegevens toevoegen](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>Resultaten met strictFilters filteren

Houd rekening met de vraag van de gebruiker 'Wanneer ondersteunt deze hotel sluiten?' waar de bedoeling is impliciet voor het restaurant 'Paradise'.

Aangezien resultaten zijn alleen vereist voor het restaurant 'Paradise', kunt u een filter instellen in de aanroep van GenerateAnswer op de metagegevens 'Restaurant Name', als volgt.

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
Het antwoord op de GenerateAnswer bevat de bijbehorende metagegevens van de overeenkomende vraag/antwoord-set als volgt.

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

Deze informatie kan worden gebruikt voor het vastleggen van de context van de vorige conversatie voor gebruik in latere conversaties. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](./create-knowledge-base.md)
