---
title: 'Snelstartgids: Met behulp van Python voor het aanroepen van de Tekstanalyse-API | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van de Tekstanalyse-API in Microsoft Cognitive Services op Azure.
services: cognitive-services
author: ashmaka
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 8e570aac2c2d89a8147d179c4b0f9155497c5188
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44298689"
---
# <a name="quickstart-using-python-to-call-the-text-analytics-cognitive-service"></a>Snelstartgids: Met behulp van Python voor het aanroepen van de Text Analytics-Cognitive Service
<a name="HOLTop"></a>

In dit scenario ziet u hoe u aan [taal detecteren](#Detect), [stemming analyseren](#SentimentAnalysis), en [Extraheer sleuteluitdrukkingen](#KeyPhraseExtraction) met behulp van de [Text Analytics-API's](//go.microsoft.com/fwlink/?LinkID=759711)met Python.

In dit voorbeeld kunt u uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=TextAnalytics.ipynb)

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereiste onderdelen

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Tekstanalyse-API**. U kunt de **gratis laag voor 5000 transacties per maand** voor dit scenario.

Ook moet u de [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u is gegenereerd tijdens de registratie. 

Als u wilt doorgaan met dit scenario, Vervang `subscription_key` met een geldig abonnement-sleutel die u eerder hebt verkregen.


```python
subscription_key = None
assert subscription_key
```

Vervolgens controleren of de regio in `text_analytics_base_url` komt overeen met de versie die u hebt gebruikt bij het instellen van de service. Als u een gratis proefversie sleutel gebruikt, hoeft u niet wijzigen.


```python
text_analytics_base_url = "https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/"
```

<a name="Detect"></a>

## <a name="detect-languages"></a>Talen detecteren

De API voor taal detecteert de taal van een document, met behulp van de [taal detecteren methode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7). Het service-eindpunt van de taaldetectie API voor uw regio is beschikbaar via de volgende URL:


```python
language_api_url = text_analytics_base_url + "languages"
print(language_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/languages


De nettolading voor de API bestaat uit een lijst met `documents`, elk van die op zijn beurt bevat een `id` en een `text` kenmerk. De `text` kenmerk slaat de tekst die moet worden geanalyseerd. 

Vervang de `documents` -woordenlijst met andere tekst voor de taaldetectie van. 


```python
documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]}
```

De volgende paar regels code aanroepen met de language detection-API via de `requests` bibliotheek in Python om te bepalen welke taal in de documenten.


```python
import requests
from pprint import pprint
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(language_api_url, headers=headers, json=documents)
languages = response.json()
pprint(languages)
```

    {'documents': [{'detectedLanguages': [{'iso6391Name': 'en',
                                           'name': 'English',
                                           'score': 1.0}],
                    'id': '1'},
                   {'detectedLanguages': [{'iso6391Name': 'es',
                                           'name': 'Spanish',
                                           'score': 1.0}],
                    'id': '2'},
                   {'detectedLanguages': [{'iso6391Name': 'zh_chs',
                                           'name': 'Chinese_Simplified',
                                           'score': 1.0}],
                    'id': '3'}],
     'errors': []}


De volgende regels code weergegeven de JSON-gegevens als een HTML-tabel.


```python
from IPython.display import HTML
table = []
for document in languages["documents"]:
    text  = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]
    langs = ", ".join(["{0}({1})".format(lang["name"], lang["score"]) for lang in document["detectedLanguages"]])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, langs))
