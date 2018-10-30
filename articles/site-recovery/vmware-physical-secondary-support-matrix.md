---
title: Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar een secundaire VMware-site met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteuning voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar een secundaire site met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: raynew
ms.openlocfilehash: 8406cf2b31b87cdb4f6c9b6ba3d06559cf1d4d41
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209799"
---
# <a name="support-matrix-for-disaster-recovery-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar een secundaire site

In dit artikel bevat een overzicht van wat wordt ondersteund wanneer u de [Azure Site Recovery](site-recovery-overview.md) fysieke servers naar een secundaire VMware-site voor herstel na noodgevallen van virtuele VMware-machines of Windows/Linux-service.

- Als u repliceren van VMware-machines of fysieke servers naar Azure wilt, raadpleegt u [deze ondersteuningsmatrix](vmware-physical-azure-support-matrix.md).
- Als u repliceren van Hyper-V-machines naar een secundaire site wilt, lees dan [deze ondersteuningsmatrix](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Replicatie van on-premises VMware-machines en fysieke servers is geleverd door InMage Scout. InMage Scout is opgenomen in de Azure Site Recovery-service-abonnement.


## <a name="host-servers"></a>Host-servers

**Besturingssysteem** | **Details**
--- | ---
vCenter-server | vCenter 5.5, 6.0 en 6.5<br/><br/> Als u 6.0 of 6.5 wordt uitgevoerd, houd er rekening mee dat alleen 5.5 functies worden ondersteund.


## <a name="replicated-vm-support"></a>Ondersteuning voor VM gerepliceerd

De volgende tabel geeft een overzicht van ondersteuning voor het besturingssysteem voor computers die worden gerepliceerd met Site Recovery. Elke workload kan worden uitgevoerd op het ondersteunde besturingssysteem.

**Besturingssysteem** | **Details**
--- | ---
Windows Server | 64-bits Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1.
Linux | Red Hat Enterprise Linux 6.7, 6,8, 6,9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6,8, 6,9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6,8 met Red Hat compatibele kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4 


## <a name="linux-machine-storage"></a>Opslag van de Linux-machine

Alleen Linux-machines met de volgende opslag kunnen worden gerepliceerd:

- Bestandssysteem (EXT3, ETX4, ReiserFS, XFS).
- Toewijzen van multipath-software-apparaat.
- Volume manager (LVM2).
- Fysieke servers met HP CCISS controller storage worden niet ondersteund.
- Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration---hostguest-vm"></a>Netwerkconfiguratie van - Host/Gast-VM

**Configuratie** | **Ondersteund**  
--- | --- 
Host - NIC-koppeling | Ja 
Host - VLAN | Ja 
Host - IPv4 | Ja 
Host - IPv6 | Nee 
Gast-VM - NIC-koppeling | Nee
Gast-VM - IPv4 | Ja
Gast-VM - IPv6 | Nee
Statische IP-adres Gues VM Windows/Linux- | Ja
Gast-VM - Multi-NIC | Ja


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Opslag van de host

**Opslag (host)** | **Ondersteund** 
--- | --- 
NFS | Ja 
SMB 3.0 | N/A 
SAN (ISCSI) | Ja 
Multipath (MPIO) | Ja 

### <a name="guest-or-physical-server-storage"></a>Gast- of fysieke server-opslag

**Configuratie** | **Ondersteund** 
--- | --- 
VMDK | Ja 
VHD/VHDX | N/A 
Generatie 2 virtuele machine | N/A 
Gedeelde clusterschijf | Ja 
Versleutelde schijf | Nee 
UEFI| Ja 
NFS | Nee 
SMB 3.0 | Nee 
RDM | Ja 
Schijf > 1 TB | Ja 
Volume met striped schijf > 1 TB<br/><br/> LVM | Ja 
Opslagruimten | Nee 
Hot toevoegen of verwijderen-schijf | Ja 
Schijf uitsluiten | Ja 
Multipath (MPIO) | N/A 

## <a name="vaults"></a>Kluizen

**Actie** | **Ondersteund** 
--- | --- 
Verplaatsen van kluizen andere resourcegroepen (binnen of tussen abonnementen) | Nee 
Verplaatsen van opslag, netwerk, Azure-VM's op resourcegroepen (binnen of tussen abonnementen) | Nee 

## <a name="mobility-service-and-updates"></a>Mobility-service en updates

De Mobility-service coördineert de replicatie tussen on-premises VMware-servers of fysieke servers en de secundaire site. Bij het instellen van replicatie, moet u ervoor zorgen dat u hebt de nieuwste versie van de Mobility-service, en andere onderdelen.

**Update** | **Details** 
--- | --- 
Scout-updates | [Meer informatie over en download](vmware-physical-secondary-disaster-recovery.md#updates) de meest recente updates voor Scout | Scout-updates zijn cumulatief.
Updates van onderdelen | Scout-updates bevatten updates voor alle onderdelen, waaronder de RX-server, configuratieserver, proces en hoofddoelservers, vContinuum-servers en bronservers die u wilt beveiligen.<br/><br/> [Meer informatie](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Volgende stappen

Download de [InMage Scout-gebruikershandleiding](https://aka.ms/asr-scout-user-guide)

- [Hyper-V-machines in VMM-clouds repliceren naar een secundaire site](tutorial-vmm-to-vmm.md)
- [Virtuele VMware-machines en fysieke servers repliceren naar een secundaire site](tutorial-vmware-to-vmware.md)
