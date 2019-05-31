---
title: Azure-VM's migreren naar Managed Disks | Microsoft Docs
description: Migreer Azure virtuele machines die zijn gemaakt met behulp van niet-beheerde schijven in de storage-accounts naar beheerde schijven.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7a390c5231f715ce778c0cc267211b8bfb5934d2
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418396"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure-VM's migreren naar Managed Disks in Azure

Azure Managed Disks vereenvoudigt het beheer van uw opslag door het verwijderen van de noodzaak voor het storage-accounts afzonderlijk beheren.  U kunt ook uw bestaande Azure-VM's migreren naar Managed Disks profiteren van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Het zorgt ervoor dat de schijven van verschillende virtuele machines in een Beschikbaarheidsset voldoende geïsoleerd van elkaar zijn om te voorkomen dat één punt van fouten. Deze plaatst automatisch schijven van verschillende virtuele machines in een Beschikbaarheidsset in verschillende kasten (stempels) zodat ze worden beperkt de gevolgen van één opslag scale unit storingen veroorzaakt door hardware en software.
Op basis van uw behoeften, kunt u kiezen uit vier verschillende opties voor opslag. Zie voor meer informatie over de typen beschikbare schijfruimte, dan het artikel [een schijf selecteren](disks-types.md)

## <a name="migration-scenarios"></a>Migratiescenario 's

U kunt migreren naar Managed Disks in de volgende scenario's:

|Scenario  |Artikel  |
|---------|---------|
|Zelfstandige virtuele machines en virtuele machines in een beschikbaarheidsset naar beheerde schijven converteren     |[Virtuele machines voor het gebruik van beheerde schijven converteren](convert-unmanaged-to-managed-disks.md)         |
|Converteert een enkele virtuele machine van klassiek naar Resource Manager op beheerde schijven     |[Een virtuele machine maken van een klassieke VHD](create-vm-specialized-portal.md)         |
|Converteert alle virtuele machines in een vNet van de klassieke naar Resource Manager op beheerde schijven     |[Migreren van IaaS-resources van klassiek naar Resource Manager](migration-classic-resource-manager-ps.md) en vervolgens [een VM van niet-beheerde schijven converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md)         |
|Upgrade uitvoeren van virtuele machines met niet-beheerde standaardschijven aan virtuele machines met beheerde premium-schijven     | Eerste, [een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks](convert-unmanaged-to-managed-disks.md). Vervolgens [bijwerken van het opslagtype van een beheerde schijf](convert-disk-storage.md).         |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Managed Disks](managed-disks-overview.md)
- Controleer de [prijzen voor Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).
