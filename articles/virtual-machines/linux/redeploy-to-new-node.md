---
title: Virtuele Linux-Machines in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van virtuele Linux-machines in Azure voor het oplossen van problemen met SSH-verbinding.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: 1bd13c35ed49aeaab1a4f4aa94c984dc28f6c111
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931545"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Linux virtuele machine naar de nieuwe Azure-knooppunt opnieuw implementeren
Als u problemen bij het oplossen van problemen SSH te maken krijgt of toegang tot toepassingen op een Linux virtuele machine (VM) in Azure, de virtuele machine opnieuw te implementeren kan helpen. Wanneer u een virtuele machine opnieuw implementeren, wordt de virtuele machine verplaatst naar een nieuw knooppunt in de Azure-infrastructuur en vervolgens wordt gebruikt door deze terug op. Alle configuratie-opties en bijbehorende resources worden bewaard. Dit artikel ziet u hoe u een virtuele machine met behulp van Azure CLI of Azure portal opnieuw implementeren.

> [!NOTE]
> Nadat u een virtuele machine opnieuw implementeren, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt. 

U kunt een virtuele machine met behulp van een van de volgende opties opnieuw implementeren. U moet alleen een optie voor uw virtuele machine opnieuw implementeren kiezen:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Azure CLI 2.0 gebruiken
Installeer de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en aan te melden bij uw Azure-account met [az login](/cli/azure/reference-index#az_login).

Opnieuw implementeren van uw virtuele machine met [az vm opnieuw implementeren](/cli/azure/vm#az_vm_redeploy). Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Gebruik de Azure CLI 1.0
Installeer de [nieuwste Azure CLI 1.0](../../cli-install-nodejs.md) en meld u aan bij uw Azure-account. Zorg ervoor dat u zich in Resource Manager-modus (`azure config mode arm`).

Het volgende voorbeeld implementeert de virtuele machine met de naam opnieuw *myVM* in de resourcegroep met de naam *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, kunt u specifieke hulp vinden op [problemen met SSH-verbindingen oplossen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [gedetailleerde stappen voor probleemoplossing SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Als geen u toegang een toepassing die wordt uitgevoerd op de virtuele machine tot, u kunt ook lezen [toepassing oplossen van problemen met](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

