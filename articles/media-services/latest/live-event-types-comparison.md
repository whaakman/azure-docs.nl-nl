---
title: Azure Media Services LiveEvent typen | Microsoft Docs
description: In dit artikel bevat een uitgebreide tabel die LiveEvent typen vergelijken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: bd4899374c06246ddd4d5fa81d0f6e3a6a1e7017
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075009"
---
# <a name="live-event-types-comparison"></a>Vergelijking van de typen Live gebeurtenis

In Azure Media Services, een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: live codering en Pass Through-query. 

## <a name="types-comparison"></a>Typen vergelijking 

De volgende tabel vergelijkt de functies van de Live gebeurtenis typen.

| Functie | Pass Through-Live-gebeurtenis | Standard of Premium1080p Live-gebeurtenis |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale beeldschermresolutie voor bijdrage feed |4K (4096 x 2160 op 60 frames per seconde) |1080p (1920 x 1088 op 30 frames per seconde)|
| Aanbevolen maximale lagen in bijdrage feed|Maximaal 12|Een audio|
| Maximum aantal lagen in de uitvoer| Hetzelfde als invoer|Maximaal 6 (Zie onderstaande systeemwaarden)|
| Maximale cumulatieve bandbreedte van de bijdrage van feed|60 Mbps|N/A|
| Maximale bitrate voor één laag van de bijdrage |20 Mbps|20 Mbps|
| Ondersteuning voor meerdere talen audionummers|Ja|Nee|
| Ondersteunde invoer videocodecs |H.264/AVC en H.265/HEVC|H.264/AVC|
| Ondersteunde uitvoer videocodecs|Hetzelfde als invoer|H.264/AVC|
| Ondersteunde diepte van de video bits, invoer en uitvoer|Maximaal 10-bits inclusief Kopregel 10/HLG|8-bit|
| Ondersteunde invoer audio-codecs|AAC-LC, hij AAC v1, v2 HE-AAC|AAC-LC, hij AAC v1, v2 HE-AAC|
| Ondersteunde uitvoer audio-codecs|Hetzelfde als invoer|AAC-LC|
| Maximale beeldschermresolutie van uitvoervideo|Hetzelfde als invoer|Standard - 720p, Premium1080p - 1080p|
| Maximale framesnelheid van invoervideo|60 frames per seconde|Standard of Premium1080p - 30 frames per seconde|
| Invoer-protocollen|RTMP, gefragmenteerde MP4 (Smooth Streaming)|RTMP, gefragmenteerde MP4 (Smooth Streaming)|
| Prijs|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|
| Maximale uitvoeringstijd| 24 uur x 365 dagen live lineaire | Maximaal 24 uur|
| Mogelijkheid om door te geven tot en met ingesloten CEA 608/708 bijschriften gegevens|Ja|Ja|
| Ondersteuning voor slates invoegen|Nee|Nee|
| Ondersteuning voor ad via de API-signalering| Nee|Nee|
| Ondersteuning voor ad-signalering via SCTE 35 in-band-berichten|Ja|Ja|
| Mogelijkheid om te herstellen van korte vertragingen in bijdrage feed|Ja|Gedeeltelijke|
| Ondersteuning voor niet-uniforme invoer GOPs|Ja|Nee – invoer GOP duur moet opgelost|
| Ondersteuning voor variabele frame tarief invoer|Ja|Nee – moet dat invoer framesnelheid worden opgelost. Kleine variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar de feed bijdrage kan de framesnelheid niet verwijderen (bijvoorbeeld tot en met 15 frames per seconde).|
| Automatisch-signalen van Live gebeurtenis als invoer-kanaal is verbroken|Nee|Na 12 uur, als er geen LiveOutput uitgevoerd|

## <a name="system-presets"></a>Systeemwaarden

De oplossingen en bitrates die is opgenomen in de uitvoer van het live coderingsprogramma wordt bepaald door de [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding). Als een **Standard** live coderingsprogramma (LiveEventEncodingType.Standard), dan zal de *Default720p* vooraf ingestelde geeft een reeks van 6 resolutie/bits tarief paren die hieronder worden beschreven. Als u met behulp van een **Premium1080p** live coderingsprogramma (LiveEventEncodingType.Premium1080p), dan zal de *Default1080p* vooraf ingestelde specifiesthe uitvoer set paren van oplossing/bits-tarief. 

### <a name="output-video-streams-for-default720p"></a>Video aan de uitvoerstromen voor Default720p

**Default720p** wordt de invoervideo coderen in de volgende 6 lagen.

| BitRate | Breedte | Hoogte | MaxFPS | Profiel | Naam van de uitvoer-Stream |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoog |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Hoog |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Hoog |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Hoog |Video_512x288_850kbps |
| 550 |384 |216 |30 |Hoog |Video_384x216_550kbps |
| 200 |340 |192 |30 |Hoog |Video_340x192_200kbps |

> [!NOTE]
> Als u nodig hebt om aan te passen van de live voorinstelling voor encoding, opent u een ondersteuningsticket via Azure Portal. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag is op 720 p, en maximaal zes lagen. Ook moet u opgeven dat u hebt een vooraf ingestelde aangevraagd voor een live codering-standaard.

### <a name="output-video-streams-for-default1080p"></a>Video aan de uitvoerstromen voor Default1080p

**Default1080p** wordt de invoervideo coderen in de volgende 6 lagen.

| BitRate | Breedte | Hoogte | MaxFPS | Profiel | Naam van de uitvoer-Stream |
| --- | --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Hoog |Video_1920x1080_5500kbps |
| 3000 |1280 |720 |30 |Hoog |Video_1280x720_3000kbps |
| 1600 |960 |540 |30 |Hoog |Video_960x540_1600kbps |
| 800 |640 |360 |30 |Hoog |Video_640x360_800kbps |
| 400 |480 |270 |30 |Hoog |Video_480x270_400kbps |
| 200 |320 |180 |30 |Hoog |Video_320x180_200kbps |

> [!NOTE]
> Als u nodig hebt om aan te passen van de live voorinstelling voor encoding, opent u een ondersteuningsticket via Azure Portal. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag bij 1080p en maximaal 6 lagen. Ook moet u opgeven dat u een definitie voor een live coderingsprogramma Premium1080p aanvraagt.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Uitvoer Audio Stream voor Default720p en Default1080p

Voor beide *Default720p* en *Default1080p* presets, audio wordt gecodeerd naar stereo AAC-LC op 128 k frequentie van 48 kHz steekproeven.

## <a name="next-steps"></a>Volgende stappen

[Overzicht van live streaming](live-streaming-overview.md)
