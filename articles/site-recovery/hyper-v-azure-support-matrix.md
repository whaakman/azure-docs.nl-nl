---
title: De ondersteuningsmatrix voor Hyper-V-replicatie naar Azure | Microsoft Docs
description: Geeft een overzicht van de ondersteunde onderdelen en de vereisten voor Hyper-V-replicatie naar Azure met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: d2c637dc742ee854c7787cf7cd883930c4eaa8bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>De ondersteuningsmatrix voor Hyper-V-replicatie naar Azure


In dit artikel bevat een overzicht van de ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van lokale Hyper-V-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Ondersteunde scenario's

**Scenario** | **Details**
--- | --- 
Hyper-V met Virtual Machine Manager | U kunt herstel na noodgevallen in Azure uitvoeren voor virtuele machines waarop Hyper-V-hosts die worden beheerd in de System Center Virtual Machine Manager-infrastructuur.<br/><br/> U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren.<br/><br/> Wanneer u Hyper-V-hosts worden beheerd door Virtual Machine Manager, kunt u ook herstel na noodgevallen aan een secundaire on-premises site uitvoeren. Lees meer informatie over dit scenario, [in deze zelfstudie](tutorial-vmm-to-vmm.md).
Hyper-V zonder Virtual Machine Manager | U kunt herstel na noodgevallen in Azure uitvoeren voor virtuele machines waarop Hyper-V-hosts die niet door Virtual Machine Manager worden beheerd.<br/><br/> U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren. 


## <a name="on-premises-servers"></a>On-premises servers

**Server** | **Vereisten** | **Details**
--- | --- | ---
Hyper-V (voert zonder Virtual Machine Manager) | Windows Server 2016, Windows Server 2012 R2 met de meest recente updates | Wanneer u een Hyper-V-site in Site Recovery configureert, wordt niet een combinatie van hosts met Windows Server 2016 en 2012 R2 ondersteund.<br/><br/> Voor virtuele machines zich bevinden op een host waarop Windows Server 2016, wordt niet herstel naar een andere locatie ondersteund.
Hyper-V (uitgevoerd met Virtual Machine Manager) | Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Als Virtual Machine Manager wordt gebruikt, moeten Windows Server 2016-hosts worden beheerd in Virtual Machine Manager 2016.<br/><br/> Een Virtual Machine Manager-cloud die Hyper-V-hosts met Windows Server 2016 en 2012 R2 combineert wordt momenteel niet ondersteund.<br/><br/> Omgevingen met een upgrade van een bestaande Virtual Machine Manager 2012 R2-server naar 2016 worden niet ondersteund.


## <a name="replicated-vms"></a>Gerepliceerde virtuele machines


De volgende tabel geeft een overzicht van de VM-ondersteuning. Site Recovery biedt ondersteuning voor alle werkbelastingen die worden uitgevoerd op een ondersteund besturingssysteem. 

 **Onderdeel** | **Details**
--- | ---
VM-configuratie | Virtuele machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#failed-over-azure-vm-requirements).
Gastbesturingssysteem | Een gastbesturingssysteem [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server wordt niet ondersteund.




## <a name="hyper-v-network-configuration"></a>Hyper-V-netwerkconfiguratie

**Onderdeel** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | ---
Netwerk host: NIC Teaming | Ja
Netwerk host: VLAN | Ja
Netwerk host: IPv4 | Ja
Netwerk host: IPv6 | Nee
Gast-VM-netwerk: NIC Teaming | Nee
Gast-VM-netwerk: IPv4 | Ja
Gast-VM-netwerk: IPv6 | Nee
Gast-VM-netwerk: statisch IP-adres (Windows) | Ja
Gast-VM-netwerk: statisch IP-adres (Linux) | Nee
Gast-VM-netwerk: meerdere NIC's | Ja



## <a name="azure-vm-network-configuration-after-failover"></a>Azure VM-netwerk-configuratie (na een failover)

**Onderdeel** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | ---
ExpressRoute van Azure | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Azure Traffic Manager | Ja | Ja
Multi-NIC | Ja | Ja
Reserved IP | Ja | Ja
IPv4 | Ja | Ja
IP-bronadres behouden | Ja | Ja
Azure Virtual Network service-eindpunten<br/><br/> (Azure Storage firewalls en virtuele netwerken) | Nee | Nee


## <a name="hyper-v-host-storage"></a>Opslag voor Hyper-V-host

**Storage** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | --- | ---
NFS | N.v.t. | N.v.t.
SMB 3.0 | Ja | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO). Getest met:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM voor CLARiiON | Ja | Ja

## <a name="hyper-v-vm-guest-storage"></a>Opslag voor Hyper-V VM-Gast

