---
title: 'Quickstart: De Bing Web Search SDK voor Python gebruiken'
titleSuffix: Azure Cognitive Services
description: Met de Bing Web Search SDK kunt u Bing Web Search eenvoudig integreren in uw Python-toepassing. In deze snelstartgids leert u hoe u een aanvraag verzendt, een JSON-antwoord ontvangt en de resultaten filtert en parseert.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: d6758fc5434406e42acf65ff3b712227b5cec0f8
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991625"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-python"></a>Quickstart: De Bing Web Search SDK voor Python gebruiken

Met de Bing Web Search SDK kunt u Bing Web Search eenvoudig integreren in uw Python-toepassing. In deze snelstartgids leert u hoe u een aanvraag verzendt, een JSON-antwoord ontvangt en de resultaten filtert en parseert.

Wilt u de code nu zien? De [voorbeelden voor de Bing Web Search SDK voor Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) zijn beschikbaar op GitHub.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="prerequisites"></a>Vereisten
De Bing Web Search SDK is compatibel met Python 2.7, 3.3, 3.4, 3.5 en 3.6. We raden u aan om een virtuele omgeving te gebruiken voor deze snelstartgids.

* Python 2.7, 3.3, 3.4, 3.5 of 3.6
* [virtualenv](https://docs.python.org/3/tutorial/venv.html) voor Python 2.7
* [venv](https://pypi.python.org/pypi/virtualenv) voor Python 3.x

## <a name="create-and-configure-your-virtual-environment"></a>Uw virtuele omgeving maken en configureren

De instructies voor het opzetten en configureren van een virtuele omgeving zijn verschillend voor Python 2.x en Python 3.x. Voer de onderstaande stappen uit om uw virtuele omgeving te maken en te initialiseren.

### <a name="python-2x"></a>Python 2.x

Maak een virtuele omgeving met `virtualenv` voor Python 2.7:

```console
virtualenv mytestenv
```

Uw omgeving activeren:

```console
cd mytestenv
source bin/activate
```

Installeer Bing Web Search SDK-afhankelijkheden:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Maak een virtuele omgeving met `venv` voor Python 3.x:

```console
python -m venv mytestenv
```

Installeer Bing Web Search SDK-afhankelijkheden:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>Een client maken en uw eerste resultaten weergeven

Nu u de virtuele omgeving hebt ingesteld en de afhankelijkheden hebt geïnstalleerd, kunnen we een client maken. De client verwerkt aanvragen voor en antwoorden afkomstig van de Bing Webzoekopdrachten-API.

Als het antwoord webpagina's, afbeeldingen, nieuws of video's bevat, wordt het eerste resultaat voor elk item weergegeven.

1. Maak een nieuw Python-project met uw favoriete IDE of editor.

1. Kopieer de volgende voorbeeldcode naar uw project:  

    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchAPI
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client and replace with your endpoint.
    client = WebSearchAPI(CognitiveServicesCredentials(subscription_key), base_url = "YOUR_ENDPOINT")

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```

1. Vervang `SUBSCRIPTION_KEY` door een geldige abonnementssleutel.

1. Vervang `YOUR_ENDPOINT` met uw eindpunt-url in de portal.

1. Voer het programma uit. Bijvoorbeeld: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>Functies definiëren en resultaten filteren

Nu u de eerste aanroep van de Bing webzoekopdrachten-API hebt aangebracht, gaan we bekijken enkele functies. De volgende gedeelten SDK-functionaliteit voor query's verfijnen en filteren van resultaten. Elke functie kan worden toegevoegd aan het Python-programma dat u in de vorige sectie hebt gemaakt.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Het aantal resultaten beperken dat door Bing wordt geretourneerd

In dit voorbeeld worden de parameters `count` en `offset` gebruikt voor het beperken van het aantal resultaten dat wordt geretourneerd met de [`search`-methode](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) van de SDK. De `name` en `url` voor het eerste resultaat worden weergegeven.

1. Voeg deze code toe aan uw Python-project:

   ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Voer het programma uit.

### <a name="filter-for-news-and-freshness"></a>Filteren op nieuws en actuele items

In dit voorbeeld worden de parameters `response_filter` en `freshness` gebruikt om de zoekresultaten te filteren met de [`search`-methode](/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations) van de SDK. De geretourneerde zoekresultaten zijn beperkt tot nieuwsartikelen en pagina's die Bing heeft gedetecteerd gedurende de afgelopen 24 uur. De `name` en `url` voor het eerste resultaat worden weergegeven.

1. Voeg deze code toe aan uw Python-project:

    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```

1. Voer het programma uit.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Gebruik de parameters voor veilig zoeken, aantal antwoorden en het filter voor het promoten van zoekresultaten

In dit voorbeeld worden de parameters `answer_count`, `promote` en `safe_search` gebruikt om de zoekresultaten te filteren met de [`search`-methode](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) van de SDK. De `name` en `url` voor het eerste resultaat worden weergegeven.

1. Voeg deze code toe aan uw Python-project:

    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
            if web_data.web_pages.value:

                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't see any Web data..")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Voer het programma uit.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met dit project, moet u uw abonnementssleutel verwijderen uit de code van het programma en uw virtuele omgeving deactiveren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voorbeelden voor Cognitive Services Python SDK](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie over Azure Python SDK](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
