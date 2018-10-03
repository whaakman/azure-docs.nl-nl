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
ms.date: 11/30/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 564e8e2968f31b9e2aef8f7eadbf848885037ccb
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040759"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Voorbeelden van Azure PowerShell van de virtuele Machine

De volgende tabel bevat koppelingen naar voorbeelden van de PowerShell-scripts die Windows virtuele machines maken en beheren.

| | |
|---|---|
|**Virtuele machines maken**||
| [Snel een virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een resourcegroep, de virtuele machine en alle gerelateerde resources met het minimaal aantal stappen.|
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een resourcegroep, de virtuele machine en alle gerelateerde resources.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u meerdere virtuele machines in een maximaal beschikbare en configuratie van taakverdeling.|
| [Een virtuele machine maken en uitvoeren van script voor configuratie](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om IIS te installeren. |
| [Een virtuele machine maken en uitvoeren van de DSC-configuratie](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een virtuele machine en de extensie Azure Desired State Configuration (DSC) gebruikt om IIS te installeren. |
| [Een VHD uploaden en virtuele machines maken](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Een lokale VHD-bestand wordt geüpload naar Azure, maakt en afbeelding van de VHD en maakt vervolgens een virtuele machine van die installatiekopie. |
| [Een virtuele machine maken vanaf een beheerde OS-schijf](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een virtuele machine door het koppelen van een bestaande beheerde schijf als besturingssysteemschijf. |
| [Een virtuele machine maken van een momentopname](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Maakt een virtuele machine van een momentopname van een door eerst een beheerde schijf maken op basis van momentopname en vervolgens de nieuwe beheerde schijf als besturingssysteemschijf te koppelen. |
|**Opslag beheren**||
| [Beheerde schijf maken op basis van een VHD in dezelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een beheerde schijf van een gespecialiseerde VHD als een besturingssysteemschijf of van een gegevens-VHD als gegevensschijf in dezelfde of een ander abonnement.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een beheerde schijf van een momentopname. |
| [Beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopieën beheerde schijf op hetzelfde of een ander abonnement maar in dezelfde regio als de bovenliggende beheerde schijf. 
| [Een momentopname exporteren als VHD naar een opslagaccount](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Een beheerde momentopname geëxporteerd als VHD naar een opslagaccount in verschillende regio's. |
| [De VHD van een beheerde schijf exporteren naar een opslagaccount](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee exporteert u de onderliggende VHD van een beheerde schijf naar een opslagaccount in verschillende regio's. |
| [Een momentopname van een VHD maken](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u momentopname van een VHD voor het maken van meerdere identieke beheerde schijven van momentopname in korte tijd.  |
| [Momentopname kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Momentopname van de exemplaren aan dezelfde of een ander abonnement maar in dezelfde regio als de bovenliggende momentopname. |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de gegevensschijven versleutelen](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Hiermee maakt u een Azure Key Vault, de versleutelingssleutel en de service-principal, en vervolgens versleutelt van een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine met Operations Management Suite bewaken](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een virtuele machine, de Operations Management Suite-agent wordt geïnstalleerd en geregistreerd van de virtuele machine in een OMS-werkruimte.  |
| | |

