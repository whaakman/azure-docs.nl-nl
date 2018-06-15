---
title: Gids voor probleemoplossing voor live streamen | Microsoft Docs
description: Dit onderwerp bevat suggesties over het oplossen van problemen met live streaming.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 84e3e9fc18671d7199eeaf638377a6681cf09fb4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33940921"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Handleiding voor het oplossen van problemen met live streaming
Dit artikel bevat suggesties over het oplossen van enkele live streaming problemen.

## <a name="issues-related-to-on-premises-encoders"></a>Problemen met on-premises coderingsprogramma 's
Deze sectie vindt u suggesties over het oplossen van problemen met betrekking tot on-premises-coderingsprogramma's die zijn geconfigureerd voor het verzenden van een single-bitrate stream AMS-kanalen die zijn ingeschakeld voor live codering.

### <a name="problem-would-like-to-see-logs"></a>Probleem: Zou willen zien Logboeken
* **Potentiële probleem**: kan niet zoeken encoder logboeken die kan helpen bij het opsporen van problemen.
  
  * **Telestream Wirecast**: meestal vindt u Logboeken onder C:\Users\{username} \AppData\Roaming\Wirecast\ 
  * **Elementaire Live**: vindt u koppelingen naar Logboeken op de beheerportal heeft. Klik op **statistieken**, klikt u vervolgens **logboeken**. Op de **logboekbestanden** pagina ziet u een lijst van Logboeken voor alle items in de LiveEvent; de optie die overeenkomt met de huidige sessie. 
  * **Flash Live Mediacoderingsprogramma**: vindt u de **logboekmap...**  door te navigeren naar de **codering logboek** tabblad.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probleem: Er is geen optie voor het uitvoeren van een progressieve stream
* **Potentiële probleem**: de codering wordt gebruikt niet automatisch interliniëring ongedaan maken. 
  
    **Stappen voor probleemoplossing**: zoek naar een ongedaan ineengestrengeld optie binnen de interface van het coderingsprogramma. Zodra de-interlacing is ingeschakeld, Controleer of opnieuw progressief uitvoerinstellingen. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probleem: Er is geprobeerd verschillende instellingen voor codering uitvoer en kan nog steeds geen verbinding maken.
* **Potentiële probleem**: Azure codering kanaal is niet juist opnieuw ingesteld. 
  
    **Stappen voor probleemoplossing**: Zorg ervoor dat de codering wordt niet langer te worden gepusht AMS, stoppen en opnieuw instellen van het kanaal. Als opnieuw uit te voeren, probeert u het coderingsprogramma verbinding te maken met de nieuwe instellingen. Als dit nog niet het probleem wordt opgelost, probeert u een nieuw kanaal volledig te maken, soms kanalen beschadigd kunnen raken na een aantal mislukte pogingen.  
* **Potentiële probleem**: de GOP grootte of Sleutelframe instellingen zijn niet optimaal. 
  
    **Stappen voor probleemoplossing**: GOP aanbevolen grootte of keyframe interval is twee seconden. Sommige coderingsprogramma's berekenen deze instelling in het aantal frames, terwijl andere seconden. Bijvoorbeeld: bij het uitvoeren van 30 fps, de grootte van de GOP zou worden 60 frames die gelijk is aan twee seconden.  
* **Potentiële probleem**: de stroom gesloten poorten blokkeren. 
  
    **Stappen voor probleemoplossing**: bij het streamen via RTMP, Controleer de firewall en proxy-instellingen om te bevestigen dat de uitgaande poorten 1935 en 1936 geopend zijn. 

> [!NOTE]
> Verzenden als nadat u de stappen die u nog steeds niet met succes streamen, een ondersteuningsticket met de Azure portal.
> 
> 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

