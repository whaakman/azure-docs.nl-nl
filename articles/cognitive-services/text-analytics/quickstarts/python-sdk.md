---
title: 'Quickstart: De Text Analytics-Service aanroepen met behulp van de python-SDK'
titleSuffix: Azure Cognitive Services
description: Informatie en code voorbeelden ophalen zodat u snel aan de slag kunt met de Text Analytics-API in azure Cognitive Services.
services: cognitive-services
author: ctufts
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: aahi
ms.openlocfilehash: c24979d9aef74b6cc840427a010b9ce70f2c0b8a
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68356951"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-python-sdk"></a>Quickstart: De Text Analytics-Service aanroepen met behulp van de python-SDK 
<a name="HOLTop"></a>

Gebruik deze Quick Start om de taal te analyseren met de Text Analytics SDK voor python. Hoewel de Text Analytics REST API compatibel is met de meeste programmeer talen, biedt de SDK een gemakkelijke manier om de service te integreren in uw toepassingen zonder JSON te serialiseren en deserialiseren. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py).

## <a name="prerequisites"></a>Vereisten

* [Python 3.x](https://www.python.org/)

* Met de Text Analytics [SDK voor python](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) kunt u het pakket installeren met:

    `pip install --upgrade azure-cognitiveservices-language-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook beschikken over het [eind punt en de toegangs sleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u zijn gegenereerd tijdens het aanmelden.

## <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

Maak een nieuwe python-toepassing in uw favoriete editor of IDE. Voeg vervolgens de volgende import instructies toe aan het bestand.

```python
from azure.cognitiveservices.language.textanalytics import TextAnalyticsClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="authenticate-your-credentials"></a>Uw referenties verifiëren

> [!Tip]
> Voor een veilige implementatie van geheimen in productie systemen raden wij u aan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)te gebruiken.
>

Nadat u een variabele hebt gemaakt voor uw Text Analytics-abonnements sleutel `CognitiveServicesCredentials` , moet u een object instantiëren.

```python
subscription_key = "enter-your-key-here"
credentials = CognitiveServicesCredentials(subscription_key)
```

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

Maak een nieuw `TextAnalyticsClient` object met `credentials` en `text_analytics_url` als para meter. Gebruik de juiste Azure-regio voor uw Text Analytics-abonnement ( `westcentralus`bijvoorbeeld).

```
text_analytics_url = "https://westcentralus.api.cognitive.microsoft.com/"
text_analytics = TextAnalyticsClient(endpoint=text_analytics_url, credentials=credentials)
```

## <a name="sentiment-analysis"></a>Sentimentanalyse

De payload van de API bestaat uit een lijst met `documents`-woorden lijsten die een `id` en een `text` -kenmerk bevatten. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn. 

```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "I had the best day of my life."
    },
    {
        "id": "2",
        "language": "en",
        "text": "This was a waste of my time. The speaker put me to sleep."
    },
    {
        "id": "3",
        "language": "es",
        "text": "No tengo dinero ni nada que dar..."
    },
    {
        "id": "4",
        "language": "it",
        "text": "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
    }
]
```

Roep de `sentiment()` functie aan en ontvang het resultaat. Herhaal vervolgens de resultaten en druk de document-ID en sentiment Score af. Een score dichter bij 0 geeft aan dat er een negatieve sentiment is, terwijl een score dichter bij 1 wijst op een positieve sentiment.

```python
response = text_analytics.sentiment(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Sentiment Score: ",
          "{:.2f}".format(document.score))
```

### <a name="output"></a>Output

```console
Document Id:  1 , Sentiment Score:  0.87
Document Id:  2 , Sentiment Score:  0.11
Document Id:  3 , Sentiment Score:  0.44
Document Id:  4 , Sentiment Score:  1.00
```

## <a name="language-detection"></a>Taaldetectie

Maak een lijst met woorden lijsten, elk met het document dat u wilt analyseren. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn. 

```python
documents = [
    {
        'id': '1',
        'text': 'This is a document written in English.'
    },
    {
        'id': '2',
        'text': 'Este es un document escrito en Español.'
    },
    {
        'id': '3',
        'text': '这是一个用中文写的文件'
    }
]
```

Gebruik de client die u eerder hebt `detect_language()` gemaakt, om het resultaat te bellen en op te halen. Herhaal vervolgens de resultaten en druk de ID van elk document af en de eerste geretourneerde taal.

