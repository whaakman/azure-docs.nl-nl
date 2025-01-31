---
title: 'Quickstart: Een model te trainen en extraheren van gegevens met behulp van de REST-API met Python - formulier-herkenning'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids gebruikt u het formulier herkenning REST-API met Python een model te trainen en extraheer gegevens uit formulieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 63b58e5e667d03d7808973b0783ed98ab4cb5447
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605876"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-python"></a>Quickstart: Een formulier herkenning-model te trainen en gegevens ophalen met behulp van de REST-API met Python

In deze snelstartgids gebruikt u de Azure formulier herkenning REST-API met Python te trainen en te beoordelen van formulieren om uit te pakken sleutel / waarde-paren en tabellen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt deze snelstartgids hebt voltooid, moet u het volgende hebben:
- Toegang tot de Preview-versie formulier herkenning beperkte toegang. Voor toegang tot de Preview-versie, invullen en verzenden de [formulier herkenning toegangsaanvraag](https://aka.ms/FormRecognizerRequestAccess) formulier.
- [Python](https://www.python.org/downloads/) geïnstalleerd (als u het voorbeeld lokaal uitvoeren wilt).
- Een set van ten minste vijf vormen van hetzelfde type. U gebruikt deze gegevens naar het model te trainen. U kunt een [verzameling voorbeeldgegevens](https://go.microsoft.com/fwlink/?linkid=2090451) voor deze Quick Start. De gegevens uploaden naar de hoofdmap van een Azure Blob Storage-account.

## <a name="create-a-form-recognizer-resource"></a>Een formulier herkenning-resource maken

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Een formulier herkenning-model te trainen

Eerst moet u een set trainingsgegevens in een Azure Storage blob-container. U moet minimaal vijf ingevuld in formulieren (PDF-documenten en/of afbeeldingen) van de dezelfde soort/structuur als uw belangrijkste invoergegevens hebben. Of u kunt een leeg formulier gebruiken met twee ingevuld in formulieren. De bestandsnaam van het leeg formulier nodig heeft om het woord 'empty'. Zie [bouwen van een set trainingsgegevens voor een aangepast model](../build-training-data-set.md) voor tips en opties voor het samenstellen van uw trainingsgegevens.

Als u wilt een formulier herkenning model trainen met de documenten in uw Azure blob-container, aanroepen de **trainen** API door het uitvoeren van de volgende python-code. Voordat u de code uitvoert, moet u deze wijzigingen:

1. Vervang `<Endpoint>` met de eindpunt-URL voor de herkenning van formulier-resource in de Azure-regio waar u de abonnementssleutels van uw hebt verkregen.
1. Vervang `<Subscription key>` met de abonnementssleutel die u hebt gekopieerd uit de vorige stap.
1. Vervang `<SAS URL>` met de Azure Blob storage-container gedeelde access signature (SAS)-URL. De SAS-URL ophalen, opent u de Microsoft Azure Storage Explorer, met de rechtermuisknop op de container en selecteer **handtekening voor gedeelde toegang ophalen**. Zorg ervoor dat de **lezen** en **lijst** machtigingen zijn ingeschakeld en klik op **maken**. Kopieer vervolgens de waarde in de **URL** sectie. Deze moet de vorm hebben: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

    ```python
    ########### Python Form Recognizer Train #############
    from requests import post as http_post

    # Endpoint URL
    base_url = r"<Endpoint>" + "/formrecognizer/v1.0-preview/custom"
    source = r"<SAS URL>"
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': '<Subscription Key>',
    }
    url = base_url + "/train" 
    body = {"source": source}
    try:
        resp = http_post(url = url, json = body, headers = headers)
        print("Response status code: %d" % resp.status_code)
        print("Response body: %s" % resp.json())
    except Exception as e:
        print(str(e))
    ```
1. Sla de code in een bestand met de extensie .py. Bijvoorbeeld, *formulier-herkennen train.py*.
1. Open een opdrachtpromptvenster.
1. Typ bij de prompt de opdracht `python` om het voorbeeld uit te voeren. Bijvoorbeeld `python form-recognize-train.py`.

U ontvangt een `200 (Success)` -antwoord met deze JSON-uitvoer:

```json
{
  "modelId": "59e2185e-ab80-4640-aebc-f3653442617b",
  "trainingDocuments": [
    {
      "documentName": "Invoice_1.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_2.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_3.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_4.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_5.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    }
  ],
  "errors": []
}
```

Houd er rekening mee de `"modelId"` waarde. U hebt deze nodig voor de volgende stappen uit.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Sleutel / waarde-paren en tabellen onttrekken aan formulieren

Vervolgens u een document analyseren en extraheren van sleutel / waarde-paren en tabellen uit. Roep de **Model - analyseren** API door het uitvoeren van het Python-script dat volgt. Voordat u de opdracht uitvoert, moet u deze wijzigingen:

1. Vervang `<Endpoint>` met het eindpunt dat u hebt verkregen met uw abonnementssleutel formulier herkenning. U vindt deze voor uw resource formulier herkenning **overzicht** tabblad.
1. Vervang `<path to your form>` met het pad van het formulier (bijvoorbeeld C:\temp\file.pdf).
1. Vervang `<modelID>` met de model-ID die u hebt ontvangen in de vorige sectie.
1. Vervang `<file type>` met het bestandstype. Ondersteunde typen: `application/pdf`, `image/jpeg`, `image/png`.
1. Vervang `<subscription key>` door uw abonnementssleutel.

    ```python
    ########### Python Form Recognizer Analyze #############
    from requests import post as http_post
    
    # Endpoint URL
    base_url = r"<Endpoint>" + "/formrecognizer/v1.0-preview/custom"
    file_path = r"<path to your form>"
    model_id = "<modelID>"
    headers = {
        # Request headers
        'Content-Type': '<file type>',
        'Ocp-Apim-Subscription-Key': '<subscription key>',
    }

    try:
        url = base_url + "/models/" + model_id + "/analyze" 
        with open(file_path, "rb") as f:
            data_bytes = f.read()  
        resp = http_post(url = url, data = data_bytes, headers = headers)
        print("Response status code: %d" % resp.status_code)    
        print("Response body:\n%s" % resp.json())   
    except Exception as e:
        print(str(e))
    ```

1. Sla de code in een bestand met de extensie .py. Bijvoorbeeld, *formulier-herkennen analyze.py*.
1. Open een opdrachtpromptvenster.
1. Typ bij de prompt de opdracht `python` om het voorbeeld uit te voeren. Bijvoorbeeld `python form-recognize-analyze.py`.

### <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagde reactie is geretourneerd in JSON. Staat voor de sleutel / waarde-paren en tabellen die zijn geëxtraheerd uit het formulier:

```bash
{
  "status": "success",
  "pages": [
    {
      "number": 1,
      "height": 792,
      "width": 612,
      "clusterId": 0,
      "keyValuePairs": [
        {
          "key": [
            {
              "text": "Address:",
              "boundingBox": [
                57.4,
                683.1,
                100.5,
                683.1,
                100.5,
                673.7,
                57.4,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "1 Redmond way Suite",
              "boundingBox": [
                57.4,
                671.3,
                154.8,
                671.3,
                154.8,
                659.2,
                57.4,
                659.2
              ],
              "confidence": 0.86
            },
            {
              "text": "6000 Redmond, WA",
              "boundingBox": [
                57.4,
                657.1,
                146.9,
                657.1,
                146.9,
                645.5,
                57.4,
                645.5
              ],
              "confidence": 0.86
            },
            {
              "text": "99243",
              "boundingBox": [
                57.4,
                643.4,
                85,
                643.4,
                85,
                632.3,
                57.4,
                632.3
              ],
              "confidence": 0.86
            }
          ]
        },
        {
          "key": [
            {
              "text": "Invoice For:",
              "boundingBox": [
                316.1,
                683.1,
                368.2,
                683.1,
                368.2,
                673.7,
                316.1,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "Microsoft",
              "boundingBox": [
                374,
                687.9,
                418.8,
                687.9,
                418.8,
                673.7,
                374,
                673.7
              ],
              "confidence": 1
            },
            {
              "text": "1020 Enterprise Way",
              "boundingBox": [
                373.9,
                673.5,
                471.3,
                673.5,
                471.3,
                659.2,
                373.9,
                659.2
              ],
              "confidence": 1
            },
            {
              "text": "Sunnayvale, CA 87659",
              "boundingBox": [
                373.8,
                659,
                479.4,
                659,
                479.4,
                645.5,
                373.8,
                645.5
              ],
              "confidence": 1
            }
          ]
        }
      ],
      "tables": [
        {
          "id": "table_0",
          "columns": [
            {
              "header": [
                {
                  "text": "Invoice Number",
                  "boundingBox": [
                    38.5,
                    585.2,
                    113.4,
                    585.2,
                    113.4,
                    575.8,
                    38.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "34278587",
                    "boundingBox": [
                      38.5,
                      547.3,
                      82.8,
                      547.3,
                      82.8,
                      537,
                      38.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Date",
                  "boundingBox": [
                    139.7,
                    585.2,
                    198.5,
                    585.2,
                    198.5,
                    575.8,
                    139.7,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/18/2017",
                    "boundingBox": [
                      139.7,
                      546.8,
                      184,
                      546.8,
                      184,
                      537,
                      139.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Due Date",
                  "boundingBox": [
                    240.5,
                    585.2,
                    321,
                    585.2,
                    321,
                    575.8,
                    240.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/24/2017",
                    "boundingBox": [
                      240.5,
                      546.8,
                      284.8,
                      546.8,
                      284.8,
                      537,
                      240.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Charges",
                  "boundingBox": [
                    341.3,
                    585.2,
                    381.2,
                    585.2,
                    381.2,
                    575.8,
                    341.3,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "$56,651.49",
                    "boundingBox": [
                      387.6,
                      546.4,
                      437.5,
                      546.4,
                      437.5,
                      537,
                      387.6,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "VAT ID",
                  "boundingBox": [
                    442.1,
                    590,
                    474.8,
                    590,
                    474.8,
                    575.8,
                    442.1,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "PT",
                    "boundingBox": [
                      447.7,
                      550.6,
                      460.4,
                      550.6,
                      460.4,
                      537,
                      447.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            }
          ]
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u het formulier herkenning REST-API met Python een model te trainen en voer deze uit in een voorbeeldscenario. Vervolgens, Zie de documentatie om te verkennen van de Form-herkenning API dieper op.

> [!div class="nextstepaction"]
> [REST-API-referentiedocumentatie](https://aka.ms/form-recognizer/api)
