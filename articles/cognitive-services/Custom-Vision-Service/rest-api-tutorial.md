---
title: 'Zelfstudie: de REST API van Custom Vision Service gebruiken'
titlesuffix: Azure Cognitive Services
description: Gebruik de REST API voor het maken, trainen, testen en exporteren van een aangepast Vision-model.
services: cognitive-services
author: blackmist
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/07/2018
ms.author: larryfr
ms.openlocfilehash: 5e5cf04bb631585c14d2ec7403bf0332f41dc626
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054206"
---
# <a name="tutorial-use-the-custom-vision-rest-api"></a>Zelfstudie: de REST API van Custom Vision gebruiken

Leer de REST API van de Custom Vision Service gebruiken om een model te maken, te trainen, te testen en te exporteren.

De informatie in dit document laat zien hoe u een REST-client kunt gebruiken om met behulp van de REST API de Custom Vision Service te trainen. De voorbeelden laten zien hoe u de API gebruikt met het `curl`-hulpprogramma vanuit een bash-omgeving en `Invoke-WebRequest` vanuit Windows PowerShell.

> [!div class="checklist"]
> * Sleutels ophalen
> * Een project maken
> * Tags maken
> * Afbeeldingen toevoegen
> * Het model trainen en testen
> * Het model exporteren

## <a name="prerequisites"></a>Vereisten

* Een elementaire kennis van Representational State Transfer (REST). Dit document gaat niet verder in op details zoals HTTP-woorden, JSON of andere elementen die doorgaans gebruikt worden met REST.

