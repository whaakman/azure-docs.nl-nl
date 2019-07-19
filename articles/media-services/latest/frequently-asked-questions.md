---
title: Veelgestelde vragen over Azure Media Services v3 | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 06/21/2019
ms.author: juliako
ms.openlocfilehash: 766208c01f27d2024025b7a202bc3724b4fc9fff
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311838"
---
# <a name="media-services-v3-frequently-asked-questions"></a>Veelgestelde vragen over Media Services v3

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Media Services (AMS).

## <a name="general"></a>Algemeen

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Wat Azure-rollen kunnen acties uitvoeren op Azure Media Services resources? 

Zie [op rollen gebaseerd toegangs beheer (RBAC) voor Media Services accounts](rbac-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Hoe kan ik gereserveerde media-eenheden configureren?

Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

Zie [Media verwerking schalen met CLI](media-reserved-units-cli-how-to.md)voor meer informatie.

### <a name="what-is-the-recommended-method-to-process-videos"></a>Wat is de aanbevolen methode om Video's te verwerken?

[Trans formaties](https://docs.microsoft.com/rest/api/media/transforms) gebruiken om algemene taken te configureren voor het coderen of analyseren van Video's. Elke **trans formatie** beschrijft een recept of een werk stroom van taken voor het verwerken van uw video-of audio bestanden. Een [taak](https://docs.microsoft.com/rest/api/media/jobs) is de daad werkelijke aanvraag om Media Services om de **trans formatie** toe te passen op een gegeven video-of audio-inhoud. Zodra de trans formatie is gemaakt, kunt u taken verzenden met behulp van Media Services Api's of een van de gepubliceerde Sdk's. Zie [Transformaties en taken](transforms-jobs-concept.md) voor meer informatie.

### <a name="how-does-pagination-work"></a>Hoe werkt de paginering?

Wanneer u paginering gebruikt, moet u altijd de volgende koppeling gebruiken om de verzameling op te sommen en niet afhankelijk van een bepaalde pagina grootte. Zie [filteren, ordenen, paginering](entities-overview.md)voor meer informatie en voor beelden.

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Welke functies zijn nog niet beschikbaar in Azure Media Services v3?

Zie voor meer informatie [functie hiaten met betrekking tot v2-api's](migrate-from-v2-to-v3.md#feature-gaps-with-respect-to-v2-apis).

### <a name="what-is-the-process-of-moving-a-media-services-account-between-subscriptions"></a>Wat is het proces van het verplaatsen van een Media Services account tussen abonnementen?  

Zie [een Media Services account verplaatsen tussen abonnementen](media-services-account-concept.md)voor meer informatie.

## <a name="live-streaming"></a>Live streamen 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Hoe kan ik pauzes/Video's en afbeeldings pastels invoegen tijdens Live Stream?

Media Services v3 Live encoding biedt nog geen ondersteuning voor het invoegen van video-of afbeeldings pastels tijdens live stream. 

U kunt een [Live on-premises encoder](recommended-on-premises-live-encoders.md) gebruiken om de bron video te scha kelen. Veel apps bieden de mogelijkheid om bronnen te wisselen, waaronder Telestream Wirecast, Switch Studio (op iOS), IB Studio (gratis app) en nog veel meer.

## <a name="content-protection"></a>Inhoudsbeveiliging

### <a name="should-i-use-an-aes-128-clear-key-encryption-or-a-drm-system"></a>Moet ik een AES-128 Clear Key Encryption of een DRM-systeem gebruiken?

Klanten zich vaak afvragen of ze AES-versleuteling of een DRM-systeem gebruiken moeten. Het belangrijkste verschil tussen de twee systemen is dat met AES-versleuteling de inhouds sleutel via TLS naar de client wordt verzonden, zodat de sleutel tijdens de overdracht wordt versleuteld, maar zonder extra versleuteling (' in de Clear '). Als gevolg hiervan is de sleutel die wordt gebruikt om de inhoud te ontsleutelen toegankelijk voor de client speler en kan deze worden weer gegeven in een netwerk tracering op de client als tekst zonder opmaak. Een AES-128 Clear sleutel versleuteling is geschikt voor gebruik waarbij de viewer een vertrouwde partij is (bijvoorbeeld het versleutelen van bedrijfs Video's die binnen een bedrijf worden gedistribueerd om door werk nemers te worden weer gegeven).

DRM-systemen zoals PlayReady, Widevine en FairPlay bieden een extra versleutelings niveau voor de sleutel die wordt gebruikt om de inhoud te ontsleutelen vergeleken met een AES-128 Clear-sleutel. De inhouds sleutel wordt versleuteld met een sleutel die wordt beveiligd door de DRM-runtime, in aanvulling op transport niveau versleuteling van TLS. Bovendien wordt ontsleuteling verwerkt in een beveiligde omgeving op het niveau van het besturingssysteem, waar is het moeilijker voor een kwaadwillende gebruiker om aan te vallen. DRM wordt aanbevolen voor gebruik gevallen waarbij de viewer mogelijk niet een vertrouwde partij en u het hoogste niveau van beveiliging nodig hebt.

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Hoe en waar u kunt JWT-token ophalen om de aanvraag-licentie of sleutel u?

1. Voor productie moet u een STS (Secure token Services) (webservice) hebben die een JWT-token verleent op een HTTPS-aanvraag. Voor testen, kunt u de code die wordt weergegeven **GetTokenAsync** methode die is gedefinieerd [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Player moet een aanvraag maken nadat een gebruiker is geverifieerd, naar de STS voor dergelijke een token en wijs deze toe aan de waarde van het token. U kunt de [API van Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Zie voor een voorbeeld van het uitvoeren van de STS, symmetrische en asymmetrische sleutel [ https://aka.ms/jwt ](https://aka.ms/jwt). 
* Zie voor een voorbeeld van een-op basis van Azure Media Player met behulp van dergelijke JWT-token speler [ https://aka.ms/amtest ](https://aka.ms/amtest) ('player_settings' koppeling om te zien van de token invoer uitvouwen).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Hoe autoriseert u aanvragen voor stream-video's met AES-versleuteling

De juiste aanpak is het gebruikmaken van de STS (Secure Token Service):

In STS, afhankelijk van het gebruikers profiel, kunt u verschillende claims toevoegen (zoals ' Premium User ', ' Basic User ', ' gebruiker met gratis proef versie '). Met andere claims in een JWT ziet de gebruiker andere inhoud. Voor andere inhoud/asset, wordt de ContentKeyPolicyRestriction beschikken over de bijbehorende RequiredClaims.

Gebruik Azure Media Services Api's voor het configureren van de levering van licenties en sleutels en het versleutelen van uw assets (zoals weer gegeven in [dit voor beeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Zie voor meer informatie:

- [Overzicht van inhoudsbeveiliging](content-protection-overview.md)
- [Ontwerp van een inhoudsbeveiligingssysteem van een multi-DRM met toegangsbeheer](design-multi-drm-system-with-access-control.md)

### <a name="http-or-https"></a>HTTP of HTTPS?
De speler ASP.NET MVC-toepassing moet ondersteunen het volgende:

* Verificatie van de gebruiker via Azure AD, deze bevindt zich onder HTTPS.
* JWT-uitwisseling tussen de client en de Azure AD, deze bevindt zich onder HTTPS.
* DRM-licentie overname door de client, die onder HTTPS worden moet als licentielevering is opgegeven door Media Services. De PlayReady-productpakket verplichten niet HTTPS voor de licentielevering van. Als uw PlayReady-licentie-server buiten het Media Services is, kunt u via HTTP of HTTPS.

De ASP.NET-player-toepassing maakt gebruik van HTTPS als een best practice, zodat de Media Player is op een HTTPS-pagina. HTTP is echter verkozen voor streaming, zodat u moet rekening houden met de uitgifte van gemengde inhoud.

* Gemengde inhoud toegestaan niet in de browser. Maar plug-ins zoals Silverlight en de invoegtoepassing voor OSMF soepel te verwerken en STREEPJES worden toegestaan. Gemengde inhoud is een beveiligingsprobleem vanwege de dreiging van de mogelijkheid om te ' injecteren ' schadelijke JavaScript, waardoor de gegevens van de klant moet lopen. Browsers deze mogelijkheid standaard geblokkeerd. De enige manier om dit oplossen door het is aan de serverzijde (oorsprong) doordat alle domeinen (ongeacht HTTPS of HTTP). Dit is waarschijnlijk niet een goed idee een.
* Vermijd gemengde inhoud. De player-toepassing en de Media Player moet HTTP of HTTPS te gebruiken. Tijdens het afspelen van gemengde inhoud, is de tech silverlightSS vereist een waarschuwing gemengde inhoud uit te schakelen. De techniek flashSS verwerkt gemengde inhoud zonder een waarschuwing gemengde inhoud.
* Als uw streaming-eindpunt is gemaakt vóór augustus 2014, niet HTTPS wordt ondersteund. In dit geval maken en gebruiken van een nieuwe streaming-eindpunt voor HTTPS.

### <a name="what-about-live-streaming"></a>Hoe zit live streamen?

Kunt u exact het dezelfde ontwerpen en implementeren om te beveiligen met live streaming in Media Services van de asset die zijn gekoppeld aan een programma als een activum VOD te behandelen. Als u een multi-DRM-beveiliging van de live-inhoud wilt bieden, moet u dezelfde instelling/verwerking Toep assen op de Asset alsof het een VOD-Asset was voordat u de Asset koppelt aan de live uitvoer.

### <a name="what-about-license-servers-outside-media-services"></a>Hoe zit het met licentieservers buiten Media Services?

Vaak klanten in een serverfarm licentie geïnvesteerd in hun eigen Datacenter of een gehost door serviceproviders DRM. Met Media Services content protection, kunt u gebruiken in de hybride-modus. De inhoud kunnen worden gehost en dynamisch in Media Services, beveiligd terwijl DRM-licenties worden geleverd door servers buiten Media Services. In dit geval kunt u overwegen de volgende wijzigingen:

* STS moet uitgeven van tokens die worden geaccepteerd en kunnen worden gecontroleerd door de licentie voor server-farm. De Widevine-licentieservers geleverd door Axinom vereisen bijvoorbeeld een specifieke JWT die een bericht rechten bevat. Daarom moet u een STS om uit te geven die een JWT hebben. 
* U moet niet meer licentieleveringsservice configureren in Media Services. U moet de licentie overname URL's opgeven (voor PlayReady, Widevine en FairPlay) wanneer u ContentKeyPolicies configureert.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Wat gebeurt er als ik wil een aangepaste STS gebruiken?

Een klant kan kiezen voor een aangepaste STS JWTs opgeven. Redenen zijn:

* STS biedt geen ondersteuning voor de id-provider die wordt gebruikt door de klant. In dit geval een aangepaste STS mogelijk een optie.
* De klant mogelijk meer flexibele of betere controle STS integreren met de klant abonnee factureringssysteem. Bijvoorbeeld, een operator MVPD biedt mogelijk meerdere OTT-abonnee pakketten, zoals basic, premium en sport. De operator wilt overeenkomen met de claims in een token met de abonnee pakket zodat alleen de inhoud in een specifiek pakket beschikbaar worden gesteld. In dit geval een aangepaste STS biedt de benodigde flexibiliteit en controle.

Wanneer u een aangepaste STS, moet twee worden gewijzigd:

* Wanneer u een service voor het leveren van licenties voor een asset configureert, moet u opgeven de beveiligingssleutel gebruikt voor verificatie door de aangepaste STS in plaats van de huidige sleutel van Azure AD. (Meer informatie volgt.) 
* Wanneer een JTW-token wordt gegenereerd, een beveiligingssleutel is opgegeven in plaats van de persoonlijke sleutel van de huidige X509 certificaat in Azure AD.

Er zijn twee soorten sleutels:

* Symmetrische sleutel: Dezelfde sleutel wordt gebruikt om een JWT te genereren en te verifiëren.
* Asymmetrische sleutel: Een combi natie van open bare en persoonlijke sleutels in een x509-certificaat wordt gebruikt met een persoonlijke sleutel voor het versleutelen/genereren van een JWT en met de open bare sleutel om het token te verifiëren.

> [!NOTE]
> Als u .NET Framework / C# als uw ontwikkelplatform, de X509 certificaat dat wordt gebruikt voor een asymmetrische beveiligingssleutel moet een sleutellengte van ten minste 2048 hebben. Dit is een vereiste van de klasse System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework. Anders is de volgende uitzondering opgetreden:
> 
> IDX10630: De ' System. Identity model. tokens. X509AsymmetricSecurityKey ' voor ondertekening mag niet kleiner zijn dan ' 2048 ' bits.

## <a name="media-services-v2-vs-v3"></a>Media Services v2 VS v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kan ik de Azure Portal gebruiken om v3-resources te beheren?

U kunt momenteel geen gebruik maken van de Azure-portal om v3-resources te beheren. Gebruik de [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) of een van de ondersteunde [SDK's](media-services-apis-overview.md#sdks).

### <a name="is-there-an-assetfile-concept-in-v3"></a>Is er een AssetFile-concept in v3?

De AssetFiles zijn verwijderd uit de AMS-API om Media Services te scheiden van de opslag-SDK-afhankelijkheid. Nu opslag, niet Media Services, houdt de informatie bij die bij de opslag hoort. 

Zie [Migrate to Media Services v3](migrate-from-v2-to-v3.md)(Engelstalig) voor meer informatie.

### <a name="where-did-client-side-storage-encryption-go"></a>Waar is de opslag versleuteling aan de client zijde gebleven?

U wordt aangeraden de opslag versleuteling aan de server zijde te gebruiken (deze is standaard ingeschakeld). Zie [Azure Storage-service versleuteling voor Data-at-rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Media Services v3](media-services-overview.md)
