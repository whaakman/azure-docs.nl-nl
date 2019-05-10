---
title: 'Quickstart: Een model te trainen en extraheren van gegevens met cURL - formulier-herkenning'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids gebruikt u het formulier herkenning REST-API met cURL naar een model te trainen en extraheer gegevens uit formulieren.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: pafarley
ms.openlocfilehash: 36f98a8dea2a732a7f8504b160da895637366fc8
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65471906"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-using-rest-api-with-curl"></a>Quickstart: Een formulier herkenning-model te trainen en extraheren van gegevens met behulp van REST-API met cURL

In deze snelstartgids gebruikt u het formulier-herkenning REST-API met cURL te trainen en te beoordelen van formulieren om uit te pakken sleutel / waarde-paren en tabellen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* U hebt u toegang tot de Preview-versie formulier herkenning beperkte toegang. Voor toegang tot de Preview-versie, vul het en verzenden de [toegangsaanvraag voor herkenning van Cognitive Services-formulier](https://aka.ms/FormRecognizerRequestAccess) formulier. 
* U moet [cURL](https://curl.haxx.se/windows/) hebben.
* U moet een abonnementssleutel hebben voor de herkenning van het formulier. Volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) abonneren op formulier-herkenning en haal uw sleutel.
* U moet een minimale set van vijf vormen van hetzelfde type hebben. U kunt een [voorbeeldgegevensset](https://go.microsoft.com/fwlink/?linkid=2090451) voor deze Quick Start.

## <a name="train-a-form-recognizer-model"></a>Een formulier herkenning-model te trainen

Eerst moet u een set trainingsgegevens. U kunt gegevens in een Azure-Blob of uw eigen lokale trainingsgegevens gebruiken. U moet minimaal vijf voorbeeldformulieren (PDF-documenten en/of afbeeldingen) van de dezelfde soort/structuur als uw belangrijkste invoergegevens hebben. U kunt ook een leeg formulier; gebruiken de bestandsnaam van het formulier bevat het woord 'lege'.

Als u wilt een formulier herkenning-model met behulp van de documenten in uw Azure Blob-container te trainen, aanroepen de **trainen** API door het uitvoeren van de volgende cURL-opdracht. Voordat u de opdracht uitvoert, moet u de volgende wijzigingen aanbrengen:

* Vervang `<Endpoint>` met het eindpunt dat u hebt verkregen via uw abonnementssleutel formulier herkenning. U vindt deze op het overzichtstabblad van formulier herkenning resource.
* Vervang `<SAS URL>` met een Azure Blob Storage-container gedeelde toegang krijgen tot signature (SAS)-URL waar de trainingsgegevens zich bevindt.  
* Vervang `<subscription key>` door uw abonnementssleutel.

```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/train" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

U ontvangt een `200 (Success)` -antwoord met de volgende JSON-uitvoer:

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

Noteer de `"modelId"` waarde; u deze nodig heeft voor de volgende stappen uit.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Sleutel / waarde-paren en tabellen onttrekken aan formulieren

Vervolgens maakt u een document analyseert en sleutel / waarde-paren en tabellen ophalen uit deze. Roep de **Model - analyseren** API door het uitvoeren van de volgende cURL-opdracht. Voordat u de opdracht uitvoert, moet u de volgende wijzigingen aanbrengen:

* Vervang `<Endpoint>` met het eindpunt dat u hebt verkregen via uw abonnementssleutel formulier herkenning. U vindt deze in uw formulier herkenning resource **overzicht** tabblad.
* Vervang `<modelID>` met de model-ID die u hebt ontvangen in de vorige stap van het model te trainen.
* Vervang `<path to your form>` met het pad naar het formulier.
* Vervang `<subscription key>` door uw abonnementssleutel.
* Vervang `<file type>` met het bestandstype. ondersteunde typen PDF-, afbeelding/jpeg, afbeelding/png.

```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/models/<modelID>/analyze" -H "Content-Type: multipart/form-data" -F "form=@\"<path to your form>\";type=application/<file type>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagde respons wordt geretourneerd in JSON en de uitgepakte sleutel / waarde-paren en tabellen in het formulier vertegenwoordigt.

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

In deze handleiding gebruikt u de Form-herkenning REST API's met cURL een model te trainen en voer deze uit in het geval van een voorbeeld. Vervolgens, Zie de documentatie om te verkennen van de Form-herkenning API dieper op.

> [!div class="nextstepaction"]
> [REST-API-referentiedocumentatie](https://aka.ms/form-recognizer/api)
