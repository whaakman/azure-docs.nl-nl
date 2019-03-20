---
title: Videospelertoepassingen ontwikkelen
description: Het onderwerp vindt u koppelingen naar de Player-Frameworks en -invoegtoepassingen die u gebruiken kunt voor het ontwikkelen van uw eigen clienttoepassingen die streamingmedia van Media Services kunnen gebruiken.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 55e419fc-4c39-4902-9c62-f41cfcd86c6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: ba8d9fd54b8b29a3e58f170402a64f5899fec002
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880684"
---
# <a name="develop-video-player-applications"></a>Videospelertoepassingen ontwikkelen
## <a name="overview"></a>Overzicht
Azure Media Services biedt de hulpprogramma's die u nodig hebt om geavanceerde, dynamische clientspelertoepassingen te maken voor de meeste platforms, waaronder: iOS-apparaten, Android-apparaten, Windows, Windows Phone, Xbox en settopboxen. In dit onderwerp bevat ook koppelingen naar SDK's en Frameworks voor spelers die u gebruiken kunt om uw eigen clienttoepassingen die voor streamingmedia van Azure Media Services gebruikt kunnen te ontwikkelen.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
 
## <a name="azure-media-player"></a>Azure Media Player
[Azure Media Player](https://aka.ms/ampinfo) is een web-videospeler gebouwd om af te spelen media-inhoud van Microsoft Azure Media Services op een groot aantal browsers en apparaten. Azure Media Player maakt gebruik van industriële standaarden, zoals HTML5, Media bron-extensies (MSE) en Encrypted Media Extensions (EME) voor een geavanceerde adaptieve streamingervaring. Wanneer deze standaarden niet beschikbaar op een apparaat of in een browser zijn, gebruikt Azure Media Player Flash en Silverlight als fallback-technologie. Ontwikkelaars heeft, welke afspeeltechnologie gebruikt, een geïntegreerde JavaScript-interface voor toegang tot API's. Hiermee wordt inhoud van Azure Media Services om te worden afgespeeld op hele-tal van apparaten en browsers zonder extra inspanningen.

Microsoft Azure Media Services kan inhoud worden aangeboden met DASH, Smooth Streaming en HLS streaming-indelingen voor het afspelen van inhoud. Azure Media Player houdt rekening met deze verschillende indelingen en automatisch de beste koppeling op basis van de platform-en browsermogelijkheden speelt. Microsoft Azure Media Services kan ook dynamische versleuteling van activa met PlayReady-versleuteling of AES-128-bits envelop codering. Azure Media Player profiteert u van de ontsleuteling van PlayReady en AES-128 bits versleutelde inhoud wanneer op de juiste wijze geconfigureerd. 

Voor meer informatie:

* [Azure Media Player](https://aka.ms/ampinfo)
* [Documentatie voor Azure Media Player](https://aka.ms/ampdocs) 
* [Azure MediaPlayer aan de slag Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
* [Registreren om te blijven met het laatste nieuws van Azure Media Player](https://aka.ms/ampsignup)
* [Nieuwe functieaanvragen, ideeën, feedback toevoegen](https://aka.ms/ampuservoice) 

## <a name="other-tools-for-creating-player-applications"></a>Andere hulpprogramma's voor het maken van toepassingen van de speler
U kunt ook een van de volgende SDK's gebruiken:

* [Smooth Streaming Client SDK](https://www.iis.net/downloads/microsoft/smooth-streaming) 
* [Smooth Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
* [Microsoft Media Platform: Player Framework](https://playerframework.codeplex.com/) 
* [HTML5 Player Framework-documentatie](https://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
* [Microsoft Smooth Streaming-invoegtoepassing voor OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
* [Licentieverlening Microsoft® Smooth Streaming Client Porting Kit](https://aka.ms/sspk) 
* [XBOX Video toepassingsontwikkeling](https://xbox.create.msdn.com/) 

## <a name="advertising"></a>Advertenties
Azure Media Services biedt ondersteuning voor advertentie-invoeging via het Windows Media-Platform: Player Frameworks. Frameworks voor spelers met ad-ondersteuning zijn beschikbaar voor Windows 8, Silverlight, Windows Phone 8 en iOS-apparaten. Elk speler framework bevat voorbeeldcode die laat zien u hoe u een player-toepassing implementeert. Er zijn drie verschillende soorten advertenties die u in uw media invoegen kunt:

Lineaire-volledige frame advertenties die de belangrijkste video onderbreken

Niet-lineaire-overlay-advertenties die worden weergegeven als de belangrijkste video wordt afgespeeld, meestal een logo of andere statische afbeelding binnen de speler geplaatst

Companion-advertenties die buiten de speler worden weergegeven

Advertenties kunnen op elk gewenst moment in de tijdlijn van de belangrijkste video worden geplaatst. U moet de speler zien bij het afspelen van de ad en welke advertenties om af te spelen. Dit wordt gedaan met behulp van een set standaard op basis van een XML-bestanden: Video Ad Service-sjabloon (VAST), digitale Video meerdere Ad afspeellijst (VMAP), Media abstracte sequentiëren sjabloon (b) en digitale Video speler Ad Interface Definition (VPAID). GROTE bestanden opgeven welke advertenties om weer te geven. VMAP bestanden opgeven wanneer verschillende advertenties afspelen en ENORME XML bevatten. B-bestanden zijn een andere manier om de volgorde advertenties die ook VAST XML kan bevatten. VPAID bestanden definieert een interface tussen de videospeler en de ad of ad-server. Zie voor meer informatie, [invoegen advertenties](https://msdn.microsoft.com/library/dn387398.aspx).

Zie voor meer informatie over de ondertiteling en ondersteuning voor advertenties in Live streaming video's [ondersteund gesloten ondertiteling en standaarden voor advertentie-invoeging](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Een adaptieve MPEG-DASH-videostream insluiten in een HTML5-toepassing met DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub-opslagplaats voor dash.js](https://github.com/Dash-Industry-Forum/dash.js)

