---
title: Meer informatie over coderings Programma's die worden aanbevolen door Azure Media Services | Microsoft Docs
description: Meer informatie over coderings Programma's die worden aanbevolen door Media Services
services: media-services
keywords: code ring; encoders; media
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 03/20/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: fc481129e652c6dacd15a5a6d039a9118393e8f1
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854247"
---
# <a name="recommended-on-premises-encoders"></a>Aanbevolen on-premises coderingsprogramma's
Wanneer u live streamt met Azure Media Services, kunt u opgeven hoe u wilt dat uw kanaal de invoer stroom ontvangt. Als u kiest voor het gebruik van een on-premises Encoder met een live encoding-kanaal, moet uw encoder een single-bitrate stream van hoge kwaliteit pushen als uitvoer. Als u kiest voor het gebruik van een on-premises Encoder met een Pass-Through kanaal, moet uw encoder een multi-bitrate stream als uitvoer met alle gewenste uitvoer kwaliteiten pushen. Zie [live streamen met on](media-services-live-streaming-with-onprem-encoders.md)-premises encoders voor meer informatie.

Azure Media Services raadt u aan om een van de volgende Live coderings Programma's te gebruiken die RTMP als uitvoer hebben:
- Adobe Flash Media Live Encoder 3.2
- Haivision Makito X HEVC
- Haivision KB
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Switcher Studio (iOS)

Azure Media Services raadt u aan om een van de volgende Live coderings Programma's te gebruiken met gefragmenteerde multi-bitrate (Smooth Streaming) als uitvoer:
- Media Excel Hero Live en Hero 4K (UHD/HEVC)
- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Denk aan de communicatie selenio MCP3

> [!NOTE]
> Een live coderings programma kan een single-bitrate stream verzenden naar een Pass-Through-kanaal, maar deze configuratie wordt niet aanbevolen omdat de client niet is toegestaan voor Adaptive Bitrate Streaming.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Hoe kan ik een on-premises coderings partner worden?
Als Azure Media Services on-premises encoder-partner Media Services propageert u uw product door uw Codeer aan te bevelen voor zakelijke klanten. Als u een on-premises encoder partner wilt worden, moet u de compatibiliteit van uw on-premises encoder controleren met Media Services. Voer hiervoor de volgende verificaties uit:

Kanaal verificatie door geven
1. Uw Azure Media Services-account maken of bezoeken
2. Een **Pass-Through-** kanaal maken en starten
3. Configureer uw encoder om een multi-bitrate Live Stream te pushen.
4. Een gepubliceerde live-gebeurtenis maken
5. Voer uw Live coderings programma uit voor ongeveer 10 minuten
6. De live-gebeurtenis stoppen
7. Maak, start een streaming-eind punt, gebruik een speler zoals [Azure Media Player](https://aka.ms/azuremediaplayer) om het gearchiveerde activum te bekijken om ervoor te zorgen dat afspelen geen zicht bare storingen heeft voor alle kwaliteits niveaus (of Bekijk en valideer en controleer deze via de Preview-URL tijdens de Live sessie vóór stap 6)
8. Registreer de Asset-ID, de gepubliceerde streaming-URL voor het Live Archief en de instellingen en versie die worden gebruikt vanuit uw Live coderings programma
9. De kanaal status opnieuw instellen na het maken van elk voor beeld
10. Herhaal stap 3 t/m 9 voor alle configuraties die worden ondersteund door uw coderings programma (met en zonder AD-Signa lering/bijschriften/verschillende coderings snelheden)

Verificatie van Live encoding-kanaal
1. Uw Azure Media Services-account maken of bezoeken
2. Een **Live** coderings kanaal maken en starten
3. Configureer uw encoder om een live stream met één bitsnelheid te pushen.
4. Een gepubliceerde live-gebeurtenis maken
5. Voer uw Live coderings programma uit voor ongeveer 10 minuten
6. De live-gebeurtenis stoppen
7. Maak, start een streaming-eind punt, gebruik een speler zoals [Azure Media Player](https://aka.ms/azuremediaplayer) om het gearchiveerde activum te bekijken om ervoor te zorgen dat afspelen geen zicht bare storingen heeft voor alle kwaliteits niveaus (of Bekijk en valideer en controleer deze via de Preview-URL tijdens de Live sessie vóór stap 6)
8. Registreer de Asset-ID, de gepubliceerde streaming-URL voor het Live Archief en de instellingen en versie die worden gebruikt vanuit uw Live coderings programma
9. De kanaal status opnieuw instellen na het maken van elk voor beeld
10. Herhaal stap 3 t/m 9 voor alle configuraties die worden ondersteund door uw coderings programma (met en zonder AD-Signa lering/bijschriften/verschillende coderings snelheden)

Duurzaamheids verificatie
1. Uw Azure Media Services-account maken of bezoeken
2. Een **Pass-Through-** kanaal maken en starten
3. Configureer uw encoder om een multi-bitrate Live Stream te pushen.
4. Een gepubliceerde live-gebeurtenis maken
5. Voer uw Live coderings programma uit voor een week of langer
6. Gebruik een speler zoals [Azure Media Player](https://aka.ms/azuremediaplayer) om de live-streaming van tijd tot tijd (of gearchiveerde Asset) te bekijken om ervoor te zorgen dat het afspelen geen zicht bare storingen heeft
7. De live-gebeurtenis stoppen
8. Registreer de Asset-ID, de gepubliceerde streaming-URL voor het Live Archief en de instellingen en versie die worden gebruikt vanuit uw Live coderings programma

Verzend ten slotte uw vastgelegde instellingen en de para meters van uw live amsstreaming@microsoft.com-archief naar Media Services door e-mail berichten. Na ontvangst voert Media Services verificatie tests uit op de voor beelden van uw Live coderings programma. U kunt contact opnemen met de Media Services met vragen over dit proces.
