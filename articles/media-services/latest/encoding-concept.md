---
title: Codering in de cloud met Azure Media Services | Microsoft Docs
description: In dit onderwerp beschrijft de codering proces als u Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/21/2018
ms.author: juliako
ms.openlocfilehash: e1c7536c59b110ae3dd753ff5f4b01195f8dadca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655166"
---
# <a name="encoding-with-azure-media-services"></a>Codering met Azure mediaservices

Azure Media Services kunt u uw digitale mediabestanden van hoge kwaliteit coderen in indelingen die op een groot aantal verschillende browsers en apparaten kunnen worden afgespeeld. U kan bijvoorbeeld stroom indelingen van uw inhoud in de Apple HLS of MPEG DASH wilt. Media Services kunt u bij het analyseren van uw video of audio-inhoud. In dit onderwerp geven richtlijnen over hoe uw inhoud codeert met Media Services v3.

Als u wilt coderen met Media Services v3, moet u een transformatie en een taak maken. Een transformatie definieert het recept voor de instellingen voor codering en de uitvoer en de taak is een exemplaar van het recept. Zie voor meer informatie [transformeert en taken](transform-concept.md)

Wanneer met Azure Media Services-codering, gebruikt u de standaardinstellingen de encoder uitgelegd hoe de invoer media-bestanden moeten worden verwerkt. U kunt bijvoorbeeld de beeldschermresolutie en/of het aantal audio kanalen die u wilt opgeven in de gecodeerde inhoud. 

U kunt snel aan de slag met een aanbevolen ingebouwde definities op basis van de beste praktijken of u kunt kiezen voor het bouwen van een aangepaste voorinstelling voor uw specifieke vereisten voor scenario of het apparaat zijn gericht. 

Meer informatie over het coderingsprogramma in vinden de [OpenAPI specificatie](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2018-03-30-preview). 

## <a name="built-in-presets"></a>Ingebouwde standaardinstellingen

Media Services ondersteunt momenteel de volgende ingebouwde codering standaardinstellingen:  

|**Vooraf ingestelde naam**|**Scenario**|**Details**|
|---|---|---|
|**AudioAnalyzerPreset**|Audio analyseren|De definitie van een vooraf gedefinieerde set analyse op basis van AI-bewerkingen, inclusief spraak schrijffouten toepassing. De vooraf ingestelde ondersteunt momenteel, verwerking van inhoud met een één-nummer.<br/>U kunt de taal voor de nettolading van de audio in de invoer met de indeling van de BCP-47 van 'taal tag-land' opgeven (bijvoorbeeld ' en-US'). De lijst met ondersteunde talen zijn, en-US, en-GB, es-ES, es-MX, fr-FR, it-IT, ja-JP, pt-BR, zh-CN.|
|**VideoAnalyzerPreset**|Audio- en analyseren|Insights (rijke metagegevens) wordt geëxtraheerd uit de audio en video opnemen, en levert als bestand met een JSON-indeling. U kunt opgeven of alleen uit te pakken audio insights bij het verwerken van een videobestand. Zie voor meer informatie [analyseren video](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Streaming|Gebruikt voor het instellen van een vooraf ingesteld voor het coderen van de invoer video met de codering-standaard ingebouwde. <br/>De volgende standaardinstellingen worden momenteel ondersteund:<br/>**EncoderNamedPreset.AdaptiveStreaming** (aanbevolen). Zie voor meer informatie [automatisch genereren van een ladder bitrate](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -produceert één MP4-bestand met alleen stereogeluid gecodeerd op 192 kbps.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -levert een set van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en aansluiting AAC audio. Resolutie begint bij 1080p en gaat naar beneden 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -levert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en aansluiting AAC audio. Resolutie begint bij 720p en gaat naar beneden 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -levert een set van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en aansluiting AAC audio. Resolutie begint bij 480p en gaat naar beneden 360 p.<br/><br/>Zie voor meer informatie [Uploading, coderen en streamen bestanden](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Streaming|Beschrijving van de instellingen moet worden gebruikt wanneer de invoer video met de codering-standaard-codering. <br/>Gebruik deze definitie bij het aanpassen van de transformatie standaardinstellingen. Zie voor meer informatie [het aanpassen van de transformatie standaardinstellingen](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Aangepaste standaardinstellingen

Media Services biedt volledige ondersteuning voor het aanpassen van alle waarden in de standaardinstellingen om te voldoen aan uw specifieke behoeften voor codering en vereisten. U gebruikt de **StandardEncoderPreset** vooraf ingesteld bij het aanpassen van de transformatie standaardinstellingen. Voor een gedetailleerde uitleg en bijvoorbeeld Zie [het aanpassen van het coderingsprogramma standaardinstellingen](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Codering in v3 schalen

Op dit moment kunnen klanten hebben met de Azure portal of AMS v2 API's RUs ingesteld (zoals beschreven in [media verwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Volgende stappen

### <a name="tutorials"></a>Zelfstudies

De volgende tutorals laten zien hoe u uw inhoud codeert met Media Services:

* [Uploaden, coderen en streamen met Azure Media Services](stream-files-tutorial-with-api.md)
* [Video's met Azure Media Services analyseren](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Codevoorbeelden

De volgende codevoorbeelden bevatten code die laat zien hoe coderen met Media Services:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [CLI 2.0](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDK's

U kunt een van de volgende ondersteunde Media Services v3 SDK's gebruiken voor het coderen van uw inhoud.

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

