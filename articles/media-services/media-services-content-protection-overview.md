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
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>Overzicht van de inhoud beveiligen
Met Microsoft Azure Media Services kunt u uw media beveiligen vanaf het moment dat deze uw computer verlaten en ze worden opgeslagen, verwerkt en afgeleverd. Media Services kunt u uw live en on-demand inhoud dynamisch worden versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste DRM-systemen leveren: Microsoft PlayReady en Google Widevine FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties (PlayReady, Widevine en FairPlay) naar geautoriseerde clients. 

De volgende afbeelding ziet u de werkstroom van de beveiliging van inhoud Azure Media Services. 

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor de beveiliging van inhoud met AMS begrijpen. Het artikel bevat ook koppelingen naar artikelen die wordt beschreven hoe om inhoud te beveiligen. 

## <a name="dynamic-encryption"></a>Dynamische versleuteling
Azure Media Services kunt u uw inhoud dynamisch worden versleuteld met AES wissen key of DRM versleuteling leveren: Microsoft PlayReady en Google Widevine FairPlay van Apple. Op dit moment kunt u de volgende streaming-indelingen kunt versleutelen: HLS, MPEG DASH en Smooth Streaming. Versleuteling op progressief downloaden wordt niet ondersteund. Elke versleutelingsmethode ondersteunt de volgende protocollen voor streaming:
- AES: MPEG DASH, Smooth Streaming- en HLS
- PlayReady: MPEG DASH, Smooth Streaming- en HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Voor het versleutelen van een asset, moet u de inhoud van een versleutelingssleutel koppelen aan uw asset en ook een autorisatiebeleid voor de sleutel te configureren. Inhoud sleutels worden opgegeven of automatisch gegenereerd door Media Services.

U moet ook de asset leveringsbeleid voor configureren. Als u een opslag gecodeerde asset streamen wilt, moet u om op te geven hoe u wilt dat door het leveringsbeleid voor Assets configureren.

Wanneer een stream is aangevraagd door een speler, gebruikt Media Services de opgegeven sleutel dynamisch versleutelen van uw inhoud met behulp van AES wissen key of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van AMS sleutellevering service. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

## <a name="aes-128-clear-key-vs-drm"></a>De lege sleutel AES-128 tegenover DRM
Klanten vraagt zich vaak af of ze AES-versleuteling of een DRM-systeem kunt gebruiken. Het belangrijkste verschil tussen het gebruik van AES-versleuteling en de DRM-systemen is dat met AES-versleuteling de inhoudssleutel wordt verzonden naar de client in een niet-versleutelde indeling ('in de wissen'). Hierdoor kan de sleutel die wordt gebruikt voor het versleutelen van de inhoud worden weergegeven in een netwerktracering maken op de client in tekst zonder opmaak. AES-128 lege sleutel is geschikt voor gevallen waarin de viewer een vertrouwde partij is (bijvoorbeeld: zakelijke video's binnen een bedrijf kan worden weergegeven door werknemers gedistribueerd versleutelen).

Schakel sleutelcodering PlayReady Widevine en FairPlay die alle een hoger niveau van versleuteling in vergelijking tot AES-128 bieden. De inhoudssleutel wordt verzonden in een versleutelde indeling. Ontsleuteling is bovendien ingang in een beveiligde omgeving op het niveau van het besturingssysteem waar is veel moeilijker is voor een kwaadwillende gebruiker voor aanvallen. DRM wordt aanbevolen voor gevallen waarin de viewer mag niet een vertrouwde partij en u nodig hebt met het hoogste niveau van beveiliging.

## <a name="storage-encryption"></a>Storage-versleuteling
U kunt versleuteling van opslag gebruiken voor het versleutelen van uw versleutelde inhoud lokaal met AES-256-bitsversleuteling en vervolgens uploaden naar Azure Storage wordt bewaard in rust versleuteld. Beveiligd met storage encryption activa zijn automatisch niet-versleuteld geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat geüpload als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld storage Encryption is wanneer u wilt uw invoer van hoge kwaliteit mediabestanden beveiligen met sterke codering in rust op schijf.

Voor het leveren van een gecodeerde asset opslag, moet u beleid voor de levering van de asset configureren zodat Media Services weet hoe u wilt uw inhoud leveren. Voordat uw asset kan worden gestreamd, wordt de streaming server ontsleutelt en streams van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld, AES, common encryption of er wordt geen versleuteling).

## <a name="types-of-encryption"></a>Typen versleuteling
PlayReady en Widevine gebruikmaken van Common Encryption (AES CTR modus). FairPlay AES CBC-modus versleuteling wordt gebruikt. AES-128 wissen sleutelcodering envelop versleuteling wordt gebruikt.

## <a name="licenses-and-keys-delivery-service"></a>Licenties en sleutels service voor het leveren
Media Services biedt een service voor het leveren van sleutel voor het leveren van licenties DRM (PlayReady, Widevine, FairPlay) en AES-sleutels naar geautoriseerde clients. U kunt [de Azure-portal](media-services-portal-protect-content.md), REST-API of Media Services SDK voor .NET configureren van beleid voor autorisatie en verificatie voor uw licenties en sleutels.

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement
U kunt bepalen wie toegang heeft tot uw inhoud door het autorisatiebeleid voor inhoudssleutels configureren. Het autorisatiebeleid voor inhoudssleutels biedt ondersteuning voor openen of token-beperking.

### <a name="open-authorization"></a>Open autorisatie
De inhoudssleutel is een open autorisatiebeleid verzonden naar elke client (geen beperking).

### <a name="token-authorization"></a>Token autorisatie
De inhoudssleutel wordt alleen verzenden naar een client met een geldige JSON Web Token (JWT) of een eenvoudige Web Token (SWT) in de sleutel-/ licentieaanvraag daarin worden met een token beperkt verificatiebeleid. Dit token moet worden uitgegeven door een Secure Token Service (STS). U kunt Microsoft Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. De Media Services sleutellevering-service wordt de aangevraagde sleutel/licentie naar de client geretourneerd als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel/licentie.

Wanneer beleid voor het configureren van het token worden beperkt, moet u de verificatie van de primaire sleutel, de uitgever en de doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met, certificaatverlener is de secure token service die de token uitgeeft. De doelgroep, wel bereik, beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="streaming-urls"></a>Streaming-URL's
Als uw asset is versleuteld met meer dan één DRM, moet u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl', versleuteling = 'xxx').

Het volgende letten:
* Niet meer dan één type van de versleuteling kan worden opgegeven.
* Versleutelingstype hoeft niet te worden opgegeven in de URL als er slechts één versleuteling is toegepast op de asset.
* Coderingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:  
  * **cenc**: voor PlayReady of Widevine (Common Encryption)
  * **cbcs-aapl**: voor FairPlay (CBC AES-versleuteling)
  * **CBC**: voor AES envelope codering (envelop versleuteling)

## <a name="next-steps"></a>Volgende stappen
Volgende stappen aan de slag met content protection wordt beschreven in de volgende artikelen:
* [Beveiligen met versleuteling van opslag](media-services-rest-storage-encryption.md)
* [Beveiligen met AES-versleuteling](media-services-protect-with-aes128.md)
* [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-playready-widevine.md)
* [Beveiligen met FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Verwante koppelingen
[Azure Media Services PlayReady licentie levering prijzen uitgelegd](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Fouten opsporen in voor de versleutelde gegevensstroom AES in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT-token verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Azure Media Services OWIN MVC-gebaseerde app integreren met Azure Active Directory en beperken van de belangrijkste inhouddistributie op basis van claims JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
