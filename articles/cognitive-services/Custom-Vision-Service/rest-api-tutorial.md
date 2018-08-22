---
title: Gebruik de een Custom Vision Service REST API - Azure Cognitive Services | Microsoft Docs
description: De REST-API gebruiken om te maken, trainen, testen en exporteren van een aangepaste vision-model.
services: cognitive-services
author: blackmist
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/07/2018
ms.author: larryfr
ms.openlocfilehash: 44fa4d45c33f3064c089724ee761a70d0a8421ab
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250261"
---
# <a name="tutorial-use-the-custom-vision-rest-api"></a>Zelfstudie: De Custom Vision REST-API gebruiken

Informatie over het gebruik van de aangepaste Vision REST-API te maken, trainen, testen en exporteren van een model.

De informatie in dit document laat zien hoe u een REST-client gebruiken om te werken met de REST-API voor het trainen van de Custom Vision service. De voorbeelden laten zien hoe u gebruik van de API kunnen doen met de `curl` hulpprogramma vanuit een bash-omgeving en `Invoke-WebRequest` vanuit Windows PowerShell.

> [!div class="checklist"]
> * Sleutels ophalen
> * Een project maken
> * -Tags maken
> * Afbeeldingen toevoegen
> * Trainen en testen van het model
> * Exporteer het model

## <a name="prerequisites"></a>Vereisten

* Een elementaire kennis met Representational State Transfer (REST). Dit document gaat niet verder in op details op zoals HTTP-termen, JSON of andere dingen die doorgaans gebruikt voor de REST.

* Een van beide een bash (Bourne opnieuw Shell) met de [curl](https://curl.haxx.se) hulpprogramma of Windows PowerShell 3.0 (of hoger).

* Een aangepaste Vision-account. Zie voor meer informatie de [een classificatie maken](getting-started-build-a-classifier.md) document.

## <a name="get-keys"></a>Sleutels ophalen

Wanneer u de REST-API gebruikt, moet u verifiëren met behulp van een sleutel. Bij het uitvoeren van beheer of trainingsbewerkingen die, gebruikt u de __training sleutel__. Wanneer u het model om voorspellingen te doen, gebruikt u de __voorspelling sleutel__.

Wanneer een aanvraag, wordt de sleutel als een aanvraagheader verzonden.

Als u de sleutels voor uw account, gaat u naar de [Custom Vision webpagina](https://customvision.ai) en selecteer de __tandwielpictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Omdat de sleutels worden gebruikt voor het verifiëren van elke aanvraag, de voorbeelden in dit document wordt ervan uitgegaan dat de sleutelwaarden zijn opgenomen in de omgevingsvariabelen. Gebruik de volgende opdrachten voor het opslaan van de sleutels voor omgevingsvariabelen voordat u een andere codefragmenten in dit document:
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

De volgende voorbeelden maken van een nieuw project met de naam `myproject` in uw aangepaste Vision service-exemplaar. Deze service standaard ingesteld op de `General` domein:

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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
> De `id` vermelding in het antwoord is de ID van het nieuwe project. Dit wordt gebruikt in andere voorbeelden verderop in dit document.

Zie voor meer informatie over deze aanvraag [CreateProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8290).

### <a name="specific-domains"></a>Specifieke domeinen

Voor het maken van een project voor een specifiek domein, kunt u opgeven de __domein-ID__ als een optionele parameter. De volgende voorbeelden ziet hoe u een lijst met beschikbare domeinen op te halen:

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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

Zie voor meer informatie over deze aanvraag [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d).

Het volgende voorbeeld ziet u een nieuw project maken dat gebruikmaakt van de __oriëntatiepunten__ domein:

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

## <a name="create-tags"></a>-Tags maken

Tag-afbeeldingen, moet u een label-ID. Het volgende voorbeeld ziet u hoe u het maken van een nieuwe tag met de naam `cat` en ophalen van een label-ID. Vervang `{projectId}` met de ID van uw project. Gebruik de `name=` parameter opgeven voor de naam van de tag:

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

Sla de `id` waarde heeft, zoals deze wordt gebruikt bij het labelen van afbeeldingen.

Zie voor meer informatie over deze aanvraag [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d).

## <a name="add-images"></a>Afbeeldingen toevoegen

De volgende voorbeelden ziet een bestand toe te voegen uit URL. Vervang `{projectId}` met de ID van uw project. Vervang `{tagId}` met de ID van de code voor de installatiekopie:

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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

Zie voor meer informatie over deze aanvraag [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287).

## <a name="train-the-model"></a>Het model trainen

De volgende voorbeelden laten zien hoe u het model te trainen. Vervang `{projectId}` met de ID van uw project:

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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

Sla de `id` waarde, zoals het wordt gebruikt om te testen en exporteer het model.

Zie voor meer informatie, [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294).

## <a name="test-the-model"></a>Het model testen

De volgende voorbeelden laten zien hoe u een test van het model uitvoeren. Vervang `{projectId}` met de ID van uw project. Vervang `{iterationId}` met de ID die wordt geretourneerd van het model te trainen. Vervang `https://linktotestimage` met het pad naar de testinstallatiekopie van de.

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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

Zie voor meer informatie, [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d).

## <a name="export-the-model"></a>Exporteer het model

Exporteren van een model is een proces in twee stappen. U moet eerst de model-indeling opgeven en vervolgens de URL voor het geëxporteerde model aanvragen.

### <a name="request-a-model-export"></a>Aanvraag voor een Modelexporteren

De volgende voorbeelden laten zien hoe u voor het exporteren van een `coreml` model. Vervang `{projectId}` met de ID van uw project. Vervang `{iterationId}` met de ID die wordt geretourneerd van het model te trainen.

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

```json
{
    "platform": "CoreML",
    "status": "Exporting",
    "downloadUri": null,
    "flavor": null
}
```

Zie voor meer informatie, [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b).

### <a name="download-the-exported-model"></a>Downloaden van het geëxporteerde model

De volgende voorbeelden laten zien hoe u de URL van het geëxporteerde model niet ophalen. Vervang `{projectId}` met de ID van uw project. Vervang `{iterationId}` met de ID die wordt geretourneerd van het model te trainen.

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

Het antwoord op de aanvraag is vergelijkbaar met de volgende JSON-document:

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

Zie voor meer informatie, [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a).