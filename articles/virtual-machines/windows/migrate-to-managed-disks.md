---
title: Azure-VM's migreren naar Managed Disks | Microsoft Docs
description: Migreer Azure virtuele machines die zijn gemaakt met behulp van niet-beheerde schijven in de storage-accounts naar beheerde schijven.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: b87e27ae914a01f03ce78eafe5792433d18e417f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193706"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure-VM's migreren naar Managed Disks in Azure

Azure Managed Disks vereenvoudigt het beheer van uw opslag door het verwijderen van de noodzaak voor het storage-accounts afzonderlijk beheren.  U kunt ook uw bestaande Azure-VM's migreren naar Managed Disks profiteren van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Dit zorgt ervoor dat de schijven van verschillende virtuele machines in een Beschikbaarheidsset voldoende geïsoleerd van elkaar om te voorkomen dat één punt van fouten. Deze plaatst automatisch schijven van verschillende virtuele machines in een Beschikbaarheidsset in verschillende kasten (stempels) zodat ze worden beperkt de gevolgen van één opslag scale unit storingen veroorzaakt door hardware en software.
Op basis van uw behoeften, kunt u kiezen uit twee typen opslag:

- [Premium Managed Disks](premium-storage.md) Solid State Drive (SSD) op basis van opslagmedia die voorziet in hoogwaardige schijfondersteuning met lage latentie voor virtuele machines met I/O-intensieve workloads. U kunt profiteren van de snelheid en prestaties van deze schijven uitvoeren door te migreren naar Premium Managed Disks.

- [Standard Managed Disks](standard-storage.md) gebruik van vasteschijfstations (HDD) op basis van opslagmedia en zijn bij uitstek geschikt voor Dev/Test- en andere onregelmatige toegangsbewerkingen die minder gevoelig zijn voor variaties in prestaties.

U kunt migreren naar Managed Disks in de volgende scenario's:

| Migreren...                                            | Koppeling voor documentatie                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zelfstandige virtuele machines en virtuele machines in een beschikbaarheidsset naar beheerde schijven converteren   | [Virtuele machines voor het gebruik van beheerde schijven converteren](convert-unmanaged-to-managed-disks.md) |
| Een enkele virtuele machine van klassiek naar Resource Manager op beheerde schijven     | [Een virtuele machine maken van een klassieke VHD](create-vm-specialized-portal.md)  | 
| Alle virtuele machines in een vNet van het klassieke implementatiemodel, naar Resource Manager op beheerde schijven     | [Migreren van IaaS-resources van klassiek naar Resource Manager](migration-classic-resource-manager-ps.md) en vervolgens [een VM van niet-beheerde schijven converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-the-conversion-to-managed-disks"></a>Plan voor de conversie naar beheerde schijven

In deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.


## <a name="location"></a>Locatie

Kies een locatie waar Azure Managed Disks beschikbaar zijn. Als u naar Premium Managed Disks overstapt, zorg er ook voor Premium storage is beschikbaar in de regio waar u van plan bent om naar te verplaatsen. Zie [Azure Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over de beschikbare locaties.

## <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar Premium Managed Disks migreert, hebt u de grootte van de virtuele machine bijwerken naar Premium Storage kunnen grootte beschikbaar in de regio waar de virtuele machine zich bevindt. Bekijk de VM-grootten die Premium-opslag die geschikt zijn. De specificaties van de grootte van virtuele Azure-machine vindt u in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die werken met Premium Storage en kiest u de meest geschikte VM-grootte die het beste bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het schijfverkeer te stimuleren.

## <a name="disk-sizes"></a>Schijfformaten

**Premium Managed Disks**

Er zijn zeven typen premium-beheerde schijven die kunnen worden gebruikt met de virtuele machine en elk heeft specifieke IOPs en doorvoer limieten. In overweging nemen deze limieten bij het kiezen van het schijftype voor Premium voor uw virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

| Schijftype voor Premium-schijven  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 32 GB| 64 GB| 128 GB| 256 GB|512 GB | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 1100  |2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 125 MB per seconde |150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde |

**Standaard beheerde schijven**

Er zijn zeven typen van standard beheerde schijven die kunnen worden gebruikt met de virtuele machine. Elk van deze andere capaciteit hebben maar dezelfde IOPS en doorvoerlimieten. Kies het type Standard beheerde schijven op basis van de capaciteitsbehoeften van uw toepassing.

| Standard-schijftype  | S4               | S6               | S10              | S15              | S20              | S30              | S40              | S50              | 
|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------| 
| Schijfgrootte           | 30 GB            | 64 GB            | 128 GB           | 256 GB           |512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOP's per schijf       | 500              | 500              | 500              | 500              |500              | 500              | 500             | 500              | 
| Doorvoer per schijf | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde |60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 

## <a name="disk-caching-policy"></a>Beleid voor caching schijf

**Premium Managed Disks**

Beleid voor caching schijf is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen / schrijven* voor de Premium-schijf die is gekoppeld aan de virtuele machine. Deze configuratieinstelling wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Voor schijven schrijfintensief of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden) uitschakelen in schijfcache zodat u betere prestaties van toepassingen kunt bereiken.

## <a name="pricing"></a>Prijzen

Controleer de [prijzen voor Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Prijzen van Premium Managed Disks is hetzelfde als de niet-beheerde Premium-schijven. Maar de prijzen voor Standard Managed Disks is anders dan standaard niet-beheerde schijven.



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Managed Disks](managed-disks-overview.md)
