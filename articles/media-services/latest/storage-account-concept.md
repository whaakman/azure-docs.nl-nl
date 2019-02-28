---
title: Uploaden en opslag met Azure Media Services | Microsoft Docs
description: In dit artikel cloud uploaden en concepten van storage.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: cda029dd11e8cb4cb07e9fce7eef95d6d4d78d7e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960209"
---
# <a name="cloud-upload-and-storage"></a>Uploaden naar en opslaan in de cloud

Als u wilt beheren, coderen, codering, analyseren en streaming van media-inhoud in Azure starten, moet u een Media Services-account maken. Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. De opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. 

Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden. Het wordt sterk aanbevolen storage-accounts op dezelfde locatie bevinden als het Media Services-account gebruiken om te voorkomen dat de kosten voor extra latentie en gegevens uitgaand

U kunt maar één **primaire** opslagaccount koppelen aan uw Media Services-account, maar een onbeperkt aantal **secundaire** opslagaccounts. Media Services ondersteunt **GPv2**-accounts (General-purpose v2) of **GPv1**-accounts (General-purpose v1). 

>[!NOTE]
> Blob-accounts kunt u niet instellen als **primaire** account. 

We raden u aan gpv2-Opslagaccounts te gebruiken, zodat u kunt profiteren van het kiezen tussen hot en cool storage-lagen. Zie voor meer informatie over de storage-accounts, [overzicht van Azure Storage-account](../../storage/common/storage-account-overview.md). 

Er zijn verschillende SKU's u voor uw storage-account kiezen kunt. Zie [Opslagaccounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest) voor meer informatie. Als u wilt experimenteren met opslagaccounts, gebruikt u `--sku Standard_LRS`. Als u echter een SKU voor productie selecteert, kunt u overwegen om `--sku Standard_RAGRS` te gebruiken. Deze biedt geografische replicatie voor bedrijfscontinuïteit. 

## <a name="assets-in-a-storage-account"></a>Assets in een storage-account

In Media Services v3, worden de Storage-API's gebruikt om bestanden te uploaden naar activa. Zie voor meer informatie, [activa concept](assets-concept.md).

> [!Note]
> U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door de Media Services SDK zonder gebruik van Media Services-API's te wijzigen.
 
## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het koppelen van een storage-account aan uw Media Services-account, [maken van een account](create-account-cli-quickstart.md).
