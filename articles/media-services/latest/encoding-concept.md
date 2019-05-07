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
ms.date: 04/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 216eae383c704125cd32d9ed4cb1309299af7336
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153401"
---
# <a name="encoding-with-media-services"></a>Codering met mediaservices

Azure Media Services kunt u uw digitale media van hoge kwaliteit bestanden coderen in adaptive bitrate MP4-bestanden, zodat uw inhoud kan worden afgespeeld op een groot aantal browsers en apparaten. Een geslaagde Media Services encoding-taak maakt uitvoer activa met een set adaptive bitrate MP4s en streaming-configuratiebestanden. De configuratiebestanden bevatten ISM, .ismc .mpi en andere bestanden die u moet niet worden gewijzigd. Zodra de coderingstaak is voltooid, kunt u profiteren van [dynamische verpakking](dynamic-packaging-overview.md) en streamen.

Om video's in de uitvoer van de Asset die beschikbaar zijn voor clients om te worden afgespeeld, die u moet maken een **Streaming-Locator gemaakt** en streaming-URL's te bouwen. Klik, op basis van de indeling die is opgegeven in het manifest, uw clients de stream ontvangt in het protocol dat ze hebben gekozen.

Het volgende diagram toont de streaming on demand met dynamische verpakking werkstroom.

![Dynamische verpakking](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

In dit onderwerp vindt u richtlijnen over hoe u uw inhoud codeert met Media Services v3.

## <a name="transforms-and-jobs"></a>Transformaties en taken

Als u wilt coderen met Media Services v3, moet u maken een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) en een [taak](https://docs.microsoft.com/rest/api/media/jobs). Een transformatie definieert het recept voor de instellingen voor codering en de uitvoer en de taak is een exemplaar van het recept. Zie voor meer informatie, [transformaties en taken](transforms-jobs-concept.md)

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

## <a name="scaling-encoding-in-v3"></a>Encoding in v3 schalen

Als u wilt mediaverwerking schalen, Zie [schaal met CLI](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

* [Coderen in een HTTPS-URL met behulp van ingebouwde voorinstellingen](job-input-from-http-how-to.md)
* [Een lokaal bestand met behulp van ingebouwde voorinstellingen coderen](job-input-from-local-file-how-to.md)
* [Bouw een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat](customize-encoder-presets-how-to.md)
* [Uploaden, coderen en streamen met Media Services](stream-files-tutorial-with-api.md)
