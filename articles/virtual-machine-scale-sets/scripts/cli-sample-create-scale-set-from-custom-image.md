---
title: Voorbeelden van Azure CLI - Een aangepaste VM-installatiekopie gebruiken | Microsoft Docs
description: Azure CLI-voorbeelden
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: a74e84b05269acc0d9f98a221b9e496dbe5fc75f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986078"
---
# <a name="create-a-virtual-machine-scale-set-from-a-custom-vm-image-with-the-azure-cli"></a>Een virtuele-machineschaalset maken op basis van een aangepaste VM-installatiekopie met de Azure CLI
Met dit script maakt u een virtuele-machineschaalset die gebruikmaakt van een aangepaste VM-installatiekopie als bron voor VM-exemplaren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-custom-vm-image/use-custom-vm-image.sh "Create a virtual machine scale set with a custom VM image")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Gebruik de volgende opdracht om de resourcegroep, de schaalset en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Uitleg van het script
In dit script worden de volgende opdrachten gebruikt voor het maken van een resourcegroep, een virtuele-machineschaalset en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Hiermee maakt u de virtuele machine en verbindt u deze met het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Om het verkeer te distribueren naar de verschillende VM-exemplaren, wordt er ook een load balancer gemaakt. Met deze opdracht geeft u ook de VM-installatiekopie op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [az group delete](/cli/azure/ad/group#delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview) voor meer informatie over de Azure CLI.

U kunt extra Azure CLI-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor virtuele-machineschaalsets](../cli-samples.md).