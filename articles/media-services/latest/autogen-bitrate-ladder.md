---
title: De codering-standaard in Azure Media Services gebruiken om te coderen, video's met behulp van een automatisch gegenereerde bitrateladder | Microsoft Docs
description: In dit onderwerp laat zien hoe de codering-standaard in Media Services gebruiken om te coderen invoer video met een automatisch gegenereerde bitrateladder, op basis van de invoerresolutie en de bitsnelheid. De invoerresolutie en bitrate wordt nooit worden overschreden. Bijvoorbeeld, als de invoer 720p op 3 Mbps, uitvoer wordt met de beste 720p blijven, en tarieven lager is dan 3 Mbps wordt gestart.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: ec1b4b88e5b9639c3ee9debbd8ac7d48544344dc
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378955"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Coderen met een automatisch gegenereerde bitrateladder

## <a name="overview"></a>Overzicht

In dit artikel wordt uitgelegd hoe u de codering-standaard in Media Services gebruiken een invoervideo coderen in een automatisch gegenereerde bitrateladder (bitrate-resolutie paren) op basis van de invoerresolutie en de bitsnelheid. Deze ingebouwde encoder instellen of vooraf gedefinieerde instellingen, wordt nooit groter zijn dan de invoerresolutie en de bitsnelheid. Bijvoorbeeld, als de invoer 720p met 3 Mbps is, uitvoer blijft 720p met de beste en tarieven lager is dan 3 Mbps wordt gestart.

### <a name="encoding-for-streaming"></a>Voor het streaming-codering

Wanneer u gebruikt de **AdaptiveStreaming** vooraf ingestelde in **transformeren**, krijgt u uitvoer die geschikt is voor levering via streaming-protocollen, zoals HLS en DASH. Wanneer u deze definitie, de service op intelligente wijze bepaalt hoeveel video lagen voor het genereren van en op welke bitrate en de resolutie. De uitvoerinhoud bevat waar AAC-gecodeerd audio en video H.264 gecodeerde is niet interleaved MP4-bestanden.

Zie voor een voorbeeld van hoe deze definitie wordt gebruikt, [Stream van een bestand](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Uitvoer

In deze sectie ziet u drie voorbeelden van de uitvoer van een video lagen die worden geproduceerd door de Media Services encoder als gevolg van de codering met de **AdaptiveStreaming** vooraf ingestelde. In alle gevallen bevat de uitvoer alleen audio MP4-bestand met stereo audio gecodeerd met een 128 kbps.

### <a name="example-1"></a>Voorbeeld 1
Bron met hoogte "1080" en "29.970" framesnelheid produceert 6 video lagen:

|Laag|Hoogte|Breedte|Bitrate (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Voorbeeld 2
Bron met hoogte "720" en "23.970" framesnelheid produceert 5 video lagen:

|Laag|Hoogte|Breedte|Bitrate (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Voorbeeld 3
Bron met hoogte "360" en "29.970" framesnelheid produceert 3 video lagen:

|Laag|Hoogte|Breedte|Bitrate (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand streamen](stream-files-dotnet-quickstart.md)
