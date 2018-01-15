---
title: Azure Site Recovery-ondersteuningsmatrix voor het repliceren van Azure naar Azure | Microsoft Docs
description: Overzicht van de ondersteunde besturingssystemen en configuraties voor Azure Site Recovery-replicatie van Azure virtuele machines (VM's) van de ene regio naar een andere voor disaster recovery (Noodherstel) nodig.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/08/2017
ms.author: sujayt
ms.openlocfilehash: c15583b9420355bb7c35bd107b899c59e80e3741
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-to-azure"></a>Azure Site Recovery-ondersteuningsmatrix voor het repliceren van Azure naar Azure


>[!NOTE]
>
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

In dit artikel bevat een overzicht van ondersteunde configuraties en -onderdelen voor Azure Site Recovery wanneer repliceren en herstellen van virtuele machines in Azure vanaf één regio naar een andere regio.

## <a name="user-interface-options"></a>Opties voor de gebruikersinterface

**De gebruikersinterface** |  **Ondersteund / niet ondersteund**
--- | ---
**Azure Portal** | Ondersteund
**Klassieke portal** | Niet ondersteund
**PowerShell** | Momenteel niet ondersteund
**REST API** | Momenteel niet ondersteund
**CLI** | Momenteel niet ondersteund


## <a name="resource-move-support"></a>Ondersteuning voor het verplaatsen van resource

**Verplaatsen van brontype** | **Ondersteund / niet ondersteund** | **Opmerkingen**  
--- | --- | ---
**Kluis over brongroepen verplaatsen** | Niet ondersteund |U kunt de Recovery services-kluis niet verplaatsen tussen resourcegroepen.
**Verplaatsen van Compute, Storage en het netwerk naar andere resourcegroepen** | Niet ondersteund |Als u een virtuele machine (of de bijbehorende onderdelen, zoals opslag en netwerk) verplaatst nadat de replicatie is ingeschakeld, moet u replicatie uitschakelen en inschakelen van replicatie voor de virtuele machine opnieuw.



## <a name="support-for-deployment-models"></a>Ondersteuning voor implementatiemodellen

**Implementatiemodel** | **Ondersteund / niet ondersteund** | **Opmerkingen**  
--- | --- | ---
**Klassiek** | Ondersteund | U kunt alleen een klassieke virtuele machine repliceren en deze herstellen als een klassieke virtuele machine. U kunt deze niet herstellen als de virtuele machine van een Resource Manager. Als u een klassieke virtuele machine zonder een virtueel netwerk en rechtstreeks naar een Azure-regio implementeert, wordt het niet ondersteund.
**Resource Manager** | Ondersteund |

>[!NOTE]
>
> 1. Azure virtuele machines van één abonnement te repliceren naar een andere voor herstel na noodgevallen wordt niet ondersteund.
> 2. Migreren Azure virtuele machines over abonnementen wordt niet ondersteund.
> 3. Migreren Azure virtuele machines in dezelfde regio wordt niet ondersteund.
> 4. Azure virtuele machines migreren van klassieke implementatiemodel naar Resource manager-implementatiemodel wordt niet ondersteund.

## <a name="support-for-replicated-machine-os-versions"></a>Ondersteuning voor gerepliceerde machine OS-versies

De onderstaande ondersteuning is van toepassing op elke workload uitgevoerd op de genoemde OS.

#### <a name="windows"></a>Windows

- WindowsServer 2016 (Server Core, Server met Bureaubladbelevenis) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 met op minste SP1

>[!NOTE]
>
> \*Windows Server 2016 Nano Server wordt niet ondersteund.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6,8, 6,9, 7.0, 7.1, 7.2, 7.3,7.4
- CentOS 6.5, 6.6, 6.7, 6,8, 6,9, 7.0, 7.1, 7.2, 7.3,7.4
- Ubuntu 14.04 TNS Server [ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 TNS Server [ (kernel-versies ondersteund)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7
- Debian 8
- Oracle Enterprise Linux 6.4, 6.5 met Red Hat compatibel kernel of Unbreakable Enterprise Kernel versie 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4

(Upgrade van de computers van SLES 11 SP3 repliceren naar SLES 11 SP4 wordt niet ondersteund. Als een gerepliceerde machine is bijgewerkt van SLES 11SP3 naar SLES 11 SP4 is geïnstalleerd, moet u replicatie uitschakelen en beveiligt de machine opnieuw na de upgrade.)

>[!NOTE]
>
> Ubuntu-servers met verificatie op basis van wachtwoorden en meld u aan, en het cloud-init-pakket voor het configureren van virtuele machines van cloud, heeft misschien op basis van wachtwoorden aanmelding uitgeschakeld bij failover (afhankelijk van de configuratie cloudinit.) Aanmelding op basis van wachtwoorden opnieuw worden ingeschakeld op de virtuele machine kan worden door het wachtwoord in het menu instellingen opnieuw instellen (onder de ondersteuning en probleemoplossing punt) van de mislukte via de virtuele machine op de Azure-portal.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde versies van Ubuntu kernel voor Azure virtual machines

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | 9.10 | 3.13.0-24-Generic naar 3.13.0-121-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-81-generic |
14.04 TNS | 9.11 | 3.13.0-24-Generic naar 3.13.0-125-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-83-generic |
14.04 TNS | 9.12 | 3.13.0-24-Generic naar 3.13.0-132-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-96-generic |
14.04 TNS | 9.13 | 3.13.0-24-Generic naar 3.13.0-137-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-104-generic |
16.04 TNS | 9.10 | 4.4.0-21-Generic naar 4.4.0-81-generic,<br/>4.8.0-34-Generic naar 4.8.0-56-generic,<br/>4.10.0-14-Generic naar 4.10.0-24-generic |
16.04 TNS | 9.11 | 4.4.0-21-Generic naar 4.4.0-83-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-27-generic |
16.04 TNS | 9.12 | 4.4.0-21-Generic naar 4.4.0-96-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-35-generic |
16.04 TNS | 9.13 | 4.4.0-21-Generic naar 4.4.0-104-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-Generic naar 4.10.0-42-generic |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Ondersteunde bestandssystemen en Gast opslagconfiguraties op Azure virtuele machines met Linux-besturingssysteem

* -Bestandssystemen: ext3, ext4, ReiserFS (Suse Linux Enterprise Server alleen), XFS
* Volumebeheer: LVM2
* Multipath-software: apparaat toewijzen

## <a name="region-support"></a>Regio-ondersteuning

U kunt repliceren en het herstellen van virtuele machines tussen twee gedeelten in hetzelfde geografische cluster.

**Geografische cluster** | **Azure-regio's**
-- | --
Amerika | Canada-Oost, Canada centraal, Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2, VS-midden, Noord-centraal VS
Europa | VK West, VK Zuid Noord-Europa, West-Europa
Azië | India Zuid, India centraal, Zuidoost-Azië Oost-Azië, Japan-Oost, Japan-West, Korea-midden, Zuid-Korea
Australië   | Australië-Oost, Australië-Zuidoost
Azure Government    | VS GOV Virginia, VS GOV Iowa, VS GOV Arizona, VS GOV Texas, VS DOD Oost, VS DOD-centraal
Duitsland | Duitsland centraal, Duitsland noordoosten
China | China Oost, China Noord

>[!NOTE]
>
> Voor de regio Brazilië-Zuid, kunt u alleen repliceren en failover naar een van de Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2 en Noordelijk Centraal, VS-regio's en failback.


## <a name="support-for-compute-configuration"></a>Ondersteuning voor Compute-configuratie

**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Grootte | Een Azure VM-grootte met ten minste 2 CPU-kernen en 1 GB RAM-geheugen | Raadpleeg [grootten voor virtuele machine van Azure](../virtual-machines/windows/sizes.md)
Beschikbaarheidssets | Ondersteund | Als u de standaardoptie tijdens stap 'Replicatie inschakelen' in de portal gebruikt, wordt de beschikbaarheidsset automatisch gemaakt op basis van de configuratie van de regio. Kunt u de doel-beschikbaarheidsset ' gerepliceerde item > Instellingen > berekening en netwerk > beschikbaarheidsset ' elk gewenst moment.
Virtuele machines hybride gebruik voordeel (HUB) | Ondersteund | Als de bron-VM HUB-licentie ingeschakeld heeft, de testfailover of Failover VM gebruikt ook de licentie HUB.
Virtuele-machineschaalsets | Niet ondersteund |
Azure-afbeeldingen - Microsoft gepubliceerd | Ondersteund | Ondersteund, zolang de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery
Installatiekopieën van galerie van Azure - gepubliceerd van derden | Ondersteund | Ondersteund, zolang de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery.
Aangepaste installatiekopieën - gepubliceerd van derden | Ondersteund | Ondersteund, zolang de virtuele machine op een ondersteund besturingssysteem wordt uitgevoerd door Site Recovery.
Virtuele machines gemigreerd met behulp van Site Recovery | Ondersteund | Als dit dat een VMware of fysieke machine migreren naar Azure met Site Recovery, die u wilt verwijderen van de oudere versie van de mobility-service en start de computer opnieuw op voordat deze worden gerepliceerd naar een andere Azure-regio.

## <a name="support-for-storage-configuration"></a>Ondersteuning voor opslagconfiguratie

**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Maximale grootte van de OS-schijven | 2048 GB | Raadpleeg [schijven die worden gebruikt door virtuele machines.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
De grootte van maximaal gegevensschijf | 4095 GB | Raadpleeg [schijven die worden gebruikt door virtuele machines.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Aantal gegevensschijven | Maximaal 64 ondersteund door een specifieke Azure VM-grootte | Raadpleeg [grootten voor virtuele machine van Azure](../virtual-machines/windows/sizes.md)
Tijdelijke schijf | Altijd uitgesloten van replicatie | Tijdelijke schijf is uitgesloten van replicatie altijd. Plaats geen permanente gegevens niet op de tijdelijke schijf aan de hand van Azure richtlijnen. Raadpleeg [tijdelijke schijf op Azure Virtual machines](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) voor meer informatie.
Snelheid van de gegevens wijzigen op de schijf | Maximaal 6 MBps per schijf | Als de gemiddelde gegevens snelheid wijzigen op de schijf is dan 6 MBps continu, replicatie wordt niet kan verwerken. Echter, als dit een ' burst ' in incidentele gegevens is en de snelheid waarmee u gegevens wijzigt is groter dan 6 MBps enige tijd en ontvangt u omlaag, replicatie wordt lopen. In dit geval ziet u mogelijk enigszins vertraagd herstelpunten.
Schijven op standaard storage-accounts | Ondersteund |
Schijven op premium storage-accounts | Ondersteund | Als een virtuele machine schijven die zijn verdeeld over premium en standard storage-accounts bevat, kunt u een ander doel storage-account voor elke schijf om te controleren of hebt u dezelfde opslagconfiguratie van de in doelregio selecteren
Standaardschijven beheerd | Niet ondersteund |  
Premium-beheerde schijven | Niet ondersteund |
Opslagruimten | Ondersteund |         
Codering in rust (SSE) | Ondersteund | Voor de cache en het doel storage-accounts, kunt u een opslagaccount SSE ingeschakeld.     
Azure Disk Encryption (ADE) | Niet ondersteund |
Schijf hot toevoegen of verwijderen | Niet ondersteund | Als u toevoegen of verwijderen van de gegevensschijf op de virtuele machine, moet u replicatie uitschakelen en inschakelen van replicatie opnieuw voor de virtuele machine.
Schijf uitsluiten | Niet ondersteund|   Tijdelijke schijf is niet standaard opgenomen.
LRS | Ondersteund |
GRS | Ondersteund |
RA-GRS | Ondersteund |
ZRS | Niet ondersteund |  
Cool en Hot Storage | Niet ondersteund | Schijven voor virtuele machine worden niet ondersteund op cool en hot storage
Virtueel netwerk Service-eindpunten (Azure Storage firewalls en virtuele netwerken)  | Nee | Waardoor de toegang tot specifieke Azure virtuele netwerken van cache storage-accounts gebruikt voor het opslaan van de gerepliceerde gegevens wordt niet ondersteund. 
V2 opslagaccounts voor algemeen gebruik (zowel Hot en Cool laag) | Nee | Transactie kosten toename vergeleken aanzienlijk met algemeen V1 storage-accounts

>[!IMPORTANT]
> Zorg ervoor dat u merkt dat de VM schijf schaalbaarheids- en prestatiedoelen voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) of [Windows](../virtual-machines/windows/disk-scalability-targets.md) virtuele machines om eventuele prestatieproblemen te voorkomen. Als u de standaardinstellingen volgt, maakt Site Recovery de vereiste schijven en storage-accounts op basis van de configuratie van de bron. Als u aanpassen en uw eigen instellingen selecteert, zorg ervoor dat u de schijf schaalbaarheids- en prestatiedoelen voor uw virtuele bronmachines volgt.

## <a name="support-for-network-configuration"></a>Ondersteuning voor netwerkconfiguratie
**Configuratie** | **Ondersteund/niet ondersteund** | **Opmerkingen**
--- | --- | ---
Netwerkinterface (NIC) | Een Resourcegroepnaam kunt u het maximum aantal NIC's ondersteund door een specifieke Azure VM-grootte | NIC's worden gemaakt wanneer de virtuele machine wordt gemaakt als onderdeel van de testfailover of failover-bewerking. Het aantal NIC's op de VM-failover is afhankelijk van het aantal NIC's de bron die VM ten tijde heeft van het inschakelen van replicatie. Als u toevoegen/verwijderen NIC nadat de replicatie is ingeschakeld, heeft deze geen gevolgen voor NIC-aantal op de VM-failover.
Internet Load Balancer | Ondersteund | U moet de vooraf geconfigureerde taakverdeling met behulp van een azure automatiseringsscript in een herstelplan koppelen.
Interne Load balancer | Ondersteund | U moet de vooraf geconfigureerde taakverdeling met behulp van een azure automatiseringsscript in een herstelplan koppelen.
Openbare IP| Ondersteund | U moet een bestaand openbaar IP-adres met de NIC koppelen of maken en koppelen aan de NIC met een azure automatiseringsscript in een herstelplan.
NSG op NIC (Resource Manager)| Ondersteund | U moet het NSG aan de NIC met een azure automatiseringsscript in een plan voor herstel koppelt.  
NSG voor subnet (Resource Manager en Classic)| Ondersteund | U moet het NSG aan de NIC met een azure automatiseringsscript in een plan voor herstel koppelt.
NSG op virtuele machine (klassiek)| Ondersteund | U moet het NSG aan de NIC met een azure automatiseringsscript in een plan voor herstel koppelt.
Gereserveerde IP-adres (statische IP) / behouden van de bron-IP | Ondersteund | Als de Netwerkadapter op de bron-VM statische IP-configuratie heeft en het doelsubnet het hetzelfde IP-adres beschikbaar is, wordt deze is toegewezen aan de VM-failover. Als het doelsubnet geen het hetzelfde IP-adres beschikbaar is, wordt een van de beschikbare IP-adressen in het subnet is gereserveerd voor deze virtuele machine. U kunt opgeven dat een vaste IP-adres van uw keuze in ' gerepliceerde item > Instellingen > berekening en netwerk > netwerkinterfaces. U kunt selecteren van de NIC en geef het subnet en IP-adres van uw keuze.
Dynamische IP| Ondersteund | Als de Netwerkadapter op de bron-VM dynamische IP-configuratie heeft, de Netwerkadapter op de failover VM is ook standaard dynamisch. U kunt opgeven dat een vaste IP-adres van uw keuze in ' gerepliceerde item > Instellingen > berekening en netwerk > netwerkinterfaces. U kunt selecteren van de NIC en geef het subnet en IP-adres van uw keuze.
Traffic Manager-integratie | Ondersteund | U kunt uw traffic manager zodanig dat het verkeer wordt doorgestuurd naar het eindpunt in bron regio op regelmatige basis en naar het eindpunt in doelregio in geval van een failover vooraf configureren.
Azure DNS worden beheerd | Ondersteund |
Aangepaste DNS  | Ondersteund |    
Niet-geverifieerde Proxy | Ondersteund | Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)    
Geverifieerde proxyserver | Niet ondersteund | Als de virtuele machine van een geverifieerde proxyserver voor uitgaande verbinding gebruikmaakt, kan niet worden gerepliceerd met Azure Site Recovery.    
Site naar Site VPN met on-premises (met of zonder ExpressRoute)| Ondersteund | Zorg ervoor dat de udr's en het nsg's zodanig dat de Site recovery-verkeer niet kan worden doorgestuurd naar on-premises zijn geconfigureerd. Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET-naar-VNET-verbinding | Ondersteund | Raadpleeg [leidraad voor netwerken.](site-recovery-azure-to-azure-networking-guidance.md)  


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [networking richtlijnen voor het repliceren van virtuele Azure-machines](site-recovery-azure-to-azure-networking-guidance.md)
- Beginnen met het beveiligen van uw werkbelastingen door [virtuele Azure-machines repliceren](site-recovery-azure-to-azure.md)
