---
title: De ondersteuningsmatrix voor Hyper-V-replicatie Azure | Microsoft Docs
description: Geeft een overzicht van de ondersteunde onderdelen en de vereisten voor Hyper-V-replicatie naar Azure met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5918c56c2b7d01c884bf846e3a7d621b3393bb96
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>De ondersteuningsmatrix voor Hyper-V-replicatie naar Azure


In dit artikel bevat een overzicht van de ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van lokale Hyper-V-machines naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="supported-scenarios"></a>Ondersteunde scenario's

**Scenario** | **Details**
--- | --- 
**Hyper-V met VMM** | U kunt herstel na noodgevallen in Azure uitvoeren voor virtuele machines waarop Hyper-V-hosts in de System Center Virtual Machine Manager (VMM)-fabric worden beheerd<br/><br/> U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren.<br/><br/> Wanneer Hyper-V-hosts worden beheerd door VMM, kunt u ook herstel na noodgevallen aan een secundaire on-premises site uitvoeren. Lees [in dit artikel](tutorial-vmm-to-vmm.md) voor meer informatie over dit scenario.
**Hyper-V zonder VMM** | U kunt herstel na noodgevallen in Azure uitvoeren voor virtuele machines waarop Hyper-V-hosts die niet worden beheerd door VMM.<br/><br/> U kunt dit scenario in de Azure portal of met behulp van Powershell implementeren. 


## <a name="on-premises-servers"></a>On-premises servers

**Server** | **Vereisten** | **Details**
--- | --- | ---
**Hyper-V (uitgevoerd zonder VMM)** | Windows Server 2016, Windows Server 2012 R2 met de meest recente updates. | Wanneer u een Hyper-V-site in Site Recovery configureert, wordt niet een combinatie van hosts met Windows Server 2016 en 2012 R2 ondersteund.<br/><br/> Voor virtuele machines zich bevinden op een host waarop Windows Server 2016, wordt niet herstel naar een andere locatie ondersteund.
**Hyper-V (uitgevoerd met VMM)** | VMM 2016, VMM 2012 R2 | Als VMM wordt gebruikt, moeten Windows Server 2016-hosts worden beheerd in VMM 2016.<br/><br/> Een VMM-clouds die Hyper-V-hosts met Windows Server 2016 en 2012 R2 combineert wordt momenteel niet ondersteund.<br/><br/> Omgevingen met een upgrade van een bestaande VMM 2012 R2-server naar 2016 worden niet ondersteund.


## <a name="replicated-vms"></a>Gerepliceerde virtuele machines


De volgende tabel geeft een overzicht van de VM-ondersteuning. Site Recovery biedt ondersteuning voor alle werkbelastingen die worden uitgevoerd op een ondersteund besturingssysteem. 

 **Onderdeel** | **Details**
--- | ---
VM-configuratie | Virtuele machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#failed-over-azure-vm-requirements).
Gastbesturingssysteem | Een gastbesturingssysteem [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server wordt niet ondersteund.




## <a name="hyper-v-network-configuration"></a>Hyper-V-netwerkconfiguratie

**Onderdeel** | **Hyper-V met VMM** | **Hyper-V zonder VMM**
--- | --- | ---
Host-netwerk: NIC-koppeling | Ja
Host-netwerk: VLAN | Ja
Host-netwerk: IPv4 | Ja
Host-netwerk: IPv6 | Nee
Gast-VM-netwerk: NIC-koppeling | Nee
Gast-VM-netwerk: IPv4 | Ja
Gast-VM-netwerk: IPv6 | Nee
Gast-VM-netwerk: statische IP (Windows) | Ja
Gast-VM-netwerk: statische IP (Linux) | Nee
Gast-VM-netwerk: Multi-NIC | Ja



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM-netwerk-configuratie (na een failover)

**Onderdeel** | **Hyper-V met VMM** | **Hyper-V zonder VMM**
--- | --- | ---
ExpressRoute | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Meerdere NIC 's | Ja | Ja
Reserved IP | Ja | Ja
IPv4 | Ja | Ja
IP-bronadres behouden | Ja | Ja
VNET-service-eindpunten<br/><br/> (Firewalls azure-opslag en virtuele netwerken) | Nee | Nee


## <a name="hyper-v-host-storage"></a>Opslag voor Hyper-V-host

**Storage** | **Hyper-V met VMM** | Hyper-V zonder VMM
--- | --- | --- | ---
NFS | N.v.t. | N.v.t.
SMB 3.0 | Ja | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO). Getest met:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM voor CLARiiON | Ja | Ja

## <a name="hyper-v-vm-guest-storage"></a>Opslag voor Hyper-V VM-Gast

