---
title: Azure Media openen paginaknipsel taken verzenden | Microsoft Docs
description: Stappen voor het indienen van paginaknipsel taken van Azure Media openen
services: media-services
keywords: clip; subclip; codering; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8372c405087c0dc7a000a65265bb99c395c3a8d6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="submit-clipping-jobs-from-azure-media-clipper"></a>Verzenden van taken paginaknipsel van Azure Media openen
Azure Media openen vereist een **submitSubclipCallback** methode voor het verwerken van de verzending van de taak paginaknipsel worden geïmplementeerd. Deze functie is voor het implementeren van een HTTP POST van de uitvoer openen met een webservice. Deze webservice is waar u de coderingstaak kunt verzenden. De uitvoer van de openen is ofwel een Media Encoder Standard codering voorinstelling voor gerenderde taken of de nettolading van de REST-API voor dynamische manifest filter aanroepen. Dit Pass Through-model is nodig omdat media services-accountreferenties niet beveiligd in de clientbrowser zijn.

De volgende reeksdiagram illustreert de werkstroom tussen de browserclient, uw web-service en Azure Media Services: ![Azure Media openen Sequence Diagram](media/media-services-azure-media-clipper-submit-job/media-services-azure-media-clipper-sequence-diagram.PNG)

In het voorgaande diagram de vier entiteiten zijn: de eindgebruiker de browser, uw webservice, de CDN-eindpunt de openen resources en Azure Media Services te hosten. Wanneer de gebruiker naar uw webpagina navigeert, haalt de pagina de openen JavaScript en CSS-bronnen van het hosting CDN-eindpunt. De eindgebruiker configureert u de taak knippen of dynamische manifest filter maken van de aanroep vanuit hun browser. Wanneer de eindgebruiker de taak of filter maken-aanroep verzendt, wordt de browser de nettolading van de taak met een webservice die u moet implementeren. Deze webservice uiteindelijk verzendt de taak knippen of filter maken van de aanroep naar Azure Media Services met behulp van uw media services-accountreferenties.

Het volgende codevoorbeeld ziet u een voorbeeld van een **submitSubclipCallback** methode. Bij deze methode kunt u het HTTP POST-protocol van de Media Encoder Standard codering voorinstelling implementeren. Als het bericht geslaagd is (**resultaat**), wordt de **belofte** is opgelost, anders wordt het bericht met foutdetails geweigerd.

```javascript
// Submit Subclip Callback
//
// Parameter:
// - subclip: object that represents the subclip (output contract).
//
// Returns: a Promise object that, when resolved, retuns true if the operation was accept in the back-end; otherwise, returns false.
var onSubmitSubclip = function (subclip) {
    var promise = new Promise(function (resolve, reject) {
        // TODO: perform the back-end AJAX request to submit the subclip job.
        var result = true;

        if (/* everything turned out fine */ result) {
            resolve(result);
        }
        else {
            reject(Error("error details"));
        }
    });

    return promise;
};

var subclipper = new subclipper({
    selector: '#root',
    restVersion: '2.0',
    submitSubclipCallback: onSubmitSubclip,
});
```
De uitvoer van de verzending van de taak is de Media Encoder Standard codering definitie voor de gerenderde taak of de nettolading van de REST-API voor dynamische manifest filters.

## <a name="submitting-encoding-job-to-create-video"></a>Codering uptaak video maken
U kunt een Media Encoder Standard codering taak voor het maken van een frame nauwkeurige video clip verzenden. Codering taak produceren video's weergegeven, een nieuwe gefragmenteerd MP4-bestand.

Het contract van de taak uitvoer voor gerenderde paginaknipsel is een JSON-object met de volgende eigenschappen:

