---
title: Ondersteuningsmatrix voor VMware-machines en fysieke servers repliceren naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en onderdelen voor het repliceren van VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: raynew
ms.openlocfilehash: 516cb69042e923a46168c7655dc3e3010d9557e6
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173789"
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Ondersteuningsmatrix voor VMware en fysieke servers repliceren naar Azure

In dit artikel bevat een overzicht van ondersteunde onderdelen en -instellingen voor herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="replication-scenario"></a>Replicatiescenario

**Scenario** | **Details**
--- | ---
Virtuele VMware-machines | Replicatie van on-premises VMware-machines naar Azure. U kunt dit scenario in Azure portal of met behulp van implementeren [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Fysieke servers | Replicatie van on-premises Windows/Linux-fysieke servers naar Azure. U kunt dit scenario in Azure portal implementeren.

## <a name="on-premises-virtualization-servers"></a>On-premises virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
VMware | vCenter Server 6.7, 6.5, 6.0 of 5.5 of vSphere 6.7, 6.5, 6.0 of 5.5 | U wordt aangeraden dat u een vCenter-server.<br/><br/> Het is raadzaam dat vSphere-hosts en vCenter-servers bevinden zich in hetzelfde netwerk bevinden als de processerver. Wordt standaard de serveronderdelen proces uitgevoerd op de configuratieserver, dus dit is het netwerk waarin u de configuratieserver hebt ingesteld, tenzij u een speciaal toegewezen proces-server instellen.
Fysiek | N/A

## <a name="site-recovery-configuration-server"></a>Site Recovery-configuratieserver

De configuratieserver is een on-premises computer met Site Recovery-onderdelen, met inbegrip van de configuratieserver, processerver en hoofddoelserver. Voor VMware-replicatie instellen u de configuratieserver met alle vereisten, met behulp van een OVF-sjabloon om een VMware-VM te maken. Voor replicatie van fysieke server instellen u de configuratie van server-machine handmatig.

**Onderdeel** | **Vereisten**
--- |---
CPU-kernen | 8
RAM | 16 GB
Aantal schijven | 3-schijven<br/><br/> Schijven bevatten de OS-schijf, cacheschijf proces en bewaarstation voor failback.
Vrije schijfruimte | 600 GB aan ruimte vereist voor de cache van de processerver.
Vrije schijfruimte | 600 GB aan ruimte vereist voor het bewaarstation.
Besturingssysteem  | Windows Server 2012 R2 of WindowsServer 2016 |
Landinstelling van het besturingssysteem | Engels (en-us)
PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") moet worden geïnstalleerd.
Windows Server-functies | Niet inschakelen: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
Groepsbeleid| Niet inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen. <br> -Toegang tot registerbewerkingsprogramma's voorkomen. <br> -Logica vertrouwen voor bestandsbijlagen. <br> -Uitvoering van Script inschakelen. <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Zorg ervoor dat u:<br/><br/> -Geen een bestaande standaardwebsite <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br> -Bestaande website /-app niet hebt luisteren op poort 443<br>
Type NIC | VMXNET3 (indien geïmplementeerd als een VMware-VM)
Type IP-adres | Statisch
Poorten | 443 gebruikt voor de orchestration-besturingselement)<br>9443 gebruikt voor het gegevenstransport

## <a name="replicated-machines"></a>Gerepliceerde machines

Site Recovery biedt ondersteuning voor replicatie van alle werkbelasting die wordt uitgevoerd op een ondersteunde machine.

