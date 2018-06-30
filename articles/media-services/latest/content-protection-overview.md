---
title: De inhoud beveiligen met Azure Media Services | Microsoft Docs
description: In dit artikel biedt een overzicht van de beveiliging van inhoud met Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.author: juliako
ms.openlocfilehash: 28c10d7c478ea7a9b1d1bfa91da79452eba3a4b6
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132884"
---
# <a name="content-protection-overview"></a>Overzicht van de beveiliging van inhoud

U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via de opslag, verwerking en levering. U kunt uw live en on-demand inhoud dynamisch worden versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digitale rechten (DRM) management systemen leveren met Media Services: Microsoft PlayReady en Google Widevine FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM-licenties (PlayReady, Widevine en FairPlay) naar geautoriseerde clients. 

De volgende afbeelding ziet u de werkstroom van de beveiliging van inhoud Media Services: 

![Inhoud beschermen](./media/content-protection/content-protection.png)

&#42;*dynamische versleuteling AES-128 'lege sleutel', CBCS en CENC ondersteunt. Zie voor meer informatie de ondersteuningsmatrix [hier](#streaming-protocols-and-encryption-types).*

Dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor het begrijpen van de beveiliging van inhoud met Media Services. Het artikel bevat ook koppelingen naar artikelen die wordt beschreven hoe om inhoud te beveiligen. 

## <a name="main-components-of-the-content-protection-system"></a>Belangrijkste onderdelen van het systeem voor de beveiliging van inhoud

Om te kunnen voltooien uw ontwerp van de system-toepassing 'content protection', moet u volledig inzicht in het bereik van de inspanning. De volgende lijst geeft een overzicht van de drie onderdelen die u moet implementeren. 