```json
{
  /* Required */
  "name": "My New Subclip Output Name",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: The REST API version, currently 2.0 is supported
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id-1",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }, {
    /* Required */
    "id": "my-asset-id-2",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "job",

    /* Required if "type" === "job" */
    /* NOTE: This is the preset for the Media Encoder Standard (MES) processor that can be used in the back-end to sumit the subclip job.
    The encoding profile ("Codecs" property) depends on the "singleBitrateMp4Profile" and "multiBitrateMp4Profile" option parameters
    specified when creating the widget instance. */
    /* REFERENCE: https://docs.microsoft.com/azure/media-services/media-services-advanced-encoding-with-mes */
    "job": {
      "Version": 1.0,
      "Codecs": [{
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [{
            "Profile": "Auto",
            "Level": "auto",
            "Bitrate": 6750,
            "MaxBitrate": 6750,
            "BufferWindow": "00:00:05",
            "Width": 1920,
            "Height": 1080,
            "BFrames": 3,
            "ReferenceFrames": 3,
            "AdaptiveBFrame": true,
            "Type": "H264Layer",
            "FrameRate": "0/1"
          }],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [{
        "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
        "Format": {
          "Type": "MP4Format"
        }
      }],
      "Sources": [{
        "AssetId": "my-asset-id-1",
        "StartTime": "0.00:01:15.000",
        "Duration": "0.00:00:25.000"
      }, {
        "AssetId": "my-asset-id-2",
        "StartTime": "0.00:20:04.000",
        "Duration": "0.00:33:07.500"
      }]
    }
  }
}
```

Uitvoeren van de coderingstaak verzenden Media Encoder Standard coderingstaak met de bijbehorende vooraf ingesteld. Zie dit artikel voor meer informatie over het verzenden van codering taken met behulp van de [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-encode-with-media-encoder-standard) of [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-encode-asset).

## <a name="quickly-creating-video-clips-without-encoding"></a>Snel maakt videoclips zonder codering
Alternatief voor het maken van een codeertaak, kunt u Azure Media openen om dynamische manifest filters te maken. Filters vereisen geen codering en snel kunnen worden gemaakt als een nieuwe asset is niet gemaakt. Het contract uitvoer voor een filter knipsel is een JSON-object met de volgende eigenschappen:

```json
{
  /* Required: Filter name (Alphanumeric characters and hyphens only, no whitespace) */
  "name": "FilterName",

  /* Required: string specifying the format of the Json file.
   */
  /* NOTE: This subclipper version always returns '2.0' in this field.
   */
  "restVersion": "2.0"

  /* Required: array containing all the input Ids (both "asset" or "filter" type) used in the subclipper timeline;
  it must contain at least one item. */
  /* NOTE: when "output"."type" === "job", all items must match the "inputsIds"[i]."type" === "asset" condition;
  this array can be used in the back-end to get the input assets for the subclipping job with the Media Encoder Standard (MES) processor. */
  /* NOTE: when "output"."type" === "filter", there must be a single item in the array ("inputsIds".length === 1)
  that can be used in the back-end to get the source for the dynamic manifest filter (create or update). */
  "inputsIds": [{
    /* Required */
    "id": "my-asset-id",

    /* Required: must be one of the following values: "asset" or "filter" */
    "type": "asset"
  }],

  /* Required */
  "output": {

    /* Required: must be one of the following values: "job" or "filter" */
    "type": "filter",

    /* Required if "type" === "filter" */
    /* REFERENCE: https://docs.microsoft.com/rest/api/media/operations/assetfilter */
    "filter": {
      "PresentationTimeRange": {
        "StartTimestamp": "340000000",
        "EndTimestamp": "580000000",
        "Timescale": "10000000"
      },
      "Tracks": [{
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "video",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "audio",
          "Operator": "Equal"
        }, {
          "Property": "Name",
          "Value": "audio-track-1",
          "Operator": "Equal"
        }]
      }, {
        "PropertyConditions": [{
          "Property": "Type",
          "Value": "text",
          "Operator": "Equal"
        }, {
          "Property": "Language",
          "Value": "en",
          "Operator": "Equal"
        }]
      }]
    }
  }
}
```

Als u de REST-aanroep om dynamische manifest filter te maken, indienen de bijbehorende filter nettolading met de [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest).