---
title: 'Quickstart: Ontvangst-gegevens met cURL - formulier herkenning ophalen'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids hebt u het formulier herkenning REST-API gebruiken met cURL gegevens te extraheren uit afbeeldingen van verkoopontvangsten.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 07/01/2019
ms.author: pafarley
ms.openlocfilehash: f9111dcb1f85b28a688282f792540128e7146a6b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566198"
---
# <a name="quickstart-extract-receipt-data-using-the-form-recognizer-rest-api-with-curl"></a>Quickstart: Het formulier herkenning REST-API gebruiken met cURL ontvangst-gegevens ophalen

In deze snelstartgids gebruikt u de REST-API van Azure formulier herkenning met cURL uitpakken en relevante informatie in verkoopontvangsten identificeren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt deze snelstartgids hebt voltooid, moet u het volgende hebben:
- Toegang tot de Preview-versie formulier herkenning beperkte toegang. Voor toegang tot de Preview-versie, invullen en verzenden de [formulier herkenning toegangsaanvraag](https://aka.ms/FormRecognizerRequestAccess) formulier.
- [cURL](https://curl.haxx.se/windows/) geïnstalleerd.
- Een URL voor een afbeelding van een ontvangst. U kunt een [voorbeeldafbeelding](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/curl/form-recognizer/contoso-receipt.png?raw=true) voor deze Quick Start.

## <a name="create-a-form-recognizer-resource"></a>Een formulier herkenning-resource maken

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="analyze-a-receipt"></a>Een ontvangst analyseren

Voor het starten van een ontvangst analyseren, u kunt aanroepen de **analyseren ontvangst** API met behulp van de volgende cURL-opdracht. Voordat u de opdracht uitvoert, moet u deze wijzigingen:

1. Vervang `<Endpoint>` met het eindpunt dat u hebt verkregen via uw abonnementssleutel formulier herkenning. U vindt deze voor uw resource formulier herkenning **overzicht** tabblad.
1. Vervang `<your receipt URL>` met het URL-adres van een installatiekopie van ontvangst.
1. Vervang `<subscription key>` met de abonnementssleutel die u hebt gekopieerd uit de vorige stap.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/asyncBatchAnalyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"url\": \"<your receipt URL>\"}"
```

U ontvangt een `202 (Success)` -reactie met een **bewerking locatie** header. De waarde van deze header bevat een bewerkings-ID die u kunt de status van de bewerking opvragen en de resultaten krijgt. In het volgende voorbeeld wordt de tekenreeks na `operations/` is de bewerking-ID.

```console
https://cognitiveservice/formrecognizer/v1.0-preview/prebuilt/receipt/operations/54f0b076-4e38-43e5-81bd-b85b8835fdfb
```

## <a name="get-the-receipt-results"></a>De resultaten van de ontvangst ophalen

Nadat u hebt met de naam de **analyseren ontvangst** -API aanroepen de **ontvangst resultaat ophalen** API voor het ophalen van de status van de uitvoering en de opgehaalde gegevens.

1. Vervang `<operationId>` met de bewerkings-ID uit de vorige stap.
1. Vervang `<subscription key>` door uw abonnementssleutel.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v1.0-preview/prebuilt/receipt/operations/<operationId>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Het antwoord bekijken

U ontvangt een `200 (Success)` -antwoord met de JSON-uitvoer. Het eerste veld `"status"`, geeft de status van de bewerking. Als de bewerking voltooid is, de `"recognitionResults"` bevat elke regel van de tekst die is geëxtraheerd uit de ontvangst van het veld en de `"understandingResults"` veld sleutel/waarde-gegevens voor de meest relevante onderdelen van de ontvangst bevat. Als de bewerking niet voltooid en is de waarde van `"status"` worden `"Running"` of `"NotStarted"`, en u moet de API wordt aangeroepen, hetzij handmatig, hetzij via een script. U wordt aangeraden een interval van één seconde of meer tussen aanroepen.

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

In deze snelstartgids gebruikt u het formulier herkenning REST-API met cURL om op te halen van de inhoud van een verkoop ontvangst. Vervolgens, Zie de documentatie om te verkennen van de Form-herkenning API dieper op.

> [!div class="nextstepaction"]
> [REST-API-referentiedocumentatie](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeReceipt)
