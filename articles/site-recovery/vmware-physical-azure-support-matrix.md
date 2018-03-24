---
title: De ondersteuningsmatrix voor virtuele VMware-machines en fysieke servers repliceren naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor het repliceren van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/20/2018
ms.author: raynew
ms.openlocfilehash: 6f2f28b36fdb3751a469d66f242f9fa2119f9ae8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matrix-ondersteuning voor VMware en fysieke server-replicatie naar Azure

In dit artikel bevat een overzicht van ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

**Scenario** | **Details**
--- | ---
Virtuele VMware-machines | U kunt herstel na noodgevallen in Azure uitvoeren voor de lokale virtuele VMware-machines. U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren.
Fysieke servers | U kunt het herstel na noodgevallen in Azure uitvoeren voor fysieke on-premises Windows of Linux-servers. U kunt dit scenario in de Azure portal kunt implementeren.

## <a name="on-premises-virtualization-servers"></a>Lokale virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
VMware | vCenter Server 6.5 6.0, of 5.5 of vSphere 6.5, 6.0 of 5.5 | Het is raadzaam dat u een vCenter-server.
Fysiek | N/A


## <a name="replicated-machines"></a>Gerepliceerde machines

De volgende tabel geeft een overzicht van de replicatie-ondersteuning voor virtuele VMware-machines en fysieke servers. Site Recovery biedt ondersteuning voor replicatie van elke werkbelasting die wordt uitgevoerd op een machine met een ondersteund besturingssysteem.

