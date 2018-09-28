---
title: Windows virtuele machines in Azure implementeren | Microsoft Docs
description: Klik hier voor meer informatie over het implementeren van Windows virtuele machines in Azure voor het oplossen van problemen met RDP-verbinding.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: genlin
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: 0ee456ee-4595-4a14-8916-72c9110fc8bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: ae5fd0d1a16d67c0649412edce6a130150f3cc6a
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413708"
---
# <a name="redeploy-windows-virtual-machine-to-new-azure-node"></a>Windows virtuele machine naar de nieuwe Azure-knooppunt opnieuw implementeren
Als u problemen is aangesloten kan het oplossen van Remote Desktop (RDP)-verbinding of de toepassing toegang en op basis van Windows Azure-machine (VM), de virtuele machine opnieuw te implementeren helpen. Wanneer u een virtuele machine opnieuw implementeren, wordt de virtuele machine verplaatst naar een nieuw knooppunt in de Azure-infrastructuur en vervolgens wordt gebruikt door deze terug op, behoud van alle configuratie-opties en bijbehorende resources. Dit artikel ziet u hoe u een virtuele machine met behulp van Azure PowerShell of Azure portal opnieuw implementeren.

> [!NOTE]
> Nadat u een virtuele machine opnieuw implementeren, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt. 


## <a name="using-azure-powershell"></a>Azure PowerShell gebruiken
Zorg ervoor dat u hebt de nieuwste Azure PowerShell 1.x op uw computer geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.

Het volgende voorbeeld wordt geïmplementeerd voor de virtuele machine met de naam `myVM` in de resourcegroep met de naam `myResourceGroup`:

```powershell
Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
```


[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Volgende stappen
Als u hebt met het maken van een verbinding met uw virtuele machine problemen, kunt u specifieke hulp vinden op [problemen met RDP-verbindingen oplossen](troubleshoot-rdp-connection.md) of [gedetailleerde stappen voor probleemoplossing RDP](detailed-troubleshoot-rdp.md). Als geen u toegang een toepassing die wordt uitgevoerd op de virtuele machine tot, u kunt ook lezen [toepassing oplossen van problemen met](../windows/troubleshoot-app-connection.md).