* Een bash (Bourne Again Shell) met het [curl](https://curl.haxx.se)-hulpprogramma of Windows PowerShell 3.0 (of hoger).

* Een Custom Vision-account. Zie het document[Een classificatie maken](getting-started-build-a-classifier.md) voor meer informatie.

## <a name="get-keys"></a>Sleutels ophalen

Bij gebruik van de REST API moet u verificatie uitvoeren met behulp van een sleutel. Bij het uitvoeren van beheer- of trainingsbewerkingen gebruikt u de __trainingssleutel__. Wanneer u het model gebruikt om voorspellingen te doen, gebruikt u de __voorspellingssleutel__.

Wanneer u een aanvraag indient, wordt de sleutel als aanvraagheader verzonden.

Als u de sleutels voor uw account wilt ophalen, gaat u naar de [Custom Vision-webpagina](https://customvision.ai) en selecteert u het __tandwielpictogram__ in de rechterbovenhoek. In de sectie __Accounts__ kopieert u de waarden uit de velden __Trainingssleutel__ en __Voorspellingssleutel__.

![Afbeelding van de gebruikersinterface van de sleutels](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Omdat de sleutels worden gebruikt om elke aanvraag te verifiëren, wordt in de voorbeelden in dit document ervan uitgegaan dat de sleutelwaarden zijn opgenomen in de omgevingsvariabelen. Gebruik de volgende opdrachten om de sleutels op te slaan in omgevingsvariabelen voordat u andere codefragmenten in dit document gebruikt:
>
> ```bash
> read -p "Enter the training key: " TRAININGKEY
> read -p "Enter the prediction key: " PREDICTIONKEY
> ```
>
> ```powershell
> $trainingKey = Read-Host 'Enter the training key'
> $predictionKey = Read-Host 'Enter the prediction key'
> ```

## <a name="create-a-new-project"></a>Een nieuw project maken

Met de volgende voorbeelden wordt een nieuw project met de naam `myproject` in uw Custom Vision Service-exemplaar gemaakt. Deze service is standaard ingesteld op het domein `General`:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
{
  "id":"45d1b19b-69b6-4a22-8e7e-d1ca37504686",
  "name":"myproject",
  "description":"",
  "settings":{
    "domainId":"ee85a72c-405e-4adc-bb47-ffa8ca0c9f31",
    "useNegativeSet":true,
    "classificationType":"Multilabel",
    "detectionParameters":null
  },
  "created":"2018-08-10T17:39:02.5633333",
  "lastModified":"2018-08-10T17:39:02.5633333",
  "thumbnailUri":null
}
```

> [!TIP]
> De `id`-vermelding in het antwoord is de ID van het nieuwe project. Deze wordt gebruikt in andere voorbeelden verderop in dit document.

Zie [CreateProject](https://go.microsoft.com/fwlink/?linkid=865446) voor meer informatie over deze aanvraag.

### <a name="specific-domains"></a>Specifieke domeinen

Als u een project voor een specifiek domein wilt maken, kunt u de __domein-ID__ opgeven als een optionele parameter. In de volgende voorbeelden ziet hoe u een lijst met beschikbare domeinen kunt ophalen:

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
[
    {
        "id": "ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
        "name": "General",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "c151d5b5-dd07-472a-acc8-15d29dea8518",
        "name": "Food",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "ca455789-012d-4b50-9fec-5bb63841c793",
        "name": "Landmarks",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    ...
]
```

Zie [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d) voor meer informatie over deze aanvraag.

Het volgende voorbeeld laat zien hoe een nieuw project wordt gemaakt dat gebruikmaakt van het domein __Landmarks__:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

## <a name="create-tags"></a>Tags maken

Om afbeeldingen te taggen – of labelen – moet u een tag-ID gebruiken. Het volgende voorbeeld laat zien hoe u een nieuwe tag met de naam `cat` kunt maken en een tag-ID kunt ophalen. Vervang `{projectId}` door de ID van het project. Gebruik de parameter `name=` om de naam van de tag op te geven:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het JSON-document: 

```json
{"id":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43","name":"cat","description":null,"imageCount":0}
```

Sla de waarde `id` op omdat deze wordt gebruikt bij het taggen van afbeeldingen.

Zie [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d) voor meer informatie over deze aanvraag.

## <a name="add-images"></a>Afbeeldingen toevoegen

In de voorbeelden hieronder wordt een bestand toegevoegd vanuit een URL. Vervang `{projectId}` door de ID van het project. Vervang `{tagId}` door de ID van de tag voor de afbeelding:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
{
    "isBatchSuccessful": true,
    "images": [
        {
            "sourceUrl": "http://myimages/cat.jpg",
            "status": "OK",
            "image": {
                "id": "081adaee-a76b-4d94-a70e-e4fd0935a28f",
                "created": "2018-08-13T13:24:22.0815638",
                "width": 640,
                "height": 480,
                "imageUri": "https://linktoimage",
                "thumbnailUri": "https://linktothumbnail",
                "tags": [
                    {
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936"
                    }
                ],
                "regions": [
                    {
                        "regionId": "40f206a1-3f8a-4de7-a6c3-c7b4643117df",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936",
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "left": 119,
                        "top": 94,
                        "width": 240,
                        "height": 140
                    }
                ]
            }
        }
    ]
}
```

Zie [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287) voor meer informatie over deze aanvraag.

## <a name="train-the-model"></a>Het model trainen

De volgende voorbeelden laten zien hoe het model wordt getraind. Vervang `{projectId}` door de ID van het project:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } 
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
{
    "id": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "name": "Iteration 1",
    "isDefault": false,
    "status": "Training",
    "created": "2018-08-10T17:39:02.5766667",
    "lastModified": "2018-08-16T17:15:07.5250661",
    "projectId": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "exportable": false,
    "domainId": null
}
```

Sla de waarde `id` op omdat deze wordt gebruikt om het model te testen en exporteren.

Zie [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294) voor meer informatie.

## <a name="test-the-model"></a>Het model testen

De volgende voorbeelden laten zien hoe het model wordt getest. Vervang `{projectId}` door de ID van het project. Vervang `{iterationId}` door de ID die wordt geretourneerd door het model te trainen. Vervang `https://linktotestimage` door het pad naar de testafbeelding.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"url":"https://linktotestimage"}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"url":"https://linktotestimage"}'
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
{
    "id": "369b010b-2a92-4f48-a918-4c1a0af91888",
    "project": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "iteration": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "created": "2018-08-16T17:39:20.7944508Z",
    "predictions": [
        {
            "probability": 0.8390652,
            "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "tagName": "cat"
        }
    ]
}
```

Zie [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d) voor meer informatie.

## <a name="export-the-model"></a>Het model exporteren

Het exporteren van een model behelst een proces in twee stappen. U moet eerst de modelindeling opgeven en vervolgens de URL voor het geëxporteerde model aanvragen.

### <a name="request-a-model-export"></a>Exporteren van een model aanvragen

De volgende voorbeelden laten zien hoe een model `coreml` wordt geëxporteerd. Vervang `{projectId}` door de ID van het project. Vervang `{iterationId}` door de ID die wordt geretourneerd door het model te trainen.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
{
    "platform": "CoreML",
    "status": "Exporting",
    "downloadUri": null,
    "flavor": null
}
```

Zie [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b) voor meer informatie.

### <a name="download-the-exported-model"></a>Het geëxporteerde model downloaden

De volgende voorbeelden laten zien hoe u de URL van het geëxporteerde model kunt ophalen. Vervang `{projectId}` door de ID van het project. Vervang `{iterationId}` door de ID die wordt geretourneerd door het model te trainen.

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

Het antwoord op de aanvraag is vergelijkbaar met het volgende JSON-document:

```json
[
    {
        "platform": "CoreML",
        "status": "Done",
        "downloadUri": "https://linktoexportedmodel",
        "flavor": null
    }
]
```

Zie [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a) voor meer informatie.
