---
title: 'Snelstartgids: Bing News Search SDK, Python'
titleSuffix: Azure Cognitive Services
description: De Bing Nieuws zoeken-SDK-consoletoepassing instellen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8e4343b053835c0fc2219373ad60f96c7b80636a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803338"
---
# <a name="quickstart-bing-news-search-sdk-with-python"></a>Snelstartgids: Bing News Search SDK met Python

De News Search SDK bevat de functionaliteit van de REST-API voor webquery's en het parseren van resultaten. 

De [broncode voor voorbeelden van de Bing News Search SDK voor Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Als u Python nog niet hebt, moet u dit eerst installeren. De SDK is compatibel met Python 2.7, 3.3, 3.4, 3.5 en 3.6.

De algemene aanbeveling voor het ontwikkelen met Python is om een [virtuele omgeving](https://docs.python.org/3/tutorial/venv.html) te gebruiken. Installeer en initialiseer de virtuele omgeving met de [venv-module](https://pypi.python.org/pypi/virtualenv). U moet virtualenv voor Python 2.7 installeren.
```
python -m venv mytestenv
```
Installeer Bing News Search SDK-afhankelijkheden:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Nieuws zoeken-client
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder *Zoeken*. Voeg imports toe:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Maak een instantie van `CognitiveServicesCredentials`. Maak een instantie van de client:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Zoek naar resultaten en geef het resultaat van de eerste webpagina weer:
```
news_result = client.news.search(query="Quantum Computing", market="en-us", count=10)
print("Search news for query \"Quantum Computing\" with market and count")

if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")

```
Zoek met filters voor het laatste nieuws over 'Artificial Intelligence' met de parameters `freshness` en `sortBy`. Controleer het aantal resultaten en geef de waarden voor `totalEstimatedMatches`, `name`, `url`, `description`, `published time` en `name of provider` weer voor het eerste nieuwsitem.
```
def news_search_with_filtering(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.search(
            query="Artificial Intelligence",
            market="en-us",
            freshness="Week",
            sort_by="Date"
        )
        print("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Zoek veilig naar nieuws in de categorie films en tv-entertainment. Controleer het aantal resultaten en geef de waarden voor `category`, `name`, `url`, `description`, `published time` en `name of provider` weer voor het eerste nieuwsitem.
```
def news_category(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.category(
            category="Entertainment_MovieAndTV",
            market="en-us",
            safe_search="strict"
        )
        print("\r\nSearch category news for movie and TV entertainment with safe search")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news category: {}".format(first_news_result.category))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))


```
Zoek naar trending nieuwsonderwerpen in Bing.  Controleer het aantal resultaten en geef de waarden voor `name`, `text of query`, `webSearchUrl`, `newsSearchUrl` en `image Url` weer voor het eerste nieuwsitem.
```
def news_trending(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_topics = client.news.trending(market="en-us")
        print("\r\nSearch news trending topics in Bing")

        if trending_topics.value:
            first_topic = trending_topics.value[0]
            print("News result count: {}".format(len(trending_topics.value)))
            print("First topic name: {}".format(first_topic.name))
            print("First topic query: {}".format(first_topic.query.text))
            print("First topic image url: {}".format(first_topic.image.url))
            print("First topic webSearchUrl: {}".format(first_topic.web_search_url))
            print("First topic newsSearchUrl: {}".format(first_topic.news_search_url))
        else:
            print("Didn't see any topics result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services Python SDK](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


