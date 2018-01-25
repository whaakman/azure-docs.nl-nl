---
title: Azure Site Recovery-ondersteuningsmatrix voor het repliceren naar Azure | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor Azure Site Recovery
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/30/2017
ms.author: rajanaki
ms.openlocfilehash: a72c9104dc2df0c8a874f757c100a19dc26c1564
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-to-azure"></a>Azure Site Recovery-ondersteuningsmatrix voor het repliceren van on-premises naar Azure


In dit artikel bevat een overzicht van ondersteunde configuraties en -onderdelen voor Azure Site Recovery wanneer repliceren en herstellen naar Azure. Zie voor meer informatie over de vereisten voor Azure Site Recovery de [vereisten](site-recovery-prereq.md).

> [!NOTE]
> Zorg ervoor dat u naar de nieuwste versie van de Site Recovery provider en de agent voor compatibiliteit met de updates in de ondersteuningsmatrix bijwerken.


## <a name="support-for-deployment-options"></a>Ondersteuning voor de implementatie-opties

**Implementatie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)** |
--- | --- | ---
**Azure Portal** | On-premises virtuele VMware-machines naar Azure-opslag, met de Azure Resource Manager of klassieke opslag en netwerken.<br/><br/> Failover naar Resource Manager gebaseerde of klassieke VM's. | On-premises Hyper-V-machines naar Azure-opslag, met Resource Manager of klassieke opslag en netwerken.<br/><br/> Failover naar Resource Manager gebaseerde of klassieke VM's.
**Klassieke portal** | Onderhoudsmodus bevinden. Nieuwe kluizen kunnen niet worden gemaakt. | Onderhoudsmodus bevinden.
**PowerShell** | Ondersteund | Ondersteund


## <a name="support-for-datacenter-management-servers"></a>Ondersteuning voor datacenter-beheerservers

### <a name="virtualization-management-entities"></a>Virtualization management-entiteiten

**Implementatie** | **Ondersteuning**
--- | ---
**VMware-virtuele machine of fysieke server** | vCenter 6.5, 6.0 of 5.5
**Hyper-V (met Virtual Machine Manager)** | System Center Virtual Machine Manager 2016 en System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > Een System Center Virtual Machine Manager 2016-cloud met een mengeling van Windows Server 2016 en 2012 R2-hosts wordt momenteel niet ondersteund.
  > Configuraties met de upgrade van een bestaande SCVMM 2012 R2 naar 2016 is momenteel niet ondersteund.
### <a name="host-servers"></a>Host-servers

**Implementatie** | **Ondersteuning**
--- | ---
**VMware-virtuele machine of fysieke server** | vSphere 6.5 6.0, 5.5
**Hyper-V (met/zonder Virtual Machine Manager)** | Windows Server 2016, Windows Server 2012 R2 met de meest recente updates.<br></br>Als SCVMM wordt gebruikt, moeten Windows Server 2016-hosts worden beheerd door SCVMM 2016.


  >[!Note]
  >Een Hyper-V-site die combineert hosts met Windows Server 2016 en 2012 R2 wordt momenteel niet ondersteund. Herstel naar een alternatieve locatie voor virtuele machines op een host met Windows Server 2016 wordt momenteel niet ondersteund.

## <a name="support-for-replicated-machine-os-versions"></a>Ondersteuning voor gerepliceerde machine OS-versies

