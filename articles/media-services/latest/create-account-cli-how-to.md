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
ms.date: 05/19/2019
ms.author: juliako
ms.openlocfilehash: f2cb2e2ee6393a59125ee879f2058516eb50d6b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65956736"
---
# <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

Als u wilt beginnen met coderen, codering, analyseren, beheren en streamen van mediainhoud in Azure, moet u een Media Services-account maken. Het Media Services-account moet worden gekoppeld aan een of meer opslagaccounts.

> [!NOTE]
> Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden. Het wordt sterk aangeraden opslagaccounts te gebruiken op dezelfde locatie als het Media Services-account om aanvullende kosten voor latentie en uitgaande data te vermijden.

Dit artikel wordt beschreven stappen voor het maken van een nieuwe Azure Media Services-account met de Azure CLI.  

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

* [Toegang tot v3 API 's](access-api-cli-how-to.md)
* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Een secundaire opslag koppelen aan een Media Services-account](https://docs.microsoft.com/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

## <a name="see-also"></a>Zie ook

[Azure-CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

