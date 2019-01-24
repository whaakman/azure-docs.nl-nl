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
ms.date: 01/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d236f00e70e08c7bce2a94c5bd4fb64f1fa99bbc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826595"
---
# <a name="encoding-with-media-services"></a>Codering met mediaservices

Azure Media Services kunt u uw digitale media van hoge kwaliteit bestanden coderen naar indelingen die kunnen worden afgespeeld op een groot aantal browsers en apparaten. Zo kunt u bijvoorbeeld inhoud streamen in de indelingen Apple HLS of MPEG DASH. In dit onderwerp vindt u richtlijnen over hoe u uw inhoud codeert met Media Services v3.

Als u wilt coderen met Media Services v3, moet u een transformatie en een taak maken. Een transformatie definieert het recept voor de instellingen voor codering en de uitvoer en de taak is een exemplaar van het recept. Zie voor meer informatie, [transformaties en taken](transform-concept.md)

Wanneer de codering met Media Services, gebruikt u voorinstellingen voor het coderingsprogramma vertellen hoe de invoer media-bestanden moeten worden verwerkt. U kunt bijvoorbeeld de video oplossing en/of het aantal audio kanalen die u wilt opgeven in de gecodeerde inhoud. 

U kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen op basis van aanbevolen procedures voor de bedrijfstak of u kunt kiezen om het bouwen van een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat. Zie voor meer informatie, [coderen met een aangepaste transformatie](customize-encoder-presets-how-to.md). 

Beginnend met januari 2019, bij het coderen met Media Encoder Standard produceren MP4-bestanden, een nieuwe .mpi-bestand is gegenereerd en toegevoegd aan de uitvoer Asset. Dit bestand MPI is bedoeld om de prestaties verbeteren voor dynamische verpakking en streaming scenario's.

> [!NOTE]
> U moet niet wijzigen of verwijderen van de MPI-bestand, of eventuele afhankelijkheden in uw service op het bestaan (of niet) van een dergelijk bestand.

## <a name="built-in-presets"></a>Ingebouwde voorinstellingen

Media Services ondersteunt momenteel de volgende ingebouwde coderingsstandaarden:  

|**Vooraf ingestelde naam**|**Scenario**|**Details**|
|---|---|---|
|**BuiltInStandardEncoderPreset**|Streaming|Gebruikt voor het instellen van een ingebouwde voorinstelling voor encoding van de invoervideo met de codering-standaard. <br/>De volgende voorinstellingen worden momenteel ondersteund:<br/>**EncoderNamedPreset.AdaptiveStreaming** (aanbevolen). Zie voor meer informatie, [automatisch een bitrateladder genereren](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -produceert één MP4-bestand met alleen stereo audio gecodeerd met een 192 kbps.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 1080p en gaat naar de 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -produceert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 720p en gaat naar de 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en stereo AAC-audio. Resolutie begint bij 480p en gaat naar de 360 p.<br/><br/>Zie voor meer informatie, [uploaden, coderen en streamen van bestanden](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Streaming|Beschrijving van de instellingen moet worden gebruikt wanneer de invoervideo met de codering-standaard-codering. <br/>Gebruik deze definitie bij het aanpassen van voorinstellingen voor transformatie. Zie voor meer informatie, [over het aanpassen van de transformatie voorinstellingen](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Aangepaste voorinstellingen

Media Services biedt volledige ondersteuning voor het aanpassen van alle waarden in de standaardinstellingen om te voldoen aan uw specifieke behoeften voor codering en vereisten. U gebruikt de **StandardEncoderPreset** vooraf ingesteld bij het aanpassen van voorinstellingen voor transformatie. Voor een gedetailleerde uitleg en voorbeeld Zie [over het aanpassen van het coderingsprogramma voorinstellingen](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Encoding in v3 schalen

Op dit moment moeten klanten de Azure portal of Media Services v2-API's gebruiken om in te stellen ru's (zoals beschreven in [mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Volgende stappen

### <a name="tutorials"></a>Zelfstudies

De volgende zelfstudie laat zien hoe uw inhoud codeert met Media Services:

* [Uploaden, coderen en streamen met Media Services](stream-files-tutorial-with-api.md)

### <a name="code-samples"></a>Codevoorbeelden

De volgende codevoorbeelden bevatten code die laat zien hoe u coderen met Media Services:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [Azure-CLI](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDK's

U kunt een van de volgende ondersteunde Media Services v3-SDK's uw inhoud codeert.

* [Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

