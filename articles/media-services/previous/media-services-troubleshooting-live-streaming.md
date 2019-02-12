---
title: Gids voor probleemoplossing voor live streamen | Microsoft Docs
description: In dit onderwerp vindt suggesties over het oplossen van problemen met live streaming.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 8bb48044fc827cd0d1dbc11ef3ec72ca1bdcb11a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997530"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Handleiding voor het oplossen van problemen met live streaming  

Dit artikel bevat suggesties over het oplossen van enkele problemen met live streaming.

## <a name="issues-related-to-on-premises-encoders"></a>Problemen met betrekking tot on-premises coderingsprogramma 's
In deze sectie biedt suggesties over het oplossen van problemen met betrekking tot on-premises coderingsprogramma's die zijn geconfigureerd voor het verzenden van een single-bitrate stream aan AMS-kanalen die zijn ingeschakeld voor live codering.

### <a name="problem-would-like-to-see-logs"></a>Probleem: Graag zou willen zien van Logboeken
* **Potentiële problemen**: Kan de encoder-logboeken die u kunnen helpen niet vinden in het opsporen van problemen.
  
  * **Telestream Wirecast**: U vindt meestal Logboeken onder C:\Users\{gebruikersnaam} \AppData\Roaming\Wirecast\ 
  * **Elemental Live**: U vindt bevat koppelingen naar Logboeken op de management portal. Klik op **statistieken**, klikt u vervolgens **logboeken**. Op de **logboekbestanden** pagina, ziet u een lijst van Logboeken voor alle items in de LiveEvent; de optie die overeenkomt met uw huidige sessie. 
  * **Flash Media Live Encoder**: U vindt de **logboekmap...**  door te navigeren naar de **codering Log** tabblad.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Probleem: Er bestaat geen optie voor het uitvoeren van een progressieve stream
* **Potentiële problemen**: De encoder die wordt gebruikt niet automatisch Interlace ongedaan maken. 
  
    **Stappen voor probleemoplossing**: Zoek naar een optie voor het ongedaan maken ineengestrengeld binnen de encoder-interface. Zodra de interlace ongedaan maken is ingeschakeld, controleren opnieuw progressieve uitvoerinstellingen. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Probleem: Heeft geprobeerd meerdere encoder uitvoerinstellingen en kan nog steeds geen verbinding maken.
* **Potentiële problemen**: Azure codering kanaal is niet juist opnieuw ingesteld. 
  
    **Stappen voor probleemoplossing**: Zorg ervoor dat het coderingsprogramma is niet meer aan AMS pushen, stoppen en opnieuw instellen van het kanaal. Zodra nogmaals uit te voeren, moet u proberen verbinding te maken van het coderingsprogramma met de nieuwe instellingen. Als dit nog niet het probleem wordt opgelost, maak een nieuw kanaal volledig, soms kanalen beschadigd kunnen raken nadat het aantal mislukte pogingen.  
* **Potentiële problemen**: De GOP grootte of sleutel frame-instellingen zijn niet optimaal. 
  
    **Stappen voor probleemoplossing**: Aanbevolen GOP-grootte of sleutelframes interval is twee seconden. Sommige coderingsprogramma's berekenen met deze instelling in het aantal frames, terwijl anderen seconden. Bijvoorbeeld: Bij het uitvoeren van 30 fps, zou de grootte van de GOP 60 frames die gelijk is aan 2 seconden.  
* **Potentiële problemen**: De stroom blokkeren gesloten poorten. 
  
    **Stappen voor probleemoplossing**: Bij het streamen via RTMP, Controleer de firewall en/of proxy-instellingen om te bevestigen dat uitgaande poorten 1935 en 1936 geopend zijn. 

> [!NOTE]
> Als na het volgen van de stappen die u nog steeds kan niet met succes streamen, dien een ondersteuningsticket met behulp van de Azure portal.
> 
> 

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

