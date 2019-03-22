---
title: Beveiligen van uw inhoud met Azure Media Services | Microsoft Docs
description: In dit artikel biedt een overzicht van de beveiliging van inhoud met Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 0d37f0596069899bbeb9dfeafa5d487aee910cd8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58183659"
---
# <a name="content-protection-overview"></a>Overzicht van de beveiliging van inhoud 

 U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. 

De volgende afbeelding ziet u de Media Services content protection-werkstroom: 

![Beveiligen met PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

In dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor informatie over de beveiliging van inhoud met Media Services. Dit artikel bevat ook koppelingen naar artikelen over hoe om inhoud te beveiligen. 

## <a name="dynamic-encryption"></a>Dynamische versleuteling
 U kunt Media Services gebruiken om uw inhoud dynamisch versleuteld met AES clear key- of DRM-versleuteling met behulp van PlayReady, Widevine en FairPlay te leveren. Op dit moment kunt u de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Versleuteling op progressieve downloads wordt niet ondersteund. Elke versleutelingsmethode ondersteunt de volgende protocollen voor streaming:

- AES: MPEG-DASH, Smooth Streaming en HLS
- PlayReady: MPEG-DASH, Smooth Streaming en HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Voor het versleutelen van een asset, moet u de inhoud van een versleutelingssleutel koppelen aan uw asset en ook een autorisatiebeleid voor de sleutel te configureren. Inhoudssleutels kunnen worden opgegeven of automatisch worden gegenereerd door Media Services.

Ook moet u het leveringsbeleid voor Assets configureren. Als u een opslag-gecodeerde asset streamen wilt, moet u om op te geven hoe u wilt dat door het leveringsbeleid voor Assets configureren.

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van Media Services-sleutelleveringsservice. Om te beslissen of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat de autorisatiebeleidsregels die u hebt opgegeven voor de sleutel.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 clear key vs. DRM
Klanten zich vaak afvragen of ze AES-versleuteling of een DRM-systeem gebruiken moeten. Het belangrijkste verschil tussen de twee systemen is dat met AES-codering van de inhoudssleutel wordt verzonden naar de client in een niet-versleutelde indeling ('in de wissen"). De sleutel die wordt gebruikt voor het versleutelen van de inhoud kan als gevolg hiervan worden weergegeven in een netwerktracering op de client in tekst zonder opmaak. AES-128 clear key-versleuteling is geschikt voor gebruik gevallen waarin de viewer voor een vertrouwde partij (bijvoorbeeld coderen bedrijfsvideo's binnen een bedrijf kan worden weergegeven door werknemers gedistribueerd).

PlayReady, Widevine en FairPlay een hogere mate van versleuteling in vergelijking tot AES-128 bieden clear key-versleuteling. De inhoudssleutel wordt verzonden in een versleutelde indeling. Bovendien wordt ontsleuteling verwerkt in een beveiligde omgeving op het niveau van het besturingssysteem, waar is het moeilijker voor een kwaadwillende gebruiker om aan te vallen. DRM wordt aanbevolen voor gebruik gevallen waarbij de viewer mogelijk niet een vertrouwde partij en u het hoogste niveau van beveiliging nodig hebt.

## <a name="storage-encryption"></a>Storage-versleuteling
U kunt versleuteling van opslag voor het versleutelen van uw niet-versleutelde inhoud lokaal met behulp van AES 256-bits versleuteling gebruiken. Vervolgens kunt u uploaden deze naar Azure-opslag, waar deze zijn opgeslagen in rust versleuteld. Activa die worden beveiligd met storage encryption worden automatisch niet versleuteld en is geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd. De assets zijn eventueel opnieuw versleuteld voordat ze worden geüpload weer als een nieuwe uitvoerasset. De primaire use-case voor storage-versleuteling is als u wilt uw invoer media van hoge kwaliteit bestanden beveiligen met sterke versleuteling in rust op schijf.

Voor het leveren van een asset opslag versleuteld, moet u het leveringsbeleid voor Assets configureren zodat mediaservices weet hoe u wilt dat uw inhoud kunt leveren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming ontsleutelt en gegevensstromen van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld: AES, algemene versleuteling of geen versleuteling).

## <a name="types-of-encryption"></a>Typen versleuteling
PlayReady en Widevine gebruikmaken van algemene versleuteling (AES CTR modus). FairPlay maakt gebruik van versleuteling met AES-CBC-modus. Envelop versleuteling maakt gebruik van AES-128 clear key-versleuteling.

## <a name="licenses-and-keys-delivery-service"></a>Leveringsservice voor licenties en sleutels
Media Services biedt een leveringsservice voor sleutels voor het leveren van licenties voor DRM (PlayReady, Widevine, FairPlay) en AES-sleutels naar geautoriseerde clients. U kunt [de Azure-portal](media-services-portal-protect-content.md), de REST-API of de Media Services SDK voor .NET om autorisatie en verificatie beleid voor uw licenties en sleutels te configureren.

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement
U kunt bepalen wie toegang heeft tot uw inhoud door het autorisatiebeleid voor inhoudssleutels configureren. Het autorisatiebeleid voor inhoudssleutels biedt ondersteuning voor openen of token-beperking.

### <a name="open-authorization"></a>Open autorisatie
De inhoudssleutel wordt met een open autorisatiebeleid verzonden naar een client (geen beperking).

### <a name="token-authorization"></a>Token autorisatie
De inhoudssleutel wordt met een autorisatiebeleid tokenbeperking voor, alleen aan een client met daarin een geldige JSON Web Token (JWT) of simple webtoken (SWT) in de aanvraag voor sleutels/licenties verzonden. Dit token moet worden uitgegeven door een beveiligingstokenservice (STS). U kunt Azure Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. De belangrijkste leveringsservice voor Media Services retourneert de aangevraagde sleutels/licenties naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutels/licenties.

Wanneer u beleid met de tokenbeperking configureert, moet u de primaire verificatiesleutel, uitgever en doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever wordt de secure token service die de token uitgeeft. De doelgroep, ook wel genoemd bereik, het doel van het token wordt beschreven of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="streaming-urls"></a>Streaming-URL's
Als uw asset is versleuteld met meer dan één DRM, gebruikt u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl' codering = 'xxx').

De volgende overwegingen zijn van toepassing:

* Er wordt slechts één type van de versleuteling kan worden opgegeven.
* Versleutelingstype hoeft te worden opgegeven in de URL als er slechts één versleuteling is toegepast op de asset.
* Coderingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:
  * **cenc**: Voor PlayReady of Widevine (common encryption)
  * **cbcs-aapl**: Voor FairPlay (AES-CBC-codering)
  * **cbc**: Voor AES-codering van envelop

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen beschrijven de volgende stappen om u aan de slag met beveiliging van inhoud te helpen:

* [Beveiligen met versleuteling van opslag](media-services-rest-storage-encryption.md)
* [Beveiligen met AES-versleuteling](media-services-protect-with-aes128.md)
* [Beveiligen met PlayReady en/of Widevine](media-services-protect-with-playready-widevine.md)
* [Beveiligen met FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Verwante koppelingen

* [JWT-tokenverificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Op basis van een Azure Media Services OWIN MVC-app integreren met Azure Active Directory en beperken de levering van inhoud sleutel op basis van JWT-claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
