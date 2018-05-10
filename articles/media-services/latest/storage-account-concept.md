---
title: Storage-accounts in Azure Media Services | Microsoft Docs
description: Dit artikel wordt beschreven hoe Media Services maakt gebruik van storage-accounts.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 6d4c21867b0b46508f348300ae2b9553a75d23b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="storage-accounts"></a>Opslagaccounts

Wanneer u een Media Services-account maakt, moet u de naam van de resource van een Azure Storage-account opgeven. Het opgegeven opslagaccount is gekoppeld aan uw Media Services-account. 

U moet een hebben **primaire** storage-account en u kunt elk gewenst aantal hebben **secundaire** storage-accounts die zijn gekoppeld aan uw Media Services-account. Media Services ondersteunt **voor algemene doeleinden v2** (GPv2) of **voor algemene doeleinden v1** accounts (GPv1). 

>[!NOTE]
> Alleen BLOB-accounts zijn niet toegestaan als **primaire**. 

Het is raadzaam dat u GPv2, zodat u kunt profiteren van kiezen tussen hot en cool opslaglagen. Zie voor meer informatie over de storage-accounts, [opties voor Azure Storage-account](../../storage/common/storage-account-options.md). 

## <a name="assets-in-a-storage-account"></a>Activa in een opslagaccount

In Media Services v3, worden de Storage-API's gebruikt om bestanden te uploaden. Bekijk informatie over het Storage-API's met Media Services gebruiken voor het uploaden van uw invoerbestanden, [maken van een taak voor invoer van een lokaal bestand](job-input-from-local-file-how-to.md). 

> [!Note]
> U moet niet proberen om de inhoud van de blob-containers die zijn gegenereerd door de Media Services SDK zonder gebruik van Media Services-API's te wijzigen.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het koppelen van een opslagaccount aan uw Media Services-account, [maken van een account](create-account-cli-quickstart.md).
