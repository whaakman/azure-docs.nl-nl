---
title: Ondersteuningsmatrix voor herstel na noodgevallen van virtuele Azure-machines tussen Azure-regio's met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de ondersteunde besturingssystemen en configuraties voor de Azure Site Recovery-replicatie van virtuele Azure-machines (VM's) van de ene regio naar een andere voor disaster recovery (DR) behoeften.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/20/2019
ms.author: raynew
ms.openlocfilehash: 4ce4cdbaf49fb743a14c7a5285ed29b9ad49faf5
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652651"
---
# <a name="support-matrix-for-replicating-azure-vms-from-one-region-to-another"></a>Ondersteuningsmatrix voor het repliceren van virtuele Azure-machines van de ene regio naar een andere

In dit artikel bevat een overzicht van ondersteunde configuraties en -onderdelen wanneer u herstel na noodgeval met replicatie, failover en herstel van virtuele Azure-machines van de ene Azure-regio naar een andere, implementeert met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="deployment-method-support"></a>Ondersteuning voor implementatie-methode

**Implementatie** |  **Ondersteuning**
--- | ---
**Azure-portal** | Ondersteund.
**PowerShell** | Ondersteund. [Meer informatie](azure-to-azure-powershell.md)
**REST API** | Ondersteund.
**CLI** | Momenteel niet ondersteund


## <a name="resource-support"></a>Resource-ondersteuning

**Resource-actie** | **Details**
--- | --- | ---
**Verplaatsen van kluizen andere resourcegroepen** | Niet ondersteund
**Compute/opslagnetwerk resources verplaatsen tussen resourcegroepen** | Wordt niet ondersteund.<br/><br/> Als u een virtuele machine of de bijbehorende onderdelen, zoals opslag/netwerk verplaatst nadat de virtuele machine repliceert, die u wilt uitschakelen en vervolgens weer inschakelen replicatie voor de virtuele machine.
**Virtuele Azure-machines repliceren van één abonnement naar een andere voor herstel na noodgevallen** | Ondersteund binnen dezelfde Azure Active Directory-tenant.
**Virtuele machines migreren tussen regio's binnen de ondersteunde geografische clusters (binnen en tussen abonnementen)** | Ondersteund binnen dezelfde Azure Active Directory-tenant.
**Migreren van VM's binnen dezelfde regio** | Wordt niet ondersteund.

## <a name="region-support"></a>Ondersteuning voor regio

U kunt repliceren en herstellen van virtuele machines tussen elke twee regio's binnen hetzelfde geografische cluster. Geografische clusters worden gedefinieerd gegevenslatentie en -soevereiniteit waarmee u rekening moet houden.


**Geografische cluster** | **Azure-regio's**
-- | --
Amerika | Canada-Oost, Canada-midden, Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2, VS-midden, Noord-centraal VS
Europa | UK-West, UK-Zuid, Noord-Europa, West-Europa, Frankrijk-centraal, Frankrijk-Zuid
Azië | Zuid-India, centraal-India, Zuidoost-Azië, Oost-Azië, Japan-Oost, Japan-West, Korea centraal, Korea Zuid
Australië   | Australië-Oost, Australië-Zuidoost, Australië-midden, Australië centraal 2
Azure Government    | VS (overheid) Virginia, VS (overheid)-Iowa, VS (overheid)-Arizona, VS (overheid)-Texas, VS DOD Oost, VS DOD-centraal
Duitsland | Duitsland-centraal, Duitsland-Noordoost
China | China-Oost, China-Noord, China North2, China-Oost 2

