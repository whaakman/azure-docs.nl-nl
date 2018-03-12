---
title: De ondersteuningsmatrix voor replicatie van virtuele VMware-machines of fysieke servers naar een secundaire site VMware met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteuning voor replicatie van VMware of fysieke server naar een secundaire site met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: b51a4573ad7a8461b7261f08d94639d2030492d9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>De ondersteuningsmatrix voor replicatie van virtuele VMware-machines en fysieke servers naar een secundaire site

In dit artikel bevat een overzicht van wat wordt ondersteund wanneer u de [Azure Site Recovery](site-recovery-overview.md) VMware-machines of Windows of Linux fysieke servers repliceren naar een secundaire site van de VMware-service.

- Als u virtuele VMware-machines of fysieke servers repliceren naar Azure wilt, raadpleegt u [deze ondersteuningsmatrix](vmware-physical-azure-support-matrix.md).
- Als u virtuele Hyper-V-machines repliceren naar een secundaire site wilt, raadpleegt u [deze ondersteuningsmatrix](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Replicatie van de lokale virtuele VMware-machines en fysieke servers wordt verstrekt door InMage Scout. InMage Scout is opgenomen in de Azure Site Recovery-service-abonnement.


## <a name="host-servers"></a>Host-servers

**Besturingssysteem** | **Details**
--- | ---
vCenter-server | vCenter 5.5, 6.0 en 6.5<br/><br/> Als u 6.0 of 6.5 uitvoert, houd er rekening mee dat alleen 5.5 functies worden ondersteund.


## <a name="replicated-vm-support"></a>Ondersteuning voor VM gerepliceerd

De volgende tabel geeft een overzicht van besturingssysteemondersteuning voor machines die worden gerepliceerd met Site Recovery. Elke werkbelasting kan worden uitgevoerd op een ondersteund besturingssysteem.

**Besturingssysteem** | **Details**
--- | ---
Windows Server | 64-bits Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6.7, 6,8, 6,9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6,8 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


## <a name="linux-machine-storage"></a>Linux-opslag

Alleen Linux-machines met de volgende opslag kunnen worden gerepliceerd:

- Bestandssysteem (EXT3, ETX4, ReiserFS, XFS).
- Multipath-software-apparaat toewijzen.
- Volume manager (LVM2).
- Fysieke servers met HP CCISS controller opslag worden niet ondersteund.
- Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration---hostguest-vm"></a>Netwerkconfiguratie - Host/Gast-VM

**Configuratie** | **Ondersteund**  
--- | --- 
Host - NIC-koppeling | Ja 
Host - VLAN | Ja 
Host - IPv4 | Ja 
Host - IPv6 | Nee 
Gast-VM - NIC-koppeling | Nee
Gast-VM - IPv4 | Ja
Gast-VM - IPv6 | Nee
Gues VM Windows of Linux - statisch IP-adres | Ja
Gast-VM - meerdere NIC 's | Ja


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Opslag van de host

**Storage (host)** | **Ondersteund** 
--- | --- 
NFS | Ja 
SMB 3.0 | N/A 
SAN (ISCSI) | Ja 
Multipath (MPIO) | Ja 

### <a name="guest-or-physical-server-storage"></a>Gast of opslag van de fysieke server

**Configuratie** | **Ondersteund** 
--- | --- 
VMDK | Ja 
VHD/VHDX | N/A 
Generatie 2 VM | N/A 
Clusterschijf gedeeld | Ja 
Versleutelde schijf | Nee 
UEFI| Ja 
NFS | Nee 
SMB 3.0 | Nee 
RDM | Ja 
Schijf > 1 TB | Ja 
Volume met striped schijf > 1 TB<br/><br/> LVM | Ja 
Opslagruimten | Nee 
Schijf hot toevoegen of verwijderen | Ja 
Schijf uitsluiten | Ja 
Multipath (MPIO) | N/A 

## <a name="vaults"></a>Kluizen

**Actie** | **Ondersteund** 
--- | --- 
Verplaatsen van kluizen naar andere resourcegroepen (binnen of tussen abonnementen) | Nee 
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen (binnen of tussen abonnementen) | Nee 

## <a name="mobility-service-and-updates"></a>Mobility-service en -updates

De Mobility-service coördineert de replicatie tussen de on-premises VMware-servers of fysieke servers en de secundaire site. Bij het instellen van replicatie, moet u ervoor zorgen dat u hebt de meest recente versie van de Mobility-service en andere onderdelen.

**Update** | **Details** 
--- | --- 
Scout updates | [Meer informatie over en download](/vmware-physical-secondary-disaster-recovery.md#updates) de meest recente updates voor Scout | Scout updates zijn cumulatief.
Updates van onderdelen | Scout updates bevatten updates voor alle onderdelen, inclusief de RX-server, configuratieserver proces en hoofddoelservers, vContinuum-servers en bronservers die u wilt beveiligen.<br/><br/> [Meer informatie](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Volgende stappen

Download de [InMage Scout gebruikershandleiding](https://aka.ms/asr-scout-user-guide)

- [Hyper-V-machines in VMM-clouds repliceren naar een secundaire site](tutorial-vmm-to-vmm.md)
- [Virtuele VMware-machines en fysieke servers repliceren naar een secundaire site](tutorial-vmware-to-vmware.md)
