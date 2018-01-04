---
title: Linux virtuele Machines in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van Linux virtuele machines in Azure te verhelpen van problemen met SSH-verbinding.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.openlocfilehash: 29e41b100f594d044e00054c91fb0095679ca627
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Nieuwe Azure knooppunt virtuele Linux-machine implementeren
Als u problemen bij het oplossen van SSH wordt geconfronteerd of toegang tot de toepassing naar een Linux virtuele machine (VM) in Azure, opnieuw distribueren van de virtuele machine kan helpen. Wanneer u een virtuele machine opnieuw implementeert, wordt de virtuele machine verplaatst naar een nieuw knooppunt in de Azure-infrastructuur en wordt deze vervolgens weer ingeschakeld. Alle configuratie-opties en bijbehorende bronnen worden bewaard. In dit artikel leest u hoe een virtuele machine met behulp van Azure CLI of Azure portal opnieuw implementeren.

> [!NOTE]
> Nadat u een virtuele machine opnieuw implementeren, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt. 

U kunt een virtuele machine met een van de volgende opties opnieuw implementeren. U moet slechts één optie voor het implementeren van uw virtuele machine kiezen:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>De Azure CLI 2.0 gebruiken
Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en zich aanmelden bij uw Azure-account met [az aanmelding](/cli/azure/#login).

Implementeren van uw virtuele machine met [az vm opnieuw distribueren](/cli/azure/vm#redeploy). Het volgende voorbeeld de virtuele machine met de naam redeploys *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Gebruik de Azure CLI 1.0
Installeer de [nieuwste Azure CLI 1.0](../../cli-install-nodejs.md) en aanmelden bij uw Azure-account. Zorg ervoor dat u in de modus Resource Manager (`azure config mode arm`).

Het volgende voorbeeld de virtuele machine met de naam redeploys *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, kunt u Help-informatie vinden op [probleemoplossing SSH-verbindingen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [gedetailleerde stappen voor probleemoplossing SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Als u geen toegang een toepassing die wordt uitgevoerd op de virtuele machine tot, kunt u ook lezen [toepassing het oplossen van problemen](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