>[!NOTE]
>
> - Voor **Brazilië-Zuid**, u kunt repliceren en failover uitvoeren naar deze regio's: Zuid-centraal VS, West-Centraal VS, VS-Oost, VS-Oost 2, VS-West, VS-West 2 en Noord-centraal VS.
> - Brazilië-Zuid kan alleen worden gebruikt als een regio van de gegevensbron waarin virtuele machines kunnen repliceren met Site Recovery. Deze kan niet fungeren als een doelregio. Dit is vanwege latentieproblemen met vanwege geografische afstand.
> - U kunt werken binnen regio's waarvoor u de juiste toegang hebt.
> - Als de regio waarin u wilt maken van een kluis niet wordt weergegeven, moet u dat uw abonnement heeft toegang tot resources in die regio maken.
> - Als een regio binnen een geografische cluster niet wordt weergegeven wanneer u replicatie inschakelt, zorg er dan voor dat uw abonnement heeft machtigingen voor het maken van virtuele machines in deze regio.



## <a name="cache-storage"></a>Cacheopslag

Deze tabel bevat een overzicht van ondersteuning voor de cache-opslagaccount gebruikt door Site Recovery tijdens de replicatie.

**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
Algemeen gebruik V2 storage-accounts (warme en koude laag) | Wordt niet ondersteund. | De beperking bestaat voor cacheopslag omdat transactiekosten voor V2 aanzienlijk hoger is dan V1-opslagaccounts zijn.
Azure Storage-firewalls voor virtuele netwerken  | Ondersteund | Als u cacheopslagaccount firewall is ingeschakeld of doel-opslagaccount gebruikt, controleert u of u ['Vertrouwde Microsoft-services toestaan'](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).


## <a name="replicated-machine-operating-systems"></a>Gerepliceerde machine-besturingssystemen

Site Recovery biedt ondersteuning voor replicatie van virtuele Azure-machines met de besturingssystemen die worden vermeld in deze sectie.

### <a name="windows"></a>Windows

**Besturingssysteem** | **Details**
--- | ---
Windows Server 2019 |
Windows Server 2016  | Server Core, Server met Bureaubladervaring
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | Met SP1 of hoger

#### <a name="linux"></a>Linux

