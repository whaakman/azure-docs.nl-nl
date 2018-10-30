---
title: Ondersteuningsmatrix voor herstel na noodgevallen van Hyper-V-machines in VMM-clouds naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van ondersteuning voor replicatie van de Hyper-V virtuele machines in VMM-clouds naar een secundaire site met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: 3203ae48163ba325c8dad2f29c9f566e330928b5
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214287"
---
# <a name="support-matrix-for-disaster-recovery-of-hyper-v-vms-to-a-secondary-site"></a>Ondersteuningsmatrix voor herstel na noodgevallen van Hyper-V-machines naar een secundaire site

In dit artikel bevat een overzicht van wat wordt ondersteund wanneer u de [Azure Site Recovery](site-recovery-overview.md) service voor het repliceren van Hyper-V-machines worden beheerd in System Center Virtual Machine Manager (VMM)-clouds naar een secundaire site. Als u virtuele Hyper-V-machines repliceren naar Azure wilt, raadpleegt u [deze ondersteuningsmatrix](hyper-v-azure-support-matrix.md).

> [!NOTE]
> U kunt alleen repliceren naar een secundaire site, wanneer uw Hyper-V-hosts worden beheerd in VMM-clouds.

  

## <a name="host-servers"></a>Host-servers

**Besturingssysteem** | **Details**
--- | ---
Windows Server 2012 R2 | Servers moeten de meest recente updates worden uitgevoerd.
Windows Server 2016 |  VMM 2016 clouds met een combinatie van Windows Server 2016 en 2012 R2-hosts worden momenteel niet ondersteund.<br/><br/> Implementaties waarvoor een upgrade van System Center 2012 R2 VMM 2012 R2 wordt uitgevoerd naar System Center 2016 worden momenteel niet ondersteund.


## <a name="replicated-vm-support"></a>Ondersteuning voor VM gerepliceerd

De volgende tabel geeft een overzicht van ondersteuning voor het besturingssysteem voor computers die worden gerepliceerd met Site Recovery. Elke workload kan worden uitgevoerd op het ondersteunde besturingssysteem.

**Windows-versie** | **Hyper-V (met VMM)**
--- | ---
Windows Server 2016 | Een besturingssysteem Gast [ondersteund door Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) op Windows Server 2016 
Windows Server 2012 R2 | Een besturingssysteem Gast [ondersteund door Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) op Windows Server 2012 R2

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
NFS | N/A
SMB 3.0 |  Ja
SAN (ISCSI) | Ja
Multipath (MPIO) | Ja

### <a name="guest-or-physical-server-storage"></a>Gast- of fysieke server-opslag

**Configuratie** | **Ondersteund**
--- | --- | 
VMDK |  N/A
VHD/VHDX | Ja (maximaal 16 schijven)
Generatie 2 virtuele machine | Ja
Gedeelde clusterschijf | Nee
Versleutelde schijf | Nee
UEFI| N/A
NFS | Nee
SMB 3.0 | Nee
RDM | N/A
Schijf > 1 TB | Ja
Volume met striped schijf > 1 TB<br/><br/> LVM | Ja
Opslagruimten | Ja
Hot toevoegen of verwijderen-schijf | Nee
Schijf uitsluiten | Ja
Multipath (MPIO) | Ja

## <a name="vaults"></a>Kluizen

**Actie** | **Ondersteund**
--- | --- 
Verplaatsen van kluizen andere resourcegroepen (binnen of tussen abonnementen) |  Nee
Verplaatsen van opslag, netwerk, Azure-VM's op resourcegroepen (binnen of tussen abonnementen) | Nee

## <a name="azure-site-recovery-provider"></a>Azure Site Recovery-Provider

De Provider coördineert de communicatie tussen VMM-servers. 

**meest recente** | **Updates**
--- | --- | --- | --- | ---
5.1.19 ([beschikbaar is via de portal](http://aka.ms/downloaddra) | [Nieuwste functies en correcties](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Volgende stappen

[Hyper-V-machines in VMM-clouds repliceren naar een secundaire site](tutorial-vmm-to-vmm.md)