**Storage** | **Hyper-V met VMM** | Hyper-V zonder VMM
--- | --- | ---
VMDK | N.v.t. | N.v.t.
VHD/VHDX | Ja | Ja
Generatie 2 VM | Ja | Ja
EFI/UEFI| Ja | Ja
Clusterschijf gedeeld | Nee | Nee
Versleutelde schijf | Nee | Nee
NFS | N.v.t. | N.v.t.
SMB 3.0 | Nee | Nee
RDM | N.v.t. | N.v.t.
Schijf > 1 TB | Ja, 4095 GB | Ja, 4095 GB
Schijf: 4K logische en fysieke sector | Niet ondersteund: Gen 1/Gen 2 | Niet ondersteund: Gen 1/Gen 2
Schijf: fysieke sector van 512 bytes en 4K logische | Ja |  Ja
Volume met striped schijf > 1 TB<br/><br/> LVM logische volumebeheer | Ja | Ja
Opslagruimten | Ja | Ja
Schijf hot toevoegen of verwijderen | Nee | Nee
Schijf uitsluiten | Ja | Ja
Multipath (MPIO) | Ja | Ja

## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Hyper-V met VMM** | **Hyper-V zonder VMM)**
--- | --- | ---
LRS | Ja | Ja
GRS | Ja | Ja
RA-GRS | Ja | Ja
Cool storage | Nee | Nee
Hot storage| Nee | Nee
Blok-blobs | Nee | Nee
Versleuteling op rest(SSE)| Ja | Ja
Premium Storage | Ja | Ja
Service voor importeren/exporteren | Nee | Nee
VNET service-eindpunten (Azure-opslag firewalls en vnet's) op het doel naar cache storage-account gebruikt voor van replicatiegegevens | Nee | Nee


## <a name="azure-compute-features"></a>Azure compute-functies

**Functie** | **Hyper-V met VMM** | **Hyper-V zonder VMM**
--- | --- | ---
Beschikbaarheidssets | Ja | Ja
HUB | Ja | Ja  
Managed Disks | Ja, voor failover<br/><br/> Failback van beheerde schijven wordt niet ondersteund | Ja, voor failover<br/><br/> Failback van beheerde schijven wordt niet ondersteund

## <a name="azure-vm-requirements"></a>Azure VM-vereisten

Lokale virtuele machines die u naar Azure repliceert moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel samengevat.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
**Gastbesturingssysteem** | Site Recovery ondersteunt alle besturingssystemen die zijn [ondersteund door Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Controle van vereisten mislukt als een niet-ondersteund.
**Architectuur van de Gast-besturingssysteem** | 64-bits | Controle van vereisten mislukt als een niet-ondersteund.
**Grootte van de besturingssysteemschijf** | Maximaal 2048 GB voor virtuele machines van generatie 1.<br/><br/> 300 GB voor virtuele machines van generatie 2.  | Controle van vereisten mislukt als een niet-ondersteund.
**Het aantal schijven voor besturingssysteem** | 1 | Controle van vereisten mislukt als een niet-ondersteund.
**Aantal gegevensschijven** | 16 of minder  | Controle van vereisten mislukt als een niet-ondersteund.
**De grootte van VHD gegevensschijf** | 4095 GB | Controle van vereisten mislukt als een niet-ondersteund.
**Netwerkadapters** | Meerdere netwerkadapters worden ondersteund |
**Gedeelde VHD** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund.
**FC-schijf** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund.
**Harde schijf-indeling** | VHD <br/><br/> VHDX | VHDX site Recovery automatisch geconverteerd naar VHD wanneer u een failover naar Azure. Wanneer u een failover naar on-premises blijven de virtuele machines gebruiken van de VHDX-indeling.
**BitLocker** | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat u replicatie voor een virtuele machine inschakelt.
**VM-naam** | Tussen 1 en 63 tekens. Alleen letters, cijfers en afbreekstreepjes. De VM-naam moet beginnen en eindigen met een letter of cijfer. | Werk de waarde in de VM-eigenschappen in Site Recovery.
**VM-type** | Generatie 1<br/><br/> Generatie 2--Windows | Generatie 2 virtuele machines met een type besturingssysteem schijf basic (waaronder een of twee gegevensvolumes die zijn opgemaakt als VHDX) en minder dan 300 GB aan schijfruimte worden ondersteund.<br></br>Virtuele machines Linux generatie 2 worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Recovery Services-kluis acties

**Actie** |  **Hyper-V met VMM** | **Hyper-V zonder VMM**
--- | --- | --- 
Kluis over brongroepen verplaatsen<br/><br/> Binnen en tussen abonnementen | Nee | Nee 
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee | Nee 


## <a name="provider-and-agent"></a>Provider en Agent

Om te zorgen dat uw implementatie is compatibel met de instellingen in dit artikel, moet dat u de meest recente provider en agent-versies uitvoert.

**Naam** | **Beschrijving** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery Provider** | Coördineert de communicatie tussen de on-premises servers en Azure <br/><br/> Hyper-V met VMM: geïnstalleerd op VMM-servers<br/><br/> Hyper-V zonder VMM: geïnstalleerd op de Hyper-V-hosts| Meest recente versie: 5.1.2700.1 (beschikbaar via de portal)<br/><br/> [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
**Microsoft Azure Recovery Services (MARS)-agent** | Coördineert de replicatie tussen Hyper-V-machines en Azure<br/><br/> Geïnstalleerd op de on-premises Hyper-V-servers (met of zonder VMM) | Meest recente agent beschikbaar is via de portal






## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [voorbereiden Azure](tutorial-prepare-azure.md) voor herstel na noodgevallen van lokale Hyper-V-machines. 
