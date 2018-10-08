---
title: 'Snelstart: Een Visual Search-query maken, Python - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar Bing Visual Search-API om inzichten op te vragen over de afbeelding.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 145749f52f64adf565eb33ab7fe92dd5494f9354
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223712"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Snelstart: Uw eerste Bing Visual Search-query in Python

Bing Visual Search-API retourneert informatie over een afbeelding die u opgeeft. U kunt de afbeelding opgeven door de URL van de afbeelding te gebruiken, een inzichttoken of door een afbeelding te uploaden. Zie [Wat is Bing Visual Search-API?](../overview.md) voor informatie over deze opties. Dit artikel demonstreert het uploaden van een afbeelding. Het uploaden van een afbeelding kan handig zijn in mobiele scenario's waarbij u een foto neemt van een bekend oriëntatiepunt en er informatie over terugkrijgt. De inzichten kunnen bijvoorbeeld trivia bevatten over het oriëntatiepunt. 

Als u een lokale afbeelding uploadt, toont het volgende de formuliergegevens die u in de POST moet opnemen. De formuliergegevens moeten de header Content-Disposition bevatten. De parameter `name` moet worden ingesteld op "image" en de parameter `filename` kan op een willekeurige tekenreeks worden ingesteld. De inhoud van het formulier is het binaire bestand van de afbeelding. De maximale afbeeldingsgrootte die u kunt uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die een Bing Visual Search-API-aanvraag verzendt en de JSON-zoekresultaten weergeeft. Hoewel deze toepassing in Python is geschreven, is de API een RESTful-webservice die compatibel is met elke programmeertaal die HTTP-aanvragen kan doen en JSON kan parseren. 

## <a name="prerequisites"></a>Vereisten

U hebt [Python 3](https://www.python.org/) nodig om deze code uit te voeren.

In deze snelstart kunt u een code van een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) of van een betaald abonnement gebruiken.

## <a name="running-the-walkthrough"></a>De voorbeeldtoepassing uitvoeren

Volg deze stappen voor het uitvoeren van deze toepassing:

1. Maak een nieuw Python-project in uw favoriete IDE of editor.
2. Maak een bestand met de naam visualsearch.py en voeg de code toe die wordt weergegeven in deze snelstart.
3. Vervang de waarde `SUBSCRIPTION_KEY` door uw abonnementscode.
3. Vervang de waarde `imagePath` door het pad van de te uploaden afbeelding.
4. Voer het programma uit.



Hieronder ziet u hoe u het bericht met behulp van meerdelige formuliergegevens in Python kunt verzenden.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Volgende stappen

[Inzichten krijgen over een afbeelding met behulp van een inzichttoken](../use-insights-token.md)  
[Zelfstudie: Afbeelding uploaden naar Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Zelfstudie: Bing Visual Search-app van één pagina](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual Search ](../overview.md)  
[Proberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een toegangscode voor een gratis proefversie aanvragen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Naslaginformatie over Bing Visual Search-API](https://aka.ms/bingvisualsearchreferencedoc)