**Onderdeel** | **Details**
--- | ---
Instellingen van de computer | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#azure-vm-requirements).
Windows-besturingssysteem | 64-bits Windows Server 2016 (Server Core, Server met Bureaubladervaring), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 met op minimaal SP1. </br></br>  [Windows Server 2008 met op minste SP2 - 32-bits en 64-bits](migrate-tutorial-windows-server-2008.md) (alleen voor de migratie). </br></br> Windows 2016 Nano Server wordt niet ondersteund.
Linux-besturingssysteem | Red Hat Enterprise Linux: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 naar 7.5 <br/><br/>CentOS: 5.2 naar 5,11, 6.1 naar 6,9, 7.0 naar 7.5 <br/><br/>Ubuntu 14.04 LTS server[ (kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server[ (kernel-versies ondersteund)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (kernel-versies ondersteund)](#debian-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 12 SP1, SP2, SP3 [ (kernel-versies ondersteund)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 * </br></br>Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibele kernel of Unbreakable Enterprise Kernel versie 3 (UEK3) <br/><br/></br>* *Gerepliceerde machines upgraden van SUSE Linux Enterprise Server 11 SP3 naar SP4 wordt niet ondersteund. Als u wilt bijwerken, replicatie uitschakelen en inschakelen na de upgrade opnieuw.*


>[!NOTE]
>
> - Op Linux-distributies, worden alleen de voorraad kernels die deel van de release-distributiepunt secundaire versie/update uitmaken ondersteund.
>
> - Beveiligde machines upgraden in belangrijke Linux distributie versies wordt niet ondersteund. Als u wilt bijwerken, schakelt u replicatie uit, werk het besturingssysteem en schakelt u de replicatie opnieuw.
>
> - Servers met Red Hat Enterprise Linux 5.2 5,11 of CentOS 5.2 naar 5,11 moeten de Linux Integration Services(LIS) componenten zijn geïnstalleerd in de volgorde voor de machines om op te starten in Azure.

### <a name="ubuntu-kernel-versions"></a>Ubuntu-kernel-versies


**Ondersteunde versie** | **Azure Site Recovery Mobility Service versie** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | 9.18 | 3.13.0-24-Generic naar 3.13.0-153-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-130-generic |
14.04 TNS | 9.17 | 3.13.0-24-Generic naar 3.13.0-149-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-127-generic |
14.04 TNS | 9.16 | 3.13.0-24-Generic naar 3.13.0-144-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-119-generic |
14.04 TNS | 9.15 | 3.13.0-24-Generic naar 3.13.0-144-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-119-generic |
|||
16.04 LTS | 9.18 | 4.4.0-21-Generic naar 4.4.0-130-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic |
16.04 LTS | 9.17 | 4.4.0-21-Generic naar 4.4.0-127-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-43-generic |
16.04 LTS | 9.16 | 4.4.0-21-Generic naar 4.4.0-119-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-38-generic |
16.04 LTS | 9.15 | 4.4.0-21-Generic naar 4.4.0-119-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-38-generic |


### <a name="debian-kernel-versions"></a>Debian kernelversies


**Ondersteunde versie** | **Azure Site Recovery Mobility Service versie** | **Kernelversie** |
--- | --- | --- |
Debian 7 | 9.17, 9.18 | 3.2.0-4-AMD64 naar 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.15, 9.16 | 3.2.0-4-AMD64 naar 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.17, 9.18 | 3.16.0-4-AMD64 naar 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-AMD64 naar 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.15 | 3.16.0-4-AMD64 naar 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.5-amd64 |

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 ondersteunde versies van de kernel

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.18 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.96-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.85-default</br></br>SP3 4.4.73-5-default naar 4.4.138-94.39-default |

## <a name="linux-file-systemsguest-storage"></a>Linux-systemen/Gast bestandsopslag

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, XFS.
Volume manager | LVM2.
Geparavirtualiseerde opslagapparaten | Apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.
Meerdere wachtrij blok i/o-apparaten | Wordt niet ondersteund.
Fysieke servers met de opslagcontroller HP CCISS | Wordt niet ondersteund.
Mappen | Deze mappen (indien ingesteld als afzonderlijke partities /-bestandssystemen) alle moeten op dezelfde schijf als besturingssysteem op de bronserver: / (hoofd), bevinden, / usr, / usr/local, /var, / etc.</br></br> bevinden moet zich op een partitie op schijf en niet een LVM-volume.<br/><br/>
Vrije schijfruimte| 2 GB op de/root-partitie <br/><br/> 250 MB op de map voor installatie
XFSv5 | XFSv5 functies op XFS bestandssystemen, zoals metagegevens controlesom worden van Mobility Service versie 9.10 stationstoewijzingen ondersteund. Gebruik het hulpprogramma xfs_info om te controleren of de superblock XFS voor de partitie. Als ftype is ingesteld op 1, klikt u vervolgens zijn XFSv5 functies in gebruik.

## <a name="vmdisk-management"></a>Beheer van de virtuele machine/schijf

**Actie** | **Details**
--- | ---
Grootte van de schijf op gerepliceerde virtuele machine wijzigen | Ondersteund.
Schijf toevoegen op de gerepliceerde virtuele machine | Schakel de replicatie voor de virtuele machine, de schijf toevoegen en vervolgens weer inschakelen replicatie. Toevoegen van een schijf op een replicerende virtuele machine wordt momenteel niet ondersteund.

## <a name="network"></a>Netwerk

**Onderdeel** | **Ondersteund**
--- | ---
Hostnetwerk NIC-koppeling | Ondersteund voor VMware-VM's. <br/><br/>Niet ondersteund voor replicatie van fysieke machine.
Hostnetwerk VLAN | Ja.
Hostnetwerk IPv4 | Ja.
Hostnetwerk IPv6 | Nee.
Gast/server-netwerk NIC-koppeling | Nee.
Gast/server-netwerk IPv4 | Ja.
Gast/server-netwerk IPv6 | Nee.
Gast/netwerk statische IP-adres van (Windows) | Ja.
Gast/netwerk statische IP-adres van (Linux) | Ja. <br/><br/>Virtuele machines zijn geconfigureerd voor het gebruik van DHCP voor failback.
Meerdere NIC's van het netwerk Gast/host-server | Ja.


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
Bron-IP-adres behouden | Ja
Azure Virtual Network-service-eindpunten<br/> (zonder Azure Storage-firewalls) | Ja
Versneld netwerken | Nee

## <a name="storage"></a>Storage
**Onderdeel** | **Ondersteund**
--- | ---
Host NFS | Ja voor VMware<br/><br/> Nee voor fysieke servers
Host (iSCSI/FC) SAN | Ja
Host virtueel SAN | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
Host-MPIO (Multipath I/O) | Ja, getest met Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM voor CLARiiON
Virtuele Hostvolumes (VVols) | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
VMDK-Gast/server | Ja
Gast/server EFI/UEFI| Gedeeltelijk (migratie naar Azure voor Windows Server 2012 en latere VMware virtuele machines alleen) </br></br> Zie de opmerking aan het einde van de tabel
De gedeelde clusterschijf Gast/host-server | Nee
Versleutelde schijf Gast/host-server | Nee
Gast/NFS-server | Nee
Gast/SMB 3.0-server | Nee
Gast/server RDM | Ja<br/><br/> N.V.T. voor fysieke servers
Gast/server schijf > 1 TB | Ja<br/><br/>Maximaal 4095 GB
Gast/server-schijf met de fysieke sectorgrootte van 4K logische en 4 k | Ja
Gast/server-schijf met de 4K logische en fysieke sectorgrootte van 512 bytes | Ja
Volume van de Gast/host-server met striped schijf > 4 TB <br><br/>Logische volumebeheer (LVM)| Ja
Gast/server - opslagruimten | Nee
Schijf Gast/server-hot toevoegen of verwijderen | Nee
Gast/server - schijf uitsluiten | Ja
Gast/server MPIO (Multipath I/O) | Nee

> [!NOTE]
> UEFI, opstarten van virtuele VMware-machines met Windows Server 2012 of later kunnen worden gemigreerd naar Azure. Er gelden de volgende beperkingen:

> - Alleen de migratie naar Azure wordt ondersteund. Failback naar on-premises VMware-site wordt niet ondersteund.
> - De server mag niet meer dan vier partities hebben op de besturingssysteemschijf.
> - Mobility Service versie 9.13 of hoger vereist.
> - Niet ondersteund voor fysieke servers.

## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Ondersteund**
--- | ---
Lokaal redundante opslag | Ja
Geografisch redundante opslag | Ja
Geografisch redundante opslag met leestoegang | Ja
Koude opslag | Nee
Hot storage| Nee
Blok-blobs | Nee
Versleuteling-at-rest (Storage Service Encryption)| Ja
Premium Storage | Ja
Import/export-service | Nee
Azure Storage-firewalls voor virtuele netwerken die zijn geconfigureerd voor opslag/cache-opslagaccount voor het doel (gebruikt voor het opslaan van replicatiegegevens) | Nee
Opslagaccounts voor algemeen gebruik v2 (zowel warme als koude lagen) | Nee

## <a name="azure-compute"></a>Azure-rekenen

**Functie** | **Ondersteund**
--- | ---
Beschikbaarheidssets | Ja
HUB | Ja
Managed Disks | Ja

## <a name="azure-vm-requirements"></a>Vereisten voor Azure VM 's

On-premises machines die u naar Azure repliceren, moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel wordt samengevat. Wanneer Site Recovery wordt uitgevoerd een controle van vereisten, mislukt dit als aan bepaalde vereisten zijn niet voldaan.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gast-besturingssysteem | Controleer of [ondersteunde besturingssystemen](#replicated-machines) voor gerepliceerde machines. | Controle mislukt als niet-ondersteund.
Architectuur van de Gast-besturingssysteem | 64-bits. | Controle mislukt als niet-ondersteund.
Grootte van de besturingssysteemschijf | Maximaal 2048 GB. | Controle mislukt als niet-ondersteund.
Aantal besturingssysteemschijven | 1 | Controle mislukt als niet-ondersteund.  
Aantal gegevensschijven | 64 of minder. | Controle mislukt als niet-ondersteund.  
Grootte van de gegevensschijf | Maximaal 4095 GB | Controle mislukt als niet-ondersteund.
Netwerkadapters | Meerdere netwerkadapters worden ondersteund. |
Gedeelde VHD | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
FC-schijf | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
BitLocker | Wordt niet ondersteund. | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt. |
VM-naam | Van 1 tot 63 tekens bevatten.<br/><br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. |  Werk de waarde in de eigenschappen van de machine in Site Recovery.


## <a name="vault-tasks"></a>Kluis-taken

**Actie** | **Ondersteund**
--- | ---
Kluis verplaatsen tussen resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee
Verplaatsen van opslag, netwerk, Azure-VM's op resourcegroepen<br/><br/> Binnen en tussen abonnementen | Nee


## <a name="mobility-service"></a>Mobility-Service

**Naam** | **Beschrijving** | **Meest recente versie** | **Details**
--- | --- | --- | --- | ---
Azure Site Recovery van geïntegreerde Setup | Coördineert de communicatie tussen on-premises VMware-servers en Azure <br/><br/> Geïnstalleerd op de on-premises VMware-servers | 9.12.4653.1 (beschikbaar via de portal) | [Nieuwste functies en correcties](https://aka.ms/latest_asr_updates)
Mobility-Service | Coördineert de replicatie tussen on-premises VMware-servers/fysieke servers en Azure/secundaire site<br/><br/> Geïnstalleerd op de VM met VMware of fysieke servers die u wilt repliceren | 9.12.4653.1 (beschikbaar via de portal) | [Nieuwste functies en correcties](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Volgende stappen
[Informatie over hoe](tutorial-prepare-azure.md) het voorbereiden van Azure voor herstel na noodgevallen van virtuele VMware-machines.
