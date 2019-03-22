---
title: Ondersteuningsmatrix voor Microsoft Azure Backup server en System Center DPM
description: In dit artikel bevat een overzicht van Azure Backup-ondersteuning wanneer u Microsoft Azure Backup Server of System Center DPM back-up on-premises en Azure VM-resources.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 704bb409d2b21e2ae258dbb2d627b1c088d80db7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57860921"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-server-or-system-center-dpm"></a>Ondersteuningsmatrix voor back-up met Microsoft Azure Backup-Server of System Center DPM

U kunt de [Azure Backup-service](backup-overview.md) back-up on-premises computers en werkbelastingen en virtuele Azure-machines (VM's). In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen voor back-ups van virtuele machines met behulp van Microsoft Azure Backup-Server (MABS) of System Center Data Protection Manager (DPM) en Azure Backup.

## <a name="about-dpmmabs"></a>Over DPM/MABS

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807) is een bedrijfsoplossing die configureert en beheert back-up en herstel van enterprise-machines en gegevens vereenvoudigt. Het onderdeel van de [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing) reeks producten.

MABS is een serverproduct dat kan worden gebruikt om back-up van fysieke on-premises servers, virtuele machines en apps die daarop worden uitgevoerd.

MABS is gebaseerd op System Center DPM en biedt een vergelijkbare functionaliteit met een paar verschillen:
- Er is geen System Center-licentie is vereist voor MABS worden uitgevoerd.
- Azure biedt voor MABS en DPM back-upopslag op lange termijn. Bovendien kunt DPM u back-up van gegevens voor langetermijnopslag op tape. MABS biedt niet deze functionaliteit.

Downloaden van MABS uit de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=57520). Deze kan worden uitgevoerd-on-premises of op een Azure-VM.

DPM en MABS ondersteuning voor back-ups van een groot aantal apps en -server en client-besturingssystemen. Ze bieden meerdere back-upscenario's:

- U kunt back-up op het machineniveau van de met systeemstatus of bare metal-back-up.
- U kunt back-up van bepaalde volumes, shares, mappen en bestanden.
- U kunt een back-up specifieke apps maken met behulp van geoptimaliseerde app-compatibele instellingen.

## <a name="dpmmabs-backup"></a>DPM/MABS back-up

Back-up met DPM/MABS en Azure Backup werkt als volgt:

1. DPM/MABS-beveiligingsagent is geïnstalleerd op elke machine die wordt een back-up.
1. Machines en apps back-ups naar de lokale opslag op DPM/MABS.
1. De Microsoft Azure Recovery Services agent (MARS) is geïnstalleerd op de DPM-server/MABS.
1. De MARS-agent back-ups van de DPM/MABS-schijven naar een back-Recovery Services-kluis in Azure met behulp van Azure Backup.

Voor meer informatie:

