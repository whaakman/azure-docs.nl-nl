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
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: dee7f831562dc1f4b2478d13b204aab1d8455e1e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733178"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-voorbeelden voor Azure Media Services

De volgende tabel bevat koppelingen naar de Azure CLI-voorbeelden voor Azure Media Services.

## <a name="examples"></a>Voorbeelden

|  |  |
|---|---|
|**Schalen**||
| [Gereserveerde schaal Media-eenheden](media-reserved-units-cli-how-to.md)|Voor de analysetaken van audio en video die worden geactiveerd door Media Services v3 of Video Indexer is het raadzaam om uw account in te richten met 10 S3 MRU’s. <br/>Het script laat zien hoe CLI gebruiken om te schalen van gereserveerde Media-eenheden (groepsbeleidsinstelling).|
|**Account**||
| [Een Azure Media Services-account maken](create-account-cli-how-to.md) | Het script maakt u een Azure Media Services-account. |
| [Opnieuw instellen van accountreferenties](./scripts/cli-reset-account-credentials.md)|Hiermee stelt u referenties voor uw account en de instellingen app.config terug worden opgehaald.|
|**Activa**||
| [Activa maken](./scripts/cli-create-asset.md)|Hiermee maakt u een Media Services-Asset om inhoud te uploaden.|
| [Een bestand uploaden](./scripts/cli-upload-file-asset.md)|Hiermee wordt een lokaal bestand geüpload naar een opslagcontainer.|
| **Transformeert** en **taken**||
| [Transformaties maken](./scripts/cli-create-transform.md)|Laat zien hoe transformaties maken. Transformaties geven een beschrijving van de eenvoudige werkstroom van taken voor het verwerken van video- of audiobestanden (vaak 'recipe' genoemd).<br/> U moet altijd controleren als een transformatie met een gewenste naam en 'recept' al bestaat. Als dit het geval is, dit opnieuw gebruiken. |
| [Coderen met een aangepaste transformatie](custom-preset-cli-howto.md) | Laat zien hoe u een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat te bouwen.|
| [Taken maken](./scripts/cli-create-jobs.md)|Verzendt een taak is een eenvoudige codering transformeren met behulp van HTTPs-URL.|
| [EventGrid maken](./scripts/cli-create-event-grid.md)|Hiermee maakt u een account op Event Grid-abonnement voor de taak de status verandert.|
| **Deliver**||
| [Een asset publiceren](./scripts/cli-publish-asset.md)| Hiermee maakt u een Streaming-Locator gemaakt en wordt in Streaming-URL's. |
| [Filter](filters-dynamic-manifest-cli-howto.md)| Hiermee configureert u een filter voor een on-Demand Video asset en laat zien hoe u met behulp van CLI maken [Accountfilters](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) en [Asset Filters](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Zie ook

- [Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
- [Snelstart: Videobestanden streamen - CLI](stream-files-cli-quickstart.md)
