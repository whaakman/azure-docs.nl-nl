---
title: Virtuele machines in Azure te migreren naar beheerde schijven | Microsoft Docs
description: Migreren van virtuele machines in Azure gemaakt met behulp van niet-beheerde schijven in opslagaccounts beheerd schijven te gebruiken.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: e23697b390e03bd2b71f2c905882070d864d62ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Virtuele machines in Azure te migreren naar beheerde schijven in Azure

Azure-beheerde schijven vereenvoudigt het beheer van uw opslag door het verwijderen van de noodzaak voor het beheren van afzonderlijk storage-accounts.  U kunt uw bestaande Azure-virtuele machines ook migreren naar schijven beheerd profiteren van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Hiermee zorgt u ervoor dat de schijven van andere virtuele machines in een Beschikbaarheidsset zal voldoende los van elkaar om te voorkomen dat één punt van fouten. Schijven met een andere virtuele machines automatisch geplaatst in een Beschikbaarheidsset in verschillende opslagunits (stempels) Hiermee beperkt u het effect van één opslag scale unit storingen veroorzaakt door de hardware en software.
Op basis van uw behoeften, kunt u kiezen uit twee soorten opties voor opslag:

- [Premium-schijven beheerd](../../storage/common/storage-premium-storage.md) Solid State station (SSD) op basis van opslagmedia die highperformance, schijfondersteuning voor virtuele machines met I/O-intensieve werkbelastingen met lage latentie zorgt. U kunt profiteren van de snelheid en prestaties van deze schijven nemen door te migreren naar de Premium-schijven worden beheerd.

- [Standard-beheerde schijven](../../storage/common/storage-standard-storage.md) opslagmedia harde schijf (HDD) op basis van gebruik en geschikt zijn voor het ontwikkelen en testen en andere werkbelastingen incidentele toegang die minder gevoelig voor prestaties variabiliteit zijn.

U kunt migreren naar schijven beheerd in de volgende scenario's:

| Migreren...                                            | Koppeling van documentatie                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zelfstandige virtuele machines en virtuele machines in een beschikbaarheidsset naar beheerde schijven converteren   | [Virtuele machines voor het gebruik van beheerde schijven converteren](convert-unmanaged-to-managed-disks.md) |
| Een enkele virtuele machine van klassiek naar Resource Manager op beheerde schijven     | [Een enkele virtuele machine migreren](migrate-single-classic-to-resource-manager.md)  | 
| Alle virtuele machines in een vNet van klassieke, naar Resource Manager op beheerde schijven     | [Migreren van IaaS-middelen van klassiek naar Resource Manager](migration-classic-resource-manager-ps.md) en vervolgens [een virtuele machine van niet-beheerde schijven converteren naar beheerde schijven](convert-unmanaged-to-managed-disks.md) | 






## <a name="plan-for-the-conversion-to-managed-disks"></a>Plan voor de conversie naar schijven beheerd

Deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.


## <a name="location"></a>Locatie

Kies een locatie waar Azure beheerd schijven beschikbaar zijn. Als u naar Premium-schijven worden beheerd overstapt, ook voor zorgen dat de Premium-opslag beschikbaar is in de regio waar u van plan bent om naar te verplaatsen. Zie [Azure-Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over beschikbare locaties.

## <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar Premium-schijven worden beheerd migreert, hebt u voor het bijwerken van de grootte van de virtuele machine naar de Premium-opslag kunnen grootte beschikbaar in de regio waar de VM zich bevindt. Bekijk de VM-grootten Premium-opslag die geschikt zijn. De Azure VM-grootte specificaties worden vermeld in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die geschikt is voor Premium-opslag en kiest u de meest geschikte VM-grootte die het beste past bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het verkeer van de schijf.

## <a name="disk-sizes"></a>Schijfformaten

**Premium beheerde schijven**

Er zijn zeven soorten beheerde premium-schijven die kunnen worden gebruikt met uw virtuele machine en elke principal heeft bepaalde IOPs en doorvoerlimieten limieten. In overweging nemen deze limieten bij het kiezen van het type Premium-schijf voor de virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek worden geladen.

| Premium-schijven Type  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Schijfgrootte           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOP's per schijf       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Doorvoer per schijf | 25 MB per seconde  | 50 MB per seconde  | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde |

**Beheerde standaardschijven**

Er zijn zeven soorten beheerde standaardschijven die kunnen worden gebruikt met uw virtuele machine. Elk van deze andere capaciteit hebben maar dezelfde IOPS en doorvoerlimieten hebben. Kies het type van de Standard-beheerde schijven op basis van de capaciteitsbehoeften van uw toepassing.

| Standard-schijftype  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Schijfgrootte           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| IOP's per schijf       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Doorvoer per schijf | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 

## <a name="disk-caching-policy"></a>Het beleid voor schijf

**Premium beheerde schijven**

Beleid voor de schijfcache is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen-schrijven* voor de Premium-schijf is gekoppeld aan de VM. Deze configuratieinstelling wordt aanbevolen de optimale prestaties voor uw toepassing IOs bereiken. Voor schijven schrijven zware of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden), uitschakelen schijfcache, zodat u kunt betere prestaties bereiken.

## <a name="pricing"></a>Prijzen

Controleer de [prijzen voor schijven beheerd](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Prijzen van beheerde Premium-schijven is hetzelfde als het niet-beheerde Premium-schijven. Maar prijzen voor beheerde standaardschijven is anders dan standaardschijven zonder begeleiding.



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [schijven beheerd](managed-disks-overview.md)
