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
ms.date: 02/05/2019
ms.author: juliako
ms.openlocfilehash: a447c359c38c2173ea42b6d717067fc8b3a88f9a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875488"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Veelgestelde vragen over Azure Media Services v3

In dit artikel geeft antwoord op veelgestelde vragen over Azure Media Services (AMS)-v3.

## <a name="v3-apis"></a>V3 API 's

### <a name="how-do-i-configure-media-reserved-units"></a>Hoe kan ik gereserveerde Media-eenheden configureren?

Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. Als u meer dan 10 S3 groepsbeleidsinstelling nodig hebt, opent u een ondersteuning ticket met de [Azure-portal](https://portal.azure.com/).

Zie voor meer informatie, [met CLI mediaverwerking schalen](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>Wat is de aanbevolen methode voor het proces video's?

Het is raadzaam dat u met behulp van een HTTP (s)-URL die verwijst naar de video-taken verzenden. Voor meer informatie, [HTTP (s) opnemen](job-input-from-http-how-to.md). U bent niet verplicht een Asset te maken met de invoervideo voordat deze kan worden verwerkt.

### <a name="how-does-pagination-work"></a>Hoe werkt de paginering?

Wanneer u paginering, moet u altijd de volgende koppeling voor het opsommen van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina gebruiken. Zie voor meer informatie en voorbeelden, [filteren, bestellen en](entities-overview.md).

## <a name="live-streaming"></a>Live streamen 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Hoe kan ik einden/video's en afbeelding invoegen slates tijdens live stream?

Live encoding van Media Services v3 nog ondersteunt geen invoegen video- of afbeeldingsbestanden slates tijdens live stream. 

U kunt een [on-premises coderingsprogramma live](recommended-on-premises-live-encoders.md) om over te schakelen de bronvideo. Veel apps bieden de mogelijkheid om over te schakelen van bronnen, met inbegrip van Telestream Wirecast, schakelbaar Studio (op iOS), IB-Studio (gratis app) en nog veel meer.

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
