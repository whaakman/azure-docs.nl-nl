---
title: Versnelde netwerken met virtuele machine van Azure-noodherstel | Microsoft Docs
description: Beschrijft hoe u om in te schakelen met Azure Site Recovery versnelde netwerken voor virtuele machine van Azure-noodherstel
services: site-recovery
documentationcenter: ''
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: bb4a3f433f213984c4c351439760ddd3327cda73
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353133"
---
# <a name="accelerated-networking-with-azure-virtual-machine-disaster-recovery"></a>Versneld netwerken met virtuele machine van Azure-noodherstel

Versneld netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) aan een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad krachtige omzeilt de host van het gegevenspad verminderen latentie en jitter CPU-gebruik, voor gebruik met de meest veeleisende workloads netwerk op ondersteunde VM-typen. De volgende afbeelding ziet u de communicatie tussen twee virtuele machines met en zonder versnelde netwerken:

![Vergelijking](./media/azure-vm-disaster-recovery-with-accelerated-networking/accelerated-networking-benefit.png)

Azure Site Recovery kunt u gebruikmaken van de voordelen van versnelde netwerken voor Azure-machines waarvoor een failover naar een andere Azure-regio is. Dit artikel wordt beschreven hoe u de versnelde netwerken kunt inschakelen voor Azure-machines met Azure Site Recovery gerepliceerd.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u begrijpt:
-   Virtuele Azure-machine [-replicatiearchitectuur](azure-to-azure-architecture.md)
-   [Instellen van replicatie](azure-to-azure-tutorial-enable-replication.md) voor virtuele machines van Azure
-   [Failover-overschakeling uitvoeren](azure-to-azure-tutorial-failover-failback.md) virtuele machines van Azure

## <a name="accelerated-networking-with-windows-vms"></a>Versneld netwerken met Windows-VM 's

Azure Site Recovery biedt ondersteuning voor inschakelen van versnelde netwerken voor gerepliceerde virtuele machines alleen als de virtuele bronmachine versnelde netwerken ingeschakeld heeft. Als de bron-VM geen versnelde netwerken ingeschakeld, leert u hoe u versneld netwerken voor Windows virtuele machines inschakelen [hier](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende distributies worden gebruiksklaar uit de galerie met Azure ondersteund:
* **Windows Server 2016 Datacenter**
* **Windows Server 2012 R2 Datacenter**

### <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versneld netwerken wordt ondersteund op de meest algemene en geoptimaliseerde exemplaargrootten met 2 of meer vcpu's.  Deze ondersteunde reeksen zijn: D/DSv2 en F/Fs

Op de VM-exemplaren met 4 of meer vcpu's wordt versnelde netwerken op instanties die ondersteuning bieden voor hyperthreading is, ondersteund. Ondersteunde reeksen zijn: D/DSv3, E/ESv3 Fsv2 en Ms-/ Mms-

Zie voor meer informatie over VM-exemplaren [Windows VM-grootten](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="accelerated-networking-with-linux-vms"></a>Versneld netwerken met virtuele Linux-machines

Azure Site Recovery biedt ondersteuning voor inschakelen van versnelde netwerken voor gerepliceerde virtuele machines alleen als de virtuele bronmachine versnelde netwerken ingeschakeld heeft. Als de bron-VM geen versnelde netwerken ingeschakeld, leert u hoe u voor het inschakelen van versnelde netwerken voor virtuele Linux-machines [hier](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende distributies worden gebruiksklaar uit de galerie met Azure ondersteund:
* **Ubuntu 16.04**
* **SLES 12 SP3**
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS-Linux**
* **Debian "uitrekken" backports kernel**
* **Oracle Linux 7.4**

### <a name="supported-vm-instances"></a>Ondersteunde VM-exemplaren
Versneld netwerken wordt ondersteund op de meest algemene en geoptimaliseerde exemplaargrootten met 2 of meer vcpu's.  Deze ondersteunde reeksen zijn: D/DSv2 en F/Fs

Op de VM-exemplaren met 4 of meer vcpu's wordt versnelde netwerken op instanties die ondersteuning bieden voor hyperthreading is, ondersteund. Ondersteunde reeksen zijn: D/DSv3, E/ESv3 Fsv2 en Ms-/ Mms.

Zie voor meer informatie over VM-exemplaren [Linux VM-grootten](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enabling-accelerated-networking-for-replicated-vms"></a>Inschakelen van versnelde netwerken voor gerepliceerde virtuele machines

Wanneer u [inschakelen replicatie](azure-to-azure-tutorial-enable-replication.md) voor virtuele machines van Azure, Site Recovery worden automatisch gedetecteerd of de virtuele machine-netwerkinterfaces versnelde netwerken ingeschakeld hebben. Als al versnelde netwerken is ingeschakeld, configureert Site Recovery automatisch versnelde netwerken op de netwerkinterfaces van de gerepliceerde virtuele machine.

De status van versnelde netwerken kan worden geverifieerd bij de **netwerkinterfaces** sectie van de **berekening en netwerk** instellingen voor de gerepliceerde virtuele machine.

![Versneld netwerken instellen](./media/azure-vm-disaster-recovery-with-accelerated-networking/compute-network-accelerated-networking.png)

Als u hebt versnelde netwerken ingeschakeld op de virtuele bronmachine na het inschakelen van replicatie, kunt u inschakelen versnelde netwerken voor de netwerkinterfaces van de gerepliceerde virtuele machine door het volgende proces:
1. Open **berekening en netwerk** instellingen voor de gerepliceerde virtuele machine
2. Klik op de naam van de netwerkinterface onder de **netwerkinterfaces** sectie
3. Selecteer **ingeschakeld** in de vervolgkeuzelijst voor versnelde netwerken onder de **doel** kolom

![Versneld netwerken inschakelen](./media/azure-vm-disaster-recovery-with-accelerated-networking/network-interface-accelerated-networking-enabled.png)

Het bovenstaande proces moet ook worden gevolgd voor bestaande gerepliceerde virtuele machines, die niet eerder ingeschakeld, worden automatisch door Site Recovery versnelde netwerken hebt.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [voordelen van versnelde netwerken](../virtual-network/create-vm-accelerated-networking-powershell.md#benefits).
- Meer informatie over beperkingen en beperkingen van versnelde netwerken voor [Windows virtuele machines](../virtual-network/create-vm-accelerated-networking-powershell.md#limitations-and-constraints) en [virtuele Linux-machines](../virtual-network/create-vm-accelerated-networking-cli.md#limitations-and-constraints).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover wilt automatiseren.
