---
title: Azure CLI-voorbeelden voor Windows | Microsoft Docs
description: Windows Azure CLI-voorbeelden
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f4b2e8a5583855df7472af3fbef01ac641caf6bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Virtuele machines van Azure CLI-voorbeelden voor Windows

De volgende tabel bevat koppelingen naar bash-scripts die worden gemaakt met de Azure CLI die Windows virtuele machines implementeren.

| | |
|---|---|
|**Virtuele machines maken**||
| [Een virtuele machine maken](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt een virtuele Windows-computer met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Een resourcegroep, virtuele machine en alle gerelateerde resources gemaakt.|
| [Maximaal beschikbare virtuele machines maken](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt verschillende virtuele machines in een maximaal beschikbare en configuratie van taakverdeling. |
| [Een virtuele machine maken en voer het script voor configuratie](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt en wordt de extensie Azure aangepast Script gebruikt om IIS te installeren. |
| [Een virtuele machine maken en uitvoeren van de DSC-configuratie](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt en wordt de extensie Azure Desired State Configuration (DSC) gebruikt om IIS te installeren. |
|**Virtuele machines**||
| [Beveiligen van netwerkverkeer tussen virtuele machines](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt twee virtuele machines, alle gerelateerde resources en een interne en externe netwerkbeveiligingsgroepen (NSG). |
|**Beveiligde virtuele machines**||
| [Versleutelen van een VM- en gegevensschijven](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Een Azure Sleutelkluis, de versleutelingssleutel en de service-principal gemaakt en vervolgens een virtuele machine versleutelt. |
|**Virtuele machines bewaken**||
| [Monitor voor een virtuele machine bij Operations Management Suite](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Een virtuele machine maakt, installeert de Operations Management Suite-agent en de virtuele machine in een OMS-werkruimte inschrijft.  |
| | |
