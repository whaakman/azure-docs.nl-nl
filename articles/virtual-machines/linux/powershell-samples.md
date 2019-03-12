---
title: Voorbeelden van de virtuele Machine van Azure PowerShell | Microsoft Docs
description: Virtuele Machine van Azure PowerShell-voorbeelden
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 53784c3d74f9e6af5f1e84cc098194113e81333b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57533354"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Voorbeelden van Azure PowerShell van de virtuele Machine

De volgende tabel bevat koppelingen naar voorbeelden van de PowerShell-scripts die maken en beheren van virtuele Linux-machines.

| | |
|---|---|
|**Virtuele machines maken**||
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een resourcegroep, de virtuele machine en alle gerelateerde resources.|
| [Een virtuele machine maken met Docker is ingeschakeld](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Een virtuele machine maakt, configureert deze virtuele machine als een Docker-host en een NGINX-container wordt uitgevoerd. |
| [Een virtuele machine maken en uitvoeren van script voor configuratie](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om NGINX te installeren. |
| [Een virtuele machine maken met WordPress geïnstalleerd](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om WordPress te installeren. |
| [Een virtuele machine maken vanaf een beheerde OS-schijf](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine door het koppelen van een bestaande beheerde schijf als besturingssysteemschijf. |
| [Een virtuele machine maken van een momentopname](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Maakt een virtuele machine van een momentopname van een door eerst een beheerde schijf maken op basis van de momentopname en vervolgens de nieuwe beheerde schijf als besturingssysteemschijf te koppelen. |
|**Opslag beheren**||
| [Een beheerde schijf maken op basis van een VHD in dezelfde of een ander abonnement](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een beheerde schijf van een gespecialiseerde VHD als een besturingssysteemschijf of van een gegevens-VHD als een gegevensschijf, in dezelfde of een ander abonnement.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een beheerde schijf van een momentopname. |
| [Een momentopname exporteren als een VHD naar een opslagaccount](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Een beheerde momentopname geëxporteerd als een VHD naar een opslagaccount in een andere regio. |
| [De VHD van een beheerde schijf exporteren naar een opslagaccount](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee exporteert u de onderliggende VHD van een beheerde schijf naar een opslagaccount in een andere regio. |
| [Een momentopname van een VHD maken](../scripts/virtual-machines-linux-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een momentopname van een VHD en gebruikt vervolgens die momentopname meerdere identieke beheerde schijven om snel te maken.  |
| [Een momentopname kopiëren naar dezelfde of een ander abonnement](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | De momentopname van de exemplaren naar dezelfde of een ander abonnement dat zich in dezelfde regio als de bovenliggende momentopname. |
|**Virtuele machines bewaken**||
| [Een virtuele machine met Azure Monitor-logboeken bewaken](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Hiermee maakt u een virtuele machine, de Log Analytics-agent wordt geïnstalleerd en geregistreerd van de virtuele machine in een Log Analytics-werkruimte.  |
| [Een beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Een beheerde schijf gekopieerd naar hetzelfde of een ander abonnement dat zich in dezelfde regio als de bovenliggende beheerde schijf.
| | |
