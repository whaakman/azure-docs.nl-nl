---
title: Videospelertoepassingen ontwikkelen
description: Het onderwerp bevat koppelingen naar Frameworks voor spelers en -invoegtoepassingen die u gebruiken kunt om uw eigen clienttoepassingen die voor streamingmedia van Media Services gebruikt kunnen te ontwikkelen.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 0e88baed8188890e80d4c2e7ee9d510fdabf6e43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="develop-video-player-applications"></a>Videospelertoepassingen ontwikkelen
## <a name="overview"></a>Overzicht
Azure Media Services biedt de hulpprogramma's die u nodig hebt om geavanceerde, dynamische clientspelertoepassingen te maken voor de meeste platforms, waaronder: iOS-apparaten, Android-apparaten, Windows, Windows Phone, Xbox en settopboxen. In dit onderwerp bevat ook koppelingen naar SDK's en Frameworks voor spelers die u gebruiken kunt om uw eigen clienttoepassingen die voor streamingmedia van Azure Media Services gebruikt kunnen te ontwikkelen.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](http://aka.ms/ampinfo) is een web-speler gebouwd om af te spelen media-inhoud van Microsoft Azure Media Services op een groot aantal verschillende browsers en apparaten. Azure Media Player maakt gebruik van industrienormen zoals HTML5 Media bron-extensies (muis) en versleuteld Media-extensies (EME) om een verrijkt adaptief streamen ervaring te bieden. Wanneer deze normen wordt voldaan, niet beschikbaar op een apparaat of in een browser zijn, gebruikt Azure Media Player Flash en Silverlight als terugval technologie. Ongeacht de afspelen technologie gebruikt, moeten ontwikkelaars een geïntegreerde JavaScript-interface voor toegang tot API's. Hierdoor kan voor inhoud die door Azure Media Services behandeld over een wide-bereik van apparaten en browsers zonder eventuele extra inspanning worden afgespeeld.

Microsoft Azure Media Services zorgt ervoor dat de inhoud naar u worden geleverd met DASH-, Smooth Streaming- en HLS streaming-indelingen voor het afspelen van inhoud. Azure Media Player rekening wordt gehouden met deze verschillende indelingen en automatisch de beste koppeling op basis van het platform/browsermogelijkheden speelt. Microsoft Azure Media Services biedt tevens mogelijkheden voor dynamische versleuteling van activa met PlayReady-versleuteling of AES-128-bits envelop codering. Azure Media Player kunnen voor het ontsleutelen van PlayReady en AES-128-bits gecodeerde inhoud wanneer er op de juiste wijze geconfigureerd. 

Voor meer informatie:

* [Azure Media Player](http://aka.ms/ampinfo)
* [Azure Media Player-documentatie](http://aka.ms/ampdocs) 
* [Azure MediaPlayer ophalen Blog gestart](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [Meld u aan het product up-to-date houden met de nieuwste versie van Azure Media Player](http://aka.ms/ampsignup)
* [Toevoegen van nieuwe functieaanvragen, ideeën, feedback](http://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Andere hulpprogramma's voor het maken van Player-toepassingen
U kunt ook een van de volgende SDK's gebruiken:

* [Vloeiende Streaming Client SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Smooth Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
* [Microsoft Media-Platform: Player Framework](http://playerframework.codeplex.com/) 
* [HTML5 Documentatie Player-Framework](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft Smooth Streaming-invoegtoepassing voor OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Licentieverlening Microsoft® Smooth Streaming Client Kit overdragen](http://aka.ms/sspk) 
* [XBOX Video toepassingsontwikkeling](http://xbox.create.msdn.com/) 

## <a name="advertising"></a>Adverteren
Azure Media Services biedt ondersteuning voor het invoegen van ad via de Windows Media-Platform: Frameworks voor spelers. Er zijn frameworks voor spelers met ad-ondersteuning beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elke player-framework bevat voorbeeldcode die laat zien hoe een toepassing player implementeren. Er zijn drie soorten advertenties die u in uw media invoegen kunt:

Lineaire – volledige frame advertenties die de belangrijkste video onderbreken

Niet-lineaire – overlay advertenties die worden weergegeven als de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding geplaatst Windows Media Player

Aanvullende – advertenties die buiten de speler worden weergegeven

Advertenties kunnen op elk punt in de tijdlijn van de belangrijkste video worden geplaatst. U moet de speler hoogte wanneer af te spelen, de advertentie en welke advertenties af te spelen. Dit wordt gedaan met behulp van een reeks standaard XML-bestanden: Video Ad-Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP), Media abstracte sequentiëren sjabloon (b) en digitale Video Player Ad Interface Definition (VPAID). GROTE bestanden opgeven welke advertenties om weer te geven. VMAP bestanden opgeven wanneer verschillende advertenties afspelen en ENORME XML bevatten. B bestanden vormen een andere reeks advertenties die tevens VAST XML kan bevatten. VPAID bestanden definiëren een interface tussen de video speler en de advertentie- of Active Directory-server. Zie voor meer informatie [invoegen advertenties](https://msdn.microsoft.com/library/dn387398.aspx).

Zie voor meer informatie over de ondertiteling en ads-ondersteuning in Live streaming video's [ondersteund gesloten ondertiteling en Ad invoegen standaarden](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Een adaptieve MPEG-DASH-videostream insluiten in een HTML5-toepassing met DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub-opslagplaats voor dash.js](https://github.com/Dash-Industry-Forum/dash.js)

