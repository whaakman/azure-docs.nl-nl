---
title: Voorbeelden van Azure CLI | Microsoft Docs
description: Voorbeelden van Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ec76e211062890b4325b5c2cb1cf186ac7b89c55
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Azure CLI-voorbeelden voor virtuele Linux-machines

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|---|---|
|**Virtuele machines maken**||
| [Een virtuele machine maken](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt een virtuele Linux-machine met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Een resourcegroep, virtuele machine en alle gerelateerde resources gemaakt.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt verschillende virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Een virtuele machine maken met Docker ingeschakeld](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt, configureert deze virtuele machine als een Docker-host en een NGINX-container wordt uitgevoerd. |
| [Een virtuele machine maken en voer het script voor configuratie](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt en wordt de extensie Azure aangepast Script gebruikt voor het installeren van NGINX. |
| [Een virtuele machine maken met WordPress geïnstalleerd](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt en wordt de extensie Azure aangepast Script gebruikt voor het installeren van WordPress. |
| [Een virtuele machine maken van een beheerde OS-schijf](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Maakt een virtuele machine door het koppelen van een bestaande schijf beheerd als besturingssysteemschijf. |
| [Een virtuele machine maken vanuit een momentopname](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Een virtuele machine maakt vanuit een momentopname eerst een beheerde schijven maken vanuit een momentopname en vervolgens de nieuwe beheerde schijf als besturingssysteemschijf te koppelen. |
|**Opslag beheren**||
| [Beheerde schijf vanaf een VHD maken](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Een beheerde schijf maakt vanaf een speciale VHD als een besturingssysteemschijf of van een gegevensschijf VHD-gegevens.  |
| [Een beheerde schijf maken vanuit een momentopname](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Maakt een beheerde schijf van een momentopname. |
| [Beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Exemplaren beheerd van schijf naar hetzelfde of een ander abonnement maar beheerd in dezelfde regio bevinden als de bovenliggende schijf. 
| [Een momentopname exporteren als VHD naar een opslagaccount](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Exporteert een momentopname van een beheerde als VHD naar een opslagaccount in andere regio. |
| [Momentopname naar hetzelfde of een ander abonnement kopiëren](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopieën momentopname aan hetzelfde of een ander abonnement, maar in dezelfde regio bevinden als de momentopname van de bovenliggende. |
|**Virtuele machines**||
| [Beveiligen van netwerkverkeer tussen virtuele machines](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt twee virtuele machines, alle gerelateerde resources en een interne en externe netwerkbeveiligingsgroepen (NSG). |
|**Beveiligde virtuele machines**||
| [Versleutelen van een VM- en gegevensschijven](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure Sleutelkluis, de versleutelingssleutel en de service-principal gemaakt en vervolgens een virtuele machine versleutelt. |
|**Virtuele machines bewaken**||
| [Monitor voor een virtuele machine bij Operations Management Suite](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt, installeert de Operations Management Suite-agent en de virtuele machine in een OMS-werkruimte inschrijft.  |
|**Problemen met virtuele machines**||
| [Problemen met een besturingssysteemschijf virtuele machines](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Koppelt de schijf van het besturingssysteem van een virtuele machine als een gegevensschijf op een tweede virtuele machine. |
| | |
