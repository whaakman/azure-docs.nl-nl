---
title: Migreren van AWS en andere platforms naar Managed Disks in Azure | Microsoft Docs
description: Virtuele machines in Azure met behulp van de VHD's van andere clouds zoals AWS of andere virtualisatieplatforms geüpload maken en te profiteren van Azure Managed Disks.
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
ms.date: 10/07/2017
ms.author: rogarana
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05e687ab31b6c19193076033e1350952549d26e0
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330746"
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Migreren van Amazon Web Services (AWS) en andere platforms naar Managed Disks in Azure

U kunt VHD-bestanden van virtualisatieoplossingen AWS of on-premises naar Azure te maken van virtuele machines die van beheerde schijven gebruikmaken uploaden. Azure Managed Disks hoeven zich niet voor het beheren van storage-accounts voor Azure IaaS VM's. U hoeft op te geven alleen het type (Premium of Standard) en de grootte van de schijf die u nodig hebt en Azure maakt en beheert de schijf voor u. 

U kunt gegeneraliseerde en gespecialiseerde VHD's uploaden. 
- **Gegeneraliseerde VHD** -heeft al uw persoonlijke accountinformatie verwijderd met behulp van Sysprep. 
- **Gespecialiseerde VHD** -onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van de oorspronkelijke virtuele machine. 

> [!IMPORTANT]
> Voordat u een VHD uploaden naar Azure, u moet volgen [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Scenario                                                                                                                         | Documentatie                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| U hebt bestaande AWS EC2-instanties die u wilt migreren naar Azure-VM's met behulp van beheerde schijven                              | [Een virtuele machine verplaatsen vanuit Amazon webservices (AWS) naar Azure](aws-to-azure.md)                           |
| U hebt een virtuele machine van een andere virtualisatieplatform die u gebruiken als een installatiekopie wilt maken van meerdere virtuele machines van Azure. | [Een gegeneraliseerde VHD uploaden en maken van een nieuwe virtuele machine in Azure](upload-generalized-managed.md) |
| U hebt een unieke aangepaste virtuele machine die u wilt opnieuw maken in Azure.                                                      | [Een gespecialiseerde VHD uploaden naar Azure en een nieuwe virtuele machine maken](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Overzicht van beheerde schijven

Azure Managed Disks vereenvoudigt het beheer van de virtuele machine door het verwijderen van de noodzaak om opslagaccounts te beheren. Managed Disks ook voordeel van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Het zorgt ervoor dat de schijven van verschillende virtuele machines in een Beschikbaarheidsset voldoende geïsoleerd van elkaar zijn om te voorkomen dat een single point of failure. Deze plaatst automatisch schijven van verschillende virtuele machines in een Beschikbaarheidsset in verschillende kasten (stempels) zodat ze worden beperkt de gevolgen van één opslag scale unit storingen veroorzaakt door hardware en software.
Op basis van uw behoeften, kunt u kiezen uit vier verschillende opties voor opslag. Zie voor meer informatie over de typen beschikbare schijfruimte, dan het artikel [Selecteer een schijftype](disks-types.md).

## <a name="plan-for-the-migration-to-managed-disks"></a>Plan voor de migratie naar Managed Disks

In deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.

Als u van plan bent over het migreren van niet-beheerde schijven naar managed disks, moet u rekening mee dat gebruikers met de [Inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol niet mogelijk de VM-grootte (indien deze vooraf conversie kunnen) wijzigen. Dit komt doordat virtuele machines met beheerde schijven moeten de gebruiker de Microsoft.Compute/disks/write-machtiging hebben voor de OS-schijven.

### <a name="location"></a>Locatie

Kies een locatie waar Azure Managed Disks beschikbaar zijn. Als u naar Premium Managed Disks migreert, zorg er ook voor Premium storage is beschikbaar in de regio waar u van plan bent om te migreren naar. Zie [Azure Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over de beschikbare locaties.

### <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar Premium Managed Disks migreert, hebt u de grootte van de virtuele machine bijwerken naar Premium Storage kunnen grootte beschikbaar in de regio waar de virtuele machine zich bevindt. Bekijk de VM-grootten die Premium-opslag die geschikt zijn. De specificaties van de grootte van virtuele Azure-machine vindt u in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die werken met Premium Storage en kiest u de meest geschikte VM-grootte die het beste bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het schijfverkeer te stimuleren.

### <a name="disk-sizes"></a>Schijfformaten

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

### <a name="disk-caching-policy"></a>Beleid voor caching schijf 

**Premium Managed Disks**

Beleid voor caching schijf is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen / schrijven* voor de Premium-schijf die is gekoppeld aan de virtuele machine. Deze configuratieinstelling wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Voor schijven schrijfintensief of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden) uitschakelen in schijfcache zodat u betere prestaties van toepassingen kunt bereiken.

### <a name="pricing"></a>Prijzen

Controleer de [prijzen voor Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Prijzen van Premium Managed Disks is hetzelfde als de niet-beheerde Premium-schijven. Maar de prijzen voor Standard Managed Disks is anders dan standaard niet-beheerde schijven.


## <a name="next-steps"></a>Volgende stappen

- Voordat u een VHD uploaden naar Azure, u moet volgen [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
