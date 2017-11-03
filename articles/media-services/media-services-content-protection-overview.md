---
title: De inhoud beveiligen met Azure Media Services | Microsoft Docs
description: In dit artikel biedt een overzicht van de beveiliging van inhoud met Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 64be4ea104bd11b8e191e2c8d4170a2de88acb47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-content-overview"></a>Overzicht van de inhoud beveiligen
Met Microsoft Azure Media Services kunt u uw media beveiligen vanaf het moment dat deze uw computer verlaten en ze worden opgeslagen, verwerkt en afgeleverd. Media Services kunt u uw live en on-demand inhoud dynamisch worden versleuteld met Advanced Encryption Standard (AES) (met behulp van coderingssleutels 128-bits) of een van de belangrijkste DRM's leveren: Microsoft PlayReady en Google Widevine FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties (PlayReady, Widevine en FairPlay) naar geautoriseerde clients. 

De volgende afbeelding ziet u de beveiliging van inhoud werkstromen die ondersteuning biedt voor AMS. 

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

Dit onderwerp wordt uitgelegd [concepten en terminologie](media-services-content-protection-overview.md) relevant zijn voor de beveiliging van inhoud met AMS begrijpen. Dit onderwerp bevat ook [koppelingen](media-services-content-protection-overview.md#common-scenarios) naar onderwerpen waarin wordt aangegeven hoe als u de taken van de beveiliging van inhoud. 

## <a name="dynamic-encryption"></a>Dynamische versleuteling
Microsoft Azure Media Services kunt u uw inhoud dynamisch worden versleuteld met AES wissen key of DRM versleuteling leveren: Microsoft PlayReady en Google Widevine FairPlay van Apple.

Op dit moment kunt u de volgende streaming-indelingen kunt versleutelen: HLS, MPEG DASH en Smooth Streaming. U kunt progressief downloaden niet coderen.

Als u voor Media Services wilt voor het versleutelen van een asset, moet u een versleutelingssleutel (CommonEncryption of EnvelopeEncryption) koppelen aan uw asset en ook autorisatiebeleid voor de sleutel te configureren.

U moet ook de asset leveringsbeleid voor configureren. Als u wilt een opslag gecodeerde asset streamen, moet u opgeven hoe u wilt dat door het leveringsbeleid voor Assets configureren.

Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel dynamisch versleutelen van uw inhoud met behulp van AES wissen key of DRM-versleuteling. Voor het ontsleutelen van de stroom, Windows media player vraagt om de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.


## <a name="storage-encryption"></a>Versleuteling van opslag
Versleuteling van opslag gebruiken voor het versleutelen van uw versleutelde inhoud lokaal met AES 256-bitsversleuteling en vervolgens uploaden naar Azure Storage wordt bewaard in rust versleuteld. Beveiligd met storage encryption activa zijn automatisch niet-versleuteld geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat geüpload als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld storage Encryption is wanneer u wilt beveiligen van uw invoer media van hoge kwaliteit bestanden met een sterke codering in rust op schijf.

Voor het leveren van een gecodeerde asset opslag, moet u beleid voor de levering van de asset configureren zodat Media Services weet hoe u wilt uw inhoud leveren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en streams van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld, AES, common encryption of er wordt geen versleuteling).

## <a name="common-encryption-cenc"></a>Common encryption (CENC)
Algemene versleuteling wordt gebruikt bij het versleutelen van uw inhoud met PlayReady of / en Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Met behulp van versleuteling cbcs-aapl
Cbcs-aapl wordt gebruikt bij het versleutelen van uw inhoud met FairPlay.

## <a name="envelope-encryption"></a>Versleuteling van de enveloppe
Gebruik deze optie als u wilt uw inhoud beveiligen met AES-128 lege sleutel. Als u een veiligere optie wilt, kunt u een van de DRM's die worden vermeld in dit onderwerp. 

## <a name="licenses-and-keys-delivery-service"></a>Licenties en sleutels service voor het leveren
Media Services biedt een service voor het leveren van licenties DRM (PlayReady, Widevine, FairPlay) en AES-sleutels geautoriseerde clients wissen. U kunt [de Azure-portal](media-services-portal-protect-content.md), REST-API of Media Services SDK voor .NET configureren van beleid voor autorisatie en verificatie voor uw licenties en sleutels.

## <a name="token-restriction"></a>Tokenbeperking
Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben: beperking voor openen of tokenbeperking. Het beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door Secure Token Service (STS). Media Services ondersteunt tokens in de indeling Simple Web Tokens (SWT) en JSON Web Token (JWT)-indeling. Media Services biedt geen Token Services beveiligen. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. De Media Services sleutellevering-service wordt de aangevraagde sleutel (of licentie) naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel (of licentie) geretourneerd.

Wanneer beleid voor het configureren van het token worden beperkt, moet u de verificatie van de primaire sleutel, de uitgever en de doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, certificaatverlener is de secure token service die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="streaming-urls"></a>Streaming-URL 's
Als uw asset is versleuteld met meer dan één DRM, moet u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl', versleuteling = 'xxx').

Het volgende letten:

* Alleen nul of één versleutelingstype kan worden opgegeven.
* Versleutelingstype hoeft niet te worden opgegeven in de url als er slechts één versleuteling is toegepast op de asset.
* Coderingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:  
  * **cenc**: Common encryption (Playready of Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: AES envelope-codering.

## <a name="common-scenarios"></a>Algemene scenario's
De volgende onderwerpen laten zien hoe u inhoud in de opslag beveiligen, dynamisch versleutelde streamingmedia leveren, AMS-service voor het leveren van sleutel/licentie gebruiken

* [Beveiligen met AES](media-services-protect-with-aes128.md) 
* [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-drm.md)
* [Uw HLS inhoud beveiligd met Apple FairPlay en/of PlayReady Stream](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Overige scenario's
* [Service voor Azure PlayReady-licenties integratie met uw eigen server encryptor/streaming](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Met behulp van castLabs DRM-licenties te leveren met Azure Media Services](media-services-castlabs-integration.md)

>[!NOTE]
>Een scenario waarin u een externe DRM server(technology) en stroom van AMS gebruiken is momenteel niet ondersteund.


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Verwante koppelingen
[PlayReady als een service en AES dynamische versleuteling met Azure Media Services aankondigen](http://mingfeiy.com/playready)

[Azure Media Services PlayReady licentie levering prijzen uitgelegd](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Fouten opsporen in voor de versleutelde gegevensstroom AES in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT-token authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC op basis van app integreren met Azure Active Directory en beperken van de belangrijkste inhouddistributie op basis van claims JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Azure ACS gebruiken voor het probleem van tokens](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