**Besturingssysteem** | **Details**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6  
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6
Ubuntu 14.04 LTS-Server | [Versies van de ondersteunde kernel](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [Ondersteunde kernelversie](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Ubuntu-servers met behulp van verificatie op basis van wachtwoorden en aanmelding en de cloud-init-pakket voor het configureren van virtuele machines, cloud mogelijk op basis van wachtwoorden aanmelding is uitgeschakeld op failover (afhankelijk van de configuratie cloudinit). Aanmelding op basis van wachtwoorden kan opnieuw worden ingeschakeld op de virtuele machine door de fabrieksinstellingen van het wachtwoord van de ondersteuning voor > probleemoplossing > menu instellingen (van de failover VM in Azure portal.
Debian 7 | [Versies van de ondersteunde kernel](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Versies van de ondersteunde kernel](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1,SP2,SP3. [(Ondersteunde kernelversie versies)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Upgrade van de machines repliceren van SP3 naar SP4 wordt niet ondersteund. Als een gerepliceerde virtuele machine is bijgewerkt, moet u replicatie uitschakelen en weer inschakelen replicatie na de upgrade.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 <br/><br/> De Red Hat compatibele kernel- of Unbreakable Enterprise Kernel versie 3 (UEK3) uitgevoerd.


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde versies van Ubuntu kernel voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
14.04 TNS | 9.23 | 3.13.0-24-Generic naar 3.13.0-165-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1037-azure |
14.04 TNS | 9.22 | 3.13.0-24-Generic naar 3.13.0-164-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1036-azure |
14.04 TNS | 9.21 | 3.13.0-24-Generic naar 3.13.0-163-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1035-azure |
14.04 TNS | 9.20 | 3.13.0-24-Generic naar 3.13.0-161-generic,<br/>3.16.0-25-Generic naar 3.16.0-77-generic,<br/>3.19.0-18-Generic naar 3.19.0-80-generic,<br/>4.2.0-18-Generic naar 4.2.0-42-generic,<br/>4.4.0-21-Generic naar 4.4.0-138-generic,<br/>4.15.0-1023-Azure naar 4.15.0-1030-azure |

||| 16.04 LTS | 9.23 | 4.4.0-21-Generic naar 4.4.0-142-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-45-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1037-azure| 16.04 LTS | 9.22 | 4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-43-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1036-azure| 16.04 LTS | 9.21 | 4.4.0-21-Generic naar 4.4.0-140-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-42-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1035-azure| 16.04 LTS | 9.20 | 4.4.0-21-Generic naar 4.4.0-138-generic,<br/>4.8.0-34-Generic naar 4.8.0-58-generic,<br/>4.10.0-14-algemeen tot 4.10.0-42-generic,<br/>4.11.0-13-Generic naar 4.11.0-14-generic,<br/>4.13.0-16-Generic naar 4.13.0-45-generic,<br/>4.15.0-13-Generic naar 4.15.0-38-generic<br/>4.11.0-1009-Azure naar 4.11.0-1016-azure,<br/>4.13.0-1005-Azure naar 4.13.0-1018-azure <br/>4.15.0-1012-Azure naar 4.15.0-1030-azure|

#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde kernelversies van Debian voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
Debian 7 | 9.19,9.20,9.21,9.22,9.23 | 3.2.0-4-AMD64 naar 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.20, 9.21,9.22, 9.23 | 3.16.0-4-amd64 to 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 to 4.9.0-0.bpo.8-amd64 |
Debian 8 | 9.19 | 3.16.0-4-amd64 to 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 to 4.9.0-0.bpo.7-amd64 |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Ondersteunde versies van SUSE Linux Enterprise Server 12 kernel voor virtuele machines van Azure

**Release** | **De versie van de Mobility-service** | **Kernelversie** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.23 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.101-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.79-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.22 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.21 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.72-default |
SUSE Linux Enterprise Server 12 (SP3 SP1, SP2) | 9.20 | SP1 3.12.49-11-default naar 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default naar 3.12.74-60.64.107-default</br></br> SP2 4.4.21-69-default naar 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default naar 4.4.121-92.98-default</br></br>SP3 4.4.73-5-default naar 4.4.162-94.69-default |


## <a name="replicated-machines---linux-file-systemguest-storage"></a>Gerepliceerde machines - Linux-systeem/Gast bestandsopslag

* -Bestandssystemen: ext3, ext4, ReiserFS (alleen Suse Linux Enterprise Server), XFS, BTRFS
* Volume manager: LVM2
* Multipath-software: Apparaten toewijzen


## <a name="replicated-machines---compute-settings"></a>Gerepliceerde machines - compute-instellingen

**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
Grootte | Elke Azure-VM-grootte met ten minste 2 CPU-kernen en 1 GB RAM-geheugen | Controleer of [Azure VM-groottes](../virtual-machines/windows/sizes.md).
Beschikbaarheidssets | Ondersteund | Als u replicatie voor een Azure VM met de standaardopties inschakelt, wordt een beschikbaarheidsset op basis van de bron regio-instellingen automatisch gemaakt. U kunt deze instellingen wijzigen.
Beschikbaarheidszones | Ondersteund |
Hybrid Use Benefit (HUB) | Ondersteund | Als de bron-VM een HUB-licentie ingeschakeld, een testfailover heeft of failover gebruikt VM ook de HUB-licentie.
Schaalsets van virtuele machines | Niet ondersteund |
Installatiekopieën van Azure-galerie - Microsoft gepubliceerd | Ondersteund | Als de virtuele machine wordt uitgevoerd op een ondersteund besturingssysteem wordt ondersteund.
Installatiekopieën van Azure-galerie - van derden gepubliceerd | Ondersteund | Als de virtuele machine wordt uitgevoerd op een ondersteund besturingssysteem wordt ondersteund.
Aangepaste installatiekopieën - van derden gepubliceerd | Ondersteund | Als de virtuele machine wordt uitgevoerd op een ondersteund besturingssysteem wordt ondersteund.
Virtuele machines gemigreerd met behulp van Site Recovery | Ondersteund | Als een VM met VMware of fysieke machine is gemigreerd naar Azure met Site Recovery, moet u de oudere versie van mobiliteitsservice wordt uitgevoerd op de computer verwijderen en start de machine opnieuw voordat u naar een andere Azure-regio te repliceren.

## <a name="replicated-machines---disk-actions"></a>Gerepliceerde machines - schijf-acties

**Actie** | **Details**
-- | ---
Grootte van de schijf op gerepliceerde virtuele machine wijzigen | Ondersteund
Een schijf toevoegen aan een gerepliceerde virtuele machine | Wordt niet ondersteund.<br/><br/> U moet aan het uitschakelen van replicatie voor de virtuele machine, de schijf toevoegen en schakelt u de replicatie opnieuw.

## <a name="replicated-machines---storage"></a>Gerepliceerde machines - opslag

Deze tabel samengevat ondersteuning voor de Azure VM-besturingssysteem schijf, de gegevensschijf en de tijdelijke schijf.

- Het is belangrijk om te bekijken van de limieten voor VM-schijf en de doelen voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) en [Windows](../virtual-machines/windows/disk-scalability-targets.md) virtuele machines om eventuele prestatieproblemen te voorkomen.
- Als u met de standaardinstellingen implementeert, maakt Site Recovery automatisch schijven en opslagaccounts op basis van de broninstellingen.
- Als u wilt aanpassen, controleert u of dat u de richtlijnen volgen.

**Onderdeel** | **Ondersteuning** | **Details**
--- | --- | ---
Maximale grootte van de Besturingssysteemschijf | 2048 GB | [Meer informatie](../virtual-machines/windows/managed-disks-overview.md) over VM-schijven.
Tijdelijke schijf | Niet ondersteund | De tijdelijke schijf is altijd uitgesloten van replicatie.<br/><br/> Geen permanente gegevens niet worden opgeslagen op de tijdelijke schijf. [Meer informatie](../virtual-machines/windows/managed-disks-overview.md).
Maximale grootte van de gegevensschijf | 4095 GB |
Maximum aantal voor gegevens-schijf | Maximaal 64, in overeenstemming met de ondersteuning voor een specifieke Azure-VM-grootte | [Meer informatie](../virtual-machines/windows/sizes.md) over VM-grootten.
Veranderingssnelheid van gegevens-schijf | Maximaal 10 MBps per schijf voor premium-opslag. Maximaal 2 MBps per schijf voor Standard-opslag. | Als de gemiddelde gegevenswijzigingssnelheid op de schijf is voortdurend hoger is dan de maximale, replicatie wordt niet actueel.<br/><br/>  Echter, als de maximale sporadisch wordt overschreden, replicatie kan bijwonen, maar ziet u mogelijk enigszins vertraagd herstelpunten.
Gegevensschijf - standard storage-account | Ondersteund |
Gegevensschijf - premium storage-account | Ondersteund | Als een virtuele machine schijven die zijn verdeeld over premium en standard storage-accounts bevat, kunt u een ander doel-opslagaccount voor elke schijf, zodat u de dezelfde opslagconfiguratie hebt in de doelregio.
Beheerde schijf - standaard | Ondersteund in Azure-regio's waar Azure Site Recovery wordt ondersteund. |
Beheerde schijf - premium | Ondersteund in Azure-regio's waar Azure Site Recovery wordt ondersteund. |
Standard - SSD | Ondersteund |
Redundantie | LRS en GRS worden ondersteund.<br/><br/> ZRS wordt niet ondersteund.
Opslag van koude en warme | Niet ondersteund | VM-schijven worden niet ondersteund op de opslag van koude en warme
Opslagruimten | Ondersteund |
Versleuteling-at-rest (SSE) | Ondersteund | SSE is de standaardinstelling op storage-accounts.   
Azure Disk Encryption (ADE) voor Windows-besturingssysteem | VM's zijn ingeschakeld voor [versleuteling met Azure AD-app](https://aka.ms/ade-aad-app) worden ondersteund |
Azure Disk Encryption (ADE) voor Linux-besturingssysteem | Niet ondersteund |
Hot toevoegen of verwijderen-schijf | Niet ondersteund | Als u toevoegen of verwijderen van de gegevensschijf op de virtuele machine, moet u replicatie uitschakelen en inschakelen van replicatie opnieuw voor de virtuele machine.
Schijf uitsluiten | Ondersteuning. U moet gebruiken [Powershell](azure-to-azure-exclude-disks.md) te configureren. |  Tijdelijke schijven niet standaard opgenomen.
Storage Spaces Direct  | Ondersteund voor crash-consistente herstelpunten. Toepassing consistente herstelpunten worden niet ondersteund. |
Scale-out bestandsserver  | Ondersteund voor crash-consistente herstelpunten. Toepassing consistente herstelpunten worden niet ondersteund. |
LRS | Ondersteund |
GRS | Ondersteund |
RA-GRS | Ondersteund |
ZRS | Niet ondersteund |
Opslag van koude en warme | Niet ondersteund | Schijven van virtuele machines worden niet ondersteund op de opslag van koude en warme
Azure Storage-firewalls voor virtuele netwerken  | Ondersteund | Als het beperken van toegang tot het virtuele netwerk naar storage-accounts, schakelt u [vertrouwde Microsoft-services toestaan](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).
Opslagaccounts voor algemeen gebruik V2 (zowel warme en koude laag) | Nee | Toename van de transactie kosten vergeleken aanzienlijk met algemeen gebruik V1-opslagaccounts

>[!IMPORTANT]
> Om prestatieproblemen te voorkomen, zorg ervoor dat u volgt VM schijf schaalbaarheids- en prestatiedoelen voor [Linux](../virtual-machines/linux/disk-scalability-targets.md) of [Windows](../virtual-machines/windows/disk-scalability-targets.md) VM's. Als u de standaardinstellingen gebruikt, maakt Site Recovery de vereiste schijven en opslagaccounts, op basis van de configuratie van de gegevensbron. Als u uw eigen instellingen selecteert en aanpassen, volgt u de schijf schaalbaarheids- en prestatiedoelen voor uw bron-VM's.

## <a name="limits-and-data-change-rates"></a>Limieten en gegevens wijzigen tarieven

De volgende tabel geeft een overzicht van Site Recovery-limieten.

- Deze limieten zijn gebaseerd op onze tests maar natuurlijk hebben geen betrekking op alle mogelijke toepassings-i/o-combinaties.
- Werkelijke resultaten kunnen variëren op basis van u app i/o-combinatie.
- Er zijn twee beperkingen rekening houden met per schijf gegevens verloop en verloop per gegevens van virtuele machines.
- Een voorbeeld: als we een P20 Premium-schijf gebruiken, zoals beschreven in de onderstaande tabel, Site Recovery kan verwerken 5 MB/s van het verloop per schijf, met op maximaal vijf dergelijke schijven per virtuele machine, vanwege de limiet van 25 MB/s totale verloop per virtuele machine.

**Doelopslag** | **Gemiddelde bronschijf i/o** |**Gemiddeld gegevensverloop van bronschijf** | **Totale gegevensverloop van bronschijf per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 8 kB  | 2 MB/s | 168 GB per schijf
Premium P10 of P15 schijf | 16 kB | 4 MB/s |  336 GB per schijf
Premium P10 of P15 schijf | 32 kB of meer | 8 MB/s | 672 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 8 kB    | 5 MB/s | 421 GB per schijf
Premium P20 of P30 of P40 of P50 schijf | 16 kB of meer |20 MB/s | 1684 GB per schijf
## <a name="replicated-machines---networking"></a>Gerepliceerde machines - netwerken
**Instelling** | **Ondersteuning** | **Details**
--- | --- | ---
NIC | Maximum aantal dat wordt ondersteund voor een specifieke Azure-VM-grootte | NIC's worden gemaakt wanneer de virtuele machine is gemaakt tijdens de failover.<br/><br/> Het aantal NIC's op de failover-VM, is afhankelijk van het aantal NIC's op de bron-VM wanneer replicatie is ingeschakeld. Als u toevoegen of verwijderen van een NIC nadat replicatie is ingeschakeld, wordt dit geen invloed op het aantal NIC's op de gerepliceerde virtuele machine na een failover.
Internet Load Balancer | Ondersteund | Koppel de vooraf geconfigureerde load balancer met behulp van een Azure Automation-script van een herstelplan te gaan.
Een interne Load balancer | Ondersteund | Koppel de vooraf geconfigureerde load balancer met behulp van een Azure Automation-script van een herstelplan te gaan.
Openbaar IP-adres | Ondersteund | Een bestaande openbare IP-adres koppelen aan de NIC. Of maak een openbaar IP-adres en koppel deze aan de NIC met behulp van een Azure Automation-script van een herstelplan te gaan.
NSG op de NIC | Ondersteund | De NSG koppelen aan de NIC met behulp van een Azure Automation-script van een herstelplan te gaan.
NSG voor subnet | Ondersteund | De NSG koppelen aan het subnet met behulp van een Azure Automation-script van een herstelplan te gaan.
Gereserveerd (statisch) IP-adres | Ondersteund | Als de NIC op de bron-VM een statisch IP-adres heeft en het doelsubnet de hetzelfde IP-adres is, deze toegewezen aan de failover VM.<br/><br/> Als het doelsubnet niet hetzelfde IP-adres beschikbaar is, wordt een van de beschikbare IP-adressen in het subnet is gereserveerd voor de virtuele machine.<br/><br/> U kunt ook opgeven een vast IP-adres en subnetmasker in **gerepliceerde items** > **instellingen** > **berekening en netwerk**  >  **Netwerkinterfaces**.
Dynamisch IP-adres | Ondersteund | Als de NIC op de broncomputer dynamische IP heeft-adressering, is het ook de NIC uitvoert voor virtuele machine op de standaard dynamisch.<br/><br/> U kunt dit wijzigen naar een vaste IP-adres indien nodig.
Traffic Manager     | Ondersteund | U kunt Traffic Manager vooraf configureren zodat verkeer wordt doorgestuurd naar het eindpunt in de bronregio op gezette tijden en naar het eindpunt in de doelregio in het geval van failover.
Azure DNS | Ondersteund |
Aangepaste DNS  | Ondersteund |
Niet-geverifieerde proxy | Ondersteund | [Meer informatie]. (site-recovery-azure-to-azure-networking-guidance.md)   
Geverifieerde Proxy | Niet ondersteund | Als de virtuele machine van een geverifieerde proxyserver voor de uitgaande connectiviteit gebruikmaakt, kan niet worden gerepliceerd met behulp van Azure Site Recovery.    
Site-naar-site-VPN-verbinding met on-premises<br/><br/>(met of zonder ExpressRoute)| Ondersteund | Zorg ervoor dat de Udr en nsg's zodanig dat het Site recovery-verkeer niet wordt doorgestuurd naar on-premises zijn geconfigureerd. [Meer informatie](site-recovery-azure-to-azure-networking-guidance.md)    
VNET-naar-VNET-verbinding | Ondersteund | [Meer informatie](site-recovery-azure-to-azure-networking-guidance.md)  
Service-eindpunten voor virtueel netwerk | Ondersteund | Zorg ervoor dat de vertrouwde Microsoft-services toegang hebben tot het opslagaccount als beperking van toegang tot het virtuele netwerk naar storage-accounts.
Versneld netwerken | Ondersteund | Versneld netwerken moet zijn ingeschakeld op de bron-VM. [Meer informatie](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Volgende stappen
- Lezen [netwerken richtlijnen](site-recovery-azure-to-azure-networking-guidance.md) voor het repliceren van virtuele Azure-machines.
- Herstel na noodgevallen door implementeren [virtuele Azure-machines repliceren](site-recovery-azure-to-azure.md).
