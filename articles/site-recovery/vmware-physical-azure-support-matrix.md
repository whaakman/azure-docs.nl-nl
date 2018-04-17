---
title: De ondersteuningsmatrix voor virtuele VMware-machines en fysieke servers repliceren naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor het repliceren van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2018
ms.author: raynew
ms.openlocfilehash: b2a6e3052c64ab6a2865a0c24a4876cb2b98d1a8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matrix-ondersteuning voor VMware en fysieke server-replicatie naar Azure

In dit artikel bevat een overzicht van ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="replication-scenario"></a>Replicatiescenario

**Scenario** | **Details**
--- | ---
Virtuele VMware-machines | Replicatie van on-premises virtuele VMware-machines naar Azure. U kunt dit scenario in de Azure portal of met behulp van PowerShell implementeren.
Fysieke servers | Replicatie van de lokale Windows-/ Linux fysieke serversto Azure. U kunt dit scenario in de Azure portal kunt implementeren.

## <a name="on-premises-virtualization-servers"></a>Lokale virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
VMware | vCenter Server 6.5 6.0, of 5.5 of vSphere 6.5, 6.0 of 5.5 | Het is raadzaam dat u een vCenter-server.<br/><br/> We raden aan dat vSphere-hosts en vCenter-servers bevinden zich in hetzelfde netwerk bevindt als de processerver. De server-onderdelen van het proces wordt standaard op de configuratieserver zodat dit is het netwerk waarin u van de configuratieserver instellen, tenzij u een speciale processerver ingesteld. 
Fysiek | N/A

## <a name="site-recovery-configuration-server"></a>Configuratie van de siteserver-herstel

De configuratieserver is een lokale machine met Site Recovery-onderdelen, met inbegrip van de configuratieserver, de processerver en de hoofddoelserver. Voor VMware-replicatie instellen u de configuratieserver aan alle vereisten met een OVF-sjabloon maken van een VMware VM. Voor replicatie van de fysieke server moet instellen u de configuratie van server-machine handmatig.

**Onderdeel** | **Vereisten**
--- |---
CPU-kernen | 8 
RAM | 12 GB
Aantal schijven | 3-schijven<br/><br/> Schijven bevatten de OS-schijf, proces server cache schijf en bewaarstation voor failback.
Vrije schijfruimte | 600 GB ruimte nodig voor servercache proces.
Vrije schijfruimte | Vereiste ruimte voor bewaarstation 600 GB.
Besturingssysteem  | Windows Server 2012 R2 of WindowsServer 2016 | 
Landinstelling van het besturingssysteem | Engels (en-us) 
PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") moet worden geïnstalleerd.
Windows Server-functies | Niet inschakelen: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
Groepsbeleid| Niet inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot het register bewerkingsopties voorkomen. <br> -Logica voor bestandsbijlagen vertrouwen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Zorg ervoor dat u:<br/><br/> -Een bestaande standaardwebsite geen hebt <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br> -Bestaande website /-app niet hebt geluisterd op poort 443<br>
NIC-type | VMXNET3 (indien geïmplementeerd als een VMware-VM) 
Type IP-adres | Statisch 
Poorten | 443 gebruikt voor het besturingselement kanaal orchestration)<br>9443 gebruikt voor het vervoer van gegevens

## <a name="replicated-machines"></a>Gerepliceerde machines

Site Recovery biedt ondersteuning voor replicatie van elke werkbelasting die wordt uitgevoerd op een ondersteunde machine.

**Onderdeel** | **Details**
--- | ---
Instellingen van de computer | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#azure-vm-requirements).
Windows-besturingssysteem | 64-bits Windows Server 2016 (Server Core, Server met Bureaubladbelevenis), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1. Windows 2016 Nano Server wordt niet ondersteund.
Linux-besturingssysteem | Red Hat Enterprise Linux: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4 <br/><br/>CentOS: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 tot 7,4 <br/><br/>Ubuntu 14.04 TNS server[ (kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 TNS server[ (kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (kernel-versies ondersteund)](#debian-kernel-versions)<br/><br/>Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 <br/><br/>SP3 gerepliceerde machines bijwerken naar SP4 wordt niet ondersteund. Als u wilt upgraden, Schakel replicatie uit en opnieuw na de upgrade.

>[!NOTE]
>
> - Op Linux-distributies, worden alleen de vooraf gedefinieerde kernels die deel van de distributie secundaire versie release/update uitmaken ondersteund.
>
> - Beveiligde machines worden bijgewerkt in primaire Linux distributie versies wordt niet ondersteund. Als u wilt upgraden, schakelt u replicatie uit, het besturingssysteem upgraden en vervolgens replicatie opnieuw inschakelen.
>

### <a name="ubuntu-kernel-versions"></a>Ubuntu kernel-versies


**Ondersteunde versie** | **Azure Site Recovery Mobility-Service-versie** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | 9.11 | 3.13.0-24-Generic naar 3.13.0-128-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-91-generic |
14.04 TNS | 9.12 | 3.13.0-24-Generic naar 3.13.0-132-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-96-generic |
14.04 TNS | 9.13 | 3.13.0-24-Generic naar 3.13.0-137-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-104-generic |
14.04 TNS | 9.14 | 3.13.0-24-Generic naar 3.13.0-142-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-116-generic |
16.04 TNS | 9.11 | 4.4.0-21-Generic naar 4.4.0-91-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-32-generic |
16.04 TNS | 9.12 | 4.4.0-21-Generic naar 4.4.0-96-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-35-generic |
16.04 TNS | 9.13 | 4.4.0-21-Generic naar 4.4.0-104-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic |
16.04 TNS | 9.14 | 4.4.0-21-Generic naar 4.4.0-116-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-36-generic,<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1011-azure |


### <a name="debian-kernel-versions"></a>Debian kernel-versies


**Ondersteunde versie** | **Azure Site Recovery Mobility-Service-versie** | **Kernelversie** |
--- | --- | --- |
Debian 7 | 9.14 | 3.2.0-4-AMD64 naar 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 8 | 9.14 | 3.16.0-4-AMD64 naar 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.5-amd64 |


## <a name="linux-file-systemsguest-storage"></a>Linux-bestandsopslag voor gast-systemen

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, XFS.
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


## <a name="mobility-service"></a>Mobility-Service

**Naam** | **Beschrijving** | **meest recente versie** | **Details**
--- | --- | --- | --- | ---
Azure Site Recovery Unified Setup | Coördineert de communicatie tussen de on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)
Mobility-Service | Coördineert de replicatie tussen de on-premises VMware servers of fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VMware-VM of fysieke servers die u wilt repliceren | 9.12.4653.1 (beschikbaar via de portal) | [Meest recente functies en oplossingen](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over hoe](tutorial-prepare-azure.md) Azure voorbereiden op herstel na noodgevallen van virtuele VMware-machines.
