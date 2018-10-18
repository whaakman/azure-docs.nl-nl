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
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 5559f9055da3a2a852427c0f27d367159cdc7655
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388503"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI-voorbeelden voor Azure Media Services

De volgende tabel bevat koppelingen naar de Azure CLI-voorbeelden voor Azure Media Services.

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