HTML("<table><tr><th>Text</th><th>Detected languages(scores)</th></tr>{0}</table>".format("\n".join(table)))
```

<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Stemming analyseren

De analyse-Gevoels-API-detexts het gevoel van een set tekstrecords, met behulp van de [Sentiment methode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). Het volgende voorbeeld beoordeelt twee documenten, één in het Engels en een andere in het Spaans.

Het service-eindpunt voor sentimentanalyse is beschikbaar voor uw regio via de volgende URL:


```python
sentiment_api_url = text_analytics_base_url + "sentiment"
print(sentiment_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment


Zoals in het voorbeeld taal detectie van de service wordt aangeboden door een woordenlijst met een `documents` sleutel die uit een lijst met documenten bestaat. Elk document is een tuple die bestaat uit de `id`, wordt de `text` kunnen worden geanalyseerd en de `language` van de tekst. U kunt de taaldetectie API uit de vorige sectie voor het vullen van dit veld gebruiken. 


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
```

Het sentiment-API kan nu worden gebruikt voor het analyseren van de documenten voor de bijbehorende sentimenten.


```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(sentiment_api_url, headers=headers, json=documents)
sentiments = response.json()
pprint(sentiments)
```

    {'documents': [{'id': '1', 'score': 0.7673527002334595},
                   {'id': '2', 'score': 0.18574094772338867},
                   {'id': '3', 'score': 0.5}],
     'errors': []}


De gevoelsscore voor een document is tussen $ $0 en $ $1, met een hogere score die wijzen op een positiever gevoel.

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Sleuteltermen ophalen

De API-sleutel woordgroep extractie sleuteltermen geëxtraheerd uit een document, met behulp van de [sleuteltermen methode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). In dit gedeelte van het scenario extraheert sleuteltermen voor Engelse en Spaanse documenten.

Het service-eindpunt voor de sleutel vindt er sleuteltermextractie plaats-service is toegankelijk via de volgende URL:


```python
key_phrase_api_url = text_analytics_base_url + "keyPhrases"
print(key_phrase_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases


De verzameling van documenten is hetzelfde als voor sentimentanalyse zijn gebruikt.


```python
documents = {'documents' : [
  {'id': '1', 'language': 'en', 'text': 'I had a wonderful experience! The rooms were wonderful and the staff was helpful.'},
  {'id': '2', 'language': 'en', 'text': 'I had a terrible time at the hotel. The staff was rude and the food was awful.'},  
  {'id': '3', 'language': 'es', 'text': 'Los caminos que llevan hasta Monte Rainier son espectaculares y hermosos.'},  
  {'id': '4', 'language': 'es', 'text': 'La carretera estaba atascada. Había mucho tráfico el día de ayer.'}
]}
headers   = {'Ocp-Apim-Subscription-Key': subscription_key}
response  = requests.post(key_phrase_api_url, headers=headers, json=documents)
key_phrases = response.json()
pprint(key_phrases)
```


    {'documents': [
        {'keyPhrases': ['wonderful experience', 'staff', 'rooms'], 'id': '1'},
        {'keyPhrases': ['food', 'terrible time', 'hotel', 'staff'], 'id': '2'},
        {'keyPhrases': ['Monte Rainier', 'caminos'], 'id': '3'},
        {'keyPhrases': ['carretera', 'tráfico', 'día'], 'id': '4'}],
     'errors': []
    }


Het JSON-object kan opnieuw worden weergegeven als een HTML-tabel met behulp van de volgende regels code:


```python
from IPython.display import HTML
table = []
for document in key_phrases["documents"]:
    text    = next(filter(lambda d: d["id"] == document["id"], documents["documents"]))["text"]    
    phrases = ",".join(document["keyPhrases"])
    table.append("<tr><td>{0}</td><td>{1}</td>".format(text, phrases))
HTML("<table><tr><th>Text</th><th>Key phrases</th></tr>{0}</table>".format("\n".join(table)))
```

## <a name="identify-linked-entities"></a>Gekoppelde entiteiten identificeren

De Entity Linking API identificeert bekende entiteiten in een document, met behulp van de [entiteiten koppelen methode](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634). Het volgende voorbeeld identificeert entiteiten voor Engels-documenten.

Het service-eindpunt voor de gekoppelde service van entiteit wordt benaderd via de volgende URL:


```python
entity_linking_api_url = text_analytics_base_url + "entities"
print(entity_linking_api_url)
```

    https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0/entities


De verzameling van documenten lager is dan:


```python
documents = {'documents' : [
  {'id': '1', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.'},
  {'id': '2', 'text': 'The Seattle Seahawks won the Super Bowl in 2014.'}
]}
```

Nu, kunnen de documenten worden verzonden naar de Tekstanalyse-API voor het ontvangen van het antwoord.

```python
headers   = {"Ocp-Apim-Subscription-Key": subscription_key}
response  = requests.post(entity_linking_api_url, headers=headers, json=documents)
entities = response.json()
```
    {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "name": "Xbox One",
                        "matches": [
                            {
                                "text": "XBox One",
                                "offset": 23,
                                "length": 8
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Xbox One",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                        "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                    },
                    {
                        "name": "Ultra-high-definition television",
                        "matches": [
                            {
                                "text": "4K",
                                "offset": 63,
                                "length": 2
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "Ultra-high-definition television",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                        "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                    }
                ]
            },
            {
                "id": "2",
                "entities": [
                    {
                        "name": "2013 Seattle Seahawks season",
                        "matches": [
                            {
                                "text": "Seattle Seahawks",
                                "offset": 4,
                                "length": 16
                            }
                        ],
                        "wikipediaLanguage": "en",
                        "wikipediaId": "2013 Seattle Seahawks season",
                        "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                        "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                    }
                ]
            }
        ],
        "errors": []
    }

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook 

 [Text Analytics-overzicht](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)
