---
title: Azure CLI-voorbeelden | Microsoft Docs
description: Azure CLI-voorbeelden
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
ms.date: 03/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1e4b7027ecbc6e70fadf8c540fa92903d276956b
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931708"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Azure CLI-voorbeelden voor virtuele Linux-machines

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|---|---|
|**Virtuele machines maken**||
| [Een virtuele machine maken](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een virtuele Linux-machine met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een resourcegroep, de virtuele machine en alle gerelateerde resources.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u meerdere virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Een virtuele machine maken en uitvoeren van script voor configuratie](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om NGINX te installeren. |
| [Een virtuele machine maken met WordPress geïnstalleerd](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een virtuele machine en de aangepaste scriptextensie van Azure gebruikt om WordPress te installeren. |
| [Een virtuele machine maken vanaf een beheerde OS-schijf](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Hiermee maakt u een virtuele machine door het koppelen van een bestaande beheerde schijf als besturingssysteemschijf. |
| [Een virtuele machine maken van een momentopname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Maakt een virtuele machine van een momentopname van een door eerst een beheerde schijf maken op basis van momentopname en vervolgens de nieuwe beheerde schijf als besturingssysteemschijf te koppelen. |
|**Opslag beheren**||
| [Beheerde schijf maken op basis van een VHD](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Hiermee maakt u een beheerde schijf op basis van een gespecialiseerde VHD als een besturingssysteemschijf of op basis van een gegevens-VHD als een gegevensschijf.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Hiermee maakt u een beheerde schijf van een momentopname. |
| [Beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopieën beheerde schijf op hetzelfde of een ander abonnement maar in dezelfde regio als de bovenliggende beheerde schijf. 
| [Een momentopname exporteren als VHD naar een opslagaccount](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Een beheerde momentopname geëxporteerd als VHD naar een opslagaccount in verschillende regio's. |
| [Momentopname kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Momentopname van de exemplaren aan dezelfde of een ander abonnement maar in dezelfde regio als de bovenliggende momentopname. |
|**Netwerk virtuele machines**||
| [Netwerkverkeer tussen virtuele machines beveiligen](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u twee virtuele machines en alle gerelateerde resources een interne en externe netwerkbeveiligingsgroepen (NSG). |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de gegevensschijven versleutelen](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een Azure Key Vault, de versleutelingssleutel en de service-principal, en vervolgens versleutelt van een virtuele machine. |
|**Virtuele machines bewaken**||
| [Een virtuele machine met Operations Management Suite bewaken](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een virtuele machine, de Operations Management Suite-agent wordt geïnstalleerd en geregistreerd van de virtuele machine in een OMS-werkruimte.  |
|**Virtuele machines oplossen**||
| [De besturingssysteemschijf van een VM's oplossen](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Koppelt de besturingssysteemschijf van een virtuele machine als gegevensschijf op een tweede virtuele machine. |
| | |
