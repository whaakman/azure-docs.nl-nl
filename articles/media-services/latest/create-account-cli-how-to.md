---
title: Een Azure Media Services-account maken met CLI 2.0 | Microsoft Docs
description: Volg de stappen van deze snelstartgids een Azure Media Services-account wilt maken.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: a9660ac61bab9f8b9eb9563aab4cc584786b25ae
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

Als u wilt versleutelen, codering, analyseren, beheren en streaming mediainhoud in Azure start, moet u een Media Services-account maken. Op het moment dat u een Media Services-account maakt, maakt u ook een bijbehorend opslagaccount (of gebruikt u een bestaand account) in hetzelfde geografische gebied als het Media Services-account.

Dit onderwerp beschrijft de stappen voor het maken van een nieuw Azure Media Services-account met CLI 2.0.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de [Azure-portal](http://portal.azure.com) en start **CloudShell** CLI-opdrachten uitvoeren, zoals wordt weergegeven in de volgende stappen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Het Azure-abonnement instellen

Geef de Azure-abonnement-ID die u wilt gebruiken voor het Media Services-account in de volgende opdracht. U ziet een lijst met abonnementen die u toegang tot hebt door te navigeren naar [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand Stream](stream-files-dotnet-quickstart.md)
