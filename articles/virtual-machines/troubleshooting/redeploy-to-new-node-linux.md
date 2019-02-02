---
title: Virtuele Linux-Machines in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van virtuele Linux-machines in Azure voor het oplossen van problemen met SSH-verbinding.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: genlin
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: ce97b815d25ff1bdc5cccfeb52cd7567f961e6a1
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55659164"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Linux virtuele machine naar de nieuwe Azure-knooppunt opnieuw implementeren
Als u problemen bij het oplossen van problemen SSH te maken krijgt of toegang tot toepassingen op een Linux virtuele machine (VM) in Azure, de virtuele machine opnieuw te implementeren kan helpen. Wanneer u een virtuele machine opnieuw implementeren, wordt de virtuele machine verplaatst naar een nieuw knooppunt in de Azure-infrastructuur en vervolgens wordt gebruikt door deze terug op. Alle configuratie-opties en bijbehorende resources worden bewaard. Dit artikel ziet u hoe u een virtuele machine met behulp van Azure CLI of Azure portal opnieuw implementeren.

> [!NOTE]
> Nadat u een virtuele machine opnieuw implementeren, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt. 


## <a name="use-the-azure-cli"></a>Azure CLI gebruiken
Installeer de meest recente [Azure CLI](/cli/azure/install-az-cli2) en aan te melden bij uw Azure-account met [az login](/cli/azure/reference-index).

Opnieuw implementeren van uw virtuele machine met [az vm opnieuw implementeren](/cli/azure/vm#az_vm_redeploy). Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-classic-cli"></a>Gebruik de klassieke Azure-CLI
Installeer de [meest recente Azure klassieke CLI](../../cli-install-nodejs.md) en meld u aan bij uw Azure-account. Zorg ervoor dat u zich in Resource Manager-modus (`azure config mode arm`).

Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, kunt u specifieke hulp vinden op [problemen met SSH-verbindingen oplossen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [gedetailleerde stappen voor probleemoplossing SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Als geen u toegang een toepassing die wordt uitgevoerd op de virtuele machine tot, u kunt ook lezen [toepassing oplossen van problemen met](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


