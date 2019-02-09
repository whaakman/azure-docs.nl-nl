---
title: Live streaming coderingsprogramma's die zijn aanbevolen door Media Services - Azure | Microsoft Docs
description: Meer informatie over live streaming on-premises coderingsprogramma's aanbevolen door Media Services
services: media-services
keywords: codering; coderingsprogramma's; media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 01/17/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: a165fac2de89d5510b21b9185d4bc61e730b09ff
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960119"
---
# <a name="recommended-live-streaming-encoders"></a>Aanbevolen live streaming-coderingsprogramma 's

In Azure Media Services, een [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) (kanaal) vertegenwoordigt een pijplijn voor het verwerken van live-streaming-inhoud. De Live gebeurtenis ontvangt live invoerstromen op twee manieren.

* Een on-premises live codering verzendt een multi-bitrate RTMP of Smooth Streaming (gefragmenteerde MP4) naar de Live gebeurtenis dat niet is ingeschakeld voor live coderen met Media Services streamen. De opgenomen streams doorgegeven via Live-gebeurtenissen zonder verdere verwerking. Deze methode wordt aangeroepen **pass-through**. Een live coderingsprogramma kan een single-bitrate stream naar een Pass Through-kanaal verzenden. Deze configuratie wordt niet aanbevolen omdat deze is niet toegestaan voor adaptive bitrate streaming aan de client.

  > [!NOTE]
  > Met behulp van een passthrough-methode is de meest voordelige manier om live te streamen.

* Een on-premises live codering verzendt een single-bitrate stream naar de Live gebeurtenis dat is ingeschakeld voor live coderen met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). De Live gebeurtenis voert live codering van de binnenkomende single-bitrate stream naar een multi-bitrate (adaptieve) videostream.

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

## <a name="live-encoders-that-output-fragmented-mp4"></a>Live coderingsprogramma's die gefragmenteerde MP4-uitvoer

