---
title: Azure Media Clipper kniptaken verzenden | Microsoft Docs
description: Stappen voor het indienen van kniptaken van Azure Media Clipper
services: media-services
keywords: clip;subclip;encoding;media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 0894c3677b87fe48c130d648253dadd0d43429f4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54821444"
---
# <a name="submit-clipping-jobs-from-azure-media-clipper"></a>Kniptaken van Azure Media Clipper verzenden
Azure Media Clipper vereist een **submitSubclipCallback** methode om te worden geïmplementeerd voor het verwerken van knipsel taken verzenden. Deze functie is voor het implementeren van een HTTP POST van de uitvoer Clipper naar een webservice. Deze webservice is waar u de coderingstaak kunt indienen. De uitvoer van de Clipper is ofwel een Media Encoder Standard codering voorinstelling voor gerenderde taken of de nettolading van de REST-API voor dynamisch-manifestfilter aanroepen. Dit Pass Through-model is nodig omdat media services-accountreferenties niet beveiligd in de browser van de client zijn.

De volgende reeksdiagram illustreert de werkstroom tussen de browserclient, uw web-service en Azure Media Services: ![Azure Media Clipper reeksdiagram](media/media-services-azure-media-clipper-submit-job/media-services-azure-media-clipper-sequence-diagram.PNG)

In het voorgaande diagram wordt de vier entiteiten zijn: de eindgebruiker de browser, de webservice, het CDN-eindpunt die als host fungeert voor de Clipper resources en Azure Media Services. Wanneer de gebruiker naar uw webpagina navigeert, wordt de pagina de Clipper JavaScript en CSS-resources vanaf de host CDN-eindpunt. De eindgebruiker configureert u de taak knippen of dynamisch-manifestfilter-aanroep voor het maken van hun browser. Wanneer de gebruiker de taak of filter aanroep voor het maken verzendt, hebt u de browser de nettolading van de taak aan een webservice die u moet implementeren. Deze webservice uiteindelijk de knippen-taak verzendt of filter maken van het aanroepen van Azure Media Services met behulp van uw media services-accountreferenties.

Het volgende codevoorbeeld ziet u een voorbeeld van een **submitSubclipCallback** methode. In deze methode implementeert u het HTTP-bericht van de Media Encoder Standard vooraf ingestelde standaardcodering. Als het bericht voltooid is (**resultaat**), wordt de **belofte** is opgelost, anders wordt het bericht geweigerd met details van fouten.

```javascript
// Submit Subclip Callback
//
// Parameter:
// - subclip: object that represents the subclip (output contract).
//
// Returns: a Promise object that, when resolved, returns true if the operation was accept in the back-end; otherwise, returns false.
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
De uitvoer van het verzenden van taken is de Media Encoder Standard voorinstelling voor encoding voor gerenderde taak of de nettolading van de REST-API voor dynamisch-manifestfilters.

## <a name="submitting-encoding-job-to-create-video"></a>Encoding-taak maken video verzenden
U kunt een Media Encoder Standard encoding-taak voor het maken van een frame nauwkeurig videoclip indienen. Codering taak produceren video's weergegeven, een nieuwe gefragmenteerd MP4-bestand.

Het contract van de taak uitvoer voor de gerenderde knippen is een JSON-object met de volgende eigenschappen:

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
    /* NOTE: This is the preset for the Media Encoder Standard (MES) processor that can be used in the back-end to submit the subclip job.
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

Om in te voeren van de coderingstaak dienen Media Encoder Standard encoding-taak met de bijbehorende definitie. Zie dit artikel voor meer informatie over het indienen van codering taken met behulp van de [.NET SDK](https://docs.microsoft.com/azure/media-services/media-services-dotnet-encode-with-media-encoder-standard) of [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-encode-asset).

## <a name="quickly-creating-video-clips-without-encoding"></a>Snel maken videoclips zonder te coderen
Alternatief voor het maken van een coderingstaak, kunt u Azure Media Clipper dynamisch-manifestfilters maken. Filters hoeven niet te coderen en snel kunnen worden gemaakt wanneer er een nieuw activum wordt niet gemaakt. Het contract uitvoer voor een knippen filter is een JSON-object met de volgende eigenschappen:

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

Om in te dienen de REST-aanroep voor het maken van de dynamisch-manifestfilter, dienen de bijbehorende filter nettolading met behulp van de [REST-API](https://docs.microsoft.com/azure/media-services/media-services-rest-dynamic-manifest).