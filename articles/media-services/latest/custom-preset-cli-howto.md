---
title: Codeer aangepaste transformeren met behulp van Media Services v3 CLI - Azure | Microsoft Docs
description: In dit onderwerp laat zien hoe Azure Media Services v3 gebruiken om een aangepaste transformeren met behulp van CLI.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: b0329c8c0bfa56e5ae36c5f4223edca91e6510a0
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755605"
---
# <a name="how-to-encode-with-a-custom-transform---cli"></a>Coderen met een aangepaste transformatie - CLI

Wanneer u met Azure Media Services encoding, u kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen, gebaseerd op best practices, zoals geïllustreerd in de [Streaming bestanden](stream-files-cli-quickstart.md#create-a-transform-for-adaptive-bitrate-encoding) Quick Start. U kunt ook een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat maken.

## <a name="considerations"></a>Overwegingen

Bij het maken van aangepaste voorinstellingen, de volgende overwegingen zijn van toepassing:

* Alle waarden voor de hoogte en breedte in AVC inhoud moet een meervoud van 4.
* In Azure Media Services v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit wijkt af van het voorbeelddiagram met onze v2 API's, dat kilobits per seconde als de eenheid gebruikt. Bijvoorbeeld, als de bitrate in v2 is opgegeven als 128 (kilobits per seconde), zou in v3 deze worden ingesteld op 128000 (bits per seconde).

## <a name="prerequisites"></a>Vereisten 

[Een Azure Media Services-account maken](create-account-cli-how-to.md). <br/>Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="define-a-custom-preset"></a>Een aangepaste voorinstelling definiëren

Het volgende voorbeeld wordt de hoofdtekst van de aanvraag van een nieuwe transformatie gedefinieerd. We definiëren van een groep van de uitvoer die we worden gegenereerd willen wanneer deze transformatie wordt gebruikt. 

In dit voorbeeld we eerst een AacAudio-laag voor de audio codering en twee H264Video lagen toevoegen voor de videocodering. In de video lagen toewijzen we labels, zodat ze kunnen worden gebruikt in de namen van de uitvoer. Vervolgens gaan we de uitvoer naar alle miniaturen. In het onderstaande voorbeeld geven we afbeeldingen in PNG-indeling, die worden gegenereerd op 50% van de omzetting van de invoervideo en op drie tijdstempels - {% 25, 50%, 75} van de lengte van de invoervideo. Tot slot geven we de indeling voor de uitvoerbestanden: één voor video en audio, en een andere voor de miniaturen. Aangezien we meerdere H264Layers hebben, hebben we het gebruik van macro's die unieke namen per laag produceren. Kunnen we gebruiken een `{Label}` of `{Bitrate}` macro, in het voorbeeld ziet u de vorige.

We gaan deze transformatie niet opslaan in een bestand. In dit voorbeeld wordt het bestand een naam `customPreset.json`. 

```json
{
    "properties": {
        "description": "Basic Transform using a custom encoding preset",
        "outputs": [
            {
                "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                "preset": {
                    "@odata.type": "#Microsoft.Media.StandardEncoderPreset",
                    "codecs": [
                        {
                            "@odata.type": "#Microsoft.Media.AacAudio",
                            "channels": 2,
                            "samplingRate": 48000,
                            "bitrate": 128000,
                            "profile": "AacLc"
                        },
                        {
                            "@odata.type": "#Microsoft.Media.H264Video",
                            "keyFrameInterval": "PT2S",
                            "stretchMode": "AutoSize",
                            "sceneChangeDetection": false,
                            "complexity": "Balanced",
                            "layers": [
                                {
                                    "width": "1280",
                                    "height": "720",
                                    "label": "HD",
                                    "bitrate": 3400000,
                                    "maxBitrate": 3400000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                },
                                {
                                    "width": "640",
                                    "height": "360",
                                    "label": "SD",
                                    "bitrate": 1000000,
                                    "maxBitrate": 1000000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                }
                            ]
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngImage",
                            "stretchMode": "AutoSize",
                            "start": "25%",
                            "step": "25%",
                            "range": "80%",
                            "layers": [
                                {
                                    "width": "50%",
                                    "height": "50%"
                                }
                            ]
                        }
                    ],
                    "formats": [
                        {
                            "@odata.type": "#Microsoft.Media.Mp4Format",
                            "filenamePattern": "Video-{Basename}-{Label}{Extension}",
                            "outputFiles": []
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngFormat",
                            "filenamePattern": "Thumbnail-{Basename}-{Index}{Extension}"
                        }
                    ]
                }
            }
        ]
    }
}

```

## <a name="create-a-new-transform"></a>Maak een nieuwe transformatie  

In dit voorbeeld maken we een **transformeren** die is gebaseerd op de aangepaste voorinstelling die we eerder hebt gedefinieerd. Bij het maken van een transformatie, moet u eerst controleren als er al bestaat. Als de transformatie bestaat, dit opnieuw gebruiken. De volgende `show` opdracht retourneert de `customTransformName` transformeren als deze bestaat:

```cli
az ams transform show -a amsaccount -g amsResourceGroup -n customTransformName
```

De volgende CLI-opdracht maakt de transformatie op basis van de aangepaste voorinstelling (eerder hebt gedefinieerd). 

```cli
az ams transform create -a amsaccount -g amsResourceGroup -n customTransformName --description "Basic Transform using a custom encoding preset" --preset customPreset.json
```

Voor Media Services toe te passen van de transformatie op de opgegeven video of audio, moet u een taak onder deze transformatie verzenden. Zie voor een compleet voorbeeld die laat zien hoe u een taak onder een transformatie verzenden, [Quick Start: Stream-video's - CLI](stream-files-cli-quickstart.md).

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
