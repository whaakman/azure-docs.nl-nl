---
title: Azure CLI-voorbeelden voor Windows | Microsoft Docs
description: Voorbeelden van Windows Azure CLI
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
ms.openlocfilehash: abc38d315bc879a06ecd9a9bf7188c15533c018a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57535005"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Virtuele machines van Azure CLI-voorbeelden voor Windows

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI en die Windows-machines kunt implementeren.

| | |
|---|---|
|**Virtuele machines maken**||
| [Maak een virtuele machine](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een Windows-machine met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resourcegroep, de virtuele machine en alle gerelateerde resources.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u meerdere virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Een virtuele machine maken en uitvoeren van script voor configuratie](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om IIS te installeren. |
| [Een virtuele machine maken en uitvoeren van de DSC-configuratie](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en de extensie Azure Desired State Configuration (DSC) gebruikt om IIS te installeren. |
|**Opslag beheren**||
| [Beheerde schijf maken op basis van een VHD](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf op basis van een gespecialiseerde VHD als een besturingssysteemschijf of op basis van een gegevens-VHD als een gegevensschijf.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf van een momentopname. |
| [Beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Kopieën beheerde schijf op hetzelfde of een ander abonnement maar in dezelfde regio als de bovenliggende beheerde schijf. 
| [Een momentopname exporteren als VHD naar een opslagaccount](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Een beheerde momentopname geëxporteerd als VHD naar een opslagaccount in verschillende regio's. |
| [De VHD van een beheerde schijf exporteren naar een opslagaccount](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee exporteert u de onderliggende VHD van een beheerde schijf naar een opslagaccount in verschillende regio's. |
| [Momentopname kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Momentopname van de exemplaren aan dezelfde of een ander abonnement maar in dezelfde regio als de bovenliggende momentopname. |
|**Netwerk virtuele machines**||
| [Netwerkverkeer tussen virtuele machines beveiligen](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u twee virtuele machines en alle gerelateerde resources een interne en externe netwerkbeveiligingsgroepen (NSG). |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de gegevensschijven versleutelen](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een Azure Key Vault, de versleutelingssleutel en de service-principal, en vervolgens versleutelt van een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine bewaken met Azure Monitor](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine, de Log Analytics-agent wordt geïnstalleerd en geregistreerd van de virtuele machine in een Log Analytics-werkruimte.  |
| | |