**Onderdeel** | **Details**
--- | ---
Instellingen van de computer | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#azure-vm-requirements).
Windows-besturingssysteem | 64-bits Windows Server 2016 (Server Core, Server met Bureaubladbelevenis), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1. Windows 2016 Nano Server wordt niet ondersteund.
Linux-besturingssysteem | Red Hat Enterprise Linux: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4 <br/><br/>CentOS: 5.2 to 5.11, 6.1 to 6.9, 7.0 to 7.4 <br/><br/>Ubuntu 14.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 TNS server[ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7/Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 <br/><br/>SP3 gerepliceerde machines bijwerken naar SP4 wordt niet ondersteund. Als u wilt upgraden, Schakel replicatie uit en opnieuw na de upgrade.

>[!NOTE]
>
> - Op Linux-distributies, worden alleen de vooraf gedefinieerde kernels die deel van de distributie secundaire versie release/update uitmaken ondersteund.
>
> - Beveiligde machines worden bijgewerkt in primaire Linux distributie versies wordt niet ondersteund. Als u wilt upgraden, schakelt u replicatie uit, het besturingssysteem upgraden en vervolgens replicatie opnieuw inschakelen.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu kernel-versies


**Ondersteunde versie** | **Azure Site Recovery Mobility-Service-versie** | **Kernelversie** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic naar 3.13.0-121-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic naar 3.13.0-128-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic naar 3.13.0-132-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic naar 3.13.0-137-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic naar 4.4.0-81-generic,<br/>4.8.0-34-Generic naar 4.8.0-56-generic,<br/>4.10.0-14-Generic naar 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic naar 4.4.0-91-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic naar 4.4.0-96-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic naar 4.4.0-104-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage"></a>Linux-bestandsopslag voor gast-systemen

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, ReiserFS (alleen voor Suse Linux Enterprise Server), XFS.
Volumebeheer | LVM2.
Multipath-software | Apparaat toewijzen.
Opslagapparaten Paravirtualized | Apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.
Meerdere wachtrij blok i/o-apparaten | Wordt niet ondersteund.
Fysieke servers met de opslagcontroller HP CCISS | Wordt niet ondersteund.
Mappen | Deze mappen (indien ingesteld als afzonderlijke partities /-bestandssystemen) alle moeten op dezelfde schijf als besturingssysteem op de bronserver: / (root), / Boot, /usr, /usr/local, /var, etc.</br></br> / Boot moet zich bevinden in de schijfpartitie van een en niet een LVM-volume.<br/><br/>
Vrije schijfruimte| 2 GB op de partitie/Root <br/><br/> 250 MB op de map voor installatie
XFSv5 | XFSv5 functies op XFS bestandssystemen, zoals metagegevens controlesom, worden ondersteund door de Mobility-Service versie 9.10 bewerken. Gebruik het hulpprogramma xfs_info om te controleren van de superblock XFS voor de partitie. Als ftype is ingesteld op 1, klikt u vervolgens zijn XFSv5 functies in gebruik.



## <a name="network"></a>Netwerk

**Onderdeel** | **Ondersteund**
--- | ---
Host-netwerk NIC-koppeling | Ondersteund voor VMware-machines. <br/><br/>Niet ondersteund voor replicatie van fysieke machine.
Netwerk van de host VLAN | Ja.
IPv4-netwerk van de host | Ja.
IPv6-netwerk van de host | Nee.
Gast/server-netwerk NIC-koppeling | Nee.
Gast/server-netwerk IPv4 | Ja.
Gast/server-netwerk IPv6 | Nee.
Gast/server-netwerk statische IP (Windows) | Ja.
Gast/server-netwerk statische IP (Linux) | Ja. <br/><br/>Virtuele machines zijn geconfigureerd voor het gebruik van DHCP voor failback.
Gast/server-netwerk meerdere NIC 's | Ja.


## <a name="azure-vm-network-after-failover"></a>Azure VM-netwerk (na een failover)

**Onderdeel** | **Ondersteund**
--- | ---
ExpressRoute van Azure | Ja
ILB | Ja
ELB | Ja
Azure Traffic Manager | Ja
Multi-NIC | Ja
Gereserveerde IP-adressen | Ja
IPv4 | Ja
IP-bronadres behouden | Ja
Azure Virtual Network service-eindpunten<br/><br/> (Azure Storage firewalls en virtuele netwerken) | Nee

## <a name="storage"></a>Storage
**Onderdeel** | **Ondersteund**
--- | ---
Host NFS | Ja voor VMware<br/><br/> Er is geen voor fysieke servers
Host-SAN (ISCSI) | Ja
Host MPIO (Multipath I/O) | Ja, getest met Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM voor CLARiiON
VMDK gast of de server | Ja
Gastbesturingssysteem of de server EFI/UEFI| Gedeeltelijk (migratie naar Azure voor Windows Server 2012 en hoger virtuele VMware-machines alleen) </br></br> Zie de opmerking aan het einde van de tabel
De gedeelde clusterschijf gast of de server | Nee
Versleutelde schijf gast of de server | Nee
NFS gast of de server | Nee
SMB 3.0 Gast of de server | Nee
RDM gast of de server | Ja<br/><br/> Niet van toepassing op fysieke servers
Gastbesturingssysteem of de server schijf > 1 TB | Ja<br/><br/>4095 GB
Gastbesturingssysteem of de server met een fysieke sectorgrootte van 4K logische en 4 k-schijf | Ja
De schijf gast of de server met 4K logische en fysieke sectorgrootte van 512 bytes | Ja
Volume van de gast of de server met striped schijf > 4 TB <br><br/>Logische volumebeheer (LVM)| Ja
Gast/server - opslagruimten | Nee
Gast/serverschijf hot toevoegen of verwijderen | Nee
Gast/server - schijf uitsluiten | Ja
Gastbesturingssysteem of de server MPIO (Multipath I/O) | N/A

> [!NOTE]
> UEFI opstart VMware virtuele machines met Windows Server 2012 of later kunnen worden gemigreerd naar Azure. Er gelden de volgende beperkingen:

> - Alleen de migratie naar Azure wordt ondersteund. Failback naar on-premises VMware-site wordt niet ondersteund.
> - De server mag niet meer dan vier partities hebben op de schijf met het besturingssysteem.
> - Versie van de Mobility-Service 9.13 of hoger vereist.
> - Niet ondersteund voor fysieke servers.

## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Ondersteund**
--- | ---
Lokaal redundante opslag | Ja
Geografisch redundante opslag | Ja
Geografisch redundante opslag met leestoegang | Ja
Cool storage | Nee
Hot storage| Nee
Blok-blobs | Nee
Codering in rust (Service-versleuteling van opslag)| Ja
Premium Storage | Ja
Service voor importeren/exporteren | Nee
Virtueel netwerk service-eindpunten<br/><br/> Firewalls voor opslag en virtuele netwerken die zijn geconfigureerd op de doel-storage-account voor opslag/cache (gebruikt voor het opslaan van gegevens van replicatie) | Nee
V2 opslagaccounts voor algemeen gebruik (hot en cool lagen) | Nee

## <a name="azure-compute"></a>Azure compute

**Functie** | **Ondersteund**
--- | ---
Beschikbaarheidssets | Ja
HUB | Ja
Managed Disks | Ja

## <a name="azure-vm-requirements"></a>Azure VM-vereisten

Lokale virtuele machines die u naar Azure repliceert moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel samengevat. Wanneer u Site Recovery wordt uitgevoerd een controle van vereisten, mislukt het als aan de vereisten zijn niet voldaan.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gastbesturingssysteem | Controleer of [ondersteunde besturingssystemen](#replicated machines). | Controle mislukt als een niet-ondersteund. 
Architectuur van de Gast-besturingssysteem | 64-bits. | Controle mislukt als een niet-ondersteund. 
Grootte van de besturingssysteemschijf | Maximaal 2048 GB. | Controle mislukt als een niet-ondersteund. 
Het aantal schijven voor besturingssysteem | 1 | Controle mislukt als een niet-ondersteund.  
Aantal gegevensschijven | 64 of minder. | Controle mislukt als een niet-ondersteund.  
De grootte van gegevensschijf | 4095 GB | Controle mislukt als een niet-ondersteund. 
Netwerkadapters | Meerdere netwerkadapters worden ondersteund. | 
Gedeelde VHD | Wordt niet ondersteund. | Controle mislukt als een niet-ondersteund. 
FC-schijf | Wordt niet ondersteund. | Controle mislukt als een niet-ondersteund. 
BitLocker | Wordt niet ondersteund. | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt. | 
VM-naam | Van 1 tot 63 tekens.<br/><br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. |  Werk de waarde in de eigenschappen van de machine in Site Recovery.


## <a name="vault-tasks"></a>Taken van de kluis

**Actie** | **Ondersteund**
--- | ---
Kluis over brongroepen verplaatsen<br/><br/> Binnen en tussen abonnementen | Nee
Verplaats de opslag, netwerk, Azure VM's via resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee


## <a name="mobility-service"></a>Mobility Service

**Naam** | **Beschrijving** | **meest recente versie** | **Details**
--- | --- | --- | --- | ---
Azure Site Recovery Unified Setup | Coördineert de communicatie tussen de on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
Mobility Service | Coördineert de replicatie tussen de on-premises VMware servers of fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VMware-VM of fysieke servers die u wilt repliceren | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over hoe](tutorial-prepare-azure.md) Azure voorbereiden op herstel na noodgevallen van virtuele VMware-machines.
