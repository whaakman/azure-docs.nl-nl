---
title: 'Quickstart: Aanroep van de Text Analytics-Service met behulp van de Python-SDK'
titleSuffix: Azure Cognitive Services
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van de Tekstanalyse-API in Azure Cognitive Services.
services: cognitive-services
author: ctufts
manager: assafi
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: aahi
ms.openlocfilehash: b319abf22f9aa4cdd9a5fef91be0628672d47bd4
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297787"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-python-sdk"></a>Quickstart: Aanroep van de Text Analytics-Service met behulp van de Python-SDK 
<a name="HOLTop"></a>

Gebruik deze Quick Start om te beginnen met het analyseren van taal met de Text Analytics-SDK voor Python. De Text Analytics REST-API is compatibel met de meeste moderne programmeertalen, biedt de SDK een eenvoudige manier om te integreren van de service in uw toepassingen zonder te serialiseren en deserialiseren van JSON. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/language/text_analytics_samples.py).

## <a name="prerequisites"></a>Vereisten

* [Python 3.x](https://www.python.org/)

* De Text Analytics [SDK voor python](https://pypi.org/project/azure-cognitiveservices-language-textanalytics/) kunt u het pakket met installeren:

    `pip install --upgrade azure-cognitiveservices-language-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ook moet u de [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u zijn gegenereerd tijdens de registratie.

## <a name="create-a-new-python-application"></a>Een nieuwe Python-toepassing maken

Maak een nieuwe Python-toepassing in uw favoriete editor of IDE. Voeg de volgende importinstructies toe aan het bestand.

```python
from azure.cognitiveservices.language.textanalytics import TextAnalyticsClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="authenticate-your-credentials"></a>Verificatie van uw referenties

> [!Tip]
> Voor de veilige distributie van geheimen in productiesystemen wordt u aangeraden [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

Nadat u een variabele voor uw abonnementssleutel Text Analytics, instantiëren een `CognitiveServicesCredentials` object met deze.

```python
subscription_key = "enter-your-key-here"
credentials = CognitiveServicesCredentials(subscription_key)
```

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

Maak een nieuwe `TextAnalyticsClient` object met `credentials` en `text_analytics_url` als parameter. Gebruik de juiste Azure-regio voor uw Text Analytics-abonnement (bijvoorbeeld `westcentralus`).

```
text_analytics_url = "https://westcentralus.api.cognitive.microsoft.com/"
text_analytics = TextAnalyticsClient(endpoint=text_analytics_url, credentials=credentials)
```

## <a name="sentiment-analysis"></a>Stemmingsanalyse

De nettolading voor de API bestaat uit een lijst met `documents`, die zijn woordenlijsten die een `id` en een `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde. 

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

Roep de `sentiment()` functioneren en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en gevoelsscore. Een score dichter bij 0 geeft aan dat een negatief gevoel, terwijl een score dichter bij 1 geeft aan een positieve stemming dat.

```python
response = text_analytics.sentiment(documents=documents)
for document in response.documents:
     print("Document Id: ", document.id, ", Sentiment Score: ", "{:.2f}".format(document.score))
```

### <a name="output"></a>Uitvoer

```console
Document Id:  1 , Sentiment Score:  0.87
Document Id:  2 , Sentiment Score:  0.11
Document Id:  3 , Sentiment Score:  0.44
Document Id:  4 , Sentiment Score:  1.00
```

## <a name="language-detection"></a>Taaldetectie

Maak een lijst van woordenlijsten, elk met het document dat u wilt analyseren. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde. 

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

Met behulp van de client eerder hebt gemaakt, aanroepen `detect_language()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en de taal van de eerste geretourneerd.

```python
response = text_analytics.detect_language(documents=documents)
for document in response.documents:
    print("Document Id: ", document.id , ", Language: ", document.detected_languages[0].name)
```

### <a name="output"></a>Uitvoer

```console
Document Id:  1 , Language:  English
Document Id:  2 , Language:  Spanish
Document Id:  3 , Language:  Chinese_Simplified
```

## <a name="entity-recognition"></a>Herkenning van entiteit

Maak een lijst van woordenlijsten, met de documenten die u wilt analyseren. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde. 


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

Met behulp van de client eerder hebt gemaakt, aanroepen `entities()` functioneren en het resultaat. Vervolgens doorlopen en de resultaten en van elk document-ID en de entiteiten die zijn opgenomen in het afdrukken.

```python
response = text_analytics.entities(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Entities:")
    for entity in document.entities:
        print("\t\t", "NAME: ",entity.name, "\tType: ", entity.type, "\tSub-type: ", entity.sub_type)
        for match in entity.matches:
            print("\t\t\tOffset: ", match.offset, "\tLength: ", match.length, "\tScore: ",
                  "{:.2f}".format(match.entity_type_score))
```


### <a name="output"></a>Uitvoer

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

Maak een lijst van woordenlijsten, met de documenten die u wilt analyseren. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde. 


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

Met behulp van de client eerder hebt gemaakt, aanroepen `key_phrases()` functioneren en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en de sleutelzinnen erin.

```python
response = text_analytics.key_phrases(documents=documents)

for document in response.documents:
    print("Document Id: ", document.id)
    print("\tKey Phrases:")
    for phrase in document.key_phrases:
        print("\t\t",phrase)
```

### <a name="output"></a>Uitvoer

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

* [Wat is de Tekstanalyse-API?](../overview.md)
* [Voorbeeld van de gebruiker moet worden opgegeven](../text-analytics-user-scenarios.md)
* [Veelgestelde vragen](../text-analytics-resource-faq.md)
