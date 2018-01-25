---
title: Azure Site Recovery-ondersteuningsmatrix voor virtuele VMware-machines en fysieke servers repliceren naar Azure | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor het virtuele VMware-machines repliceren naar Azure met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: raynew
ms.openlocfilehash: ead133318d8660e8b8f4b3e9c5dddb6d75878b19
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matrix-ondersteuning voor VMware en fysieke server-replicatie naar Azure


In dit artikel bevat een overzicht van ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="supported-scenarios"></a>Ondersteunde scenario's

**Scenario** | **Details** 
--- | --- 
**Virtuele VMware-machines** | U kunt herstel na noodgevallen in Azure uitvoeren voor de lokale virtuele VMware-machines. U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren.
**Fysieke servers** | U kunt het herstel na noodgevallen in Azure uitvoeren voor fysieke on-premises Windows of Linux-servers. U kunt dit scenario in de Azure portal kunt implementeren.

## <a name="on-premises-virtualization-servers"></a>Lokale virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
**VMware** | vCenter Server 6.5, 6.0 of 5.5 of vSphere 6.5 6.0, 5.5 | We raden dat u een vCenter-server gebruiken
**Fysieke servers** | N.v.t.


## <a name="replicated-machines"></a>Gerepliceerde machines

De volgende tabel geeft een overzicht van de Replicatieondersteuning voor machines. Site Recovery biedt ondersteuning voor replicatie van elke werkbelasting die wordt uitgevoerd op een machine met een ondersteund besturingssysteem.

**Onderdeel** | **Details**
--- | ---
Machineconfiguratie | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#failed-over-azure-vm-requirements).
Machine besturingssysteem (Windows) | 64-bits Windows Server 2016 (Server Core, Server met Bureaubladbelevenis)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minste SP1
Besturingssysteem van de machine (Linux) | Red Hat Enterprise Linux: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4 <br/><br/>CentOS: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4 <br/><br/>Ubuntu 14.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Upgrade van de computers van SLES 11 SP3 repliceren naar SLES 11 SP4 wordt niet ondersteund. Als een gerepliceerde machine is bijgewerkt van SLES 11SP3 naar SLES 11 SP4 is geïnstalleerd, moet u replicatie uitschakelen en beveiligt de machine opnieuw na de upgrade.)

>[!NOTE]
>
> - Op Linux-distributies, worden alleen de vooraf gedefinieerde kernels die deel van de secundaire versie release/bijwerken van de verdeling uitmaken ondersteund.
>
> - Upgrades tussen primaire versies van een Linux-distributie op een Azure Site Recovery beveiligde virtuele VMware-machine of fysieke server wordt niet ondersteund. Tijdens het upgraden van het besturingssysteem op de primaire versies (bijvoorbeeld CentOS 6.* naar CentOS 7.*), schakelt u replicatie voor de machine uit, werk het besturingssysteem op de machine en vervolgens replicatie opnieuw inschakelen.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu kernel-versies


**Ondersteunde versie** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic naar 3.13.0-121-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic naar 3.13.0-128-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic naar 3.13.0-132-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic naar 3.13.0-137-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic naar 4.4.0-81-generic,<br/>4.8.0-34-Generic naar 4.8.0-56-generic,<br/>4.10.0-14-Generic naar 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic naar 4.4.0-91-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic naar 4.4.0-96-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic naar 4.4.0-104-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage-configurations"></a>Linux-bestand Gast-systemen opslagconfiguraties

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, ReiserFS (alleen voor Suse Linux Enterprise Server), XFS
Volumebeheer | LVM2
Multipath-software | Apparaat toewijzen
Opslagapparaten Paravirtualized | Apparaten die zijn geëxporteerd door paravirtualized stuurprogramma's worden niet ondersteund.
Meerdere wachtrij blok i/o-apparaten | Wordt niet ondersteund.
Fysieke servers met de opslagcontroller HP CCISS | Wordt niet ondersteund.
Mappen | Deze mappen (indien ingesteld als afzonderlijke partities /-bestandssystemen) moet worden op dezelfde schijf als besturingssysteem op de bronserver: / (root), / Boot, /usr, /usr/local, /var, etc </br></br>  Als / volume (root) is een volume LVM en vervolgens/Boot moet zich bevinden op een afzonderlijke partitie op dezelfde schijf en niet een LVM-volume.<br/><br/>
XFSv5 | XFSv5 functies op XFS bestandssystemen zoals metagegevens controlesom worden van versie 9.10 van de Mobility-service en hoger ondersteund. Gebruik het hulpprogramma xfs_info om te controleren van de superblock XFS voor de partitie. Als ftype is ingesteld op 1, klikt u vervolgens zijn XFSv5 functies in gebruik.



