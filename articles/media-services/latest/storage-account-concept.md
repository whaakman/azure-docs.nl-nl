---
title: Storage-accounts in Azure Media Services | Microsoft Docs
description: In dit artikel wordt beschreven hoe Media Services maakt gebruik van storage-accounts.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 4668ff23710121e5495395d71902b9ffa4c01238
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220345"
---
# <a name="storage-accounts"></a>Opslagaccounts

Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. De opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. 

U kunt maar één **primaire** opslagaccount koppelen aan uw Media Services-account, maar een onbeperkt aantal **secundaire** opslagaccounts. Media Services ondersteunt **GPv2**-accounts (General-purpose v2) of **GPv1**-accounts (General-purpose v1). 

>[!NOTE]
> Blob-accounts kunt u niet instellen als **primaire** account. 

We raden u aan gpv2-Opslagaccounts te gebruiken, zodat u kunt profiteren van het kiezen tussen hot en cool storage-lagen. Zie voor meer informatie over de storage-accounts, [overzicht van Azure Storage-account](../../storage/common/storage-account-overview.md). 

De Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement. Het verdient aanbeveling gebruik van storage-accounts op dezelfde locatie bevinden als het Media Services-account.

## <a name="assets-in-a-storage-account"></a>Assets in een storage-account

In Media Services v3, worden de Storage-API's gebruikt om bestanden te uploaden. Bekijk voor meer informatie over het gebruik van Storage-API's met Media Services voor uw invoerbestanden uploaden, [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md). 

> [!Note]
> U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door de Media Services SDK zonder gebruik van Media Services-API's te wijzigen.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het koppelen van een storage-account aan uw Media Services-account, [maken van een account](create-account-cli-quickstart.md).
