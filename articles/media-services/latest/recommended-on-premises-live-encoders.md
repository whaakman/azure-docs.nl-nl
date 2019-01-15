---
title: Meer informatie over live streaming on-premises coderingsprogramma's aanbevolen door Media Services - Azure | Microsoft Docs
description: Meer informatie over live streaming on-premises coderingsprogramma's aanbevolen door Media Services
services: media-services
keywords: codering; coderingsprogramma's; media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 12/14/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 4d25e4cd94fec35f31594544b619aa054a35d58d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302337"
---
# <a name="recommended-live-streaming-encoders"></a>Aanbevolen live streaming-coderingsprogramma 's

In Media Services, een [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) (kanaal) vertegenwoordigt een pijplijn voor het verwerken van live-streaming-inhoud. Een LiveEvent ontvangt live invoerstromen op twee manieren:

* Een on-premises live codering verzendt een multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) naar de LiveEvent dat niet is ingeschakeld voor live coderen met Media Services streamen. De opgenomen streams passeren LiveEvents zonder verdere verwerking. Deze methode wordt aangeroepen **pass-through**. Een live coderingsprogramma kan een single-bitrate stream naar een pass through-kanaal verzenden, maar deze configuratie wordt niet aanbevolen omdat deze niet is toegestaan voor adaptive bitrate streaming aan de client.

  > [!NOTE]
  > Met behulp van een passthrough-methode is de meest voordelige manier om live te streamen.

* Een on-premises live codering verzendt een single-bitrate stream naar het LiveEvent dat is ingeschakeld voor live coderen met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). De LiveEvent voert live codering van de binnenkomende single-bitrate stream naar een multi-bitrate (adaptieve) videostream.

Zie voor gedetailleerde informatie over live coderen met Media Services, [Live streamen met Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Live coderingsprogramma's die voeren RTMP uit

Media Services raadt het gebruik van een van de volgende live coderingsprogramma's waarvoor RTMP als uitvoer. De ondersteunde URL-schema's zijn `rtmp://` of `rtmps://`.

> [!NOTE]
 > Bij het streamen via RTMP, Controleer de firewall en/of proxy-instellingen om te bevestigen dat uitgaande TCP-poorten 1935 en 1936 geopend zijn.<br/>
 Bij het streamen via RTMPS, Controleer de firewall en/of proxy-instellingen om te bevestigen dat uitgaande TCP-poorten 2935 en 2936 geopend zijn.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Overschakelen naar Studio (iOS)
- Telestream Wirecast 8.1 +
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Live coderingsprogramma's die gefragmenteerd MP4-uitvoer

Media Services raadt het gebruik van een van de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming (gefragmenteerde MP4) als uitvoer. De ondersteunde URL-schema's zijn `http://` of `https://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 ALG III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live en Hero 4K (UHD/HEVC)

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Hoe u een on-premises coderingsprogramma partner

Als een Azure Media Services on-premises coderingsprogramma partner verhogen Media Services uw product door uw encoder aanbeveelt voor zakelijke klanten. Een on-premises coderingsprogramma als partner wilt worden, moet u controleren of de compatibiliteit van uw on-premises-codering met Media Services. Voer de volgende verificaties om dit te doen:

### <a name="pass-through-liveevent-verification"></a>Pass Through-LiveEvent verificatie

1. Maak of Ga naar uw Azure Media Services-account.
2. Maak en start een **pass-through** LiveEvent.
3. Configureer uw encoder als u wilt een multi-bitrate live stream pushen.
4. Een gepubliceerde live gebeurtenis maken.
5. Voer het live coderingsprogramma voor ongeveer 10 minuten.
6. Stop de live-gebeurtenis.
7. Maken, een Streaming-eindpunt starten, gebruikt u een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) bekijken van de gearchiveerde asset om ervoor te zorgen dat afspelen heeft geen haperingen zichtbaar voor alle kwaliteitsniveaus (of u kunt ook bekijken en valideren via de voorbeeld-URL tijdens de live-sessie vóór stap 6).
8. Noteer de Asset-ID, streaming-URL voor het livearchief en de instellingen en de versie die wordt gebruikt vanuit uw live coderingsprogramma gepubliceerd.
9. De LiveEvent-status opnieuw instellen na het maken van elk voorbeeld.
10. Herhaal stap 3 t/m 9 voor alle configuraties ondersteund door uw encoder (met en zonder ad-signalering/bijschriften/verschillende snelheden codering).

### <a name="live-encoding-liveevent-verification"></a>LiveEvent verificatie van realtime codering

1. Maak of Ga naar uw Azure Media Services-account.
2. Maak en start een **van realtime codering** LiveEvent.
3. Configureer uw encoder als u wilt een single-bitrate live stream pushen.
4. Een gepubliceerde live gebeurtenis maken.
5. Voer het live coderingsprogramma voor ongeveer 10 minuten.
6. Stop de live-gebeurtenis.
7. Maken, een Streaming-eindpunt starten, gebruikt u een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) bekijken van de gearchiveerde asset om ervoor te zorgen dat afspelen heeft geen haperingen zichtbaar voor alle kwaliteitsniveaus (of u kunt ook bekijken en valideren via de voorbeeld-URL tijdens de live-sessie vóór stap 6).
8. Noteer de Asset-ID, streaming-URL voor het livearchief en de instellingen en de versie die wordt gebruikt vanuit uw live coderingsprogramma gepubliceerd.
9. De LiveEvent-status opnieuw instellen na het maken van elk voorbeeld.
10. Herhaal stap 3 t/m 9 voor alle configuraties ondersteund door uw encoder (met en zonder ad-signalering/bijschriften/verschillende snelheden codering).

### <a name="longevity-verification"></a>Duurzaamheid verificatie

1. Maak of Ga naar uw Azure Media Services-account.
2. Maak en start een **pass-through** kanaal.
3. Configureer uw encoder als u wilt een multi-bitrate live stream pushen.
4. Een gepubliceerde live gebeurtenis maken.
5. Voer het live coderingsprogramma 1 week voor een of meer.
6. Gebruik een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) om te bekijken van de live streaming van tijd naar een keer (of gearchiveerde asset) om ervoor te zorgen dat afspelen heeft geen zichtbare storingen.
7. Stop de live-gebeurtenis.
8. Noteer de Asset-ID, streaming-URL voor het livearchief en de instellingen en de versie die wordt gebruikt vanuit uw live coderingsprogramma gepubliceerd.

Ten slotte, e-instellingen van uw opgenomen en archive-parameters voor Azure Media Services op live amsstreaming@microsoft.com als een melding dat alle zelf verificatiecontroles is verstreken. Ook uw contactgegevens voor elke volgende-ups. U kunt contact opnemen met het Azure Media Services-team met vragen met betrekking tot dit proces.

## <a name="next-steps"></a>Volgende stappen

[Live streamen met Media Services v3](live-streaming-overview.md)
