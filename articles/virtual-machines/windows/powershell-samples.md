---
title: Voorbeelden van de virtuele Machine van Azure PowerShell | Microsoft Docs
description: Virtuele Machine van Azure PowerShell-voorbeelden
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 576fe268bec12c16c7c2e2076dfa066c908693d5
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539527"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Voorbeelden van Azure PowerShell van de virtuele Machine

De volgende tabel bevat koppelingen naar voorbeelden van de PowerShell-scripts maken en beheren van Windows-machines (VM's).

| | |
|---|---|
|**Virtuele machines maken**||
| [Snel een virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resourcegroep, een virtuele machine en alle gerelateerde resources, met een minimum van prompts.|
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resourcegroep, een virtuele machine en alle gerelateerde resources.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u meerdere virtuele machines in een configuratie met hoge beschikbaarheid en taakverdeling.|
| [Een virtuele machine maken en een configuratiescript uitvoeren](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om IIS te installeren. |
| [Een virtuele machine maken en uitvoeren van een DSC-configuratie](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en de extensie Azure Desired State Configuration (DSC) gebruikt om IIS te installeren. |
| [Een VHD uploaden en virtuele machines maken](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Een lokale VHD-bestand wordt geüpload naar Azure, maakt een installatiekopie van de VHD en maakt vervolgens een virtuele machine van die installatiekopie. |
| [Een virtuele machine maken vanaf een beheerde OS-schijf](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine door het koppelen van een bestaande beheerde schijf als besturingssysteemschijf. |
| [Een virtuele machine maken van een momentopname](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Maakt een virtuele machine van een momentopname van een door eerst een beheerde schijf maken op basis van de momentopname en vervolgens de nieuwe beheerde schijf als besturingssysteemschijf te koppelen. |
|**Opslag beheren**||
| [Een beheerde schijf maken op basis van een VHD in dezelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf van een gespecialiseerde VHD als een besturingssysteemschijf of van een gegevens-VHD als een gegevensschijf, in dezelfde of een ander abonnement.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf van een momentopname. |
| [Een beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement dat zich in dezelfde regio als de bovenliggende beheerde schijf.
| [Een momentopname exporteren als een VHD naar een opslagaccount](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Een beheerde momentopname geëxporteerd als een VHD naar een opslagaccount in een andere regio. |
| [De VHD van een beheerde schijf exporteren naar een opslagaccount](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee exporteert u de onderliggende VHD van een beheerde schijf naar een opslagaccount in een andere regio. |
| [Een momentopname van een VHD maken](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een momentopname van een VHD en gebruikt vervolgens die momentopname meerdere identieke beheerde schijven om snel te maken.  |
| [Een momentopname kopiëren naar dezelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | De momentopname van de exemplaren naar dezelfde of een ander abonnement dat zich in dezelfde regio als de bovenliggende momentopname. |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de gegevensschijven versleutelen](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Een Azure-sleutelkluis, een versleutelingssleutel gemaakt en een service-principal maakt en vervolgens versleutelt een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine bewaken met Azure Monitor](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine, de Azure Log Analytics-agent wordt geïnstalleerd en geregistreerd van de virtuele machine in een Log Analytics-werkruimte.  |
| | |