1. Azure Media Services-code
  
  * Licentie-sjablonen voor Widevine, PlayReady en/of FairPlay. De sjablonen kunnen u rechten en machtigingen voor elk van de gebruikte DRM's configureren
  * Licentie levering autorisatie, geven de logica van autorisatie controle op basis van claims in JWT
  * Inhoud sleutels, streaming protocollen en bijbehorende DRM's toegepast, definiërende DRM-versleuteling

  > [!NOTE]
  > U kunt elke asset met meerdere coderingstypen (AES-128, PlayReady, Widevine, FairPlay) versleutelen. Zie [Streaming protocollen en versleutelingstypen](#streaming-protocols-and-encryption-types), om te zien wat zinvol om u te combineren.
  
  Het volgende artikel stappen voor het versleutelen van inhoud met AES weergeven: [beveiligen met AES-versleuteling](protect-with-aes128.md)
 
2. Speler met AES of DRM-client. Een video player-app op basis van een speler SDK (systeemeigen of browser gebaseerde) moet aan de volgende vereisten voldoen:
  * Windows media player SDK biedt ondersteuning voor de benodigde DRM-clients
  * Windows media player SDK biedt ondersteuning voor de vereiste streaming protocollen: Smooth, DASH of HLS
  * Windows media player SDK moet kunnen voor het afhandelen van een JWT-token doorgeven in een licentieaanvraag overname
  
    U kunt een speler maken met behulp van de [Azure Media Player-API](http://amp.azure.net/libs/amp/latest/docs/). Gebruik de [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) om op te geven welke DRM-technologie gebruiken op verschillende DRM-platforms.

    Voor testen AES of CENC (Widevine + PlayReady) gecodeerde inhoud, kunt u [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). Controleer of klik op 'Geavanceerde options' en AES controleren en geef het token.

    Als u testen FairPlay versleutelde inhoud wilt, gebruikt u [deze test player](http://aka.ms/amtest). Windows media player ondersteunt Widevine, PlayReady en FairPlay DRM's, evenals de AES-128 sleutelcodering wissen. U moet de juiste browser voor het testen van verschillende DRM's kiezen: Opera-Chrome/Firefox voor Widevine, MS Edge/IE11 voor PlayReady, Safari op maOS voor FairPlay.

3. Secure Token Service (STS), JSON Web Token (JWT) geeft als toegangstoken voor toegang tot de back-end-bronnen. Als de bron van de back-end kunt u de AMS-services voor het leveren van licenties. Een STS heeft voor het definiëren van het volgende:

  * Uitgever en de doelgroep (of bereik)
  * Claims, die afhankelijk van de zakelijke vereisten in de beveiliging van inhoud zijn
  * Symmetrisch als asymmetrisch verificatie voor handtekeningverificatie
  * Sleutelrollover-ondersteuning (indien nodig)

    U kunt [dit hulpprogramma STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) test STS, die ondersteuning biedt voor alle 3 soorten verificatie sleutel: symmetrische, asymmetrische of AAD met sleutelrollover. 

> [!NOTE]
> Het wordt nadrukkelijk aanbevolen zich richten en volledige test uit voor elk onderdeel (hierboven) voordat u doorgaat naar het volgende gedeelte. U kunt testen uw systeem 'content protection' met de hulpprogramma's voor het opgegeven in de bovenstaande lijst.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocollen en versleutelingstypen streaming

Media Services kunt u uw inhoud dynamisch versleuteld met AES wissen key of DRM codering met behulp van PlayReady of Widevine FairPlay leveren. U kunt op dit moment wordt de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Elk protocol ondersteunt de volgende versleutelingsmethoden:

|Protocol|Containerindeling|Versleutelingsmethode|
|---|---|---|---|
|MPEG-DASH|Alle|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Alle|AES|
||MPG2 TS |CBCS (Fairplay) |
||MPG2 TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Smooth Streaming|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Dynamische versleuteling

In Media Services-v3 een inhoudssleutel is gekoppeld aan StreamingLocator (Zie [in dit voorbeeld](protect-with-aes128.md)). Als het Media Services sleutellevering-service wordt gebruikt, moet u automatisch genereren van de inhoudssleutel. U moet genereren de inhoudssleutel zelf als u u eigen sleutellevering service of als u nodig hebt voor het afhandelen van een hoge beschikbaarheid-scenario waarin u wilt hebben dezelfde inhoud sleutel in twee datacentra.

Wanneer een stream is aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van het Media Services sleutellevering service of de sleutellevering-service die u hebt opgegeven. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 wissen sleutel vs. DRM

Klanten vraagt zich vaak af of ze AES-versleuteling of een DRM-systeem kunt gebruiken. Het belangrijkste verschil tussen de twee systemen is dat met AES-versleuteling de inhoudssleutel wordt verzonden naar de client in een niet-versleutelde indeling ('in de wissen'). Hierdoor kan de sleutel die wordt gebruikt voor het versleutelen van de inhoud worden weergegeven in een netwerktracering maken op de client in tekst zonder opmaak. Wis sleutel AES-128-versleuteling is geschikt voor gevallen waarin de viewer een vertrouwde partij (bijvoorbeeld versleutelen zakelijke video's binnen een bedrijf is kan worden weergegeven door werknemers gedistribueerd).

Schakel sleutelcodering PlayReady Widevine en FairPlay die alle een hoger niveau van versleuteling in vergelijking tot AES-128 bieden. De inhoudssleutel wordt verzonden in een versleutelde indeling. Bovendien wordt ontsleuteling uitgevoerd in een beveiligde omgeving op het niveau van het besturingssysteem, waar het moeilijker voor een kwaadwillende gebruiker is voor aanvallen. DRM wordt aanbevolen voor gevallen waarin de viewer kan mogelijk niet worden van een vertrouwde partij en u nodig hebt met het hoogste niveau van beveiliging.

## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de clientzijde

Ter bescherming van uw Assets in rust, moeten de activa door de versleuteling van opslag aan de clientzijde worden versleuteld. De volgende tabel ziet u hoe de versleuteling van opslag aan de clientzijde in Media Services v3 werkt:

|Versleutelingsoptie|Beschrijving|Media Services v3|
|---|---|---|---|
|Media Services-versleuteling van opslag| AES-256-versleuteling, key beheerd door Media Services|Niet ondersteund<sup>(1)</sup>|
|[Service-versleuteling van opslag voor gegevens in rust](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling aan de clientzijde van de server die worden aangeboden door Azure Storage-sleutel beheerd door Azure of door de klant|Ondersteund|
|[Versleuteling van opslag aan de clientzijde](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Versleuteling van clientzijde die worden aangeboden door de Azure-opslag, beheerd door de klant in de Sleutelkluis sleutel|Niet ondersteund|

<sup>1</sup> in Media Services-v3 versleuteling van opslag (AES-256-versleuteling) wordt alleen ondersteund voor compatibiliteit met eerdere wanneer uw Assets met Media Services-v2 zijn gemaakt. Dit betekent dat v3 werkt met bestaande opslag activa versleuteld, maar staat niet toe dat het maken van nieuwe activiteiten.

## <a name="licenses-and-keys-delivery-service"></a>Licenties en sleutels service voor het leveren

Media Services biedt een service voor het leveren van sleutel voor het leveren van licenties DRM (PlayReady, Widevine, FairPlay) en AES-sleutels naar geautoriseerde clients. U kunt de REST API of een Media Services-clientbibliotheek gebruiken voor het configureren van beleidsregels voor autorisatie en verificatie voor uw licenties en sleutels.

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement

U kunt bepalen wie toegang heeft tot uw inhoud door het beleid voor de inhoud van de sleutels te configureren. Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. U kunt het beleid voor de inhoud van de sleutels moet configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het beleid voor de inhoud van de sleutels kan hebben **openen** of **token** beperking. 

Met een token van beperkte inhoud sleutel beleid, wordt de inhoudssleutel alleen verzonden naar een client die een geldige JSON Web Token (JWT) of een eenvoudige web token (SWT) in de sleutel-/ licentieaanvraag geeft. Dit token moet worden uitgegeven door een beveiligingstokenservice (STS). U kunt Azure Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutel/licentie, stuurt de Media Services sleutellevering-service de aangevraagde sleutel/licentie naar de client.

Wanneer u beleid met de tokenbeperking configureert, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met. De certificaatverlener is de secure token service die de token uitgeeft. De doelgroep, wel bereik, beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="streaming-urls"></a>Streaming-URL's

Als uw asset is versleuteld met meer dan één DRM, gebruikt u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl', versleuteling = 'xxx').

Het volgende letten:

* Versleutelingstype hoeft niet te worden opgegeven in de URL als er slechts één versleuteling is toegepast op de asset.
* Coderingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:
  * **cenc**: voor PlayReady of Widevine (common encryption)
  * **cbcs-aapl**: voor FairPlay (CBC AES-versleuteling)
  * **CBC**: voor AES envelope-versleuteling

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

Gebruik de volgende informatie voor probleemoplossing voor hulp bij problemen met implementatie.

* De verlener URL met eindigen moet '/'. De doelgroep moet de player client-id op. Ook toe te voegen "/" aan het einde van de uitgevers-URL.

  ```
  <add key="ida:audience" value="[Application Client ID GUID]" />
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />
  ```

* Machtigingen voor de toepassing in Azure AD toevoegen aan de **configureren** tabblad van de toepassing. Machtigingen zijn vereist voor elke toepassing, zowel lokaal als geïmplementeerde versie.
* Gebruik de juiste verlener wanneer u dynamische CENC beveiliging instelt.

  ```
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
  ```

  De volgende werkt niet:

  ```
  <add key="ida:issuer" value="https://username.onmicrosoft.com/" />
  ```

  De GUID is de Azure AD-tenant-ID. De GUID vindt u in de **eindpunten** pop-upmenu van de Azure-portal.

* Verleen het lidmaatschap van claims van bevoegdheden. Zorg dat het volgende in het manifestbestand voor Azure AD-toepassing: 

    'groupMembershipClaims': 'Alle' (de standaardwaarde is null)

## <a name="next-steps"></a>Volgende stappen

[Beveiligen met AES-versleuteling](protect-with-aes128.md)
