---
title: Python-Quickstart voor Bing visuele zoekopdrachten-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar de Bing visuele zoekopdrachten-API en weer toegang krijgen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 96bd94e37c75d10726245fbcea7044d4ae2ed07e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070372"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Uw eerste Bing visuele zoekopdrachten-query in Python

Bing visuele zoekopdrachten-API retourneert informatie over een afbeelding die u opgeeft. U kunt de installatiekopie opgeven met behulp van de URL van de afbeelding, een insights token, of door een installatiekopie te uploaden. Zie voor meer informatie over deze opties [wat Bing visuele zoekopdrachten-API is?](../overview.md) In dit artikel ziet u een installatiekopie uploaden. Uploaden van een afbeelding kan nuttig zijn in mobiele scenario's waarbij u een afbeelding van een bekende oriëntatiepunt en informatie erover weer toegang krijgen. Bijvoorbeeld, kunnen de insights bevat ook algemene vragen over de oriëntatiepunt. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De gegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'afbeelding' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die een Bing visuele zoekopdrachten-API-aanvraag verzendt en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in Python, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON. 

## <a name="prerequisites"></a>Vereisten

U moet [Python 3](https://www.python.org/) deze code uit te voeren.

In deze Quick Start kunt u een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-walkthrough"></a>De procedures uitvoeren

Om uit te voeren deze toepassing, de volgende stappen uit:

1. Maak een nieuwe Python-project in uw favoriete IDE of editor.
2. Maak een bestand met de naam visualsearch.py en voeg de code die wordt weergegeven in deze Quick Start.
3. Vervang de `SUBSCRIPTION_KEY` waarde met de abonnementssleutel van uw.
3. Vervang de `imagePath` waarde met het pad van de afbeelding te uploaden.
4. Voer het programma.



Hieronder ziet u hoe u het bericht met behulp van meerdelige formuliergegevens in Python te verzenden.

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
[Bing visuele zoekopdrachten-installatiekopie uploaden zelfstudie](../tutorial-visual-search-image-upload.md)
[Bing visuele zoekopdrachten-app met één pagina zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Bing visuele zoekopdrachten-overzicht](../overview.md)  
[Nu uitproberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing visuele zoekopdrachten-API-verwijzing](https://aka.ms/bingvisualsearchreferencedoc)
