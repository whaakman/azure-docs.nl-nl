---
title: Codering in de cloud met Media Services - Azure | Microsoft Docs
description: In dit onderwerp beschrijft het coderingsproces bij het gebruik van Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/10/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 25b3209bed98ea217db9e414caa6f08cee6d8c89
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65761888"
---
# <a name="encoding-with-media-services"></a>Codering met mediaservices

De term codering in Media Services is van toepassing op het proces van het converteren van bestanden met digitale video-en/of audio van een standard-indeling naar een ander doel te (a) het verkleinen van de bestanden en/of (b) produceert een indeling die compatibel is met een breed scala aan apparaten en toepassingen. Dit proces wordt ook wel video compressie of transcodering genoemd. Zie de [gegevenscompressie](https://en.wikipedia.org/wiki/Data_compression) en de [Wat Is er codering en -transcodering?](https://www.streamingmedia.com/Articles/Editorial/What-Is-/What-Is-Encoding-and-Transcoding-75025.aspx) voor verdere bespreking van de concepten.

Video's worden doorgaans geleverd aan apparaten en toepassingen door [progressief downloaden](https://en.wikipedia.org/wiki/Progressive_download) of via [streaming met adaptieve bitrates](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming). 

* Om te leveren via progressief downloaden, kunt u Azure Media Services gebruiken om te converteren een uw digitale media-bestand (tussentijds) in een [MP4](https://en.wikipedia.org/wiki/MPEG-4_Part_14) -bestand met de video die is gecodeerd met de [H.264](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC) verbeterde codecs, en audio die is gecodeerd met de [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) codec. Deze MP4-bestand wordt geschreven naar een activum in uw storage-account. Kunt u de Azure Storage-API's of de SDK's (bijvoorbeeld [REST API voor Storage](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md), of [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) het bestand rechtstreeks downloaden. Als u de uitvoer gemaakt Asset met de naam van een specifieke container in opslag, gebruikt u die locatie. Anders kunt u Media Services kunt [de URL van de container asset](https://docs.microsoft.com/rest/api/media/assets/listcontainersas). 
* Om voor te bereiden inhoud voor de levering van streaming met adaptieve bitrates, moet het tussentijdse bestand worden gecodeerd in meerdere bitsnelheden (hoog tot laag). Om ervoor te zorgen correcte overgang van de kwaliteit, zoals de bitrate wordt verlaagd, zodat u de resolutie van de video is. Dit resulteert in een zogenaamde codering ladder: een tabel met oplossingen en bitrates (Zie [automatisch gegenereerde adaptieve bitrateladder](autogen-bitrate-ladder.md)). Met Media Services kunt u uw tussentijds-bestanden in meerdere bitsnelheden – in dat geval coderen, krijgt u een set MP4-bestanden en bijbehorende streaming configuratiebestanden, naar een activum in uw storage-account geschreven. Vervolgens kunt u de [dynamische pakketten](dynamic-packaging-overview.md) -mogelijkheid in Media Services leveren van de video via het streaming-protocollen, zoals [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) en [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming). Hiervoor moet u maakt een [Streaming-Locator gemaakt](streaming-locators-concept.md) en streaming-URL's die overeenkomt met de ondersteunde protocollen kunnen vervolgens worden doorgegeven aan apparaten/toepassingen op basis van hun mogelijkheden te bouwen.

Het volgende diagram toont de werkstroom voor het op aanvraag coderen met dynamisch verpakken.

![Dynamische verpakking](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

In dit onderwerp vindt u richtlijnen over hoe u uw inhoud codeert met Media Services v3.

## <a name="transforms-and-jobs"></a>Transformaties en taken

Als u wilt coderen met Media Services v3, moet u maken een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) en een [taak](https://docs.microsoft.com/rest/api/media/jobs). De transformatie definieert een recept voor de instellingen voor codering en de uitvoer; de taak is een exemplaar van het recept. Zie voor meer informatie, [transformaties en taken](transforms-jobs-concept.md)

Wanneer de codering met Media Services, gebruikt u voorinstellingen voor het coderingsprogramma vertellen hoe de invoer media-bestanden moeten worden verwerkt. U kunt bijvoorbeeld de video oplossing en/of het aantal audio kanalen die u wilt opgeven in de gecodeerde inhoud. 

U kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen op basis van aanbevolen procedures voor de bedrijfstak of u kunt kiezen om het bouwen van een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat. Zie voor meer informatie, [coderen met een aangepaste transformatie](customize-encoder-presets-how-to.md). 

Beginnend met januari 2019, bij het coderen met Media Encoder Standard produceren MP4-bestanden, een nieuwe .mpi-bestand is gegenereerd en toegevoegd aan de uitvoer Asset. Dit bestand MPI is bedoeld voor het verbeteren van de prestaties voor [dynamische verpakking](dynamic-packaging-overview.md) en streaming-scenario's.

> [!NOTE]
> U moet niet wijzigen of verwijderen van de MPI-bestand, of eventuele afhankelijkheden in uw service op het bestaan (of niet) van een dergelijk bestand.

## <a name="built-in-presets"></a>Ingebouwde voorinstellingen

Media Services ondersteunt momenteel de volgende ingebouwde coderingsstandaarden:  

### <a name="builtinstandardencoderpreset-preset"></a>Voorinstelling voor BuiltInStandardEncoderPreset

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) wordt gebruikt voor het instellen van een ingebouwde voorinstelling voor encoding van de invoervideo met de codering-standaard. 

De volgende voorinstellingen worden momenteel ondersteund:

- **EncoderNamedPreset.AACGoodQualityAudio** -produceert één MP4-bestand met alleen stereo audio gecodeerd met een 192 kbps.
- **EncoderNamedPreset.AdaptiveStreaming** (aanbevolen). Zie voor meer informatie, [automatisch een bitrateladder genereren](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.ContentAwareEncodingExperimental** -wordt aangegeven dat een experimentele voorinstelling voor het coderen van inhoud behouden. Opgegeven invoer materiaal, probeert de service om automatisch te bepalen het optimale aantal lagen, de juiste bitrate en de resolutie-instellingen voor de levering van adaptief streamen. De onderliggende algoritmen blijft loop der tijd veranderen. Uitvoer van de bevat MP4-bestanden met video en audio interleaved. Zie voor meer informatie, [Experimental voorinstelling voor encoding op inhoud-bewuste](cae-experimental.md).
- **EncoderNamedPreset.H264MultipleBitrate1080p** -produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 1080p en gaat naar de 360 p.
- **EncoderNamedPreset.H264MultipleBitrate720p** -produceert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 720p en gaat naar de 360 p.
- **EncoderNamedPreset.H264MultipleBitrateSD** -produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 480p en gaat naar de 360 p.
- **EncoderNamedPreset.H264SingleBitrate1080p** -produceert een MP4-bestand waarin de video is gecodeerd met H.264 codec 6750 kbps en de hoogte van een afbeelding van 1080 pixels en de stereo audio is gecodeerd met AAC-LC codec met 64 kbps.
- **EncoderNamedPreset.H264SingleBitrate720p** -produceert een MP4-bestand waarin de video is gecodeerd met H.264 codec 4500 kbps en de hoogte van een afbeelding van 720 pixels en de stereo audio is gecodeerd met AAC-LC codec met 64 kbps.
- **EncoderNamedPreset.H264SingleBitrateSD** -produceert een MP4-bestand waarin de video is gecodeerd met H.264 codec 2200 kbps en de hoogte van een afbeelding van 480 pixels en de stereo audio is gecodeerd met AAC-LC codec met 64 kbps.

Zie voor de meest recente lijst met voorinstellingen [ingebouwde voorinstellingen moet worden gebruikt voor het coderen van video's](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

Als u wilt zien hoe de standaardinstellingen worden gebruikt, Bekijk [uploaden, coderen en streamen van bestanden](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Voorinstelling voor StandardEncoderPreset

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) beschrijft instellingen moet worden gebruikt wanneer de invoervideo met de codering-standaard-codering. Gebruik deze definitie bij het aanpassen van voorinstellingen voor transformatie. 

#### <a name="considerations"></a>Overwegingen

Bij het maken van aangepaste voorinstellingen, de volgende overwegingen zijn van toepassing:

- Alle waarden voor de hoogte en breedte in AVC inhoud moet een meervoud van 4.
- In Azure Media Services v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit wijkt af van het voorbeelddiagram met onze v2 API's, dat kilobits per seconde als de eenheid gebruikt. Bijvoorbeeld, als de bitrate in v2 is opgegeven als 128 (kilobits per seconde), zou in v3 deze worden ingesteld op 128000 (bits per seconde).

#### <a name="examples"></a>Voorbeelden

Media Services biedt volledige ondersteuning voor het aanpassen van alle waarden in de standaardinstellingen om te voldoen aan uw specifieke behoeften voor codering en vereisten. Zie voor voorbeelden die laten zien hoe het coderingsprogramma voorinstellingen aanpassen:

- [Voorinstellingen met .NET aanpassen](customize-encoder-presets-how-to.md)
- [Voorinstellingen met CLI aanpassen](custom-preset-cli-howto.md)
- [Voorinstellingen met REST aanpassen](custom-preset-rest-howto.md)

## <a name="preset-schema"></a>Vooraf ingestelde schema

Voorinstellingen zijn in Media Services v3, sterk getypeerde entiteiten in de API zelf. U vindt de definitie van de 'schema' voor deze objecten in [Open API-specificatie (of Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). U kunt ook de vooraf ingestelde definities weergeven (zoals **StandardEncoderPreset**) in de [REST-API](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset), [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (of andere SDK-referentiedocumentatie voor Media Services v3).

## <a name="scaling-encoding-in-v3"></a>Encoding in v3 schalen

Als u wilt mediaverwerking schalen, Zie [schaal met CLI](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

* [Coderen in een HTTPS-URL met behulp van ingebouwde voorinstellingen](job-input-from-http-how-to.md)
* [Een lokaal bestand met behulp van ingebouwde voorinstellingen coderen](job-input-from-local-file-how-to.md)
* [Bouw een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat](customize-encoder-presets-how-to.md)
* [Uploaden, coderen en streamen met Media Services](stream-files-tutorial-with-api.md)
