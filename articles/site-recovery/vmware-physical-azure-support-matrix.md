---
title: Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van ondersteuning voor herstel na noodgevallen van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: raynew
ms.openlocfilehash: 5dc98048099264942552862498b5137b4954c200
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491650"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Ondersteuningsmatrix voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure

In dit artikel bevat een overzicht van ondersteunde onderdelen en de instellingen voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met [Azure Site Recovery](site-recovery-overview.md).

- [Meer informatie](vmware-azure-architecture.md) over virtuele VMware-machine/fysieke server disaster recovery-architectuur.
- Volg onze [zelfstudies](tutorial-prepare-azure.md) voor het uitproberen van herstel na noodgevallen.

## <a name="deployment-scenarios"></a>Implementatiescenario's

**Scenario** | **Details**
--- | ---
Herstel na noodgevallen van virtuele VMware-machines | Replicatie van on-premises VMware-machines naar Azure. U kunt dit scenario in Azure portal of met behulp van implementeren [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Herstel na noodgeval voor fysieke servers | Replicatie van on-premises Windows/Linux-fysieke servers naar Azure. U kunt dit scenario in Azure portal implementeren.

## <a name="on-premises-virtualization-servers"></a>On-premises virtualisatieservers

**Server** | **Vereisten** | **Details**
--- | --- | ---
vCenter Server | Versie 6.7, 6.5, 6.0 of 5.5 | U wordt aangeraden dat u een vCenter-server in uw implementatie na een noodgeval gebruiken.
vSphere-hosts | Versie 6.7, 6.5, 6.0 of 5.5 | Het is raadzaam dat vSphere-hosts en vCenter-servers bevinden zich in hetzelfde netwerk bevinden als de processerver. De processerver wordt standaard uitgevoerd op de configuratieserver. [Meer informatie](vmware-physical-azure-config-process-server-overview.md).


## <a name="site-recovery-configuration-server"></a>Site Recovery-configuratieserver

De configuratieserver is een on-premises computer met Site Recovery-onderdelen, met inbegrip van de configuratieserver, processerver en hoofddoelserver.

- Voor VMware-VM's kunt u de configuratieserver instellen door te downloaden van een OVF-sjabloon voor het maken van een VMware-VM.
- Voor fysieke servers instellen u de configuratie van server-machine handmatig.

**Onderdeel** | **Vereisten**
--- |---
CPU-kernen | 8
RAM | 16 GB
Aantal schijven | 3-schijven<br/><br/> Schijven bevatten de OS-schijf, cacheschijf proces en bewaarstation voor failback.
Vrije schijfruimte | 600 GB vrije ruimte voor de cache van de processerver.
Vrije schijfruimte | 600 GB vrije ruimte voor de retentieschijf.
Besturingssysteem  | Windows Server 2012 R2 of Windows Server 2016 met Bureaubladervaring |
Landinstelling van het besturingssysteem | Engels (en-us)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | Niet nodig voor de versie van de configuratieserver [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) of hoger. 
Windows Server-functies | Active Directory Domain Services; niet inschakelen Internet informatieservices (IIS) of Hyper-V. 
Groepsbeleid| -Toegang tot de opdrachtprompt voorkomen. <br/> -Toegang tot registerbewerkingsprogramma's voorkomen. <br/> -Logica vertrouwen voor bestandsbijlagen. <br/> -Uitvoering van Script inschakelen. <br/> - [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Zorg ervoor dat u:<br/><br/> -Geen een bestaande standaardwebsite <br/> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br/> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br/> -Bestaande website /-app niet hebt luisteren op poort 443<br/>
Type NIC | VMXNET3 (indien geïmplementeerd als een VMware-VM)
Type IP-adres | Statisch
Poorten | 443 gebruikt voor de orchestration-besturingselement<br/>9443 voor gegevenstransport

## <a name="replicated-machines"></a>Gerepliceerde machines

Site Recovery biedt ondersteuning voor replicatie van alle werkbelasting die wordt uitgevoerd op een ondersteunde machine.

**Onderdeel** | **Details**
--- | ---
Instellingen van de computer | Machines die worden gerepliceerd naar Azure moeten voldoen aan [Azure-vereisten](#azure-vm-requirements).
Machine-werkbelasting | Site Recovery biedt ondersteuning voor replicatie van alle werkbelasting die wordt uitgevoerd op een ondersteunde machine. [Meer informatie](https://aka.ms/asr_workload).
Windows | -Windows Server 2019 (ondersteund vanaf [Update Rollup 34](https://support.microsoft.com/help/4490016) (versie 9.22 van de Mobility-service) en hoger.<br/> -Windows Server 2016 (64-bits Server Core, Server met Bureaubladervaring)<br/> - Windows Server 2012 R2, Windows Server 2012<br/> -Windows Server 2008 R2 met op minimaal SP1.<br/> -Windows Server 2008, 64-bits en 32-bits met op minste SP2]. Voor alleen migratie ondersteund. [Meer informatie](migrate-tutorial-windows-server-2008.md).<br/> -Windows 10, Windows 8.1, Windows 8, Windows 7, 64-bits (ondersteund vanaf [Update Rollup 36](https://support.microsoft.com/help/4503156) (versie 9.22 van de Mobility-service en hoger). Windows 7 RTM wordt niet ondersteund. 
Linux | Alleen 64-bits systeem wordt ondersteund. 32-bits systeem wordt niet ondersteund.<br/><br/>Elke Linux-server moet beschikken over [onderdelen van Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) geïnstalleerd. Het is vereist voor het starten van de server in Azure na een failover/testfailover. Als LIS onderdelen ontbreken, zorg er dan voor het installeren van de [onderdelen](https://www.microsoft.com/download/details.aspx?id=55106) vóór het inschakelen van replicatie voor de machines om op te starten in Azure. <br/><br/> Site Recovery deelt failover voor Linux-servers uitvoeren in Azure. Linux-leveranciers kunnen echter ondersteuning om alleen distributie-versies die nog niet hebt bereikt einde van de levenscyclus te beperken.<br/><br/> Op Linux-distributies, worden alleen de voorraad kernels die deel van de release-distributiepunt secundaire versie/update uitmaken ondersteund.<br/><br/> Beveiligde machines upgraden in belangrijke Linux distributie versies wordt niet ondersteund. Als u wilt bijwerken, schakelt u replicatie uit, werk het besturingssysteem en schakelt u de replicatie opnieuw.<br/><br/> [Meer informatie](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) over ondersteuning voor Linux en open-source-technologie in Azure.
Linux Red Hat Enterprise | 5.2-5,11</b><br/> 6.1-6.10</b><br/> 7.0-7,6<br/> <br/> Servers met Red Hat Enterprise Linux 5.2-5,11 & 6.1 6.10 geen [onderdelen van Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) vooraf zijn geïnstalleerd. Zorg ervoor dat voor het installeren van de [onderdelen](https://www.microsoft.com/download/details.aspx?id=55106) vóór het inschakelen van replicatie voor de machines om op te starten in Azure.
Linux: CentOS | 5.2-5,11</b><br/> 6.1-6.10</b><br/> 7.0-7,6<br/> <br/> Servers met CentOS 5.2-5,11 & 6.1 6.10 geen [onderdelen van Linux Integration Services (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) vooraf zijn geïnstalleerd. Zorg ervoor dat voor het installeren van de [onderdelen](https://www.microsoft.com/download/details.aspx?id=55106) vóór het inschakelen van replicatie voor de machines om op te starten in Azure.
Ubuntu | Ubuntu 14.04 LTS server [(revisie ondersteund kernelversies)](#ubuntu-kernel-versions)<br/><br/>Ubuntu 16.04 LTS server [(revisie ondersteund kernelversies)](#ubuntu-kernel-versions)
Debian | Debian 7/Debian 8 [(revisie ondersteund kernelversies)](#debian-kernel-versions)
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4 [(revisie ondersteund kernelversies)](#suse-linux-enterprise-server-12-supported-kernel-versions)<br/> SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4<br/> Gerepliceerde machines upgraden van SUSE Linux Enterprise Server 11 SP3 naar SP4 wordt niet ondersteund. Als u wilt bijwerken, schakelt u replicatie uit en opnieuw inschakelen na de upgrade.
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6<br/><br/> De Red Hat compatibele kernel of Unbreakable Enterprise Kernel versie 3, 4 en 5 (UEK3, UEK4, UEK5) 


### <a name="ubuntu-kernel-versions"></a>Ubuntu-kernel-versies


**Ondersteunde versie** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | [9.24][9.25 UR]  | 3.13.0-24-Generic naar 3.13.0-169-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-146-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1042-azure |
14.04 TNS | [9.24][9.24 UR] | 3.13.0-24-Generic naar 3.13.0-167-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-143-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1040-azure |
14.04 TNS | [9.23][9.23 UR] | 3.13.0-24-Generic naar 3.13.0-165-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1037-azure |
14.04 TNS | [9.22][9.22 UR] | 3.13.0-24-Generic naar 3.13.0-164-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1036-azure |
|||
16.04 LTS | [9.25][9.25 UR] | 4.4.0-21-Generic naar 4.4.0-146-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-48-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1042-azure|
16.04 LTS | [9.24][9.24 UR] | 4.4.0-21-Generic naar 4.4.0-143-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-46-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1040-azure|
16.04 LTS | [9.23][9.23 UR] | 4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-45-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1037-azure|
16.04 LTS | [9.22][9.22 UR] | 4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-43-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1036-azure|

### <a name="debian-kernel-versions"></a>Debian kernelversies


**Ondersteunde versie** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
Debian 7 | [9.21][9.21 UR], [9.22][9.22 UR],[9.23][9.23 UR], [9,24][9.24 UR]| 3.2.0-4-AMD64 naar 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.25][9.25 UR] | 3.16.0-4-amd64 to 3.16.0-8-amd64, 4.9.0-0.bpo.4-amd64 to 4.9.0-0.bpo.8-amd64 |
Debian 8 | [9.22][9.22 UR],[9.23][9.23 UR], [9,24][9.24 UR] | 3.16.0-4-amd64 to 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 to 4.9.0-0.bpo.8-amd64 |


### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>SUSE Linux Enterprise Server 12 ondersteunde versies van de kernel

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3 en SP4) | [9.25][9.25 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.104-default</br></br>SP3 4.4.73-5-default naar 4.4.176-94.88-default</br></br>SP3 4.4.138-4.7-azure naar 4.4.176-4.25-azure</br></br>SP4 4.12.14-94.41-default naar 4.12.14-95.13-default</br>SP4 4.12.14-6.3-azure naar 4.12.14-6.9-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3 en SP4) | [9.24][9.24 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.101-default</br></br>SP3 4.4.73-5-default naar 4.4.175-94.79-default</br></br>SP4 4.12.14-94.41-default naar 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3 en SP4) | [9.23][9.23 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.101-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.69-default</br></br>SP4 4.12.14-94.41-default naar 4.12.14-95.6-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | [9.22][9.22 UR] | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.72-default |


## <a name="linux-file-systemsguest-storage"></a>Linux-systemen/Gast bestandsopslag

**Onderdeel** | **Ondersteund**
--- | ---
Bestandssystemen | ext3, ext4, XFS
Volume manager | -LVM wordt ondersteund.<br/> - / boot op LVM wordt ondersteund vanaf [Update Rollup 31](https://support.microsoft.com/help/4478871/) (versie 9.20 van de Mobility-service) en hoger. Het wordt niet ondersteund in eerdere versies van de Mobility-service.<br/> -Er zijn meerdere besturingssysteemschijven worden niet ondersteund.
Geparavirtualiseerde opslagapparaten | Apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.
Meerdere wachtrij blok i/o-apparaten | Wordt niet ondersteund.
Fysieke servers met de opslagcontroller HP CCISS | Wordt niet ondersteund.
Apparaat/koppelpunt punt naamconventie | De naam van apparaat of de naam van koppelpunt moet uniek zijn.<br/> Zorg ervoor dat er geen twee apparaten/koppelpunten hoofdlettergevoelige namen hebben. Naamgeving van bijvoorbeeld apparaten voor dezelfde virtuele machine als *device1* en *Device1* wordt niet ondersteund.
Mappen | Als u een versie van de Mobility-service ouder dan versie 9.20 uitvoert (die zijn uitgebracht [Update Rollup 31](https://support.microsoft.com/help/4478871/)), en vervolgens deze beperkingen gelden:<br/><br/> -Deze mappen (indien ingesteld als afzonderlijke partities-bestandssystemen) moet op dezelfde schijf als besturingssysteem op de bronserver: / (hoofd), bevinden, / usr, / usr/local, /var, / etc.</br> -De map bevinden moet zich op een partitie op schijf en niet een LVM-volume.<br/><br/> Vanaf versie 9.20 en hoger, zijn deze beperkingen niet van toepassing. 
Opstartmap | -Meerdere opstartschijven worden ondersteund op een virtuele machine <br/> - / opstarten op een volume LVM in meer dan één schijf wordt niet ondersteund.<br/> -Een virtuele machine zonder een opstartschijf kan niet worden gerepliceerd.
Vrije schijfruimte| 2 GB op de/root-partitie <br/><br/> 250 MB op de map voor installatie
XFSv5 | XFSv5 functies op XFS bestandssystemen, zoals metagegevens controlesom, worden ondersteund (Mobility serviceversie 9.10 en hoger).<br/> Gebruik het hulpprogramma xfs_info om te controleren of de superblock XFS voor de partitie. Als `ftype` is ingesteld op 1, XFSv5 functies worden gebruikt.
BTRFS | BTRFS wordt ondersteund vanaf [Update Rollup 34](https://support.microsoft.com/help/4490016) (versie 9.22 van de Mobility-service) en hoger. BTRFS wordt niet ondersteund als:<br/><br/> -De BTRFS bestand system subvolume wordt gewijzigd nadat bescherming is ingeschakeld.</br> -De BTRFS-bestandssysteem is verspreid over meerdere schijven.</br> -De bestandssysteem BTRFS biedt ondersteuning voor RAID.

## <a name="vmdisk-management"></a>Beheer van de virtuele machine/schijf

**Actie** | **Details**
--- | ---
Grootte van de schijf op gerepliceerde virtuele machine wijzigen | Ondersteund.
Schijf toevoegen op de gerepliceerde virtuele machine | Wordt niet ondersteund.<br/> Schakel de replicatie voor de virtuele machine, de schijf toevoegen en replicatie opnieuw inschakelen.

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
Service-eindpunten voor een virtueel Azure-netwerk<br/> | Ja
Versneld netwerken | Nee

## <a name="storage"></a>Opslag
**Onderdeel** | **Ondersteund**
--- | ---
Dynamische schijf | OS-schijf moet een standaardschijf. <br/><br/>Gegevensschijven mogen dynamische schijven
De configuratie van de docker-schijf | Nee
Host NFS | Ja voor VMware<br/><br/> Nee voor fysieke servers
Host (iSCSI/FC) SAN | Ja
Host vSAN | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
Host-MPIO (Multipath I/O) | Ja, getest met Microsoft DSM, EMC PowerPath 5.7 SP4 EMC PowerPath DSM voor CLARiiON
Virtuele Hostvolumes (VVols) | Ja voor VMware<br/><br/> N.V.T. voor fysieke servers
VMDK-Gast/server | Ja
De gedeelde clusterschijf Gast/host-server | Nee
Versleutelde schijf Gast/host-server | Nee
Gast/NFS-server | Nee
Gast/server iSCSI | Nee
Gast/SMB 3.0-server | Nee
Gast/server RDM | Ja<br/><br/> N.V.T. voor fysieke servers
Gast/server schijf > 1 TB | Ja<br/><br/>Maximaal 4095 GB<br/><br/> Schijf moet groter zijn dan 1024 MB zijn.
Gast/server-schijf met de fysieke sectorgrootte van 4K logische en 4 k | Nee
Gast/server-schijf met de 4K logische en fysieke sectorgrootte van 512 bytes | Nee
Volume van de Gast/host-server met striped schijf > 4 TB <br/><br/>Logische volumebeheer (LVM)| Ja
Gast/server - opslagruimten | Nee
Schijf Gast/server-hot toevoegen of verwijderen | Nee
Gast/server - schijf uitsluiten | Ja
Gast/server MPIO (Multipath I/O) | Nee
Gast/server GPT-partities | Vijf partities worden ondersteund vanaf [Update Rollup 37](https://support.microsoft.com/help/4508614/) (versie 9.25 van de Mobility-service) en hoger. Eerder werden vier ondersteund.
Gast/server EFI/UEFI opstarten | -Wanneer u Mobility serviceversie 9.13 of hoger uitvoert ondersteund.<br/> -Ondersteund bij het migreren van VMware-machines of fysieke servers met Windows Server 2012 of later naar Azure.<br/> -U kunt alleen virtuele machines repliceren voor migratie. Failback naar on-premises wordt niet ondersteund.<br/> -Alleen NTFS wordt ondersteund, & beveiligde UEFI-opstarttype wordt niet ondersteund. <br/> -Sector schijfgrootte moet 512 bytes per fysieke sector.

## <a name="replication-channels"></a>Replicatie-kanalen

|**Type replicatie**   |**Ondersteund**  |
|---------|---------|
|Offloaded Data Transfer (ODX)    |       Nee  |
|Offline seeding        |   Nee      |
| Azure Data Box | Nee

## <a name="azure-storage"></a>Azure Storage

**Onderdeel** | **Ondersteund**
--- | ---
Lokaal redundante opslag | Ja
Geografisch redundante opslag | Ja
Geografisch redundante opslag met leestoegang | Ja
Koude opslag | Nee
Hot storage| Nee
Blok-blobs | Nee
Versleuteling-at-rest (SSE)| Ja
Premium Storage | Ja
Import/export-service | Nee
Azure Storage-firewalls voor vnet 's | Ja.<br/> Geconfigureerd voor opslag/cache-opslagaccount voor het doel (gebruikt voor het opslaan van replicatiegegevens).
Opslagaccounts voor algemeen gebruik v2 (warme en koude lagen) | Ja (transactie kosten aanzienlijk hogere voor V2 vergeleken met V1 zijn)

## <a name="azure-compute"></a>Azure-rekenen

**Functie** | **Ondersteund**
--- | ---
Beschikbaarheidssets | Ja
Beschikbaarheidszones | Nee
HUB | Ja
Managed Disks | Ja

## <a name="azure-vm-requirements"></a>Vereisten voor Azure VM 's

On-premises virtuele machines worden gerepliceerd naar Azure moeten voldoen aan de virtuele machine van Azure-vereisten in deze tabel wordt samengevat. Wanneer Site Recovery een controle van vereisten voor replicatie voert, mislukt de controle als aan bepaalde vereisten zijn niet voldaan.

**Onderdeel** | **Vereisten** | **Details**
--- | --- | ---
Gast-besturingssysteem | Controleer of [ondersteunde besturingssystemen](#replicated-machines) voor gerepliceerde machines. | Controle mislukt als niet-ondersteund.
Architectuur van de Gast-besturingssysteem | 64-bit. | Controle mislukt als niet-ondersteund.
Grootte van de besturingssysteemschijf | Maximaal 2048 GB. | Controle mislukt als niet-ondersteund.
Aantal besturingssysteemschijven | 1 | Controle mislukt als niet-ondersteund.
Aantal gegevensschijven | 64 of minder. | Controle mislukt als niet-ondersteund.
Grootte van de gegevensschijf | Maximaal 4095 GB | Controle mislukt als niet-ondersteund.
Netwerkadapters | Meerdere netwerkadapters worden ondersteund. |
Gedeelde VHD | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
FC-schijf | Wordt niet ondersteund. | Controle mislukt als niet-ondersteund.
BitLocker | Wordt niet ondersteund. | BitLocker moet worden uitgeschakeld voordat u replicatie voor een machine inschakelt. |
VM-naam | Van 1 tot 63 tekens bevatten.<br/><br/> Alleen letters, cijfers en afbreekstreepjes.<br/><br/> Naam van de machine moet beginnen en eindigen met een letter of cijfer. |  Werk de waarde in de eigenschappen van de machine in Site Recovery.

## <a name="churn-limits"></a>Limieten voor verloop

De volgende tabel bevat de Azure Site Recovery-limieten. 
- Deze limieten zijn gebaseerd op onze tests, maar hebben geen betrekking op alle mogelijke app i/o-combinaties.
- De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie.
- Voor optimale resultaten, wordt aangeraden dat u uitvoert het [hulpprogramma Deployment Planner](site-recovery-deployment-planner.md), en uitgebreide toepassing testen met behulp van testfailovers krijgt de prestaties voor uw app.

**Replicatiedoel** | **Gemiddelde I/O-grootte van bronschijf** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |20 MB/s | 1684 GB per schijf


**brongegevensverloop** | **Maximumaantal**
---|---
Gemiddeld gegevensverloop per VM| 25 MB/s
Piekgegevensverloop over alle schijven op een VM | 54 MB/s
Maximumgegevensverloop per dag dat wordt ondersteund door een processerver | 2 TB

- Dit zijn gemiddelden uitgaande van een I/O-overlapping van 30%.
- Site Recovery kan een hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag.
- Deze getallen wordt ervan uitgegaan dat een typische backlog van ongeveer vijf minuten. Dat wil zeggen dat de gegevens na het uploaden binnen vijf minuten worden verwerkt en er een herstelpunt is gemaakt.

## <a name="vault-tasks"></a>Kluis-taken

**Actie** | **Ondersteund**
--- | ---
Kluis verplaatsen tussen resourcegroepen | Nee
Verplaatsen van de kluis binnen en tussen abonnementen | Nee
Verplaatsen van opslag, netwerk, Azure-VM's op resourcegroepen | Nee
Opslag, netwerk, Azure-VM's binnen en tussen abonnementen verplaatsen. | Nee


## <a name="obtain-latest-components"></a>Meest recente onderdelen verkrijgen

**Name** | **Beschrijving** | **Details**
--- | --- | ---
Configuratieserver | Geïnstalleerde on-premises.<br/> Coördineert de communicatie tussen on-premises VMware-servers of fysieke machines en Azure. | - [Meer informatie over](vmware-physical-azure-config-process-server-overview.md) de configuratieserver.<br/> - [Meer informatie over](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) upgraden naar de meest recente versie.<br/> - [Meer informatie over](vmware-azure-deploy-configuration-server.md) instellen van de configuratieserver. 
Processerver | standaard geïnstalleerd op de configuratieserver.<br/> Ontvangt replicatiegegevens, optimaliseert met caching, compressie en versleuteling en verzendt ze naar Azure.<br/> Naarmate uw implementatie groeit, kunt u extra processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen. | - [Meer informatie over](vmware-physical-azure-config-process-server-overview.md) de processerver.<br/> - [Meer informatie over](vmware-azure-manage-process-server.md#upgrade-a-process-server) upgraden naar de meest recente versie.<br/> - [Meer informatie over](vmware-physical-large-deployment.md#set-up-a-process-server) scale-out processervers instellen.
Mobility-Service | Geïnstalleerd op de VM met VMware of fysieke servers die u wilt repliceren.<br/> Coördineert de replicatie tussen on-premises VMware-servers/fysieke servers en Azure.| - [Meer informatie over](vmware-physical-mobility-service-overview.md) de Mobility-service.<br/> - [Meer informatie over](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) upgraden naar de meest recente versie.<br/> 



## <a name="next-steps"></a>Volgende stappen
[Informatie over hoe](tutorial-prepare-azure.md) het voorbereiden van Azure voor herstel na noodgevallen van virtuele VMware-machines.

[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
