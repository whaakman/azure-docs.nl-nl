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
ms.openlocfilehash: 13447fd9193374d80ed5c2e6af8543f11b95e709
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="content-protection-overview"></a>Overzicht van de beveiliging van inhoud
 U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. U kunt uw live en on-demand inhoud dynamisch worden versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digitale rechten (DRM) management systemen leveren met Media Services: Microsoft PlayReady en Google Widevine FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties (PlayReady, Widevine en FairPlay) naar geautoriseerde clients. 

De volgende afbeelding ziet u de werkstroom van de beveiliging van inhoud Media Services: 

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor het begrijpen van de beveiliging van inhoud met Media Services. Het artikel bevat ook koppelingen naar artikelen die wordt beschreven hoe om inhoud te beveiligen. 

## <a name="dynamic-encryption"></a>Dynamische versleuteling
 Media Services kunt u uw inhoud dynamisch versleuteld met AES wissen key of DRM codering met behulp van PlayReady of Widevine FairPlay leveren. U kunt op dit moment wordt de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Versleuteling op progressief downloaden wordt niet ondersteund. Elke versleutelingsmethode ondersteunt de volgende protocollen voor streaming:

- AES: MPEG DASH, Smooth Streaming- en HLS
- PlayReady: MPEG DASH, Smooth Streaming- en HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Voor het versleutelen van een asset, moet u de inhoud van een versleutelingssleutel koppelen aan uw asset en ook een autorisatiebeleid voor de sleutel te configureren. Inhoud sleutels worden opgegeven of automatisch gegenereerd door Media Services.

U moet ook de asset leveringsbeleid voor configureren. Als u een opslag-gecodeerde asset streamen wilt, moet u om op te geven hoe u wilt dat door het leveringsbeleid voor Assets configureren.

Wanneer een stream is aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van Media Services sleutellevering-service. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 wissen sleutel vs. DRM
Klanten vraagt zich vaak af of ze AES-versleuteling of een DRM-systeem kunt gebruiken. Het belangrijkste verschil tussen de twee systemen is dat met AES-versleuteling de inhoudssleutel wordt verzonden naar de client in een niet-versleutelde indeling ('in de wissen'). Hierdoor kan de sleutel die wordt gebruikt voor het versleutelen van de inhoud worden weergegeven in een netwerktracering maken op de client in tekst zonder opmaak. Wis sleutel AES-128-versleuteling is geschikt voor gevallen waarin de viewer een vertrouwde partij (bijvoorbeeld versleutelen zakelijke video's binnen een bedrijf is kan worden weergegeven door werknemers gedistribueerd).

Schakel sleutelcodering PlayReady Widevine en FairPlay die alle een hoger niveau van versleuteling in vergelijking tot AES-128 bieden. De inhoudssleutel wordt verzonden in een versleutelde indeling. Bovendien wordt ontsleuteling uitgevoerd in een beveiligde omgeving op het niveau van het besturingssysteem, waar het moeilijker voor een kwaadwillende gebruiker is voor aanvallen. DRM wordt aanbevolen voor gevallen waarin de viewer kan mogelijk niet worden van een vertrouwde partij en u nodig hebt met het hoogste niveau van beveiliging.

## <a name="storage-encryption"></a>Storage-versleuteling
U kunt versleuteling van opslag gebruiken voor het versleutelen van uw versleutelde inhoud lokaal met behulp van AES 256-bits versleuteling. U kunt uploaden naar Azure Storage wordt bewaard in rust versleuteld. Beveiligd met storage encryption activa zijn automatisch niet-versleuteld en geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd. De activa zijn optioneel opnieuw versleutelde voorafgaand aan het uploaden als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld storage Encryption is wanneer u wilt uw invoer van hoge kwaliteit mediabestanden beveiligen met sterke codering in rust op schijf.

Voor het leveren van een activum opslag versleuteld, moet u beleid voor de levering van de asset configureren zodat Media Services weet hoe u wilt uw inhoud leveren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming ontsleutelt en streams van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld, AES, common encryption of er wordt geen versleuteling).

## <a name="types-of-encryption"></a>Typen versleuteling
PlayReady en Widevine gebruikmaken van algemene versleuteling (AES CTR modus). FairPlay CBC-modus van AES-versleuteling wordt gebruikt. AES-128 wissen sleutelcodering envelop versleuteling wordt gebruikt.

## <a name="licenses-and-keys-delivery-service"></a>Licenties en sleutels service voor het leveren
Media Services biedt een service voor het leveren van sleutel voor het leveren van licenties DRM (PlayReady, Widevine, FairPlay) en AES-sleutels naar geautoriseerde clients. U kunt [de Azure-portal](media-services-portal-protect-content.md), de REST API of de Media Services SDK voor .NET configureren van beleid voor autorisatie en verificatie voor uw licenties en sleutels.

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement
U kunt bepalen wie toegang heeft tot uw inhoud door het autorisatiebeleid voor inhoudssleutels configureren. Het autorisatiebeleid voor inhoudssleutels biedt ondersteuning voor openen of token-beperking.

### <a name="open-authorization"></a>Open autorisatie
De inhoudssleutel is een open autorisatiebeleid verzonden naar elke client (geen beperking).

### <a name="token-authorization"></a>Token autorisatie
Met een token beperkt verificatiebeleid wordt de inhoudssleutel alleen verzonden naar een client die een geldige JSON Web Token (JWT) of een eenvoudige web token (SWT) in de sleutel-/ licentieaanvraag geeft. Dit token moet worden uitgegeven door een beveiligingstokenservice (STS). U kunt Azure Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel/licentie, stuurt de Media Services sleutellevering-service de aangevraagde sleutel/licentie naar de client.

Wanneer u beleid met de tokenbeperking configureert, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met. De certificaatverlener is de secure token service die de token uitgeeft. De doelgroep, wel bereik, beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="streaming-urls"></a>Streaming-URL's
Als uw asset is versleuteld met meer dan één DRM, gebruikt u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl', versleuteling = 'xxx').

Het volgende letten:

* Niet meer dan één type van de versleuteling kan worden opgegeven.
* Versleutelingstype hoeft niet te worden opgegeven in de URL als er slechts één versleuteling is toegepast op de asset.
* Coderingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:
  * **cenc**: voor PlayReady of Widevine (common encryption)
  * **cbcs-aapl**: voor FairPlay (CBC AES-versleuteling)
  * **CBC**: voor AES envelope-versleuteling

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen beschrijven de volgende stappen om u aan de slag met beveiliging van inhoud te helpen:

* [Beveiligen met versleuteling van opslag](media-services-rest-storage-encryption.md)
* [Beveiligen met AES-versleuteling](media-services-protect-with-aes128.md)
* [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-playready-widevine.md)
* [Beveiligen met FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Verwante koppelingen

* [Azure Media Services PlayReady licentie levering prijzen uitgelegd](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)
* [Foutopsporing voor de versleutelde gegevensstroom AES in Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)
* [JWT-token verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Azure Media Services OWIN MVC-gebaseerde app integreren met Azure Active Directory en sleutel inhouddistributie op basis van claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
