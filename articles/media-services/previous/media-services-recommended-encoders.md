---
title: Meer informatie over coderingsprogramma's die zijn aanbevolen door Azure Media Services | Microsoft Docs
description: Meer informatie over coderingsprogramma's die zijn aanbevolen door mediaservices
services: media-services
keywords: codering; coderingsprogramma's; media
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d0c5536d2339470eac058250cc14e1f250b86d90
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="recommended-on-premises-encoders"></a>Aanbevolen on-premises coderingsprogramma 's
Wanneer een live streamen met Azure Media Services, kunt u opgeven hoe u uw kanaal voor het ontvangen van de invoerstroom wilt. Als u een on-premises coderingsprogramma gebruiken met een live codering kanaal kiest, moet het coderingsprogramma een hoge kwaliteit single-bitrate stream pushen als uitvoer. Als u een on-premises coderingsprogramma gebruiken met een pass through-kanaal kiest, moet het coderingsprogramma een multi-bitrate stream pushen als uitvoer met alle gewenste uitvoer kenmerken. Zie voor meer informatie [Live streamen met on-premises coderingsprogramma's](media-services-live-streaming-with-onprem-encoders.md).

Azure Media Services raadt het gebruik van een van de volgende live coderingsprogramma's die RTMP als uitvoer hebt:
- Adobe Flash-Media Live coderingsprogramma 3.2
- Haivision Makito X HEVC
- Telestream Wirecast 8.1
- Teradek segment 756
- TriCaster 8000
- HD Tricaster Mini-4

Azure Media Services raadt het gebruik van een van de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming als uitvoer hebt:
- Ateme TITAN Live
- Cisco digitale Media Encoder 2200
- Live elemental
- Envivio 4Caster C4 Gen III
- Stel communicatie Selenio MCP3
- Media Excel Hero Live

> [!NOTE]
> Een live coderingsprogramma een single-bitrate stream naar een pass through-kanaal kunt verzenden, maar deze configuratie wordt niet aanbevolen omdat het niet mogelijk voor adaptive bitrate streaming aan de client.

## <a name="how-to-become-an-on-prem-encoder-partner"></a>Hoe u een on-premises coderingsprogramma partner
Als Azure Media Services on-premises coderingsprogramma partner energiestatus Media Services van uw product door het coderingsprogramma aanbeveelt voor enterprise-klanten. Om te worden van een on-premises coderingsprogramma partner, moet u controleren of uw on-premises coderingsprogramma compatibel is met Media Services. Voer de volgende verificaties om dit te doen:

Kanaal verificatie doorgeven
1. Maken of Ga naar uw Azure Media Services-account
2. Maak en start een **pass-through** kanaal
3. Het coderingsprogramma dat een multi-bitrate live stream push configureren.
4. Een gepubliceerde live gebeurtenis maken
5. Het live coderingsprogramma ongeveer 10 minuten worden uitgevoerd
6. De live gebeurtenis stoppen
7. Noteer de Asset-ID, gepubliceerd streaming-URL voor het live archief en de instellingen en de versie van het live coderingsprogramma gebruikt
8. De kanaal-status opnieuw instellen na het maken van elk voorbeeld
9. Herhaal stap 3 tot en met 8 voor alle configuraties worden ondersteund door de encoder (met en zonder ad-signalering/bijschriften/verschillende snelheden codering)

De verificatie van kanaal voor Live codering
1. Maken of Ga naar uw Azure Media Services-account
2. Maak en start een **live codering** kanaal
3. Het coderingsprogramma dat een single-bitrate livestream push configureren.
4. Een gepubliceerde live gebeurtenis maken
5. Het live coderingsprogramma ongeveer 10 minuten worden uitgevoerd
6. De live gebeurtenis stoppen
7. Noteer de Asset-ID, gepubliceerd streaming-URL voor het live archief en de instellingen en de versie van het live coderingsprogramma gebruikt
8. De kanaal-status opnieuw instellen na het maken van elk voorbeeld
9. Herhaal stap 3 tot en met 8 voor alle configuraties worden ondersteund door de encoder (met en zonder ad-signalering/bijschriften/verschillende snelheden codering)

Verificatie van de levensduur
1. Maken of Ga naar uw Azure Media Services-account
2. Maak en start een **pass-through** kanaal
3. Het coderingsprogramma dat een multi-bitrate live stream push configureren.
4. Een gepubliceerde live gebeurtenis maken
5. Het live coderingsprogramma uitvoeren voor één week of langer
6. De live gebeurtenis stoppen
7. Noteer de Asset-ID, gepubliceerd streaming-URL voor het live archief en de instellingen en de versie van het live coderingsprogramma gebruikt

Ten slotte de instellingen van uw geregistreerde verzenden en live archief parameters met Media Services door een e-mail amsstreaming@microsoft.com. Bij ontvangst voert Media Services verificatietests op de voorbeelden van het live coderingsprogramma. Neem contact op met de Media Services met vragen met betrekking tot dit proces.