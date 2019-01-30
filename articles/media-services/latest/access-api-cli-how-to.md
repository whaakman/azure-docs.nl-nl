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
ms.openlocfilehash: 43f9443e4b5cd700500bd9803f2737ed9e0aa633
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223158"
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

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
