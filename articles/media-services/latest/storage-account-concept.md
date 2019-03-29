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
ms.date: 03/28/2019
ms.author: juliako
ms.openlocfilehash: 96c3a3eb5e4c07ad9cad8ea5060a27c0c33eec5f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621295"
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
 
## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel laat zien hoe de versleuteling van opslag aan de serverzijde in Media Services v3 werkt:

|Optie voor opslagversleuteling|Description|Media Services v3|
|---|---|---|
|Media Services-Storage-versleuteling| AES-256-codering, sleutel beheerd door Media Services|Niet ondersteund<sup>(1)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|

<sup>1</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over het koppelen van een storage-account aan uw Media Services-account, [maken van een account](create-account-cli-quickstart.md).
