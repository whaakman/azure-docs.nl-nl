---
title: Codering-standaard in Azure Media Services gebruiken voor het coderen van video's met een automatisch gegenereerde bitrate ladder | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe de codering-standaard in Media Services gebruiken voor het coderen van invoer video met een automatisch gegenereerde bitrate ladder, op basis van de invoer resolutie en de bitsnelheid. De invoer resolutie en bitrate wordt nooit worden overschreden. Bijvoorbeeld, als de invoer is 720p op 3 Mbps, uitvoer wordt met de beste 720p blijven en wordt gestart volgens de tarieven voor lager is dan 3 Mbps.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 5d13c711d7d71df7469e6408ce78cf0df611632b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Met een automatisch gegenereerde bitrate ladder coderen

## <a name="overview"></a>Overzicht

Dit artikel wordt uitgelegd hoe u de codering-standaard in Media Services met een invoer video coderen in een automatisch gegenereerde bitrate ladder (bitrate resolutie paren) op basis van de invoer resolutie en de bitsnelheid. Deze ingebouwde encoder instellen of voorinstelling wordt nooit overschrijden de invoer resolutie en de bitsnelheid. Bijvoorbeeld, als de invoer 720p met 3 Mbps is, uitvoer blijft 720p met de beste en volgens de tarieven voor lager is dan 3 Mbps wordt gestart.

### <a name="encoding-for-streaming"></a>Codering voor streaming

Als de naam wordt voorgesteld, als u de **AdaptiveStreaming** vooraf ingesteld bij het maken van een codering transformatie, ontvangt u uitvoer die geschikt is voor levering via streaming protocollen zoals HLS, DASH, CMAF enzovoort. Wanneer u dit **AdaptiveStreaming** definitie het coderingsprogramma op intelligente wijze bepaalt hoeveel video lagen te genereren en in welke bitrate en -resolutie. De uitvoer van de Asset bevat waar AAC gecodeerde audio en video H.264-codering is niet interleaved MP4-bestanden.

Zie voor een voorbeeld van hoe deze definitie wordt gebruikt, [Stream van een bestand](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Uitvoer

Deze sectie ziet u drie voorbeelden van de video lagen van uitvoer geproduceerd door de encoder Media Services als gevolg van codering met de **AdaptiveStreaming** vooraf ingestelde. In alle gevallen wordt de uitvoer alleen audio MP4-bestand bevatten met stereogeluid gecodeerd met 128 kbps.

### <a name="example-1"></a>Voorbeeld 1
Bron met hoogte '1080' en '29.970' framesnelheid produceert 6 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Voorbeeld 2
Bron met hoogte '720' en '23.970' framesnelheid produceert 5 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Voorbeeld 3
Bron met hoogte '360' en '29.970' framesnelheid produceert 3 video lagen:

|Laag|Hoogte|Breedte|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand Stream](stream-files-dotnet-quickstart.md)