**Storage** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
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
Volume met striped schijf > 1 TB<br/><br/> Logische volumebeheer (LVM) | Ja | Ja
Opslagruimten | Ja | Ja
Schijf hot toevoegen of verwijderen | Nee | Nee
Schijf uitsluiten | Ja | Ja
Multipath (MPIO) | Ja | Ja

## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | ---
Lokaal redundante opslag | Ja | Ja
Geografisch redundante opslag | Ja | Ja
Geografisch redundante opslag met leestoegang | Ja | Ja
Cool storage | Nee | Nee
Hot storage| Nee | Nee
Blok-blobs | Nee | Nee
Codering in rust (SSE)| Ja | Ja
Premium Storage | Ja | Ja
Service voor importeren/exporteren | Nee | Nee
Azure Virtual Network service-eindpunten (Azure Storage firewalls en virtuele netwerken) op het doel naar cache storage-account gebruikt voor van replicatiegegevens | Nee | Nee


## <a name="azure-compute-features"></a>Azure compute-functies

**Functie** | **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | ---
Beschikbaarheidssets | Ja | Ja
HUB | Ja | Ja  
Managed Disks | Ja, voor failover.<br/><br/> Failback van beheerde schijven wordt niet ondersteund. | Ja, voor failover.<br/><br/> Failback van beheerde schijven wordt niet ondersteund.

## <a name="azure-vm-requirements"></a>Azure VM-vereisten

Lokale virtuele machines die u naar Azure repliceert moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel samengevat.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gastbesturingssysteem | Site Recovery ondersteunt alle besturingssystemen die zijn [ondersteund door Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Controle van vereisten mislukt als een niet-ondersteund.
Architectuur van de Gast-besturingssysteem | 64-bits | Controle van vereisten mislukt als een niet-ondersteund.
Grootte van de besturingssysteemschijf | Maximaal 2048 GB voor virtuele machines van generatie 1.<br/><br/> 300 GB voor virtuele machines van generatie 2.  | Controle van vereisten mislukt als een niet-ondersteund.
Het aantal schijven voor besturingssysteem | 1 | Controle van vereisten mislukt als een niet-ondersteund.
Aantal gegevensschijven | 16 of minder  | Controle van vereisten mislukt als een niet-ondersteund.
De grootte van VHD gegevensschijf | 4095 GB | Controle van vereisten mislukt als een niet-ondersteund.
Netwerkadapters | Meerdere netwerkadapters worden ondersteund |
Gedeelde VHD | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund.
FC-schijf | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund.
Harde schijf-indeling | VHD <br/><br/> VHDX | VHDX site Recovery automatisch geconverteerd naar VHD wanneer u een failover naar Azure. Wanneer u een failover naar on-premises, blijven de virtuele machines gebruiken van de VHDX-indeling.
BitLocker | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat u replicatie voor een virtuele machine inschakelt.
VM-naam | 1 tot 63 tekens. Alleen letters, cijfers en afbreekstreepjes. De VM-naam moet beginnen en eindigen met een letter of cijfer. | Werk de waarde in de VM-eigenschappen in Site Recovery.
VM-type | Generatie 1<br/><br/> Generatie 2--Windows | Generatie 2 virtuele machines met een type besturingssysteem schijf basic (waaronder een of twee gegevensvolumes die zijn opgemaakt als VHDX) en minder dan 300 GB aan schijfruimte worden ondersteund.<br></br>Virtuele machines Linux generatie 2 worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Recovery Services-kluis acties

**Actie** |  **Hyper-V met Virtual Machine Manager** | **Hyper-V zonder Virtual Machine Manager**
--- | --- | --- 
Kluis over brongroepen verplaatsen<br/><br/> Binnen en tussen abonnementen | Nee | Nee 
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee | Nee 


## <a name="provider-and-agent"></a>Provider en agent

Om te zorgen dat uw implementatie is compatibel met de instellingen in dit artikel, moet dat u de meest recente provider en agent-versies uitvoert.

**Naam** | **Beschrijving** | **Details**
--- | --- | --- | --- | ---
Azure Site Recovery-provider | Coördineert de communicatie tussen de on-premises servers en Azure <br/><br/> Hyper-V met Virtual Machine Manager: geïnstalleerd op de Virtual Machine Manager-servers<br/><br/> Hyper-V zonder Virtual Machine Manager: geïnstalleerd op de Hyper-V-hosts| Meest recente versie: 5.1.2700.1 (beschikbaar via de Azure-portal)<br/><br/> [Meest recente functies en oplossingen](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure Recovery Services-agent | Coördineert de replicatie tussen Hyper-V-machines en Azure<br/><br/> Geïnstalleerd op de on-premises Hyper-V-servers (met of zonder Virtual Machine Manager) | Meest recente agent beschikbaar is via de portal






## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [voorbereiden Azure](tutorial-prepare-azure.md) voor herstel na noodgevallen van lokale Hyper-V-machines. 
