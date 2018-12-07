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
ms.date: 11/27/2018
ms.author: juliako
ms.openlocfilehash: 2dbc2b4e316155e880d3baa900196d5cdd31be72
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011773"
---
# <a name="liveevent-types-comparison"></a>LiveEvent typen vergelijking

In Azure Media Services, een [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: live codering en Pass Through-query. 

## <a name="types-comparison"></a>Typen vergelijking 

De volgende tabel vergelijkt de functies van de twee LiveEvent typen.

| Functie | Pass Through-LiveEvent | Standard (standaard) LiveEvent |
| --- | --- | --- |
| Single-bitrate-invoer is gecodeerd in meerdere bitsnelheden in de cloud |Nee |Ja |
| Maximale beeldschermresolutie voor bijdrage feed |4K (4096 x 2160 op 60 frames per seconde) |1080p (1920 x 1088 op 30 frames per seconde)|
| Aanbevolen maximale lagen in bijdrage feed|Maximaal 12|Een audio|
| Maximum aantal lagen in de uitvoer| Hetzelfde als invoer|Maximaal 7|
| Maximale cumulatieve bandbreedte van de bijdrage van feed|60 Mbps|N/A|
| Maximale bitrate voor één laag van de bijdrage |20 Mbps|20 Mbps|
| Ondersteuning voor meerdere talen audionummers|Ja|Nee|
| Ondersteunde invoer videocodecs |H.264/AVC en H.265/HEVC|H.264/AVC|
| Ondersteunde uitvoer videocodecs|Hetzelfde als invoer|H.264/AVC|
| Ondersteunde diepte van de video bits, invoer en uitvoer|Maximaal 10-bits inclusief Kopregel 10/HLG|8-bits|
| Ondersteunde invoer audio-codecs|AAC-LC, hij AAC v1, v2 HE-AAC|AAC-LC, hij AAC v1, v2 HE-AAC|
| Ondersteunde uitvoer audio-codecs|Hetzelfde als invoer|AAC-LC|
| Maximale beeldschermresolutie van uitvoervideo|Hetzelfde als invoer|720p (op 30 frames per seconde)|
| Invoer-protocollen|RTMP, gefragmenteerde MP4 (Smooth Streaming)|RTMP, gefragmenteerde MP4 (Smooth Streaming)|
| Prijs|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/media-services/) en klik op het tabblad "Live Video"|
| Maximale uitvoeringstijd|24 x 365 live lineaire|24x7|
| Mogelijkheid om door te geven tot en met ingesloten CEA 608/708 bijschriften gegevens|Ja|Ja|
| Ondersteuning voor slates invoegen|Nee|Nee|
| Ondersteuning voor ad via de API-signalering| Nee|Nee|
| Ondersteuning voor ad-signalering via SCTE 35 in-band-berichten|Ja|Ja|
| Mogelijkheid om te herstellen van korte vertragingen in bijdrage feed|Ja|Nee (LiveEvent begint slating na 6 + seconden zonder invoergegevens)|
| Ondersteuning voor niet-uniforme invoer GOPs|Ja|Nee – invoer GOP duur moet opgelost|
| Ondersteuning voor variabele frame tarief invoer|Ja|Nee – moet dat invoer framesnelheid worden opgelost. Kleine variaties zijn toegestaan, bijvoorbeeld tijdens hoge beweging schermen. Maar de feed bijdrage kan de framesnelheid (bijvoorbeeld om 15 frames per seconde) niet verwijderen.|
| Automatisch-signalen van LiveEvent als invoer-kanaal is verbroken|Nee|Na 12 uur, als er geen LiveOutput uitgevoerd|

## <a name="next-steps"></a>Volgende stappen

[Overzicht van live streaming](live-streaming-overview.md)
