---
title: Azure CLI Sample Script - de Stack licht in een virtueel Machin taakverdeling Schaalset implementeren | Microsoft Docs
description: Een extensie voor aangepaste scripts gebruiken voor het implementeren van de Stack licht in een load = taakverdeling virtuele-machineschaalset ingesteld op Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 23170923d7c05c9b7230cf331725250b2a3c0f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>De stack licht in een taakverdeling virtuele-machineschaalset implementeren

In dit voorbeeld maakt een virtuele-machineschaalset en van toepassing is een uitbreiding met een aangepast script voor het implementeren van de stack licht op elke virtuele machine in de schaalset.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Verbinding maken

Gebruik deze code om te zien hoe u verbinding maken met uw virtuele machines en schaal ingesteld.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Voer de volgende opdracht om de resourcegroep, de schaalset en virtuele machines te verwijderen en alle bijbehorende resources.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ vmss maken](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Hiermee maakt u een virtuele-machineschaalset |
| [AZ network Load Balancer-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Een eindpunt met gelijke taakverdeling toevoegen |
| [AZ vmss extensie instellen](https://docs.microsoft.com/cli/azure/vmss/extension#az_vmss_extension_set) | Het toestelnummer op dat de aangepaste script wordt uitgevoerd op de implementatie van een virtuele machine maken |
| [AZ vmss update-exemplaren](https://docs.microsoft.com/cli/azure/vmss#az_vmss_update_instances) | Het aangepaste script uitvoeren op de VM-exemplaren die zijn geïmplementeerd voordat u de uitbreiding is toegepast op de scale-set. |
| [AZ vmss schaal](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) | De schaal ingesteld door het toevoegen van meer VM-instanties opschalen. Aangepast script wordt uitgevoerd op deze wanneer ze zijn geïmplementeerd. |
| [lijst met AZ netwerken openbare-ip](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_list) | De IP-adressen van de virtuele machines gemaakt door de steekproef worden opgehaald. |
| [AZ netwerk lb weergeven](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_show) | Lees het frontend en backend poorten die worden gebruikt door de load balancer. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
