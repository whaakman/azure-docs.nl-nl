---
title: Toegang tot de Azure Media Services-API - Azure CLI | Microsoft Docs
description: Volg de stappen van deze instructies voor toegang tot de API van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 257fe51cae245708816cd9a7bb0c33b6edf5aa05
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756006"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Toegang tot Azure mediaservices API met de Azure CLI
 
U moet de verificatie van Azure AD-service-principal gebruiken om verbinding maken met de API van Azure Media Services. De toepassing nodig heeft om aan te vragen van een Azure AD-token met de volgende parameters:

* Azure AD-tenant-eindpunt
* Media Services-resource-URI
* Resource-URI voor de REST-mediaservices
* Waarden van Azure AD-toepassing: de client-ID en clientgeheim

In dit artikel leest u hoe de Azure CLI gebruiken voor een Azure AD-toepassing en service-principal maken en ophalen van de waarden die nodig zijn voor toegang tot resources van Azure Media Services.

## <a name="prerequisites"></a>Vereisten 

[Een Azure Media Services-account maken](create-account-cli-how-to.md).

Vergeet niet de waarden die u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Zie ook

- [Gereserveerde media-eenheden schalen - CLI](media-reserved-units-cli-how-to.md)
- [Een Azure Media Services-account maken - CLI](./scripts/cli-create-account.md) 
- [De referenties voor het account opnieuw instellen - CLI](./scripts/cli-reset-account-credentials.md)
- [Assets maken - CLI](./scripts/cli-create-asset.md)
- [Een bestand uploaden - CLI](./scripts/cli-upload-file-asset.md)
- [Transformaties maken - CLI](./scripts/cli-create-transform.md)
- [Taken maken - CLI](./scripts/cli-create-jobs.md)
- [EventGrid maken - CLI](./scripts/cli-create-event-grid.md)
- [Een asset publiceren - CLI](./scripts/cli-publish-asset.md)
- [Filteren - CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="next-steps"></a>Volgende stappen

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
