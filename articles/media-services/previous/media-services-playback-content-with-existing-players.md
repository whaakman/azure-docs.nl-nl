---
title: Gebruik bestaande spelers afspelen uw inhoud - Azure | Microsoft Docs
description: In dit onderwerp een lijst met bestaande spelers dat u uw inhoud om af te spelen gebruiken kunt.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: c96710d6dcca9f5ef99b3a02a0bc875d433f814d
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58257428"
---
# <a name="playing-your-content-with-existing-players"></a>Uw inhoud met bestaande spelers afspelen
Azure Media Services biedt ondersteuning voor veel populaire streaming-indelingen, zoals Smooth Streaming, HTTP Live Streaming en MPEG-Dash. In dit onderwerp verwijst u naar bestaande spelers die u gebruiken kunt voor het testen van uw stromen.

### <a name="the-azure-portal-media-services-content-player"></a>De Azure portal Media Services-speler
De **Azure** portal biedt een speler die u gebruiken kunt om uw video te testen.

Klik op de gewenste video (Zorg ervoor dat er [gepubliceerd](media-services-portal-publish.md)) en klik op de **afspelen** knop aan de onderkant van de portal.

Hierbij geldt het volgende:

* **MEDIA SERVICES CONTENT PLAYER** speelt af vanaf het standaard streaming-eindpunt. Als u wilt afspelen vanaf een ander streaming-eindpunt, gebruikt u een andere speler. Bijvoorbeeld, [Azure Media Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Gebruik [Azure Media Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html) af te spelen uw inhoud (wissen of beveiligde) in een van de volgende indelingen:

* Smooth Streaming
* MPEG DASH
* HLS
* Progressieve MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES-versleuteling met een Token
[https://aestoken.azurewebsites.net](https://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Silverlight Players

#### <a name="playready-with-token"></a>PlayReady met Token
[https://sltoken.azurewebsites.net](https://sltoken.azurewebsites.net)

### <a name="dash-players"></a>DASH-spelers
[https://dashplayer.azurewebsites.net](https://dashplayer.azurewebsites.net)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Overige
Test HLS-URL's die u kunt ook gebruiken als volgt:

* **Safari** op een iOS-apparaat of
* **3ivx HLS Player** op Windows.

## <a name="developing-video-players"></a>Videospelers ontwikkelen
Zie voor meer informatie over het ontwikkelen van uw eigen spelers [videospelers ontwikkelen](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
