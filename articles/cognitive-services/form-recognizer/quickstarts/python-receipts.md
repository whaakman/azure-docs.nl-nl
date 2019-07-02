---
title: 'Quickstart: Ontvangst-gegevens met behulp van Python - formulier herkenning ophalen'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids gebruikt u het formulier herkenning REST-API met Python gegevens te extraheren uit afbeeldingen van genereert.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: da699c9e5ca933c182c83a33dd0c09b49a0726c3
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503439"
---
# <a name="quickstart-extract-receipt-data-using-the-form-recognizer-rest-api-with-python"></a>Quickstart: Ontvangst-gegevens met behulp van het formulier herkenning REST-API met Python ophalen

In deze snelstartgids gebruikt u de REST-API van Azure formulier herkenning met Python voor het uitpakken en relevante informatie in verkoopontvangsten identificeren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt deze snelstartgids hebt voltooid, moet u het volgende hebben:
- Toegang tot de Preview-versie formulier herkenning beperkte toegang. Voor toegang tot de Preview-versie, invullen en verzenden de [formulier herkenning toegangsaanvraag](https://aka.ms/FormRecognizerRequestAccess) formulier.
- [Python](https://www.python.org/downloads/) geïnstalleerd (als u het voorbeeld lokaal uitvoeren wilt).
- Een URL voor een afbeelding van een ontvangst. U kunt een [voorbeeldafbeelding](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/contoso-receipt.png?raw=true) voor deze Quick Start.

## <a name="create-a-form-recognizer-resource"></a>Een formulier herkenning-resource maken

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-receipt"></a>Een ontvangst analyseren

Voor het starten van een ontvangst analyseren, u kunt aanroepen de **analyseren ontvangst** API met behulp van de onderstaande Python-script. Voordat u het script uitvoert, moet u deze wijzigingen:

1. Vervang `<Endpoint>` met het eindpunt dat u hebt verkregen via uw abonnementssleutel formulier herkenning. U vindt deze voor uw resource formulier herkenning **overzicht** tabblad.
1. Vervang `<your receipt URL>` met het URL-adres van een installatiekopie van ontvangst.
1. Vervang `<subscription key>` met de abonnementssleutel die u hebt gekopieerd uit de vorige stap.

    ```python
    import http.client, urllib.request, urllib.parse, urllib.error, base64

    source = r"<your receipt URL>"
    body = {"url":source}
    body = json.dumps(body)

    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': '<subscription key>',
    }

    try:
        conn = http.client.HTTPSConnection('<Endpoint>')
        conn.request("POST", "/formrecognizer/v1.0-preview/prebuilt/receipt/asyncBatchAnalyze", body, headers)
        response = conn.getresponse()
        data = response.read()
        operationURL = "" + response.getheader("Operation-Location")
        print ("Operation-Location header: " + operationURL)
        conn.close()
    except Exception as e:
        print(e)
        exit()
    ```

1. Sla de code in een bestand met de extensie .py. Bijvoorbeeld, *formulier-herkenning-receipts.py*.
1. Open een opdrachtpromptvenster.
1. Typ bij de prompt de opdracht `python` om het voorbeeld uit te voeren. Bijvoorbeeld `python form-recognizer-receipts.py`.

U ontvangt een `202 (Success)` -reactie met een **bewerking locatie** koptekst, die het script naar de console wordt afgedrukt. Deze header bevat een bewerkings-ID die u gebruiken kunt om te vragen van de status van de bewerking en resultaten van de analyse. In het volgende Voorbeeldwaarde, de tekenreeks na `operations/` is de bewerking-ID.

```console
https://cognitiveservice/formrecognizer/v1.0-preview/prebuilt/receipt/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-receipt-results"></a>De resultaten van de ontvangst ophalen

Nadat u hebt met de naam de **analyseren ontvangst** -API aanroepen de **ontvangst resultaat ophalen** API voor het ophalen van de status van de uitvoering en de opgehaalde gegevens. Voeg de volgende code naar de onderkant van het Python-script. Hiermee haalt de waarde van de bewerking-ID en wordt doorgegeven aan een nieuwe API-aanroep. De bewerking is asynchroon, zodat dit script de API met regelmatige tussenpozen aanroept tot de resultaten beschikbaar zijn. U wordt aangeraden een interval van één seconde of meer.

```python
operationId = operationURL.split("operations/")[1]

conn = http.client.HTTPSConnection('<Endpoint>')
while True:
    try:
        conn.request("GET", f"/formrecognizer/v1.0-preview/prebuilt/receipt/operations/{operationId}", "", headers)
        responseString = conn.getresponse().read().decode('utf-8')
        responseDict = json.loads(responseString)
        conn.close()
        print(responseString)
        if 'status' in responseDict and responseDict['status'] not in ['NotStarted','Running']:
            break
        time.sleep(1)
    except Exception as e:
        print(e)
        exit()
```

1. Sla het script.
1. Het gebruik van de `python` opdracht uit te voeren van het voorbeeld. Bijvoorbeeld `python form-recognize-analyze.py`.

### <a name="examine-the-response"></a>Het antwoord bekijken

Het script wordt antwoorden naar de console afgedrukt totdat de analyse-bewerking is voltooid. Deze wordt vervolgens de gegevens geëxtraheerde tekst in JSON-indeling afgedrukt. De `"recognitionResults"` bevat elke regel van de tekst die is geëxtraheerd uit de ontvangst van het veld en de `"understandingResults"` veld sleutel/waarde-gegevens voor de meest relevante onderdelen van de ontvangst bevat.

Zie die de volgende afbeelding van de ontvangst en de bijbehorende JSON-uitvoer. De uitvoer is ingekort voor de leesbaarheid.

![Een ontvangst van Contoso store](../media/contoso-receipt.png)

```json
{
  "status": "Succeeded",
  "recognitionResults": [{
    "page": 1,
    "clockwiseOrientation": 0.36,
    "width": 1688,
    "height": 3000,
    "unit": "pixel",
    "lines": [{
      "boundingBox": [616, 291, 1050, 278, 1053, 384, 620, 397],
      "text": "Contoso",
      "words": [{
        "boundingBox": [619, 292, 1051, 284, 1052, 382, 620, 398],
        "text": "Contoso"
      }]
    }, {
      "boundingBox": [322, 588, 501, 600, 497, 655, 318, 643],
      "text": "Contoso",
      "words": [{
        "boundingBox": [330, 590, 501, 602, 499, 654, 326, 644],
        "text": "Contoso"
      }]
    },
    ...
    ]
  }],
  "understandingResults": [{
    "pages": [1],
    "fields": {
      "Subtotal": {
        "valueType": "numberValue",
        "value": 1098.99,
        "text": "1098.99",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/14/words/1"
        }]
      },
      "Total": {
        "valueType": "numberValue",
        "value": 1203.39,
        "text": "1203.39",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/18/words/1"
        }]
      },
      "Tax": {
        "valueType": "numberValue",
        "value": 104.4,
        "text": "$104.40",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/16/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/16/words/1"
        }]
      },
      "MerchantAddress": {
        "valueType": "stringValue",
        "value": "123 Main Street Redmond, WA 98052",
        "text": "123 Main Street Redmond, WA 98052",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/2/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/2/words/2"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/0"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/1"
        }, {
          "$ref": "#/recognitionResults/0/lines/3/words/2"
        }]
      },
      "MerchantName": {
        "valueType": "stringValue",
        "value": "Contoso",
        "text": "Contoso",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/1/words/0"
        }]
      },
      "MerchantPhoneNumber": {
        "valueType": "stringValue",
        "value": null,
        "text": "123-456-7890",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/4/words/0"
        }]
      },
      "TransactionDate": {
        "valueType": "stringValue",
        "value": "2019-06-10",
        "text": "6/10/2019",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/0"
        }]
      },
      "TransactionTime": {
        "valueType": "stringValue",
        "value": "13:59:00",
        "text": "13:59",
        "elements": [{
          "$ref": "#/recognitionResults/0/lines/5/words/1"
        }]
      }
    }
  }]
}
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u het formulier herkenning REST-API met Python een model te trainen en voer deze uit in een voorbeeldscenario. Vervolgens, Zie de documentatie om te verkennen van de Form-herkenning API dieper op.

> [!div class="nextstepaction"]
> [REST-API-referentiedocumentatie](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt)