## <a name="network"></a>Netwerk

**Onderdeel** | **Ondersteund** 
--- | --- 
Host netwerk NIC-koppeling | Ondersteund voor VMware-machines <br/><br/>Niet ondersteund voor replicatie van fysieke machine
Netwerk van de host VLAN | Ja 
IPv4-netwerk van de host | Ja 
IPv6-netwerk van de host | Nee 
Gastbesturingssysteem of de server netwerk NIC-koppeling | Nee 
Gast/server-netwerk IPv4 | Ja 
Gast/server-netwerk IPv6 | Nee 
Gast/server-netwerk statische IP (Windows) | Ja 
Gast/server-netwerk statische IP (Linux) | Ja <br/><br/>Virtuele machines zijn geconfigureerd voor gebruik van DHCP op failback  
Gast/server-netwerk meerdere NIC 's | Ja 


## <a name="azure-vm-network-after-failover"></a>Azure VM-netwerk (na een failover)

**Onderdeel** | **Ondersteund** 
--- | --- 
ExpressRoute | Ja 
ILB | Ja 
ELB | Ja 
Traffic Manager | Ja 
Multi-NIC | Ja 
Gereserveerde IP-adressen | Ja 
IPv4 | Ja 
IP-bronadres behouden | Ja 
Virtual Network Service-eindpunten<br/><br/> (Azure-opslag firewalls en vnet's) | Nee 


## <a name="storage"></a>Storage


**Onderdeel** | **Ondersteund** 
--- | --- 
Host NFS | Ja voor VMware<br/><br/> Er is geen voor fysieke servers 
Host-SAN (ISCSI) | Ja
Host Multipath (MPIO) | Ja - getest met de: Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM voor CLARiiON
VMDK gast of de server | Ja 
Gastbesturingssysteem of de server EFI/UEFI| Gedeeltelijk (migratie naar Azure voor WindowsServer 2012 en nieuwere versies.) </br></br> ** Opmerking aan het einde van de tabel zien.
De gedeelde clusterschijf gast of de server | Nee 
Versleutelde schijf gast of de server | Nee 
NFS gast of de server | Nee 
SMB 3.0 Gast of de server | Nee
RDM gast of de server | Ja<br/><br/> Niet van toepassing op fysieke servers 
Gastbesturingssysteem of de server schijf > 1 TB | Ja<br/><br/>4095 GB 
Gastbesturingssysteem of de server met een fysieke sectorgrootte van 4K logische en 4 k-schijf | Ja
De schijf gast of de server met 4K logische en fysieke sectorgrootte van 512 bytes | Ja 
Volume van de gast of de server met striped schijf > 1 TB<br/><br/> LVM logische volumebeheer Gast/server - opslagruimten | Er is geen schijf van gastbesturingssysteem of de server hot toevoegen of verwijderen | Er is geen Gast/server - schijf uitsluiten | Ja gast of de server met meerdere paden (MPIO) | N.V.T.

> [!NOTE]
> ** UEFI, opstarten virtuele VMware-machines of fysieke servers met Windows Server 2012 of later kunnen worden gemigreerd naar Azure. Volgende beperkingen worden toegepast.
> - Alleen de migratie naar Azure wordt ondersteund. Failback naar on-premises VMware-site niet ondersteund.
> - De server moet niet meer dan 4 partities hebben op de schijf met het besturingssysteem.
> - Versie van Azure Site Recovery Mobility-service 9.13 of hoger vereist.


## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Ondersteund** 
--- | --- 
LRS | Ja 
GRS | Ja 
RA-GRS | Ja 
Cool storage | Nee 
Hot storage| Nee 
Blok-blobs | Nee 
Versleuteling op rest(SSE)| Ja 
Premium Storage | Ja 
Service voor importeren/exporteren | Nee 
Virtual Network Service-eindpunten<br/><br/> Azure-opslag firewalls en VNETs die zijn geconfigureerd op de doel-storage-account voor opslag/cache (gebruikt voor het opslaan van gegevens van replicatie) | Nee 
V2 opslagaccounts voor algemeen gebruik (zowel Hot en Cool laag) | Nee 


## <a name="azure-compute"></a>Azure compute

**Featuree** | **Ondersteund** 
--- | --- 
Beschikbaarheidssets | Ja 
HUB | Ja   
Managed Disks | Ja 

## <a name="azure-vm-requirements"></a>Azure VM-vereisten

Lokale virtuele machines die u naar Azure repliceert moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel samengevat.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
**Gastbesturingssysteem** | Controleer of [ondersteunde besturingssystemen](#replicated machines) | Controle van vereisten mislukt als een niet-ondersteund.
**Architectuur van de Gast-besturingssysteem** | 64-bits | Controle van vereisten mislukt als een niet-ondersteund
**Grootte van de besturingssysteemschijf** | Maximaal 2048 GB | Controle van vereisten mislukt als een niet-ondersteund
**Het aantal schijven voor besturingssysteem** | 1 | Controle van vereisten mislukt als een niet-ondersteund.
**Aantal gegevensschijven** | 64- of minder als u repliceert **virtuele VMware-machines naar Azure**; 16 of minder als u repliceert **Hyper-V-machines naar Azure** | Controle van vereisten mislukt als een niet-ondersteund
**De grootte van VHD gegevensschijf** | 4095 GB | Controle van vereisten mislukt als een niet-ondersteund
**Netwerkadapters** | Meerdere netwerkadapters worden ondersteund | 
**Gedeelde VHD** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund
**FC-schijf** | Niet ondersteund | Controle van vereisten mislukt als een niet-ondersteund
**Harde schijf-indeling** | VHD <br/><br/> VHDX | Hoewel VHDX wordt momenteel niet ondersteund in Azure, Site Recovery automatisch geconverteerd VHDX naar VHD wanneer u een failover naar Azure. Wanneer u een failover naar on-premises blijven de virtuele machines gebruiken van de VHDX-indeling.
**Bitlocker** | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt.
**VM-naam** | 1 - 63 tekens.<br/><br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. | Werk de waarde in de eigenschappen van de machine in Site Recovery.
**VM-type** | Generatie 1<br/><br/> Generatie 2--Windows | Generatie 2 virtuele machines met een type besturingssysteem schijf basic (waaronder een of twee gegevensvolumes die zijn opgemaakt als VHDX) en minder dan 300 GB aan schijfruimte worden ondersteund.<br></br>Virtuele machines Linux generatie 2 worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="vault-tasks"></a>Taken van de kluis

**Actie** | **Ondersteund** 
--- | --- 
Kluis over brongroepen verplaatsen<br/><br/> Binnen en tussen abonnementen | Nee 
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee 


## <a name="mobility-service"></a>Mobility-service

**Naam** | **Beschrijving** | **Meest recente versie** | **Details**
--- | --- | --- | --- | ---
** Azure Site Recovery Unified Setup ** | Coördineert de communicatie tussen de on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
**Mobility-service** | Coördineert de replicatie tussen de on-premises VMware servers of fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VMware-VM of fysieke servers die u wilt repliceren  | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over hoe](tutorial-prepare-azure.md) Azure voorbereiden op herstel na noodgevallen van virtuele VMware-machines.
