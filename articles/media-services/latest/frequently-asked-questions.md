---
title: Veelgestelde vragen over Azure Media Services v3 | Microsoft Docs
description: In dit artikel geeft antwoord op veelgestelde vragen over Azure Media Services-v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/20/2019
ms.author: juliako
ms.openlocfilehash: a815d03dd4e7163d4d9f00ce8f9c16f1b3055ce9
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56652328"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Veelgestelde vragen over Azure Media Services v3

In dit artikel geeft antwoord op veelgestelde vragen over Azure Media Services (AMS)-v3.

## <a name="v3-apis"></a>V3 API 's

### <a name="how-do-i-configure-media-reserved-units"></a>Hoe kan ik gereserveerde Media-eenheden configureren?

Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

Zie voor meer informatie, [met CLI mediaverwerking schalen](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Wat is de aanbevolen methode voor het proces video's?

Gebruik [transformeert](https://docs.microsoft.com/rest/api/media/transforms) het configureren van algemene taken voor het coderen of video's analyseren. Elke **transformeren** beschrijft een recept of een workflow van taken voor het verwerken van uw video- of audio-bestanden. Een [taak](https://docs.microsoft.com/rest/api/media/jobs) is van de werkelijke aanvraag voor het Media Services om toe te passen de **transformeren** aan een bepaalde invoer video of audio-inhoud. Zodra de transformatie is gemaakt, kunt u taken met behulp van Media Services-API's of een van de gepubliceerde SDK's kunt indienen. Zie [Transformaties en taken](transforms-jobs-concept.md) voor meer informatie.

### <a name="how-does-pagination-work"></a>Hoe werkt de paginering?

Wanneer u paginering, moet u altijd de volgende koppeling voor het opsommen van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina gebruiken. Zie voor meer informatie en voorbeelden, [filteren, bestellen en](entities-overview.md).

## <a name="live-streaming"></a>Live streamen 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Hoe kan ik einden/video's en afbeelding invoegen slates tijdens live stream?

Live encoding van Media Services v3 nog ondersteunt geen invoegen video- of afbeeldingsbestanden slates tijdens live stream. 

U kunt een [on-premises coderingsprogramma live](recommended-on-premises-live-encoders.md) om over te schakelen de bronvideo. Veel apps bieden de mogelijkheid om over te schakelen van bronnen, met inbegrip van Telestream Wirecast, schakelbaar Studio (op iOS), IB-Studio (gratis app) en nog veel meer.

## <a name="content-protection"></a>Inhoudsbeveiliging

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Hoe en waar u kunt JWT-token ophalen om de aanvraag-licentie of sleutel u?

1. Voor de productie moet u een beveiligde Token Services (STS) (webservice) geeft JWT-token van een HTTPS-aanvraag hebt. Voor testen, kunt u de code die wordt weergegeven **GetTokenAsync** methode die is gedefinieerd [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Player moet een aanvraag maken nadat een gebruiker is geverifieerd, naar de STS voor dergelijke een token en wijs deze toe aan de waarde van het token. U kunt de [API van Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/).

* Zie voor een voorbeeld van het uitvoeren van de STS, symmetrische en asymmetrische sleutel [ http://aka.ms/jwt ](https://aka.ms/jwt). 
* Zie voor een voorbeeld van een-op basis van Azure Media Player met behulp van dergelijke JWT-token speler [ http://aka.ms/amtest ](https://aka.ms/amtest) ('player_settings' koppeling om te zien van de token invoer uitvouwen).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Hoe autoriseert u aanvragen voor stream-video's met AES-versleuteling

De juiste aanpak is het gebruikmaken van de STS (Secure Token Service):

STS, afhankelijk van het gebruikersprofiel, voeg verschillende claims (zoals 'Premium-gebruiker', 'Basic gebruiker', 'Gratis proefversie van gebruiker'). Met andere claims in een JWT ziet de gebruiker andere inhoud. Voor andere inhoud/asset, wordt de ContentKeyPolicyRestriction beschikken over de bijbehorende RequiredClaims.

Gebruik Azure Media Services-API's voor het configureren van/licentiesleutel levering en uw activa versleutelen (zoals wordt weergegeven in [in dit voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs).

Zie voor meer informatie:

- [Overzicht van de beveiliging van inhoud](content-protection-overview.md)
- [Ontwerp van een multi-DRM-beveiliging van inhoud-systeem met toegangsbeheer](design-multi-drm-system-with-access-control.md)

## <a name="media-services-v2-vs-v3"></a>Media Services v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kan ik de Azure-portal gebruiken om v3-resources te beheren?

Nog niet. U kunt een van de ondersteunde SDK's gebruiken. Zie de zelfstudies en voorbeelden in deze set doc-bestand.

### <a name="is-there-an-assetfile-concept-in-v3"></a>Is er een concept AssetFile in v3?

De AssetFiles zijn verwijderd uit de AMS-API om te scheiden van Media Services van afhankelijkheid van de Storage-SDK. Opslag, geen Media Services, houdt u nu de informatie die deel uitmaakt van in opslag. 

Zie voor meer informatie, [migreren naar Media Services v3](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>Waar is de versleuteling van opslag van de client-side gebleven?

Het verdient aanbeveling nu gebruik van de server-side-storage-versleuteling (dit is standaard ingeschakeld). Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht van Media Services v3](media-services-overview.md)
