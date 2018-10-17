---
title: Snelstart - Een Azure Media Services-account maken met Azure CLI| Microsoft Docs
description: Volg de stappen van deze snelstartgids om een Azure Media Services-account te maken.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: abed9fd8d466b582b534b365f4be4257f4986435
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735999"
---
# <a name="quickstart-create-an-azure-media-services-account"></a>Snelstartgids: Een Azure Media Services-account maken

> [!NOTE]
> De nieuwste versie van Azure Media Services (30-03-2018) is in preview. Deze versie wordt ook wel v3 genoemd. 

Ongeacht of u een ontwikkelaar of een maker van media-inhoud bent, hebt u een Media Services-account nodig voor het opslaan, versleutelen, coderen, beheren en streamen van media-inhoud in Azure. Als u een Media Services-account gaat maken, moet u de id van een Azure Storage-accountresource opgeven. Het opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. Deze opslagaccountresource moet zich in dezelfde geografische regio bevinden als het Media Services-account.  

In deze snelstart worden de stappen beschreven voor het maken van een nieuw Azure Media Services-account met behulp van de Azure-CLI.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij [Azure Portal](http://portal.azure.com) en start **CloudShell** om CLI-opdrachten uitvoeren, zoals wordt beschreven in de volgende stappen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Het Azure-abonnement instellen

Geef in de volgende opdracht de id van het Azure-abonnement op dat u wilt gebruiken voor het Media Services-account. U kunt een lijst met abonnementen bekijken waartoe u toegang hebt door naar [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) te gaan.

```azurecli-interactive
az account set --subscription <mySubscriptionId>
```

## <a name="create-an-azure-resource-group"></a>Een Azure-resourcegroep maken

Met de volgende opdracht maakt u de resourcegroep waarin u het Storage- en Media Services-account wilt opslaan. Vervang de tijdelijke aanduiding *myresourcegroup* door de naam die u hebt gebruiken voor de resourcegroep.

```azurecli-interactive
az group create -n <myresourcegroup> -l westus2
```

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Als u een Media Services-account gaat maken, moet u de id van een Azure Storage-accountresource opgeven. De opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. 

U kunt maar één **primaire** opslagaccount koppelen aan uw Media Services-account, maar een onbeperkt aantal **secundaire** opslagaccounts. Media Services ondersteunt **algemeen gebruik v2** of **algemeen gebruik v1**-accounts. Blob-opslagaccounts zijn niet toegestaan als **primaire** account. Zie [Overzicht van Azure-opslagaccount](../../storage/common/storage-account-overview.md) voor meer informatie over opslagaccounts. 

Met de volgende opdracht maakt u het opslagaccount dat wordt gekoppeld aan het Media Services-account (primair). Vervang in het onderstaande script de tijdelijke aanduiding *storageaccountforams*. De accountnaam moet uit minder dan 24 tekens bestaan.

```azurecli-interactive
az storage account create -n <storageaccountforams> -g <myresourcegroup>
```

## <a name="create-an-azure-media-services-account"></a>Een Azure Media Services-account maken

Hieronder vindt u de Azure CLI-opdrachten waarmee u een nieuw Media Services-account maakt. U hoeft alleen de volgende gemarkeerde waarden te vervangen:

* *myamsaccountname*
* *myresourcegroup*
* *storageaccountforams*

```azurecli-interactive
az ams create -n <myamsaccountname> -g <myresourcegroup> --storage-account <storageaccountforams>
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources in de resourcegroep niet meer nodig hebt, met inbegrip van het Media Services-account dat u hebt gemaakt in deze snelstartgids, verwijdert u de resourcegroep.

Voer in **CloudShell** de volgende opdracht uit:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand streamen](stream-files-dotnet-quickstart.md)
