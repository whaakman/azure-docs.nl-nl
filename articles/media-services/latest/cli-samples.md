---
title: Azure CLI-voorbeelden - Azure Media Services | Microsoft Docs
description: Azure CLI-voorbeelden voor Azure Media Services-service
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: bbd57933993e22dd32b84f1d44175bb3b3d749c9
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672422"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-voorbeelden voor Azure Media Services

De volgende tabel bevat koppelingen naar de Azure CLI-voorbeelden voor Azure Media Services.

## <a name="examples"></a>Voorbeelden

|  |  |
|---|---|
|**Schalen**||
| [Gereserveerde schaal Media-eenheden](media-reserved-units-cli-how-to.md)|Voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, is het raadzaam om in te richten van uw account met 10 S3 groepsbeleidsinstelling. <br/>Het script laat zien hoe CLI gebruiken om te schalen van gereserveerde Media-eenheden (groepsbeleidsinstelling).|
|**Account**||
| [Een Media Services-account maken](create-account-cli-how-to.md) | Hiermee maakt u een Azure Media Services-account. Bovendien maakt een service-principal die toegang tot API's voor het programmatisch beheren van het account kan worden gebruikt. |
| [Opnieuw instellen van accountreferenties](./scripts/cli-reset-account-credentials.md)|Hiermee stelt u referenties voor uw account en de instellingen app.config terug worden opgehaald.|
|**Activa**||
| [Activa maken](./scripts/cli-create-asset.md)|Hiermee maakt u een Media Services-Asset om inhoud te uploaden.|
| [Een bestand uploaden](./scripts/cli-upload-file-asset.md)|Hiermee wordt een lokaal bestand geüpload naar een opslagcontainer.|
| **Transformeert** en **taken**||
| [Transformaties maken](./scripts/cli-create-transform.md)|Laat zien hoe transformaties maken. Transformaties geven een beschrijving van de eenvoudige werkstroom van taken voor het verwerken van video- of audiobestanden (vaak 'recipe' genoemd).<br/> Kijk altijd eerst of er al een transformatie bestaat met de gewenste naam 'recipe'. Als dit het geval is, dit opnieuw gebruiken. |
| [Taken maken](./scripts/cli-create-jobs.md)|Verzendt een taak is een eenvoudige codering transformeren met behulp van HTTPs-URL.|
| [EventGrid maken](./scripts/cli-create-event-grid.md)|Hiermee maakt u een account op Event Grid-abonnement voor de taak de status verandert.|
| **Deliver**||
| [Een asset publiceren](./scripts/cli-publish-asset.md)| Hiermee maakt u een Streaming-Locator gemaakt en wordt in Streaming-URL's. |
| [Filter](filters-dynamic-manifest-cli-howto.md)| Hiermee configureert u een filter voor een on-Demand Video asset en laat zien hoe u met behulp van CLI maken [Accountfilters](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) en [Asset Filters](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Zie ook

- [Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
- [Snelstart: Stream-video's - CLI](stream-files-cli-quickstart.md)
