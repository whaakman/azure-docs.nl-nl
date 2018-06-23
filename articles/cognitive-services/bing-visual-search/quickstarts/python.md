---
title: Python-Quick Start voor Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar Bing Visual zoeken-API en terughalen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a520466825eb429e45e0500b52bd7af502c0a38c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345638"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Uw eerste Bing Visual zoekopdracht in Python

Bing Visual zoeken-API retourneert informatie over de installatiekopie die u opgeeft. Met behulp van de URL van de afbeelding, een insights token, of door een afbeelding te uploaden, kunt u de installatiekopie opgeven. Zie voor meer informatie over deze opties [wat Bing Visual zoeken-API is?](../overview.md) In dit artikel ziet u een afbeelding te uploaden. Een afbeelding te uploaden kan nuttig zijn in mobiele scenario's waarin u een afbeelding van een bekende kenmerkende en informatie over het terughalen. De insights kunnen bijvoorbeeld de algemene vragen over de kenmerkende bevatten. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'installatiekopie' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de afbeelding. De grootte van de maximale installatiekopie die u kan uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die verzendt een aanvraag Bing Visual zoeken-API en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in Python, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 

## <a name="prerequisites"></a>Vereisten

U moet [Python 3](https://www.python.org/) deze code uit te voeren.

U kunt gebruiken voor deze snelstartgids een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-walkthrough"></a>De procedure wordt uitgevoerd

Voor het uitvoeren van deze toepassing, de volgende stappen uit:

1. Maak een nieuwe Python-project in uw favoriete IDE of -editor.
2. Maak een bestand met de naam visualsearch.py en voeg de code in deze snelstartgids wordt weergegeven.
3. Vervang de `SUBSCRIPTION_KEY` waarde met de abonnementssleutel van uw.
3. Vervang de `imagePath` waarde met het pad van de afbeelding te uploaden.
4. Voer het programma.



Hieronder ziet u hoe u het bericht met meerdelige formuliergegevens in Python verzendt.

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

[Inzichten over een afbeelding met behulp van een token inzichten verkrijgen](../use-insights-token.md)  
[Bing Visual Search één pagina app-zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual zoeken](../overview.md)  
[Probeer deze](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API-verwijzingen](https://aka.ms/bingvisualsearchreferencedoc)
