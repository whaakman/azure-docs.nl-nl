---
title: Azure CLI-voorbeeldscript - virtuele machines opnieuw opstarten | Microsoft Docs
description: Azure CLI-voorbeeldscript - virtuele machines opnieuw starten door de tag en -ID
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
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ea114f484c774573b7d219cff9102a7308af356e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="restart-vms"></a>Opnieuw opstarten van virtuele machines

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Dit voorbeeld wordt een aantal manieren verkrijgen van een aantal virtuele machines en deze opnieuw opstarten.

De eerste start opnieuw op de virtuele machines in de resourcegroep.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

De tweede haalt de gecodeerde virtuele machines met `az resouce list` filters op de resources die VM's zijn en deze VMs opnieuw wordt opgestart.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Dit voorbeeld werkt in een Bash-shell. Zie voor opties op Azure CLI-scripts die worden uitgevoerd op Windows-client [uitvoeren van de Azure CLI in Windows](../windows/cli-options.md).


## <a name="sample-script"></a>Voorbeeld van een script

Het voorbeeld heeft drie scripts.
De eerste die voorziet in de virtuele machines.
De optie Nee wachten wordt gebruikt zodat de opdracht retourneert zonder te wachten op op elke virtuele machine moeten worden ingericht.
De tweede wacht tot de virtuele machines naar volledig is ingericht.
Het derde script start opnieuw alle virtuele machines die ingericht en vervolgens alleen zijn de gecodeerde virtuele machines.

### <a name="provision-the-vms"></a>De virtuele machines inrichten

Dit script maakt een resourcegroep en maakt vervolgens deze drie virtuele machines op te starten.
Twee van deze zijn gelabeld.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Wait

Dit script wordt gecontroleerd op de status van de inrichting elke 20 seconden totdat alle drie virtuele machines zijn ingericht, of een van deze om in te richten is mislukt.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>De virtuele machines opnieuw opstarten

Dit script in de resourcegroep alle virtuele machines opnieuw wordt opgestart en deze alleen de gecodeerde virtuele machines opnieuw wordt opgestart.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroepen, virtuele machines en alle gerelateerde resources te verwijderen.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten voor het maken van een resourcegroep, virtuele machine, beschikbaarheidsset, load balancer en alle gerelateerde resources. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [AZ vm maken](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Maakt de virtuele machines.  |
| [AZ vm lijst](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Gebruikt met `--query` om te controleren of de virtuele machines zijn ingericht voordat opnieuw wordt opgestart, en vervolgens om op te halen van de id's van de virtuele machines te starten. |
| [lijst met resources AZ](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Gebruikt met `--query` de id's van de virtuele machines met het label opgehaald. |
| [AZ vm opnieuw opstarten](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Start opnieuw op de virtuele machines. |
| [AZ groep verwijderen](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Hiermee verwijdert u een resourcegroep met inbegrip van alle ingesloten resources. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure CLI [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Extra virtuele machine CLI scriptvoorbeelden vindt u in de [Azure Linux VM documentatie](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