```python
response = text_analytics.detect_language(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id, ", Language: ",
          document.detected_languages[0].name)
```

### <a name="output"></a>Output

```console
Document Id:  1 , Language:  English
Document Id:  2 , Language:  Spanish
Document Id:  3 , Language:  Chinese_Simplified
```

## <a name="entity-recognition"></a>Entiteit herkenning

Maak een lijst met woorden lijsten die de documenten bevatten die u wilt analyseren. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn. 


```python
documents = [
    {
        "id": "1",
        "language": "en",
        "text": "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."
    },
    {
        "id": "2",
        "language": "es",
        "text": "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."
    }
]
```

Gebruik de-client die u eerder `entities()` hebt gemaakt, de functie aanroepen en het resultaat ophalen. Herhaal vervolgens de resultaten en druk de document-ID en de entiteiten daarin af.

```python
response = text_analytics.entities(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Entities:")
    for entity in document.entities:
        print("\t\t", "NAME: ", entity.name, "\tType: ",
              entity.type, "\tSub-type: ", entity.sub_type)
        for match in entity.matches:
            print("\t\t\tOffset: ", match.offset, "\tLength: ", match.length, "\tScore: ",
                  "{:.2f}".format(match.entity_type_score))
```


### <a name="output"></a>Output

```console
Document Id:  1
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  0  Length:  9  Score:  1.00
         NAME:  Bill Gates  Type:  Person   Sub-type:  None
            Offset:  25     Length:  10     Score:  1.00
         NAME:  Paul Allen  Type:  Person   Sub-type:  None
            Offset:  40     Length:  10     Score:  1.00
         NAME:  April 4     Type:  Other    Sub-type:  None
            Offset:  54     Length:  7  Score:  0.80
         NAME:  April 4, 1975   Type:  DateTime     Sub-type:  Date
            Offset:  54     Length:  13     Score:  0.80
         NAME:  BASIC   Type:  Other    Sub-type:  None
            Offset:  89     Length:  5  Score:  0.80
         NAME:  Altair 8800     Type:  Other    Sub-type:  None
            Offset:  116    Length:  11     Score:  0.80
Document Id:  2
    Key Entities:
         NAME:  Microsoft   Type:  Organization     Sub-type:  None
            Offset:  21     Length:  9  Score:  1.00
         NAME:  Redmond (Washington)    Type:  Location     Sub-type:  None
            Offset:  60     Length:  7  Score:  0.99
         NAME:  21 kilómetros   Type:  Quantity     Sub-type:  Dimension
            Offset:  71     Length:  13     Score:  0.80
         NAME:  Seattle     Type:  Location     Sub-type:  None
            Offset:  88     Length:  7  Score:  1.00
```

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

Maak een lijst met woorden lijsten die de documenten bevatten die u wilt analyseren. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn. 


```python
documents = [
    {
        "id": "1",
        "language": "ja",
        "text": "猫は幸せ"
    },
    {
        "id": "2",
        "language": "de",
        "text": "Fahrt nach Stuttgart und dann zum Hotel zu Fu."
    },
    {
        "id": "3",
        "language": "en",
        "text": "My cat might need to see a veterinarian."
    },
    {
        "id": "4",
        "language": "es",
        "text": "A mi me encanta el fútbol!"
    }
]
```

Gebruik de-client die u eerder `key_phrases()` hebt gemaakt, de functie aanroepen en het resultaat ophalen. Vervolgens doorloopt u de resultaten en drukt u de ID van elk document af en de sleutel zinnen erin.

```python
response = text_analytics.key_phrases(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Phrases:")
    for phrase in document.key_phrases:
        print("\t\t", phrase)
```

### <a name="output"></a>Output

```console
Document Id:  1
    Phrases:
         幸せ
Document Id:  2
    Phrases:
         Stuttgart
         Hotel
         Fahrt
         Fu
Document Id:  3
    Phrases:
         cat
         veterinarian
Document Id:  4
    Phrases:
         fútbol
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook

* [Wat is de Text Analytics-API?](../overview.md)
* [Voor beelden van gebruikers scenario's](../text-analytics-user-scenarios.md)
* [Veelgestelde vragen](../text-analytics-resource-faq.md)
