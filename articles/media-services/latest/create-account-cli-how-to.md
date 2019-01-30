---
title: Een Media Services-account maken met de Azure CLI - Azure | Microsoft Docs
description: Volg de stappen van deze snelstartgids om een Azure Media Services-account te maken.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: b01958dc320ff6c81bb27c85ff4a5e508eabb01b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209941"
---
# <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

Als u wilt beginnen met coderen, codering, analyseren, beheren en streamen van mediainhoud in Azure, moet u een Media Services-account maken. Op het moment u een Media Services-account create, maakt u ook een gekoppelde storage-account (of gebruik een bestaande resourcegroep).  

> [!NOTE]
> De Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement. Het verdient aanbeveling gebruik van storage-accounts op dezelfde locatie bevinden als het Media Services-account.

Dit artikel wordt beschreven stappen voor het maken van een nieuwe Azure Media Services-account met de Azure CLI.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="set-the-azure-subscription"></a>Het Azure-abonnement instellen

Geef in de volgende opdracht de id van het Azure-abonnement op dat u wilt gebruiken voor het Media Services-account. U kunt een lijst met abonnementen bekijken waartoe u toegang hebt door naar [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) te gaan.

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

