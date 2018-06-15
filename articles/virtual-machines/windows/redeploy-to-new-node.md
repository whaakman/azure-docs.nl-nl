---
title: Windows virtuele machines in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van Windows virtuele machines in Azure te verhelpen RDP-verbindingsproblemen.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: f0bda14634e6c8bea5800b9798086fc38279030a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34160322"
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Windows virtuele machine naar de nieuwe Azure knooppunt opnieuw implementeren
Als u problemen zijn gericht kan probleemoplossing Remote Desktop (RDP) verbinding of de toepassing toegang tot Windows Azure virtuele machine (VM) opnieuw distribueren van de virtuele machine helpen. Wanneer u een virtuele machine opnieuw implementeert, wordt de virtuele machine verplaatst naar een nieuw knooppunt in de Azure-infrastructuur en vervolgens wordt het weer ingeschakeld, alle configuratieopties en bijbehorende bronnen behouden. In dit artikel leest u hoe een virtuele machine met Azure PowerShell of de Azure-portal opnieuw implementeren.

> [!NOTE]
> Nadat u een virtuele machine opnieuw implementeren, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt. 


## <a name="using-azure-powershell"></a>Azure PowerShell gebruiken
Zorg ervoor dat de nieuwste Azure PowerShell 1.x op deze computer geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

Het volgende voorbeeld wordt geïmplementeerd voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Volgende stappen
Als u verbinding maakt met uw virtuele machine problemen ondervindt, kunt u Help-informatie vinden op [probleemoplossing RDP-verbindingen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [gedetailleerde stappen voor probleemoplossing RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Als u geen toegang een toepassing die wordt uitgevoerd op de virtuele machine tot, kunt u ook lezen [toepassing het oplossen van problemen](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