- [Meer informatie](backup-architecture.md#architecture-back-up-to-dpmmabs) over de architectuur van MABS.
- [Bekijk wat er wordt ondersteund](backup-support-matrix-mars-agent.md) voor de MARS-agent.

## <a name="supported-scenarios"></a>Ondersteunde scenario's 

**Scenario** | **Agent** | **Locatie**
--- | --- | ---
**Een back-up on-premises machines/werkbelastingen** | DPM/MABS-beveiligingsagent op de machines die u back wilt-up wordt uitgevoerd.<br/><br/> De MARS-agent op DPM/MABS-server. | DPM/MABS moet on-premises worden uitgevoerd.
**Back-ups van Azure-VM's / workloads** | DPM/MABS-beveiligingsagent op beveiligde computer.<br/><br/> De MARS-agent op DPM/MABS-server. | DPM/MABS moet worden uitgevoerd op een Azure-VM.

## <a name="supported-deployments"></a>Ondersteunde implementaties

DPM/MABS kan worden geïmplementeerd, zoals samengevat in de volgende tabel.

**Implementatie** | **Ondersteuning** | **Details**
--- | --- | ---
**On-premises geïmplementeerd** | Fysieke server<br/><br/>Hyper-V-VM<br/><br/> Virtuele VMware-machine | Als DPM/MABS als een VMware-VM is geïnstalleerd, wordt alleen back-ups van virtuele VMware-machines en werkbelastingen die worden uitgevoerd op deze virtuele machines.
**Geïmplementeerd als een Azure Stack-VM** | Alleen MABS | DPM kan niet worden gebruikt om back-up van virtuele machines van Azure Stack.
**Geïmplementeerd als een Azure-VM** | Beveiligt virtuele machines van Azure en werkbelastingen die worden uitgevoerd op deze virtuele machines | DPM/MABS worden uitgevoerd in Azure kan geen back-up on-premises computers.


## <a name="supported-mabs-and-dpm-operating-systems"></a>Ondersteunde besturingssystemen voor MABS en DPM

Azure Backup een back-up DPM/MABS-instanties die een van de volgende besturingssystemen worden uitgevoerd. Besturingssystemen moet worden uitgevoerd, de nieuwste servicepacks en updates.

**Scenario** | **DPM/MABS** 
--- | --- 
**MABS op een Azure-VM** | Windows Server 2012 R2.<br/><br/> Windows 2016 Datacenter.<br/><br/> Windows 2019 Datacenter.<br/><br/> Het is raadzaam dat u met een installatiekopie van de marketplace begint.<br/><br/> Minimale A2 Standard met twee kernen en 3,5 GB aan RAM-geheugen. 
**DPM op een Azure-VM** | System Center 2012 R2 met Update 3 of hoger.<br/><br/> Windows-besturingssysteem als [vereist voor System Center](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).<br/><br/> Het is raadzaam dat u met een installatiekopie van de marketplace begint.<br/><br/> Minimale A2 Standard met twee kernen en 3,5 GB aan RAM-geheugen. 
**MABS on-premises** | 64-bits besturingssystemen ondersteund:<br/><br/> MABS v3 en hoger: WindowsServer (Standard, Datacenter, Essentials) 2019. <br/><br/> MABS v2 en hoger: WindowsServer 2016 (Standard, Datacenter, Essentials).<br/><br/> Alle MABS-versies: Windows Server 2012 R2, Windows Server 2012 (Standard, Datacenter, Foundation).<br/><br/>Alle MABS-versies: Windows Storage Server 2012 R2, Windows Server 2012 (Standard, Workgroup).
**DPM on-premises** | Fysieke server/Hyper-V virtuele machine: System Center 2012 SP1 of hoger.<br/><br/> Virtuele VMware-machine: System Center 2012 R2 met Update 5 of hoger. 



## <a name="management-support"></a>Ondersteuning

**Probleem** | **Details**
--- | ---
**Installatie** | Installeer DPM/MABS op een machine met één doel.<br/><br/> Installeer DPM/MABS niet op een domeincontroller, op een computer met de installatie van de rol toepassingsserver, op een computer die Microsoft Exchange Server of System Center Operations Manager wordt uitgevoerd of op een clusterknooppunt.<br/><br/> [Alle DPM-systeemvereisten controleren](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).
**Domein** | DPM/MABS moet worden toegevoegd aan een domein. Eerst installeren en vervolgens DPM/MABS toevoegen aan een domein. DPM/MABS verplaatsen naar een nieuw domein na implementatie wordt niet ondersteund.
**Storage** | Moderne back-upopslag (MB/s) wordt ondersteund vanaf DPM 2016/MABS v2 en hoger. Het is niet beschikbaar voor MABS v1.
**MABS-upgrade** | U kunt rechtstreeks MABS v3 installeren of upgraden naar MABS v3 van MABS v2. [Meer informatie](backup-azure-microsoft-azure-backup.md#upgrade-mabs).
**MABS verplaatsen** | MABS verplaatsen naar een nieuwe server behoudt de opslag wordt ondersteund als u MB/s.<br/><br/> De server moet dezelfde naam als de oorspronkelijke hebben. U kunt de naam niet wijzigen als u wilt behouden van dezelfde opslaggroep en de dezelfde MABS-database gebruiken voor het opslaan van gegevens herstelpunten.<br/><br/> U moet een back-up van de MAB-database omdat, moet u deze herstellen.


## <a name="mabs-support-on-azure-stack"></a>MABS-ondersteuning in Azure Stack

U kunt MABS op een virtuele machine van Azure Stack kunt implementeren, zodat u back-up van Azure Stack-VM's en workloads vanaf één locatie beheren kunt.

**Onderdeel** | **Details**
--- | --- 
**MABS op Azure Stack VM** | Ten minste het formaat van A2. U wordt aangeraden dat u beginnen met een installatiekopie van Windows Server 2012 R2 of Windows Server 2016 op Azure Marketplace.<br/><br/> Installeer geen iets anders op de VM MABS.
**MABS-opslag** | Gebruik een afzonderlijk opslagaccount voor de VM MABS. De MARS-agent die wordt uitgevoerd op MABS, heeft tijdelijke opslag voor de locatie van een cache en voor het opslaan van gegevens hersteld vanuit de cloud.
**MABS-opslaggroep** | De grootte van de MAB-opslaggroep wordt bepaald door het aantal en de grootte van schijven die zijn gekoppeld aan de VM MABS. Elke Azure Stack-VM-grootte heeft een maximale aantal schijven. A2 is bijvoorbeeld vier schijven.
**MABS bewaren** | Geen back-ups van gegevens op de lokale MABS-schijven niet meer dan vijf dagen bewaren.
**MABS schalen omhoog** | Als u wilt schalen van uw implementatie, kunt u de grootte van de VM MABS verhogen. U kunt bijvoorbeeld wijzigen van A naar D-serie.<br/><br/> U kunt er ook voor zorgen dat u bent offloading van gegevens met back-up naar Azure regelmatig. Indien nodig, kunt u extra MABS-servers kunt implementeren.
**.NET framework voor MABS** | De VM MABS moet .NET Framework 3.3 SP1 of hoger is geïnstalleerd.
**MABS-domein** | De VM MABS moet worden toegevoegd aan een domein. Een gebruiker met beheerdersbevoegdheden moet MABS installeren op de virtuele machine.
**Azure Stack VM gegevensback-up** | U kunt back-up van bestanden, mappen en apps.
**Ondersteunde back-up** | Deze besturingssystemen worden ondersteund voor virtuele machines die u wilt back-up:<br/><br/> Windows Server semi-Annual-kanaal (Datacenter, Enterprise, Standard)<br/><br/> Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2
**SQL Server-ondersteuning voor Azure Stack-VM 's** | Back up SQL Server 2016, SQL Server 2014, SQL Server 2012 SP1.<br/><br/> Back-up en herstellen van een database.
**SharePoint-ondersteuning voor Azure Stack-VM 's** | SharePoint 2016, SharePoint 2013, SharePoint 2010.<br/><br/> Back-up en herstellen van een farm, database, front-end en webserver.
**Netwerkvereisten voor back-ups van virtuele machines** | Alle virtuele machines in Azure Stack-werkbelasting moeten deel uitmaken van hetzelfde virtuele netwerk en behoren tot hetzelfde abonnement.

## <a name="dpmmabs-networking-support"></a>DPM/MABS netwerkondersteuning

### <a name="url-access"></a>URL-toegang

De DPM-server/MABS moet toegang hebben tot deze URL's:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="dpmmabs-connectivity-to-azure-backup"></a>DPM/MABS-verbinding met Azure Backup

Verbinding met de Azure Backup-service is vereist voor back-ups te laten functioneren en de Azure-abonnement actief mag zijn. De volgende tabel ziet u het gedrag als deze twee dingen niet plaats.

**MABS naar Azure** | **Abonnement** | **Back-up/herstellen** 
--- | --- | --- 
Verbonden | Actief | Back-up naar schijf voor DPM/MABS.<br/><br/> Back-up naar Azure.<br/><br/> Terugzetten vanaf schijf.<br/><br/> Herstellen van Azure.
Verbonden | Verlopen/de inrichting ongedaan gemaakt | Er is geen back-up op schijf of op Azure.<br/><br/> Als het abonnement is verlopen, kunt u herstellen vanaf schijf of Azure.<br/><br/> Als het abonnement uit bedrijf genomen is, kunt u niet kunt herstellen vanaf schijf of Azure. De Azure-herstelpunten worden verwijderd.
Er is geen verbinding meer dan 15 dagen | Actief | Er is geen back-up op schijf of op Azure.<br/><br/> U kunt herstellen vanaf schijf of Azure.
Er is geen verbinding meer dan 15 dagen | Verlopen/de inrichting ongedaan gemaakt | Er is geen back-up op schijf of op Azure.<br/><br/> Als het abonnement is verlopen, kunt u herstellen vanaf schijf of Azure.<br/><br/> Als het abonnement uit bedrijf genomen is, kunt u niet kunt herstellen vanaf schijf of Azure. De Azure-herstelpunten worden verwijderd.

## <a name="dpmmabs-storage-support"></a>Ondersteuning voor DPM/MABS-opslag

Gegevens die de back-up naar DPM/MABS worden opgeslagen op de lokale schijfopslag. 

**Storage** | **Details**
--- | ---
**MBS** | Moderne back-upopslag (MB/s) wordt ondersteund vanaf DPM 2016/MABS v2 en hoger. Het is niet beschikbaar voor MABS v1.
**MABS-opslag op virtuele Azure-machine** | Gegevens worden opgeslagen op Azure-schijven die zijn gekoppeld aan de DPM/MABS-VM, en die worden beheerd in DPM/MABS. Het aantal schijven dat kan worden gebruikt voor DPM/MABS-opslaggroep wordt beperkt door de grootte van de virtuele machine.<br/><br/> A2 VM: 4-schijven. A3 VM: 8 schijven. A4 VM: 16 schijven, met een maximale grootte van 1 TB voor elke schijf. Hiermee bepaalt u het totale aantal back-upopslag die beschikbaar is.<br/><br/> De hoeveelheid gegevens die u kunt back-up, is afhankelijk van het aantal en de grootte van de gekoppelde schijven.
**MABS het bewaren van gegevens op Azure VM** | U wordt aangeraden dat u gegevens gedurende één dag op de DPM/MABS-Azure-schijf bewaren en back-up van DPM/MABS naar de kluis voor een langere periode. U kunt dus een grotere hoeveelheid gegevens beveiligen door het offloaden van het back-up van Azure.


### <a name="modern-backup-storage-mbs"></a>Moderne back-upopslag (MB/s)
Van DPM 2016/MABS v2 (uitgevoerd op Windows Server 2016) en hoger, kunt u profiteren van modern backup storage (MB/s).

- MB/s back-ups worden opgeslagen op een schijf Resilient File System (ReFS).
- MB/s gebruikt ReFS blokklonering voor snellere back-ups en efficiënter gebruik van opslagruimte.
- Als u volumes aan de lokale DPM/MABS-opslaggroep toevoegt, kunt u ze configureren met de stationsletters. Vervolgens kunt u de werkbelasting opslag configureren op verschillende volumes.
- Als u beveiligingsgroepen aan back-ups naar DPM/MABS maakt, selecteert u het station dat u wilt gebruiken. U kan bijvoorbeeld opslaan van back-ups voor SQL of andere hoge IOPS een high-performance workloads station en opslaan van workloads die minder vaak op een lagere prestaties station worden ondersteund.


## <a name="supported-backups-to-mabs"></a>Ondersteunde back-ups naar MABS

De volgende tabel geeft een overzicht van wat kan worden back-ups op MABS van on-premises machines en virtuele Azure-machines.


**Een back-up maken** | **Versies** | **MABS** | **Details** |
--- | --- | --- | --- |
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(64-32-bits) | MABS v3, v2, V1 | On-premises. | Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund.<br/><br/> Volumes moeten ten minste 1 GB en NTFS. |
**Windows Server 2016 (Datacenter, Standard, not Nano)**<br/><br/> 32-64-bits | MABS v3, v2 | On-premises/Azure VM.| Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**Windows Server 2012 R2 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v3, v2, v1 | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VMprotection**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**WindowsServer 2012 met SP1 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v3, v2, v1<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**WindowsServer 2012 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v1 | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**Windows 2008 R2 met SP1 (Standard en Enterprise)**<br/><br/> 32-64-bits | Ondersteund door MABS v3, v2, v1.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. | On-premises/Azure VM. |   **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**Windows 2008 R2 (Standard en Enterprise)**<br/><br/> 32-64-bits | MABS v1. Voor MABS moet het besturingssysteem v2/v3 SP1 worden uitgevoerd. | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund. |
**WindowsServer 2008 met SP2**<br/><br/> 32-64-bits | MABS v1, v2, v3 | Alleen v1 wordt ondersteund bij MABS wordt geïmplementeerd als een on-premises fysieke computer of een Hyper-V-VM.<br/><br/> MABS v1, 2, 3 wordt ondersteund bij MABS wordt geïmplementeerd als een VMware-VM.<br/><br/> Niet ondersteund voor MABS die worden uitgevoerd op virtuele Azure-machine. | Volume/share/map/bestand. systeem-status/bare metal. |
**Windows Storage Server 2008** | MABS v1, v2, v3 | MABS als on-premises fysieke server/Hyper-V virtuele machine. <br/><br/> Niet ondersteund voor MABS die worden uitgevoerd op virtuele Azure-machine. | Volume/share/map/bestand. systeem-status/bare metal.
**SQL Server 2017** | MABS v3 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund. |
**SQL Server 2016/2016 met SP1** | MABS v3, v2 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund. |
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | MABS v3, v2, v1 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund. |
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | MABS v3, v2, V1 | On-premises. | Back-up zelfstandige Exchange server, database onder een DAG.<br/><br/> Postvak, postvakdatabase onder een DAG herstellen.<br/><br/> ReFS niet wordt ondersteund.<br/><br/> Back-up niet-gedeelde schijfclusters.<br/><br/> Back-up van Exchange Server die is geconfigureerd voor continue replicatie. |
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | MABS v3, v2, v1 | On-premises/Azure VM. | Back-up-farm, front-endwebserver.<br/><br/> Herstellen-farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, front-endwebserver.<br/><br/> U kunt geen back-up van een farm met behulp van SQL Server AlwaysOn voor inhoudsdatabases. |
**Hyper-V in WindowsServer 2016**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (with SP1)** | MABS v3, v2, v1 | On-premises. | **MAB-agent op Hyper-V-host**: Back-up volledige VM's en gegevensbestanden van de host. Back-up van virtuele machines met lokale opslag, virtuele machines in een cluster met CSV-opslag, virtuele machines met SMB file server storage.<br/><br/> **MAB-agent in Gast-VM**: Back-up van workloads die worden uitgevoerd op de virtuele machine. CSV's.<br/><br/> **Recovery**: Virtuele machine, herstel op itemniveau van VHD/volume/mappen/bestanden.<br/><br/> **Virtuele Linux-machines**: Back-up wanneer de Hyper-V wordt uitgevoerd op Windows Server 2012 R2 en hoger. Herstel voor virtuele Linux-machines is voor de hele machine. |
**Virtuele VMware-machines: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1<br/><br/> MABS v1 moet updatepakket 1) | On-premises. | Back-up van virtuele VMware-machines op CSV's, NFS en SAN-opslag.<br/><br/> Volledige VM herstellen.<br/><br/> Back-up van Windows/Linux.<br/><br/> Herstel op itemniveau van de map/voor Windows-VM's alleen bestanden.<br/><br/> VMware vApps worden niet ondersteund.<br/><br/> Herstel voor virtuele Linux-machines is voor de hele machine. | 



## <a name="supported-backups-to-dpm"></a>Ondersteunde back-ups van DPM

De volgende tabel geeft een overzicht van wat kan een back-up aan DPM van de on-premises machines en virtuele Azure-machines.



**Een back-up maken** | **DPM** | **Details**
--- | --- | --- 
**Windows 10<br/>Windows 8.1<br/>Windows 8<br/>Windows 7**<br/><br/>(64-32-bits) | Alleen on-premises.<br/><br/> Voor back-ups van Windows 10 met DPM 2012 R2, het beste installeren [Update 11](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager). | Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes wordt ondersteund.<br/><br/> Volumes moeten ten minste 1 GB en NTFS.
**Windows Server 2016 (Datacenter, Standard, not Nano)**<br/><br/> 32-64-bits | On-premises/Azure VM.<br/><br/> DPM 2016.| Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> Ontdubbelde volumes wordt ondersteund.
**Windows Server 2012 R2 (Datacenter en Standard)**<br/><br/> 32-64-bits | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes met DPM 2012 R2 en hoger ondersteund.
**WindowsServer 2012 met SP1 (Datacenter en Standard)**<br/><br/> 32-64-bits | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes met DPM 2012 R2 en hoger ondersteund.
**WindowsServer 2012 (Datacenter en Standard)**<br/><br/> 32-64-bits |  On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.<br/><br/> Ontdubbelde volumes met DPM 2012 R2 en hoger ondersteund.
**Windows 2008 R2 met SP1 (Standard en Enterprise)**<br/><br/> 32-64-bits | On-premises/Azure VM.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. |   **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.
**Windows 2008 R2 (Standard en Enterprise)**<br/><br/> 32-64-bits | On-premises.<br/><br/> DPM kan niet worden geïnstalleerd als een VMware-VM.<br/><br/> DPM wordt uitgevoerd op een Azure-VM wordt niet ondersteund. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare metal.
**WindowsServer 2008 met SP2**<br/><br/> 32-64-bits | Alleen on-premises.<br/><br/> DPM wordt ondersteund bij het uitvoeren als een VMware-VM. Service wordt uitgevoerd als een fysieke server of Hyper-V-VM wordt niet ondersteund. | Volume/share/map/bestand. systeem-status/bare metal.
**Windows Storage Server 2008** | DPM on-premises uitgevoerd als een fysieke server of Hyper-V-VM. | Volume/share/map/bestand. systeem-status/bare metal.
**SQL Server 2017** | DPM SAC; DPM 2016 Update Roll up 5 of hoger uitvoeren.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2016 met SP1** | Niet ondersteund voor DPM 2012 R2; Ondersteund voor DPM SAC DPM 2016 Update Rollup 4 of hoger uitvoeren.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2016** | Niet ondersteund voor DPM 2012 R2. Ondersteund voor DPM SAC DPM 2016 vanaf updatepakket 2 en hoger.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | SQL Server 2014 met DPM 2012 R2 met updatepakket 4 en hoger.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | Voor Exchange 2016 moet DPM 2012 R2 updatepakket 9 of hoger.<br/><br/> On-premises | Back-up zelfstandige Exchange server, database onder een DAG.<br/><br/> Postvak, postvakdatabase onder een DAG herstellen.<br/><br/> ReFS niet wordt ondersteund.<br/><br/> Back-up niet-gedeelde schijfclusters.<br/><br/> Back-up van Exchange Server die is geconfigureerd voor continue replicatie.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | SharePoint 2016 op DPM 2016 en hoger.<br/><br/>On-premises/Azure VM. | Back-up-farm, front-endwebserver.<br/><br/> Herstellen-farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, front-endwebserver.<br/><br/> U kunt geen back-up van een farm met behulp van SQL Server AlwaysOn voor inhoudsdatabases.
**Hyper-V in WindowsServer 2016**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (with SP1)** | Hyper-V op 2016 ondersteund voor DPM 2016 en hoger.<br/><br/> On-premises. | **MAB-agent op Hyper-V-host**: Back-up volledige VM's en gegevensbestanden van de host. Back-up van virtuele machines met lokale opslag, virtuele machines in een cluster met CSV-opslag, virtuele machines met SMB file server storage.<br/><br/> **MAB-agent in Gast-VM**: Back-up van workloads die worden uitgevoerd op de virtuele machine. CSV's.<br/><br/> **Recovery**: Virtuele machine, herstel op itemniveau van VHD/volume/mappen/bestanden.<br/><br/> **Virtuele Linux-machines**: Back-up wanneer de Hyper-V wordt uitgevoerd op Windows Server 2012 R2 en hoger. Herstel voor virtuele Linux-machines is voor de hele machine.
**Virtuele VMware-machines: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1<br/><br/> DPM 2012 R2 moet System Center Update Rollup 1) <br/><br/>On-premises. | Back-up van virtuele VMware-machines op CSV's, NFS en SAN-opslag.<br/><br/> Volledige VM herstellen.<br/><br/> Back-up van Windows/Linux.<br/><br/> Herstel op itemniveau van de map/voor Windows-VM's alleen bestanden.<br/><br/> VMware vApps worden niet ondersteund.<br/><br/> Herstel voor virtuele Linux-machines is voor de hele machine.


- Houd er rekening mee dat geclusterde werkbelastingen een back-up door DPM/MABS in hetzelfde domein als de DPM/MABS of in een onderliggende/vertrouwd domein moet. 
- Verificatie via NTLM/certificaat kunt u back-up van gegevens in niet-vertrouwde domeinen of werkgroepen.



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](backup-architecture.md#architecture-back-up-to-dpmmabs) over de architectuur van MABS.
- [Beoordeling](backup-support-matrix-mars-agent.md) wat wordt ondersteund voor de MARS-agent.
- [Instellen van](backup-azure-microsoft-azure-backup.md) een MABS-server.
- [Instellen van DPM](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180).
