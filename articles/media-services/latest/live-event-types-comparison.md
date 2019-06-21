---
title: Azure Media Services LiveEvent typen | Microsoft Docs
description: In dit artikel bevat een uitgebreide tabel waarin LiveEvent typen vergeleken.
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
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 93f01513841d1174fea796f1615ab05df0a41af4
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150387"
---
# <a name="live-event-types-comparison"></a>Vergelijking van de typen Live gebeurtenis

In Azure Media Services, een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) kan bestaan uit een van de twee typen: live codering en Pass Through-query. 

## <a name="types-comparison"></a>Typen vergelijking 

De volgende tabel vergelijkt de functies van de Live gebeurtenis typen. De typen zijn ingesteld tijdens het maken met behulp van [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType.None** - An on-premises live encoder sends a multiple bitrate stream. De opgenomen streams wordt doorgegeven via de Live gebeurtenis zonder verdere verwerking. 
* **LiveEventEncodingType.Standard** : een on-premises live codering verzendt een single-bitrate stream aan de Live gebeurtenis en Media Services wordt gemaakt van meerdere bitrate streams. Als de feed van de bijdrage van 720p of hogere resolutie, is de **Default720p** definitie wordt een set 6 resolutie/bitrate paren (meer informatie volgt later in dit artikel) coderen.
* **LiveEventEncodingType.Premium1080p** : een on-premises live codering verzendt een single-bitrate stream aan de Live gebeurtenis en Media Services wordt gemaakt van meerdere bitrate streams. De definitie Default1080p Hiermee geeft u de uitvoer set resolutie/bitrate paren (meer informatie volgt later in dit artikel). 

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

De oplossingen en bitrates die is opgenomen in de uitvoer van het live coderingsprogramma worden bepaald door de [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding). Als een **Standard** live coderingsprogramma (LiveEventEncodingType.Standard), dan zal de *Default720p* vooraf ingestelde geeft een reeks van 6 resolutie/bitrate-paren die hieronder worden beschreven. Als u met behulp van een **Premium1080p** live coderingsprogramma (LiveEventEncodingType.Premium1080p), dan zal de *Default1080p* definitie Hiermee geeft u de uitvoer set van oplossing/bitrate-paren.

> [!NOTE]
> U kunt de Default1080p vooraf ingesteld op een Live gebeurtenis als deze is ingesteld voor live codering standaard: u een foutmelding krijgt niet toepassen. U krijgt een foutmelding als u probeert toe te passen van de vooraf ingesteld op een live coderingsprogramma Premium1080p Default720p.

### <a name="output-video-streams-for-default720p"></a>Video aan de uitvoerstromen voor Default720p

Als de feed van de bijdrage van 720p of hogere resolutie, is de **Default720p** definitie wordt de feed in de volgende 6 lagen coderen. In de onderstaande tabel is de Bitrate in kbps, MaxFPS vertegenwoordigt die maximale toegestane framesnelheid (in frames per seconde), profiel vertegenwoordigt de H.264-profiel dat wordt gebruikt.

| Bitrate | Breedte | Hoogte | MaxFPS | Profiel |
| --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoog |
| 2200 |960 |540 |30 |Hoog |
| 1350 |704 |396 |30 |Hoog |
| 850 |512 |288 |30 |Hoog |
| 550 |384 |216 |30 |Hoog |
| 200 |340 |192 |30 |Hoog |

> [!NOTE]
> Als u nodig hebt om aan te passen van de live voorinstelling voor encoding, opent u een ondersteuningsticket via Azure Portal. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag is op 720 p, en maximaal zes lagen. Ook moet u opgeven dat u hebt een vooraf ingestelde aangevraagd voor een live codering-standaard.
> De specifieke waarden van de bitsnelheden en oplossingen kunnen worden aangepast na verloop van tijd

### <a name="output-video-streams-for-default1080p"></a>Video aan de uitvoerstromen voor Default1080p

Als de feed van de bijdrage van 1080p resolutie, is de **Default1080p** definitie wordt de feed in de volgende 6 lagen coderen.

| Bitrate | Breedte | Hoogte | MaxFPS | Profiel |
| --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Hoog |
| 3000 |1280 |720 |30 |Hoog |
| 1600 |960 |540 |30 |Hoog |
| 800 |640 |360 |30 |Hoog |
| 400 |480 |270 |30 |Hoog |
| 200 |320 |180 |30 |Hoog |

> [!NOTE]
> Als u nodig hebt om aan te passen van de live voorinstelling voor encoding, opent u een ondersteuningsticket via Azure Portal. Geef de gewenste tabel met resoluties en bitrates op. Controleer of er slechts één laag bij 1080p en maximaal 6 lagen. Ook moet u opgeven dat u een definitie voor een live coderingsprogramma Premium1080p aanvraagt.
> De specifieke waarden van de bitsnelheden en oplossingen kunnen na verloop van tijd worden aangepast.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Uitvoer Audio Stream voor Default720p en Default1080p

Voor beide *Default720p* en *Default1080p* presets, audio naar stereo AAC-LC op 128 k is gecodeerd. De samplefrequentie volgt die van het audiospoor in de bijdrage van de feed.

## <a name="implicit-properties-of-the-live-encoder"></a>Impliciete eigenschappen van het live coderingsprogramma

De vorige sectie beschrijft de eigenschappen van het live coderingsprogramma die expliciet, kunnen worden beheerd via de definitie -, zoals het aantal lagen, oplossingen en bitrates. In deze sectie wordt uitleg gegeven over de impliciete eigenschappen.

### <a name="group-of-pictures-gop-duration"></a>Groep van de duur van de afbeeldingen (GOP)

Het live coderingsprogramma volgt de [GOP](https://en.wikipedia.org/wiki/Group_of_pictures) structuur van de bijdrage feed - wat betekent dat de uitvoer-lagen hebben dezelfde GOP duur. Daarom is het aanbevolen dat u de on-premises coderingsprogramma te produceren van een bijdrage feed vaste GOP duur (meestal 2 seconden) configureren. Dit zorgt ervoor dat de uitgaande HLS en MPEG-DASH-streams van de service is verholpen, ook GOP duur. Kleine variaties faciliteren voor de duur van GOP waarschijnlijk worden toegestaan door de meeste apparaten.

### <a name="frame-rate"></a>Framesnelheid

Het live coderingsprogramma volgt ook de duur van de afzonderlijke videoframes in de feed - bijdrage wat betekent dat de uitvoer-lagen heeft frames met de dezelfde duur. Daarom wordt aanbevolen dat u de on-premises coderingsprogramma te produceren van een bijdrage feed vaste framesnelheid configureert (maximaal 30 frames per seconde). Dit zorgt ervoor dat de uitgaande HLS en MPEG-DASH-streams van de service ook is verholpen, frame tarieven duur. Kleine variaties in framesnelheden kunnen worden toegestaan door de meeste apparaten, maar er is geen garantie dat het live coderingsprogramma levert een uitvoer die worden afgespeeld. Uw on-premises live coderingsprogramma moet niet worden beelden (bv. voorwaarden van de batterij laag) of de framesnelheid op geen enkele manier wijzigen.

### <a name="resolution-of-contribution-feed-and-output-layers"></a>Resolutie van de bijdrage van invoer en uitvoer lagen

Het live coderingsprogramma is geconfigureerd om te voorkomen dat upconverting bijdrage van de feed. Als gevolg hiervan de maximale resolutie van de uitvoer-lagen wordt niet langer zijn dan die van de bijdrage feed.

Bijvoorbeeld, als u een bijdrage op 720p-kanaal verzenden een Live gebeurtenis geconfigureerd voor Default1080p live codering, de uitvoer heeft alleen 5 lagen, beginnend met 720p op 3 Mbps, Ga omlaag naar 1080 p op 200 kbps. Of als u een bijdrage aan 360p-kanaal verzenden in een Live gebeurtenis geconfigureerd voor Standard live codering, de uitvoer bevat 3 lagen (met een resolutie van 288p, 216p en 192p). In het geval losstaande als u een bijdrage-feed van 160 x 90 pixels bijvoorbeeld naar een live codering-standaard, stuurt bevat de uitvoer één laag resolutie van 160 x 90 op de dezelfde bitrate als die van de bijdrage van de feed.

### <a name="bitrate-of-contribution-feed-and-output-layers"></a>Bitrate van de bijdrage van invoer en uitvoer lagen

Het live coderingsprogramma is geconfigureerd om te voldoen aan de bitrate-instellingen in de vooraf gedefinieerde instellingen, ongeacht de bitrate van de bijdrage feed. Als gevolg hiervan de bitrate van de uitvoer-lagen kan groter zijn dan die van de bijdrage feed. Bijvoorbeeld, als u in een feed met een resolutie van 720p met 1 Mbps bijdrage verzendt, de uitvoer-lagen blijven hetzelfde als in de [tabel](live-event-types-comparison.md#output-video-streams-for-default720p) hierboven.

## <a name="next-steps"></a>Volgende stappen

[Overzicht van live streaming](live-streaming-overview.md)
