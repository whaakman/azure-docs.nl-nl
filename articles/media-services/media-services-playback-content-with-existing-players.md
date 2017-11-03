---
title: Bestaande spelers te spelen gebruiken de inhoud - Azure | Microsoft Docs
description: In dit onderwerp worden de bestaande spelers dat u uw inhoud voor het afspelen gebruiken kunt.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: 48f373b013b1192c353352b801876d706d91dd28
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="playing-your-content-with-existing-players"></a>Uw inhoud met bestaande spelers te spelen
Azure Media Services biedt ondersteuning voor veel populaire streaming-indelingen, zoals Smooth Streaming, HTTP Live Streaming en MPEG-Dash. In dit onderwerp verwijst u naar de bestaande spelers die u gebruiken kunt voor het testen van uw stromen.

### <a name="the-azure-portal-media-services-content-player"></a>De Azure portal Media Services content player
De **Azure** portal biedt een speler voor inhoud die u gebruiken kunt voor het testen van uw video.

Klik op de gewenste video (Zorg ervoor dat deze werd [gepubliceerd](media-services-portal-publish.md)) en klik op de **afspelen** knop aan de onderkant van de portal.

Hierbij geldt het volgende:

* **MEDIA SERVICES CONTENT PLAYER** speelt af vanaf het standaard streaming-eindpunt. Als u wilt afspelen vanaf een ander streaming-eindpunt, gebruikt u een andere speler. Bijvoorbeeld: [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Gebruik [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) afspelen uw inhoud (wissen of beveiligde) in een van de volgende indelingen:

* Smooth Streaming
* MPEG DASH
* HLS
* Progressieve MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES-versleuteling met een Token
[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight spelers
#### <a name="monitoring"></a>Bewaking
[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a>PlayReady met Token
[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH spelers
[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Overige
Testen HLS-URL's die u kunt ook gebruiken:

* **Safari** op een iOS-apparaat of
* **3ivx HLS Player** in Windows.

## <a name="developing-video-players"></a>Video spelers ontwikkelen
Zie voor meer informatie over het ontwikkelen van uw eigen spelers [video spelers ontwikkelen](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