Media Services raadt het gebruik van een van de volgende live coderingsprogramma's die multi-bitrate Smooth Streaming (gefragmenteerde MP4) als uitvoer. De ondersteunde URL-schema's zijn `http://` of `https://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 ALG III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live en Hero 4K (UHD/HEVC)

## <a name="configuring-on-premises-live-encoder-settings"></a>Configureren on-premises live coderingsprogramma instellingen

Zie voor meer informatie over welke instellingen geldig voor uw live-gebeurtenistype zijn [Live gebeurtenis van het type vergelijking](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Vereisten voor afspelen

Als u wilt afspelen van inhoud, moet zowel een stream audio en video aanwezig zijn. Het afspelen van de video alleen-lezen stroom wordt niet ondersteund.

### <a name="configuration-tips"></a>Tips voor de configuratie

- Gebruik indien mogelijk een internetverbinding ' hardwired '.
- Wanneer u bepaalt bandbreedtevereisten, dubbele de streaming bitsnelheden. Hoewel dit niet verplicht is, kunt u deze eenvoudige regel beperken de gevolgen van opstoppingen in het netwerk.
- Wanneer u coderingsprogramma's op basis van software, sluit u alle onnodige programma's.
- De configuratie van het coderingsprogramma wijzigen nadat deze is gestart pushen heeft negatieve gevolgen voor de gebeurtenis. Wijzigingen in de configuratie kunnen leiden tot de gebeurtenis instabiel. 
- Zorg ervoor dat u zelf voldoende tijd geeft voor het instellen van de gebeurtenis. Gebeurtenissen op grote schaal wordt u aangeraden de installatie van een uur voordat de gebeurtenis te starten.

## <a name="becoming-an-on-premises-encoder-partner"></a>U een on-premises coderingsprogramma partner

Als een Azure Media Services on-premises coderingsprogramma partner verhogen Media Services uw product door uw encoder aanbeveelt voor zakelijke klanten. Een on-premises coderingsprogramma als partner wilt worden, moet u controleren of de compatibiliteit van uw on-premises-codering met Media Services. Voer de volgende verificaties om dit te doen.

### <a name="pass-through-live-event-verification"></a>Pass through-verificatie voor Live gebeurtenis

1. In Media Services-account, zorg ervoor dat de **Streaming-eindpunt** wordt uitgevoerd. 
2. Maak en start de **pass-through** Live gebeurtenis. <br/> Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
3. De opname-URL's ophalen en configureert u uw on-premises coderingsprogramma voor het gebruik van de URL voor het verzenden van een multi-bitrate live stream met Media Services.
4. De voorbeeld-URL ophalen en deze gebruiken om te controleren dat de invoer van het coderingsprogramma daadwerkelijk worden ontvangen.
5. Maak een nieuwe **Asset** object.
6. Maak een **uitvoer Live** en gebruikt u de assetnaam van de die u hebt gemaakt.
7. Maak een **Streaming-Locator gemaakt** met de ingebouwde **beleid Streaming** typen.
8. Lijst van de paden op de **Streaming-Locator gemaakt** om terug te gaan de URL's te gebruiken.
9. De naam van de host voor de **Streaming-eindpunt** die u wilt streamen uit.
10. De URL in stap 8 worden gecombineerd met de naam van de host in stap 9 om op te halen van de volledige URL.
11. Voer het live coderingsprogramma voor ongeveer 10 minuten.
12. Stop de livegebeurtenis. 
13. Gebruik een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) om te bekijken van de gearchiveerde asset om ervoor te zorgen dat afspelen heeft geen zichtbare haperingen op alle kwaliteitsniveaus. Of, bekijken en te valideren via de voorbeeld-URL tijdens de live-sessie.
14. Noteer de asset-ID, de gepubliceerde streaming-URL voor het livearchief en de instellingen en de versie die wordt gebruikt vanuit uw live coderingsprogramma.
15. De status van de Live gebeurtenis opnieuw instellen na het maken van elk voorbeeld.
16. Herhaal stap 5 tot en met 15 voor alle configuraties die worden ondersteund door uw encoder (met en zonder de ad-signalering, bijschriften of verschillende snelheden voor codering).

### <a name="live-encoding-live-event-verification"></a>Live encoding Live gebeurtenis-verificatie

1. In Media Services-account, zorg ervoor dat de **Streaming-eindpunt** wordt uitgevoerd. 
2. Maak en start de **van realtime codering** Live gebeurtenis. <br/> Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
3. De opname-URL's ophalen en configureren uw encoder als u wilt een single-bitrate live stream pushen met Media Services.
4. De voorbeeld-URL ophalen en deze gebruiken om te controleren dat de invoer van het coderingsprogramma daadwerkelijk worden ontvangen.
5. Maak een nieuwe **Asset** object.
6. Maak een **uitvoer Live** en gebruikt u de assetnaam van de die u hebt gemaakt.
7. Maak een **Streaming-Locator gemaakt** met de ingebouwde **beleid Streaming** typen.
8. Lijst van de paden op de **Streaming-Locator gemaakt** om terug te gaan de URL's te gebruiken.
9. De naam van de host voor de **Streaming-eindpunt** die u wilt streamen uit.
10. De URL in stap 8 worden gecombineerd met de naam van de host in stap 9 om op te halen van de volledige URL.
11. Voer het live coderingsprogramma voor ongeveer 10 minuten.
12. Stop de livegebeurtenis.
13. Gebruik een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) om te bekijken van de gearchiveerde asset om ervoor te zorgen dat afspelen heeft geen haperingen zichtbaar voor alle kwaliteitsniveaus. Of, bekijken en te valideren via de voorbeeld-URL tijdens de live-sessie.
14. Noteer de asset-ID, de gepubliceerde streaming-URL voor het livearchief en de instellingen en de versie die wordt gebruikt vanuit uw live coderingsprogramma.
15. De status van de Live gebeurtenis opnieuw instellen na het maken van elk voorbeeld.
16. Herhaal stap 5 tot en met 15 voor alle configuraties die worden ondersteund door uw encoder (met en zonder de ad-signalering, bijschriften of verschillende snelheden voor codering).

### <a name="longevity-verification"></a>Duurzaamheid verificatie

Volg dezelfde stappen als in [Live gebeurtenis van Pass through-verificatie](#pass-through-live-event-verification) , met uitzondering van stap 11. <br/>In plaats van 10 minuten, het live coderingsprogramma 1 week voor een of meer worden uitgevoerd. Gebruik een speler zoals [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) om te bekijken van de live streaming van tijd naar een keer (of een gearchiveerde asset) om ervoor te zorgen dat afspelen heeft geen zichtbare storingen.

### <a name="email-your-recorded-settings"></a>E-instellingen van uw opgenomen

Ten slotte, e-instellingen van uw opgenomen en archive-parameters voor Azure Media Services op live amsstreaming@microsoft.com als een melding dat alle zelf verificatiecontroles is verstreken. Bovendien zijn uw contactgegevens voor elke communityleden reageert. U kunt contact opnemen met het Azure Media Services-team met vragen over dit proces.

## <a name="next-steps"></a>Volgende stappen

[Live streamen met Media Services v3](live-streaming-overview.md)
