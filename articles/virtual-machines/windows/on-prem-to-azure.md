---
title: Migreren van AWS en andere platforms naar schijven beheerd in Azure | Microsoft Docs
description: Virtuele machines in Azure met behulp van VHD's geüpload uit andere clouds zoals AWS of andere virtualisatieplatforms maken en te profiteren van beheerde Azure-schijven.
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
ms.date: 10/07/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0440eccbdf76fc58fadf46de2508d362c0de6cc3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32190782"
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Migreren van Amazon Web Services (AWS) en andere platforms naar schijven beheerd in Azure

U kunt VHD-bestanden van virtualisatieoplossingen AWS of on-premises naar Azure te maken van virtuele machines die van beheerde schijven gebruikmaken uploaden. Azure-beheerde schijven verwijdert de behoefte storage-accounts beheren voor Azure IaaS VM's. U moet alleen opgeven van het type (Premium of standaard) en de grootte van de schijf moet u en Azure maken en beheren van de schijf voor u. 

U kunt algemene en gespecialiseerde VHD's uploaden. 
- **VHD gegeneraliseerd** -heeft al uw persoonlijke accountgegevens verwijderd met behulp van Sysprep. 
- **VHD gespecialiseerde** -onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM. 

> [!IMPORTANT]
> Voordat u de VHD uploadt naar Azure, u moet volgen [voorbereiden van een Windows-VHD of VHDX uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>


| Scenario                                                                                                                         | Documentatie                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| U hebt bestaande EC2 AWS-exemplaren die u wilt migreren naar Azure VM's met beheerde schijven                              | [Een virtuele machine verplaatsen van Amazon webservices (AWS) naar Azure](aws-to-azure.md)                           |
| U hebt een virtuele machine van een andere virtualisatieplatform die u gebruiken als een afbeelding wilt voor het maken van meerdere virtuele machines van Azure. | [Een gegeneraliseerde VHD uploaden en deze gebruiken voor het maken van een nieuwe virtuele machine in Azure](upload-generalized-managed.md) |
| U hebt een unieke aangepaste VM die u wilt opnieuw maken in Azure.                                                      | [Een speciale VHD uploaden naar Azure en een nieuwe virtuele machine maken](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Overzicht van beheerde schijven

Azure-beheerde schijven vereenvoudigt VM beheer door het verwijderen van de noodzaak voor het beheren van storage-accounts. Schijven die worden beheerd ook voordeel van betere betrouwbaarheid van virtuele machines in een Beschikbaarheidsset. Dit zorgt ervoor dat de schijven van andere virtuele machines in een Beschikbaarheidsset voldoende los van elkaar om te voorkomen dat een potentieel risico. Schijven met een andere virtuele machines automatisch geplaatst in een Beschikbaarheidsset in verschillende opslagunits (stempels) Hiermee beperkt u het effect van één opslag scale unit storingen veroorzaakt door de hardware en software. Op basis van uw behoeften, kunt u kiezen uit twee soorten opties voor opslag: 
 
- [Premium-schijven beheerd](premium-storage.md) Solid State station (SSD) op basis van opslagmedia, die hoge prestaties, lage latentie schijfondersteuning voor virtuele machines met I/O-intensieve werkbelastingen biedt. U kunt profiteren van de snelheid en prestaties van deze schijven nemen door te migreren naar de Premium-schijven worden beheerd.  

- [Standard-beheerde schijven](standard-storage.md) opslagmedia harde schijf (HDD) op basis van gebruik en geschikt zijn voor het ontwikkelen en testen en andere werkbelastingen incidentele toegang die minder gevoelig voor prestaties variabiliteit zijn.  

## <a name="plan-for-the-migration-to-managed-disks"></a>Plannen voor de migratie naar schijven beheerd

Deze sectie helpt u bij het maken van de beste beslissing op schijf en VM-typen.

Als u van plan bent over het migreren van niet-beheerde schijven aan beheerde schijven, moet u zich bewust zijn dat gebruikers met de [Virtual Machine Contributor](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol kan niet worden de VM-grootte wijzigen (zoals ze vooraf conversie kunnen). Dit is omdat de virtuele machines met beheerde schijven moeten de gebruiker gemachtigd is Microsoft.Compute/disks/write op de OS-schijven.

### <a name="location"></a>Locatie

Kies een locatie waar Azure beheerd schijven beschikbaar zijn. Als u naar Premium-schijven worden beheerd migreert, ook voor zorgen dat de Premium-opslag beschikbaar is in de regio waar u van plan bent om naar te migreren. Zie [Azure-Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over beschikbare locaties.

### <a name="vm-sizes"></a>Formaten van virtuele machines

Als u naar Premium-schijven worden beheerd migreert, hebt u voor het bijwerken van de grootte van de virtuele machine naar de Premium-opslag kunnen grootte beschikbaar in de regio waar de VM zich bevindt. Bekijk de VM-grootten Premium-opslag die geschikt zijn. De Azure VM-grootte specificaties worden vermeld in [grootten voor virtuele machines](sizes.md).
Bekijk de prestatiekenmerken van virtuele machines die geschikt is voor Premium-opslag en kiest u de meest geschikte VM-grootte die het beste past bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het verkeer van de schijf.

### <a name="disk-sizes"></a>Schijfformaten

**Premium beheerde schijven**

Er zijn drie soorten beheerde Premium-schijven die kunnen worden gebruikt met uw virtuele machine en elke principal heeft bepaalde IOPs en doorvoerlimieten limieten. U kunt deze limieten bij het kiezen van het type Premium-schijf voor de virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

| Premium-schijven Type  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Schijfgrootte           | 128 GB            | 512 GB            | 1024 GB (1 TB)    |
| IOP's per schijf       | 500               | 2300              | 5000              |
| Doorvoer per schijf | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde |

**Beheerde standaardschijven**

Er zijn vijf typen Standard-beheerde schijven die kunnen worden gebruikt met uw virtuele machine. Elk van deze andere capaciteit hebben maar dezelfde IOPS en doorvoerlimieten hebben. Kies het type van de Standard-beheerde schijven op basis van de capaciteitsbehoeften van uw toepassing.

| Standard-schijftype  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Schijfgrootte           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   |
| IOP's per schijf       | 500              | 500              | 500              | 500              | 500              |
| Doorvoer per schijf | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde | 60 MB per seconde |

### <a name="disk-caching-policy"></a>Het beleid voor schijf 

**Premium beheerde schijven**

Beleid voor de schijfcache is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen-schrijven* voor de Premium-schijf is gekoppeld aan de VM. Deze configuratieinstelling wordt aanbevolen de optimale prestaties voor uw toepassing IOs bereiken. Voor schijven schrijven zware of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden), uitschakelen schijfcache, zodat u kunt betere prestaties bereiken.

### <a name="pricing"></a>Prijzen

Controleer de [prijzen voor schijven beheerd](https://azure.microsoft.com/pricing/details/managed-disks/). Prijzen van beheerde Premium-schijven is hetzelfde als het niet-beheerde Premium-schijven. Maar prijzen voor beheerde standaardschijven is anders dan standaardschijven zonder begeleiding.


## <a name="next-steps"></a>Volgende stappen

- Voordat u de VHD uploadt naar Azure, u moet volgen [voorbereiden van een Windows-VHD of VHDX uploaden naar Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