Virtuele machines die zijn beveiligd, moet voldoen aan [Azure-vereisten](#failed-over-azure-vm-requirements) bij het repliceren naar Azure.
De volgende tabel geeft een overzicht van ondersteuning voor gerepliceerde besturingssystemen in verschillende scenario's tijdens het gebruik van Azure Site Recovery. Deze ondersteuning geldt voor elke werkbelasting die wordt uitgevoerd op de genoemde OS.

 **VMware of fysieke server** | **Hyper-V (met/zonder VMM)** |
--- | --- |
64-bits Windows Server 2016 (Server Core, Server met Bureaubladbelevenis)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minste SP1<br/><br/> Red Hat Enterprise Linux: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4<br/><br/>CentOS: 5.2 to 5.11, 6.1 to 6.9, 7.0 to 7.4 <br/><br/>Ubuntu 14.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Upgrade van de computers van SLES 11 SP3 repliceren naar SLES 11 SP4 wordt niet ondersteund. Als een gerepliceerde machine is bijgewerkt van SLES 11SP3 naar SLES 11 SP4 is geïnstalleerd, moet u replicatie uitschakelen en beveiligt de machine opnieuw na de upgrade.) | Een gastbesturingssysteem [ondersteund door Azure](https://technet.microsoft.com/library/cc794868.aspx)

>[!NOTE]
>
> \*Windows Server 2016 Nano Server wordt niet ondersteund.
>
> Op Linux-distributies, worden alleen de vooraf gedefinieerde kernels die deel van de secundaire versie release/bijwerken van de verdeling uitmaken ondersteund.
>
> Upgrades tussen primaire versies van een Linux-distributie op een Azure Site Recovery beveiligde virtuele VMware-machine of fysieke server wordt niet ondersteund. Tijdens het upgraden van het besturingssysteem op de primaire versies (bijvoorbeeld CentOS 6.* naar CentOS 7.*), schakelt u replicatie voor de machine uit, werk het besturingssysteem op de machine en vervolgens replicatie opnieuw inschakelen.
> 


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>Ondersteunde versies van Ubuntu kernel voor VMware/fysieke servers

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic naar 3.13.0-121-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic naar 3.13.0-128-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic naar 3.13.0-132-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic naar 3.13.0-137-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic naar 4.4.0-81-generic,<br/>4.8.0-34-Generic naar 4.8.0-56-generic,<br/>4.10.0-14-Generic naar 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic naar 4.4.0-91-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic naar 4.4.0-96-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic naar 4.4.0-104-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Ondersteunde bestandssystemen en Gast opslagconfiguraties op Linux (VMware of fysieke servers)

De volgende bestandssystemen en opslag configuratiesoftware worden op Linux-servers met VMware of fysieke servers ondersteund:
* -Bestandssystemen: ext3, ext4, ReiserFS (Suse Linux Enterprise Server alleen), XFS
* Volumebeheer: LVM2
* Multipath-software: apparaat toewijzen

Paravirtualized-opslagapparaten (apparaten die zijn geëxporteerd door paravirtualized stuurprogramma's) worden niet ondersteund.<br/>
Meerdere wachtrij blok i/o-apparaten worden niet ondersteund.<br/>
Fysieke servers met de opslagcontroller HP CCISS worden niet ondersteund.<br/>

>[!Note]
> Op Linux-servers de volgende mappen (indien ingesteld als afzonderlijke partities /-bestandssystemen) moet worden op dezelfde schijf (schijf met het besturingssysteem) op de bronserver: / (root), / Boot, /usr, /usr/local, /var, etc; en/Boot moet zich bevinden in de schijfpartitie van een en niet een volume LVM<br/><br/>
>


## <a name="support-for-network-configuration"></a>Ondersteuning voor netwerkconfiguratie
De volgende tabellen geven een overzicht van configuratie-netwerkondersteuning in verschillende scenario's die gebruikmaken van Azure Site Recovery worden gerepliceerd naar Azure.

### <a name="host-network-configuration"></a>Host-netwerkconfiguratie

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | ---
NIC-koppeling | Ja<br/><br/>Niet ondersteund wanneer fysieke machines worden gerepliceerd.| Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nee | Nee

### <a name="guest-vm-network-configuration"></a>Configuratie van Gast-VM-netwerken

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | ---
NIC-koppeling | Nee | Nee
IPv4 | Ja | Ja
IPv6 | Nee | Nee
Vaste IP-adres (Windows) | Ja | Ja
Vaste IP-adres (Linux) | Ja <br/><br/>Virtuele machines zijn geconfigureerd voor gebruik van DHCP op failback  | Nee
Multi-NIC | Ja | Ja

### <a name="failed-over-azure-vm-network-configuration"></a>Configuratie van failover Azure VM-netwerken

**Azure-netwerken** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | ---
ExpressRoute | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Multi-NIC | Ja | Ja
Reserved IP | Ja | Ja
IPv4 | Ja | Ja
Behouden van de bron-IP | Ja | Ja
Virtueel netwerk Service-eindpunten (Azure Storage firewalls en virtuele netwerken) | Nee | Nee


## <a name="support-for-storage"></a>Ondersteuning voor opslag
De volgende tabellen geven een overzicht van ondersteuning voor opslag in verschillende scenario's die gebruikmaken van Azure Site Recovery worden gerepliceerd naar Azure.

### <a name="host-storage-configuration"></a>Host-opslagconfiguratie

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | --- | ---
NFS | Ja voor VMware<br/><br/> Er is geen voor fysieke servers | N/A
SMB 3.0 | N/A | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO)<br></br>Getest met de: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM voor CLARiiON | Ja | Ja

### <a name="guest-or-physical-server-storage-configuration"></a>Gast of fysieke server opslagconfiguratie

**Configuratie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | ---
VMDK | Ja | N/A
VHD/VHDX | N/A | Ja
Generatie 2 VM | N/A | Ja
EFI/UEFI| Migratie naar Azure voor WindowsServer 2012 en nieuwere versies. </br></br> ** Opmerking aan het einde van de tabel zien.  | Ja
Clusterschijf gedeeld | Nee | Nee
Versleutelde schijf | Nee | Nee
NFS | Nee | N/A
SMB 3.0 | Nee | Nee
RDM | Ja<br/><br/> Niet van toepassing op fysieke servers | N/A
Schijf > 1 TB | Ja<br/><br/>Upto 4095 GB | Ja<br/><br/>Upto 4095 GB
Schijf met een fysieke sectorgrootte van 4K logische en 4 k | Ja | Niet ondersteund voor virtuele machines van generatie 1<br/><br/>Niet ondersteund voor virtuele machines van generatie 2.
Schijf met de 4K logische en fysieke sectorgrootte van 512 bytes | Ja |  Ja
Volume met striped schijf > 1 TB<br/><br/> LVM logische volumebeheer | Ja | Ja
Opslagruimten | Nee | Ja
Schijf hot toevoegen of verwijderen | Nee | Nee
Schijf uitsluiten | Ja | Ja
Multipath (MPIO) | N/A | Ja

> [!NOTE]
> ** UEFI, opstarten virtuele VMware-machines of fysieke servers met Windows Server 2012 of later kunnen worden gemigreerd naar Azure. Volgende beperkingen worden toegepast.
> - Alleen migratie naar Azure. Failback naar on-premises VMware-site niet ondersteund.
> - Niet meer dan 4 partities worden op schijf met het besturingssysteem van de server ondersteund.
> - Versie van Azure Site Recovery Mobility-service 9.13 of hoger vereist.

**Azure Storage** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
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
Virtueel netwerk Service-eindpunten (Azure Storage firewalls en virtuele netwerken) geconfigureerd op de doelopslag account of storage-account gebruikt voor het opslaan van gegevens van replicatie in de cache | Nee | Nee
V2 opslagaccounts voor algemeen gebruik (zowel Hot en Cool laag) | Nee | Nee


## <a name="support-for-azure-compute-configuration"></a>Ondersteuning voor Azure compute-configuratie

**COMPUTE-functie** | **VMware of fysieke server** | **Hyper-V (met/zonder Virtual Machine Manager)**
--- | --- | ---
Beschikbaarheidssets | Ja | Ja
HUB | Ja | Ja  
Managed Disks | Ja | Ja<br/><br/>Failback naar on-premises van de virtuele machine van Azure met beheerde schijven is momenteel niet ondersteund.

## <a name="failed-over-azure-vm-requirements"></a>Vereisten voor failover-virtuele machine in Azure

U kunt Site Recovery implementeren voor het repliceren van virtuele machines en fysieke servers met een besturingssysteem dat wordt ondersteund door Azure. Dit omvat de meeste versies van Windows en Linux. Een on-premises virtuele machines die u wilt repliceren, moeten voldoen aan de volgende vereisten voor Azure repliceren naar Azure.

**Entiteit** | **Vereisten** | **Details**
--- | --- | ---
**Gastbesturingssysteem** | Hyper-V naar Azure replicatie: Site Recovery ondersteunt alle besturingssystemen die zijn [ondersteund door Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Voor VMware en fysieke server-replicatie: Controleer de Windows- en Linux [vereisten](site-recovery-vmware-to-azure-classic.md) | Controle van vereisten mislukt als een niet-ondersteund.
**Architectuur van de Gast-besturingssysteem** | 64-bits | Controle van vereisten mislukt als een niet-ondersteund
**Grootte van de besturingssysteemschijf** | Maximaal 2048 GB als u repliceert **VMware-machines of fysieke servers naar Azure**.<br/><br/>Maximaal 2048 GB voor **Hyper-V Generation 1** virtuele machines.<br/><br/>Een Resourcegroepnaam 300 GB voor **Hyper-V Generation 2 virtuele machines**.  | Controle van vereisten mislukt als een niet-ondersteund
**Het aantal schijven voor besturingssysteem** | 1 | Controle van vereisten mislukt als een niet-ondersteund.
**Aantal gegevensschijven** | 64- of minder als u repliceert **virtuele VMware-machines naar Azure**; 16 of minder als u repliceert **Hyper-V-machines naar Azure** | Controle van vereisten mislukt als een niet-ondersteund
**De grootte van VHD gegevensschijf** | 4095 GB | Controle van vereisten mislukt als een niet-ondersteund
**Netwerkadapters** | Meerdere netwerkadapters worden ondersteund |
**Gedeelde VHD** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund
**FC-schijf** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund
**Harde schijf-indeling** | VHD <br/><br/> VHDX | Hoewel VHDX wordt momenteel niet ondersteund in Azure, Site Recovery automatisch geconverteerd VHDX naar VHD wanneer u een failover naar Azure. Wanneer u een failover naar on-premises blijven de virtuele machines gebruiken van de VHDX-indeling.
**Bitlocker** | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat u een virtuele machine beveiligt.
**VM-naam** | Tussen 1 en 63 tekens. Alleen letters, cijfers en afbreekstreepjes. De VM-naam moet beginnen en eindigen met een letter of cijfer. | Werk de waarde in de eigenschappen van de virtuele machine in Site Recovery.
**VM-type** | Generatie 1<br/><br/> Generatie 2--Windows | Generatie 2 virtuele machines met een type besturingssysteem schijf basic (waaronder een of twee gegevensvolumes die zijn opgemaakt als VHDX) en minder dan 300 GB aan schijfruimte worden ondersteund.<br></br>Virtuele machines Linux generatie 2 worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Ondersteuning voor Recovery Services-kluis acties

**Actie** | **VMware of fysieke server** | **Hyper-V (geen Virtual Machine Manager)** | **Hyper-V (met Virtual Machine Manager)**
--- | --- | --- | ---
Kluis over brongroepen verplaatsen<br/><br/> Binnen en tussen abonnementen | Nee | Nee | Nee
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee | Nee | Nee


## <a name="support-for-provider-and-agent"></a>Ondersteuning voor de Provider en Agent

**Naam** | **Beschrijving** | **Meest recente versie** | **Details**
--- | --- | --- | --- | ---
**Azure Site Recovery Provider** | Coördineert de communicatie tussen de on-premises servers en Azure <br/><br/> Op de on-premises Virtual Machine Manager-servers of op de Hyper-V-servers worden geïnstalleerd als er geen Virtual Machine Manager-server | 5.1.2700.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
**Azure Site Recovery Unified Setup (VMware naar Azure)** | Coördineert de communicatie tussen de on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
**Mobility-service** | Coördineert de replicatie tussen de on-premises VMware servers of fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VMware-VM of fysieke servers die u wilt repliceren  | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
**Microsoft Azure Recovery Services (MARS)-agent** | Coördineert de replicatie tussen Hyper-V-machines en Azure<br/><br/> Geïnstalleerd op de on-premises Hyper-V-servers (met of zonder een Virtual Machine Manager-beheerserver) | Nieuwste agent (beschikbaar via de portal) |






## <a name="next-steps"></a>Volgende stappen
[Vereisten controleren](site-recovery-prereq.md)
