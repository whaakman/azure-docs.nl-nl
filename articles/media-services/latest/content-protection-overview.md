---
title: Beveiligen van uw inhoud met Media Services - Azure | Microsoft Docs
description: In dit artikel biedt een overzicht van de beveiliging van inhoud met Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 02c786209c81e755b05cc3875778f98faf8a1ae1
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56650934"
---
# <a name="content-protection-overview"></a>Overzicht van de beveiliging van inhoud

U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services, kunt u uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste digital rights management (DRM)-systemen leveren: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. 

De volgende afbeelding ziet u de Media Services content protection-werkstroom: 

![Inhoud beveiligen](./media/content-protection/content-protection.png)

&#42;*dynamische versleuteling ondersteunt AES-128 "clear key', CBCS en CENC. Zie voor meer informatie de ondersteuningsmatrix [hier](#streaming-protocols-and-encryption-types).*

In dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor informatie over de beveiliging van inhoud met Media Services. Het artikel heeft ook de [Veelgestelde vragen over](#faq) sectie en vindt u koppelingen naar artikelen die laten zien hoe om inhoud te beveiligen. 

## <a name="main-components-of-a-content-protection-system"></a>Belangrijkste onderdelen van een systeem voor de beveiliging van inhoud

Als u wilt uw systeem-/ toepassingsontwerp 'content protection' is voltooid, moet u volledig inzicht in het bereik van de inspanningen. De volgende lijst biedt een overzicht van de drie onderdelen die u moet implementeren. 

1. Azure Media Services-code
  
  De [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) voorbeeld ziet u het implementeren van multi-DRM-systeem met Media Services v3 en Media Services-service voor het leveren van licentiesleutel/ook gebruiken. U kunt elke asset met meerdere versleutelingstypen versleutelen (AES-128, PlayReady, Widevine, FairPlay). Zie [Streamingprotocollen en versleutelingstypen](#streaming-protocols-and-encryption-types) voor nuttige combinaties.
  
  Het voorbeeld wordt getoond hoe u:

  1. Maak en configureer ContentKeyPolicies.

    * Definieer license delivery autorisatie, de logica van autorisatie-controle op basis van claims in JWT op te geven.
    * DRM-codering configureren door de inhoudssleutel op te geven.
    * Configureer [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md), en [FairPlay](fairplay-license-overview.md) licenties. De sjablonen kunnen u rechten en machtigingen voor elk van de gebruikte DRM's configureren.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
  2. Maak een StreamingLocator die is geconfigureerd voor het streamen van een gecodeerde asset. 

    U kunt bijvoorbeeld StreamingLocator.StreamingPolicyName instellen voor het beleid 'Predefined_MultiDrmCencStreaming'. Dit beleid geeft aan dat u wilt dat er twee inhoudssleutels (envelop en CENC) worden gegenereerd en ingesteld voor de locator. Er worden dan de envelop-, PlayReady- en Widevine-coderingen toegepast (de sleutel wordt aan de afspeelclient geleverd op basis van de geconfigureerde DRM-licenties). Als u wilt dat uw stream ook met CBCS (FairPlay) wordt versleuteld, gebruikt u Predefined_MultiDrmStreaming.
  3. Maken van een test-token.

    De **GetTokenAsync** methode geeft over het maken van een test-token.
  4. De streaming-URL maken.

    De **GetDASHStreamingUrlAsync** methode laat zien hoe u de streaming-URL maken. In dit geval wordt de URL-stromen de **DASH** inhoud.

2. De speler met AES of DRM-client. Een videospeler-app op basis van een player SDK (systeemeigen of browsergebaseerde) moet voldoen aan de volgende vereisten:
  * Windows media player SDK biedt ondersteuning voor de benodigde DRM-clients
  * Windows media player SDK biedt ondersteuning voor de vereiste protocollen voor streaming: Smooth, DASH en/of HLS
  * Windows media player SDK moet kunnen zijn voor het afhandelen van een JWT-token wordt doorgegeven in licentie overname aanvraag
  
    U kunt een speler maken met behulp van de [API van Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Gebruik de [API van Azure Media Player ProtectionInfo](http://amp.azure.net/libs/amp/latest/docs/) om op te geven welke DRM-technologie kunt gebruiken voor verschillende DRM-platforms.

    Voor testen AES of CENC (Widevine en/of PlayReady) gecodeerde inhoud, kunt u [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). Zorg ervoor dat u op 'Geavanceerde opties' en de opties voor versleuteling.

    Als u testen van FairPlay gecodeerde inhoud wilt, gebruikt u [deze test player](https://aka.ms/amtest). Windows media player ondersteunt Widevine, PlayReady, en FairPlay DRM's, evenals de AES-128 clear key-versleuteling. 
    
    U moet de juiste browser voor het testen van verschillende DRM's kiezen: Chrome/Opera/Firefox voor Widevine, Microsoft Edge/IE11 voor PlayReady, Safari op macOS voor FairPlay.

3. Secure Token Service (STS), geeft u JSON Web Token (JWT) als het toegangstoken voor toegang tot back-end. U kunt de AMS-services voor het leveren van licentie gebruiken als de back endresource. Een STS heeft voor het definiëren van het volgende:

  * Verlener en doelgroep (of bereik)
  * Claims die afhankelijk van de zakelijke vereisten in de beveiliging van inhoud zijn
  * Symmetrisch als asymmetrisch controle voor controle van handtekening
  * Rollover van ondertekeningssleutel ondersteuning (indien nodig)

    U kunt gebruiken [dit hulpprogramma STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) test STS, die ondersteuning biedt voor alle 3 soorten of verificatiesleutel: symmetrisch, asymmetrisch of Azure AD met sleutelrollover. 

> [!NOTE]
> Het is zeer aanbevolen zich richten en elk onderdeel (hierboven beschreven) volledig hebt getest voordat u verplaatsen naar het volgende gedeelte. U kunt uw systeem 'content protection' testen met de hulpprogramma's opgegeven in de bovenstaande lijst.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocollen voor streaming en versleutelingstypen

U kunt Media Services gebruiken om uw inhoud dynamisch versleuteld met AES clear key- of DRM-versleuteling met behulp van PlayReady, Widevine en FairPlay te leveren. Op dit moment kunt u de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Elk protocol ondersteunt de volgende versleutelingsmethoden:

|Protocol|Containerindeling|Versleutelingsmethode|
|---|---|---|---|
|MPEG-DASH|Alle|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Alle|AES|
||MPG2-TS |CBCS (Fairplay) |
||MPG2-TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Smooth Streaming|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Dynamische versleuteling

In Media Services v3, een inhoudssleutel is gekoppeld aan StreamingLocator (Zie [in dit voorbeeld](protect-with-aes128.md)). Als u de belangrijkste leveringsservice voor Media Services gebruikt, moet u automatisch genereren van de inhoudssleutel. U moet genereren de inhoudssleutel uzelf als u u eigen sleutelleveringsservice, of als u nodig hebt voor het afhandelen van een hoge beschikbaarheid-scenario waarin u wilt de dezelfde inhoudssleutel hebben in twee datacenters.

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van Media Services-sleutelleveringsservice of de sleutelleveringsservice die u hebt opgegeven. Om te beslissen of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat het beleid voor het sleutels van inhoud die u hebt opgegeven voor de sleutel.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 clear key vs. DRM

Klanten zich vaak afvragen of ze AES-versleuteling of een DRM-systeem gebruiken moeten. Het belangrijkste verschil tussen de twee systemen is dat met AES-codering van de inhoudssleutel wordt verzonden naar de client in een niet-versleutelde indeling ('in de wissen"). De sleutel die wordt gebruikt voor het versleutelen van de inhoud kan als gevolg hiervan worden weergegeven in een netwerktracering op de client in tekst zonder opmaak. AES-128 clear key-versleuteling is geschikt voor gebruik gevallen waarin de viewer voor een vertrouwde partij (bijvoorbeeld coderen bedrijfsvideo's binnen een bedrijf kan worden weergegeven door werknemers gedistribueerd).

PlayReady, Widevine en FairPlay een hogere mate van versleuteling in vergelijking tot AES-128 bieden clear key-versleuteling. De inhoudssleutel wordt verzonden in een versleutelde indeling. Bovendien wordt ontsleuteling verwerkt in een beveiligde omgeving op het niveau van het besturingssysteem, waar is het moeilijker voor een kwaadwillende gebruiker om aan te vallen. DRM wordt aanbevolen voor gebruik gevallen waarbij de viewer mogelijk niet een vertrouwde partij en u het hoogste niveau van beveiliging nodig hebt.

## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel laat zien hoe de versleuteling van opslag aan de serverzijde in Media Services v3 werkt:

|Optie voor opslagversleuteling|Description|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling| AES-256-codering, sleutel beheerd door Media Services|Niet ondersteund<sup>(1)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|

<sup>1</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="licenses-and-keys-delivery-service"></a>Leveringsservice voor licenties en sleutels

Media Services biedt een leveringsservice voor sleutels voor het leveren van licenties voor DRM (PlayReady, Widevine, FairPlay) en AES-sleutels naar geautoriseerde clients. U kunt de REST-API of een Media Services-clientbibliotheek gebruiken om autorisatie en verificatie beleid voor uw licenties en sleutels te configureren.

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement

U kunt bepalen wie toegang heeft tot uw inhoud door het configureren van het beleid voor de inhoud van de sleutels. Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het beleid voor de inhoud van de sleutels, moet u configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het beleid voor de inhoud van de sleutels kan hebben **open** of **token** beperking. 

Met een tokenbeperking content key-beleid, is de inhoudssleutel alleen verzonden naar een client met daarin een geldige JSON Web Token (JWT) of simple webtoken (SWT) in de aanvraag voor sleutels/licenties. Dit token moet worden uitgegeven door een beveiligingstokenservice (STS). U kunt Azure Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. De belangrijkste leveringsservice voor Media Services retourneert de aangevraagde sleutels/licenties naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutels/licenties.

Wanneer u beleid met de tokenbeperking configureert, moet u de primaire verificatiesleutel, uitgever en doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever wordt de secure token service die de token uitgeeft. De doelgroep, ook wel genoemd bereik, het doel van het token wordt beschreven of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

## <a name="next-steps"></a>Volgende stappen

* [Beveiligen met AES-versleuteling](protect-with-aes128.md)
* [Beveiligen met DRM](protect-with-drm.md)
* [Multi-drm beveiligde inhoud systeem met toegangsbeheer ontwerpen](design-multi-drm-system-with-access-control.md)
* [Veelgestelde vragen](frequently-asked-questions.md)


