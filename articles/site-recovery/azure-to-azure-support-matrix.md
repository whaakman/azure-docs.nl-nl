---
title: Ondersteuningsmatrix voor Azure Site Recovery voor het repliceren van Azure naar Azure | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en configuraties voor de Azure Site Recovery-replicatie van virtuele Azure-machines (VM's) van de ene regio naar een andere voor disaster recovery (DR) behoeften.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: sujayt
ms.openlocfilehash: 43955cd516e9779200b66608270797a66565f53b
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378462"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Ondersteuningsmatrix voor het repliceren van de ene Azure-regio naar een andere



In dit artikel bevat een overzicht van ondersteunde configuraties en -onderdelen wanneer u repliceren en herstellen van virtuele machines van Azure van de ene regio naar een andere regio met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

## <a name="user-interface-options"></a>Opties voor de gebruikersinterface

**Gebruikersinterface** |  **Ondersteund / niet ondersteund**
--- | ---
**Azure Portal** | Ondersteund
**PowerShell** | [Replicatie van Azure naar Azure met PowerShell](azure-to-azure-powershell.md)
**REST API** | Momenteel niet ondersteund
**CLI** | Momenteel niet ondersteund


## <a name="resource-support"></a>Resource-ondersteuning

**Resourcetype verplaatsen** | **Details**
--- | --- | ---
**Kluis verplaatsen tussen resourcegroepen** | Niet ondersteund<br/><br/> U kunt een Recovery services-kluis niet verplaatsen tussen resourcegroepen.
**Compute/opslagnetwerk resources verplaatsen tussen resourcegroepen** | Wordt niet ondersteund.<br/><br/> Als u een virtuele machine of de bijbehorende onderdelen, zoals opslag/netwerk verplaatsen wanneer deze wordt gerepliceerd, moet u replicatie uitschakelen en weer inschakelen replicatie voor de virtuele machine.
**Virtuele Azure-machines repliceren van één abonnement naar een andere voor herstel na noodgevallen** | Binnen dezelfde Azure Active Directory-tenant voor 'Resource manager-implementatiemodel' virtuele machines wordt ondersteund. Wordt niet ondersteund voor 'Klassieke implementatiemodel' virtuele machines.
**Virtuele machines migreren tussen regio's binnen de ondersteunde geografische clusters (binnen en tussen abonnementen)** | Binnen dezelfde Azure Active Directory-tenant voor 'Resource manager-implementatiemodel' virtuele machines wordt ondersteund. Wordt niet ondersteund voor 'Klassieke implementatiemodel' virtuele machines.
**Migreren van VM's binnen dezelfde regio** | Wordt niet ondersteund.


## <a name="support-for-replicated-machine-os-versions"></a>Ondersteuning voor de gerepliceerde machine besturingssysteemversies

De hieronder ondersteuning is van toepassing op alle werkbelasting die wordt uitgevoerd op het besturingssysteem vermeld.

#### <a name="windows"></a>Windows

- WindowsServer 2016 (Server Core, Server met Bureaubladervaring) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 met op minste SP1

>[!NOTE]
>
> \* Windows Server 2016 Nano Server wordt niet ondersteund.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6,8, 6,9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5   
- CentOS 6.5, 6.6, 6.7, 6,8, 6,9, 6.10, 7.0, 7.1, 7.2, 7.3,7.4, 7.5
- Ubuntu 14.04 LTS Server [ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7 [ (kernel-versies ondersteund)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Debian 8 [ (kernel-versies ondersteund)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 12 SP1, SP2, SP3 [ (kernel-versies ondersteund)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4
- Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibele kernel of Unbreakable Enterprise Kernel versie 3 (UEK3)

(Upgrade van de machines repliceren van SLES 11 SP3 naar SLES 11 SP4 wordt niet ondersteund. Als een gerepliceerde virtuele machine is bijgewerkt van SLES 11SP3 naar SLES 11 SP4, moet u replicatie uitschakelen en het beveiligen van de machine opnieuw na de upgrade.)

>[!NOTE]
>
> Ubuntu-servers met behulp van verificatie op basis van wachtwoorden en meld u aan en configureren van virtuele machines van cloud met behulp van het cloud-init-pakket mogelijk op basis van wachtwoorden aanmelding uitgeschakeld bij failover (afhankelijk van de configuratie cloudinit.) Aanmelding op basis van wachtwoorden kan opnieuw worden ingeschakeld op de virtuele machine door de fabrieksinstellingen van het wachtwoord in het instellingenmenu (onder de ondersteuning + probleemoplossing punt) van de mislukte via virtuele machine op de Azure-portal.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde versies van Ubuntu kernel voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | 9.19 | 3.13.0-24-Generic naar 3.13.0-153-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-131-generic |
14.04 TNS | 9.18 | 3.13.0-24-Generic naar 3.13.0-151-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-128-generic |
14.04 TNS | 9.17 | 3.13.0-24-Generic naar 3.13.0-147-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-124-generic |
14.04 TNS | 9.16 | 3.13.0-24-Generic naar 3.13.0-144-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-119-generic |
|||
16.04 LTS | 9.19 | 4.4.0-21-Generic naar 4.4.0-131-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-30-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-Generic naar 4.4.0-128-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure |
16.04 LTS | 9.17 | 4.4.0-21-Generic naar 4.4.0-124-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-41-generic,<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1016-azure |
16.04 LTS | 9.16 | 4.4.0-21-Generic naar 4.4.0-119-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-38-generic,<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1012-azure |


### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde kernelversies van Debian voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
Debian 7 | 9.17,9.18,9.19 | 3.2.0-4-AMD64 naar 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.16 | 3.2.0-4-AMD64 naar 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.19 | 3.16.0-4-AMD64 naar 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.17, 9.18 | 3.16.0-4-AMD64 naar 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-AMD64 naar 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 naar 4.9.0-0.bpo.5-amd64 |

### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde versies van SUSE Linux Enterprise Server 12 kernel voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.18 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default naar 4.4.138-94.39-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.17 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.88-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default naar 4.4.126-94.22-default |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Ondersteunde bestandssystemen en Gast opslagconfiguraties op Azure virtual machines met Linux-besturingssysteem

* -Bestandssystemen: ext3, ext4, ReiserFS (Suse Linux Enterprise Server alleen), XFS
* Volume manager: LVM2
* Multipath-software: apparaat toewijzen

## <a name="region-support"></a>Ondersteuning voor regio

U kunt repliceren en herstellen van virtuele machines tussen elke twee regio's binnen hetzelfde geografische cluster.

**Geografische cluster** | **Azure-regio's**
-- | --
Amerika | Canada-Oost, Canada-midden, Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2, VS-midden, Noord-centraal VS
Europa | UK-West, UK-Zuid, Noord-Europa, West-Europa, Frankrijk-centraal, Frankrijk-Zuid
Azië | Zuid-India, centraal-India, Zuidoost-Azië, Oost-Azië, Japan-Oost, Japan-West, Korea centraal, Korea Zuid
Australië   | Australië-Oost, Australië-Zuidoost
Azure Government    | VS (overheid) Virginia, VS (overheid)-Iowa, VS (overheid)-Arizona, VS (overheid)-Texas, VS DOD Oost, VS DOD-centraal
Duitsland | Duitsland-centraal, Duitsland-Noordoost
China | China-Oost, China-Noord

>[!NOTE]
>
> Voor de regio Brazilië-Zuid, kunt u alleen repliceren en failover naar een van Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2 en Noord-centraal VS-regio's en een failback uitvoeren.

## <a name="support-for-vmdisk-management"></a>Ondersteuning voor het beheer van de virtuele machine/schijf

**Actie** | **Details**
-- | ---
Grootte van de schijf op gerepliceerde virtuele machine wijzigen | Ondersteund
Schijf toevoegen aan de gerepliceerde virtuele machine | Wordt niet ondersteund. U moet aan het uitschakelen van replicatie voor de virtuele machine, de schijf toevoegen en schakelt u de replicatie opnieuw.


## <a name="support-for-compute-configuration"></a>Ondersteuning voor Compute-configuratie

**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Grootte | Elke Azure-VM-grootte met ten minste 2 CPU-kernen en 1 GB RAM-geheugen | Raadpleeg [Azure VM-grootten](../virtual-machines/windows/sizes.md)
Beschikbaarheidssets | Ondersteund | Als u de standaardoptie tijdens de stap 'Replicatie inschakelen' in de portal gebruikt, is de beschikbaarheidsset automatisch gemaakt op basis van de configuratie van de gegevensbron regio. U kunt de doel-beschikbaarheidsset in wijzigen ' gerepliceerd item > Instellingen > berekening en netwerk > beschikbaarheidsset ' elk gewenst moment.
Beschikbaarheidszones | Niet ondersteund | Virtuele machines die worden geïmplementeerd in beschikbaarheidszones worden momenteel niet ondersteund.
Virtuele machines Hybrid Use Benefit (HUB) | Ondersteund | Als de bron-VM HUB license ingeschakeld heeft, de testfailover of Failover-VM gebruikt ook de HUB-licentie.
Virtuele-machineschaalsets | Niet ondersteund |
Azure-galerie met installatiekopieën - Microsoft gepubliceerd | Ondersteund | Ondersteund als de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery
Installatiekopieën van Azure-galerie - van derden gepubliceerd | Ondersteund | Ondersteund als de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery.
Aangepaste installatiekopieën - van derden gepubliceerd | Ondersteund | Ondersteund als de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery.
Virtuele machines gemigreerd met behulp van Site Recovery | Ondersteund | Als het is dat een VMware-/ fysieke machine migreren naar Azure met Site Recovery, moet u de oudere versie van mobiliteitsservice verwijderen en start de machine opnieuw voordat u deze repliceert naar een andere Azure-regio.

## <a name="support-for-storage-configuration"></a>Ondersteuning voor opslagconfiguratie

**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Maximale grootte van de Besturingssysteemschijf | 2048 GB | Raadpleeg [schijven die worden gebruikt door virtuele machines.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maximale grootte van de gegevensschijf | 4095 GB | Raadpleeg [schijven die worden gebruikt door virtuele machines.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Aantal gegevensschijven | Maximaal 64 als ondersteund door een specifieke Azure-VM-grootte | Raadpleeg [Azure VM-grootten](../virtual-machines/windows/sizes.md)
Tijdelijke schijf | Altijd worden uitgesloten van replicatie | Tijdelijke schijf is uitgesloten van replicatie wordt altijd. Plaats geen permanente gegevens niet op de tijdelijke schijf aan de hand van Azure-richtlijnen. Raadpleeg [tijdelijke schijf op Azure Virtual machines](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) voor meer informatie.
De veranderingssnelheid van gegevens op de schijf | Maximaal 10 MBps per schijf voor Premium-opslag en 2 MBps per schijf voor Standard-opslag | Als de gemiddelde gegevenswijzigingssnelheid op de schijf meer dan 10 (voor Premium) en 2 MBps (voor Standard) continu is, replicatie wordt niet kan verwerken. Echter, als dit een gegevensburst af en toe is en de veranderingssnelheid van gegevens is groter dan 10 MBps (voor Premium) en 2 MBps (voor Standard) voor enige tijd en wordt geleverd omlaag, replicatie wordt achterstand. In dit geval ziet u enigszins vertraagd herstelpunten.
Schijven op standard storage-accounts | Ondersteund |
Schijven op premium storage-accounts | Ondersteund | Als een virtuele machine schijven die zijn verdeeld over premium en standard storage-accounts bevat, kunt u een ander doel-opslagaccount voor elke schijf om te controleren of u beschikt over de opslagconfiguratie van de dezelfde in doelregio selecteren
Standard Managed disks | Ondersteund in Azure-regio's waar Azure Site Recovery wordt ondersteund. |  
Premium beheerde schijven | Ondersteund in Azure-regio's waar Azure Site Recovery wordt ondersteund. |
Opslagruimten | Ondersteund |         
Versleuteling-at-rest (SSE) | Ondersteund | SSE is de standaardinstelling op storage-accounts.   
Azure Disk Encryption (ADE) | Niet ondersteund |
Hot toevoegen of verwijderen-schijf | Niet ondersteund | Als u toevoegen of verwijderen van de gegevensschijf op de virtuele machine, moet u replicatie uitschakelen en inschakelen van replicatie opnieuw voor de virtuele machine.
Schijf uitsluiten | Niet ondersteund|   Tijdelijke schijf is uitgesloten standaard.
Storage Spaces Direct  | Niet ondersteund|
Scale-out bestandsserver  | Niet ondersteund|
LRS | Ondersteund |
GRS | Ondersteund |
RA-GRS | Ondersteund |
ZRS | Niet ondersteund |  
Opslag van koude en warme | Niet ondersteund | Schijven van virtuele machines worden niet ondersteund op de opslag van koude en warme
Azure Storage-firewalls voor virtuele netwerken  | Nee | Toestaan van toegang tot specifieke Azure-netwerken op cacheopslagaccounts gebruikt voor het opslaan van gerepliceerde gegevens wordt niet ondersteund.
Opslagaccounts voor algemeen gebruik V2 (zowel warme en koude laag) | Nee | Toename van de transactie kosten vergeleken aanzienlijk met algemeen gebruik V1-opslagaccounts

>[!IMPORTANT]
> Zorg ervoor dat u ziet dat de virtuele machine schijf schaalbaarheids- en prestatiedoelen voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) of [Windows](../virtual-machines/windows/disk-scalability-targets.md) virtuele machines om eventuele prestatieproblemen te voorkomen. Als u de standaardinstellingen volgt, maakt Site Recovery de benodigde schijven en opslagaccounts op basis van de configuratie van de gegevensbron. Als u aanpassen en uw eigen instellingen selecteert, zorgt u ervoor dat u de schijf schaalbaarheids- en prestatiedoelen voor de bron-VM's volgt.

## <a name="support-for-network-configuration"></a>Ondersteuning voor netwerkconfiguratie
**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Netwerkinterface (NIC) | Maximumaantal NIC's die worden ondersteund door een specifieke Azure-VM-grootte | NIC's worden gemaakt wanneer de virtuele machine is gemaakt als onderdeel van de testfailover of failover-bewerking. Het aantal NIC's op de failover-VM is afhankelijk van het aantal NIC's de bron die virtuele machine op het moment heeft van het inschakelen van replicatie. Als u toevoegen/verwijderen NIC na het inschakelen van replicatie, heeft dit geen invloed op aantal NIC op de failover-VM.
Internet Load Balancer | Ondersteund | U moet koppelen van de vooraf geconfigureerde load-balancer met behulp van een azure automatiseringsscript van een herstelplan te gaan.
Een interne Load balancer | Ondersteund | U moet koppelen van de vooraf geconfigureerde load-balancer met behulp van een azure automatiseringsscript van een herstelplan te gaan.
Openbare IP| Ondersteund | U moet een bestaande openbare IP-adres aan de NIC koppelen of er een maken en koppelen aan de NIC met behulp van een azure automatiseringsscript van een herstelplan te gaan.
NSG op de NIC (Resource Manager)| Ondersteund | U moet de NSG koppelen aan de NIC met behulp van een azure automatiseringsscript van een herstelplan te gaan.  
NSG voor subnet (Resource Manager en klassiek)| Ondersteund | U moet de NSG aan het subnet met behulp van een azure automatiseringsscript in een herstelplan koppelen.
NSG op de virtuele machine (klassiek)| Ondersteund | U moet de NSG koppelen aan de NIC met behulp van een azure automatiseringsscript van een herstelplan te gaan.
Gereserveerd IP (statisch IP-adres) / bron-IP-adres behouden | Ondersteund | Als de NIC op de bron-VM statische IP-configuratie heeft en het doelsubnet het hetzelfde IP-adres beschikbaar is, wordt deze toegewezen aan de failover-VM. Als het doelsubnet is geen het hetzelfde IP-adres beschikbaar is, wordt een van de beschikbare IP-adressen in het subnet is gereserveerd voor deze virtuele machine. U kunt opgeven dat een vaste IP-adres van uw keuze in ' gerepliceerd item > Instellingen > berekening en netwerk > netwerkinterfaces. U kunt de NIC selecteren en geef het subnet en IP-adres van uw keuze.
Dynamische IP-Adressen| Ondersteund | Als de NIC op de bron-VM dynamische IP-configuratie heeft, de NIC op de failover-VM is ook dynamisch standaard. U kunt opgeven dat een vaste IP-adres van uw keuze in ' gerepliceerd item > Instellingen > berekening en netwerk > netwerkinterfaces. U kunt de NIC selecteren en geef het subnet en IP-adres van uw keuze.
Traffic Manager-integratie | Ondersteund | U kunt vooraf traffic manager zodanig dat het verkeer wordt doorgestuurd naar het eindpunt in de regio van de gegevensbron op gezette tijden en naar het eindpunt in de doelregio in het geval van failover configureren.
Azure beheerde DNS | Ondersteund |
Aangepaste DNS  | Ondersteund |    
Niet-geverifieerde Proxy | Ondersteund | Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)    
Geverifieerde Proxy | Niet ondersteund | Als de virtuele machine van een geverifieerde proxyserver voor de uitgaande connectiviteit gebruikmaakt, kan niet worden gerepliceerd met behulp van Azure Site Recovery.    
Site-naar-Site-VPN met on-premises (met of zonder ExpressRoute)| Ondersteund | Zorg ervoor dat de Udr en nsg's zodanig dat het Site recovery-verkeer niet wordt doorgestuurd naar on-premises zijn geconfigureerd. Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET-naar-VNET-verbinding | Ondersteund | Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)  
Service-eindpunten voor virtueel netwerk | Ondersteund | Azure Storage-firewalls voor virtuele netwerken worden niet ondersteund. Toestaan van toegang tot specifieke Azure-netwerken op cacheopslagaccounts gebruikt voor het opslaan van gerepliceerde gegevens wordt niet ondersteund.
Versneld netwerken | Niet ondersteund | Een virtuele machine met versnelde netwerken ingeschakeld kan worden gerepliceerd, maar de failover VM geen versnelde netwerken ingeschakeld. Versneld netwerken worden ook uitgeschakeld voor de bron-VM voor failback.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [netwerken richtlijnen voor het repliceren van virtuele Azure-machines](site-recovery-azure-to-azure-networking-guidance.md)
- Beginnen met het beveiligen van uw workloads door [virtuele Azure-machines repliceren](site-recovery-azure-to-azure.md)
