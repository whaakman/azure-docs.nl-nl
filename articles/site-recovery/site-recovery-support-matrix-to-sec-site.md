---
title: De ondersteuningsmatrix voor replicatie naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: da120d8e325867eaf9eb8b9be1ae8d9152db54c4
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>De ondersteuningsmatrix voor replicatie naar een secundaire site met Azure Site Recovery

In dit artikel bevat een overzicht van wat wordt ondersteund wanneer u de [Azure Site Recovery](site-recovery-overview.md) service naar een secundaire on-premises site.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

**Implementatie** | **Details** 
--- | ---
**VMware naar VMware** | Herstel na noodgevallen van lokale virtuele VMware-machines naar secundaire VMware-site.<br/><br/> Download de [InMage Scout gebruikershandleiding](https://aka.ms/asr-scout-user-guide)
**Hyper-V naar Hyper-V** | Herstel na noodgevallen van lokale Hyper-V-machines in VMM-clouds naar een secundaire VMM-cloud.<br></br> Zonder VMM niet wordt ondersteund.



  

## <a name="host-servers"></a>Host-servers

**Implementatie** | **Ondersteuning**
--- | ---
**VMware-virtuele machine of fysieke server** | vCenter 5.5, 6.0 en 6.5 (ondersteuning voor alleen 5.5 functies)
**Hyper-V met VMM** | Windows Server 2016 en Windows Server 2012 R2 met de meest recente updates.<br/><br/> Windows Server 2016 hosts moeten worden beheerd door VMM 2016.<br/><br/> VMM 2016 clouds met een mengeling van Windows Server 2016 en 2012 R2-hosts worden momenteel niet ondersteund.<br/><br/> Implementatie met een upgrade van een bestaande VMM 2012 R2 naar System Center 2016 worden momenteel niet ondersteund.


## <a name="support-for-replicated-machine-os-versions"></a>Ondersteuning voor gerepliceerde machine OS-versies

De volgende tabel geeft een overzicht van besturingssysteemondersteuning voor machines die worden gerepliceerd met Site Recovery. Elke werkbelasting kan worden uitgevoerd op een ondersteund besturingssysteem.

**VMware of fysieke server** | **Hyper-V (met VMM)**
--- | ---
64-bits Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minste SP1<br/><br/> Red Hat Enterprise Linux 6.7, 6,8, 6,9, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6.7, 6,8, 6,9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6,8 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3, 11 SP4  | Een besturingssysteem Gast [ondersteund door Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)

## <a name="linux-machine-storage"></a>Linux-opslag

Alleen Linux-machines met de volgende opslag kunnen worden gerepliceerd:

- Bestandssysteem (EXT3, ETX4, ReiserFS, XFS).
- Multipath-software-apparaat toewijzen.
- Volume manager (LVM2).
- Fysieke servers met HP CCISS controller opslag worden niet ondersteund.
- Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration"></a>Netwerkconfiguratie

### <a name="hosts"></a>Hosts

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met VMM)**
--- | --- | ---
NIC-koppeling | Ja | Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nee | Nee

### <a name="guest-vms"></a>Gast-VM 's

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met VMM)**
--- | --- | ---
NIC-koppeling | Nee | Nee
IPv4 | Ja | Ja
IPv6 | Nee | Nee
Vaste IP-adres (Windows) | Ja | Ja
Vaste IP-adres (Linux) | Ja | Ja
Meerdere NIC 's | Ja | Ja


## <a name="storage"></a>Storage

### <a name="host-storage"></a>Opslag van de host

**Storage (host)** | **VMware of fysieke server** | **Hyper-V (met VMM)**
--- | --- | ---
NFS | Ja | N.v.t.
SMB 3.0 | N.v.t. | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO) | Ja | Ja

### <a name="guest-or-physical-server-storage"></a>Gast of opslag van de fysieke server

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met VMM)**
--- | --- | ---
VMDK | Ja | N.v.t.
VHD/VHDX | N.v.t. | Ja (maximaal 16 schijven)
Generatie 2 VM | N.v.t. | Ja
Clusterschijf gedeeld | Ja  | Nee
Versleutelde schijf | Nee | Nee
UEFI| Ja | N.v.t.
NFS | Nee | Nee
SMB 3.0 | Nee | Nee
RDM | Ja | N.v.t.
Schijf > 1 TB | Ja | Ja
Volume met striped schijf > 1 TB<br/><br/> LVM | Ja | Ja
Opslagruimten | Nee | Ja
Schijf hot toevoegen of verwijderen | Ja | Nee
Schijf uitsluiten | Ja | Ja
Multipath (MPIO) | N.v.t. | Ja

## <a name="vaults"></a>Kluizen

**Actie** | **VMware of fysieke server** | **Hyper-V (met VMM)**
--- | --- | ---
Verplaatsen van kluizen naar andere resourcegroepen (binnen of tussen abonnementen) | Nee | Nee
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen (binnen of tussen abonnementen) | Nee | Nee

## <a name="provider-and-agent"></a>Provider en agent

**Naam** | **Beschrijving** | **Meest recente versie** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery Provider** | Coördineert de communicatie tussen de on-premises servers en Azure <br/><br/> Op de on-premises VMM-servers of op de Hyper-V-servers worden geïnstalleerd als er geen VMM-server | 5.1.19 ([beschikbaar is via de portal](http://aka.ms/downloaddra)) | [Meest recente functies en oplossingen](https://support.microsoft.com/kb/3155002)
**Mobility-service** | Coördineert de replicatie tussen de on-premises VMware-servers of fysieke servers en de secundaire site<br/><br/> Geïnstalleerd op de VMware-VM of fysieke servers dat u wilt repliceren  | N.V.T. (beschikbaar via de portal) | N.v.t.


## <a name="next-steps"></a>Volgende stappen

- [Hyper-V-machines in VMM-clouds repliceren naar een secundaire site](tutorial-vmm-to-vmm.md)
- [Virtuele VMware-machines en fysieke servers repliceren naar een secundaire site](tutorial-vmware-to-vmware.md)
