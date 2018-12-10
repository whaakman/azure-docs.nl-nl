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
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 9df9279a3922fa639385657660d6d0f4a55b5a4d
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53132769"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-voorbeelden voor Azure Media Services

De volgende tabel bevat koppelingen naar de Azure CLI-voorbeelden voor Azure Media Services.

## <a name="examples"></a>Voorbeelden

|  |  |
|---|---|
|**Account**||
| [Een Media Services-account maken](./scripts/cli-create-account.md) | Hiermee maakt u een Azure Media Services-account. Bovendien maakt een service-principal die toegang tot API's voor het programmatisch beheren van het account kan worden gebruikt. |
| [Opnieuw instellen van accountreferenties](./scripts/cli-reset-account-credentials.md)|Hiermee stelt u referenties voor uw account en de instellingen app.config terug worden opgehaald.|
|**Activa**||
| [Activa maken](./scripts/cli-create-asset.md)|Hiermee maakt u een Media Services-Asset om inhoud te uploaden.|
| [Een bestand uploaden](./scripts/cli-upload-file-asset.md)|Hiermee wordt een lokaal bestand geüpload naar een opslagcontainer.|
| [Een asset publiceren](./scripts/cli-publish-asset.md)| Hiermee maakt u een Streaming-Locator gemaakt en wordt in Streaming-URL's. |
| **Transformeert** en **taken**||
| [Transformaties maken](./scripts/cli-create-transform.md)|Laat zien hoe transformaties maken. Transformaties geven een beschrijving van de eenvoudige werkstroom van taken voor het verwerken van video- of audiobestanden (vaak 'recipe' genoemd).<br/> Kijk altijd eerst of er al een transformatie bestaat met de gewenste naam 'recipe'. Als dit het geval is, dit opnieuw gebruiken. |
| [Taken maken](./scripts/cli-create-jobs.md)|Verzendt een taak is een eenvoudige codering transformeren met behulp van HTTPs-URL.|
| [EventGrid maken](./scripts/cli-create-event-grid.md)|Hiermee maakt u een account op Event Grid-abonnement voor de taak de status verandert.|

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
