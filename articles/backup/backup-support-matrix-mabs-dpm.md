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
ms.openlocfilehash: 0544782763bebac42b4b68fda42cff80b18ff067
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56670731"
---
# <a name="support-matrix-for-backup-with-microsoft-azure-backup-serversystem-center-dpm"></a>Ondersteuningsmatrix voor de back-up met Microsoft Azure Backup Server/System Center DPM

U kunt de [Azure Backup-service](backup-overview.md) back-up on-premises computers en werkbelastingen en virtuele Azure-machines. In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen voor back-ups van machines met behulp van Microsoft Azure Backup-Server (MABS) of System Center Data Protection Manager (DPM) en Azure Backup.

## <a name="about-mabsdpm"></a>Over MABS/DPM

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview?view=sc-dpm-1807) is een bedrijfsoplossing die configureert, vereenvoudigt het uitvoeren en Hiermee beheert u back-up en herstel van enterprise-machines en gegevens. Het onderdeel van de [System Center](https://www.microsoft.com/cloud-platform/system-center-pricing) reeks producten.


Microsoft Azure Backup-Server (MABS) is een serverproduct dat kan worden gebruikt om back-up on-premises fysieke servers, virtuele machines (VM's) en apps die worden uitgevoerd op, en Azure-VM's.

- MABS is gebaseerd op System Center Data Protection Manager (DPM) en biedt een vergelijkbare functionaliteit met een paar verschillen:
    - Er is geen System Center-licentie is vereist voor MABS worden uitgevoerd.
    - Biedt op de lange termijn back-upopslag voor zowel MABS als DPM Azure. Bovendien kunt DPM u back-up van gegevens voor langetermijnopslag op tape. MABS biedt niet deze functionaliteit.
- Downloaden van MABS-server uit de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=57520). Deze kan worden uitgevoerd on-premises of op een Azure-VM in Azure.

DPM en MABS ondersteuning voor back-ups van een groot aantal apps en -server en client-besturingssystemen. Ze bieden meerdere back-upscenario's:
    - U kunt back-up op het machineniveau van de met systeemstatus of bare metal-back-up.
    - U kunt back-up van bepaalde volumes, shares, mappen en bestanden.
    - U kunt back-up specifieke apps met behulp van geoptimaliseerde app-compatibele instellingen.

## <a name="mabsdpm-backup"></a>MABS/DPM back-up

Back-up met MABS/DPM en Azure Backup werkt als volgt:

1. DPM/MABS beveiligingsagent is geïnstalleerd op elke computer die de back-ups. 
2. Machines/apps-back-ups naar de lokale opslag op DPM/MABS.
3. De Microsoft Azure Recovery Services agent (MARS) is geïnstalleerd op de DPM-server/MABS.
4. De MARS-agent back-ups van de DPM/MABS-schijven naar een back-Recovery Services-kluis in Azure met behulp van Azure Backup.

Voor meer informatie:
- [Meer informatie](backup-architecture.md#architecture-back-up-to-dpmmabs) over de architectuur van MABS.
- [Beoordeling](backup-support-matrix-mars-agent.md) wat wordt ondersteund voor de MARS-agent.

## <a name="supported-scenarios"></a>Ondersteunde scenario's 

**Scenario** | **Agent** | **Locatie**
--- | --- | ---
**Een back-up on-premises machines/werkbelastingen** | DPM/MABS-beveiligingsagent wordt uitgevoerd op de machines die u back wilt-up maken<br/><br/> De MARS-agent op MABS/DPM-server | MABS/DPM moet worden uitgevoerd op on-premises
**Back-ups van Azure-VM's / workloads** | DPM/MABS-beveiligingsagent op beveiligde computer<br/><br/> MARS-agent op MABS/DPM | DPM/MABS moet worden uitgevoerd op een Azure-VM.

## <a name="supported-deployments"></a>Ondersteunde implementaties

DPM/MABS kan worden geïmplementeerd, zoals samengevat in de tabel.

**Implementatie** | **Ondersteuning** | **Details**
--- | --- | ---
**On-premises geïmplementeerd** | Fysieke server<br/><br/>Hyper-V-VM<br/><br/> Virtuele VMware-machine | Als DPM/MABS is geïnstalleerd als een VMware-VM wordt alleen back-ups van virtuele VMware-machines en workloads die op deze virtuele machines worden uitgevoerd.
**Geïmplementeerd als een Azure Stack-VM** | Alleen MABS | DPM kan niet worden gebruikt om back-up van virtuele machines van Azure Stack.
**Geïmplementeerd als een Azure-VM** | Azure VM's en workloads die worden uitgevoerd op deze virtuele machines beveiligt. | MABS/DPM uitgevoerd in Azure kan geen back-up on-premises computers.


## <a name="supported-mabs-and-dpm-operating-systems"></a>Ondersteunde besturingssystemen voor MABS en DPM

Azure Backup een back-up MABS/DPM uit te voeren op een van de volgende besturingssystemen. Besturingssystemen moet worden uitgevoerd, de nieuwste servicepacks en updates.

**Scenario** | **MABS/DPM** 
--- | --- 
**MABS op een Azure-VM** | Windows Server 2012 R2<br/><br/> Windows 2016 Datacenter<br/><br/> Windows 2019 Datacenter<br/><br/> U wordt aangeraden dat u begint met een installatiekopie van de marketplace.<br/><br/> Minimale A2 Standard met twee kernen en 3,5 GB RAM-geheugen. 
**DPM op een Azure-VM** | System Center 2012 R2 met Update 3 of hoger.<br/><br/> Windows-besturingssysteem als [vereist voor System Center](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server).<br/><br/> U wordt aangeraden dat u begint met een installatiekopie van de marketplace.<br/><br/> Minimale A2 Standard met twee kernen en 3,5 GB RAM-geheugen. 
**MABS on-premises** | 64-bits besturingssystemen ondersteund:<br/><br/> MABS v3 en hoger: WindowsServer 2019 (Standard, Datacenter, Essentials) <br/><br/> MABS v2 en hoger: WindowsServer 2016 (Standard, Datacenter, Essentials)<br/><br/> Alle MABS-versies: Windows Server 2012 R2/2012 (Standard, Datacenter, Foundation)<br/><br/>Alle MABS-versies: Windows Storage Server 2012 R2/2012 (Standard/Workgroup)
**DPM on-premises** | Fysieke server/Hyper-V virtuele machine: System Center 2012 SP1 of hoger.<br/><br/> Virtuele VMware-machine: System Center 2012 R2 met Update 5 of hoger. 



## <a name="management-support"></a>Ondersteuning
**Probleem** | **Details**
--- | ---
**Installatie** | DPM/MABS moet worden geïnstalleerd op een machine met één doel.<br/><br/> Installeer DPM/MABS niet op een domeincontroller, op een computer met de installatie van de rol toepassingsserver, op een computer met Exchange Server of System Center Operations Manager of op een clusterknooppunt.<br/><br/> [Beoordeling](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1807#dpm-server) alle systeemvereisten voor DPM.
**Domein** | De DPM-server/MABS moet worden toegevoegd aan een domein. Eerst installeren en vervolgens DPM/MABS toevoegen aan een domein. DPM/MABS verplaatsen naar een nieuw domein na implementatie wordt niet ondersteund.
**Storage** | Moderne back-upopslag (MB/s) wordt ondersteund vanaf DPM 2016/MABS v2 en hoger. Het is niet beschikbaar voor MABS v1.
**MABS-upgrade** | U kunt rechtstreeks MABS v3 installeren of upgraden naar MABS v3 van MABS v2. [Meer informatie](backup-azure-microsoft-azure-backup.md#upgrade-mabs).
**MABS verplaatsen** | MABS verplaatsen naar een nieuwe server behoudt de opslag wordt ondersteund als u MB/s.<br/><br/> De server moet dezelfde naam als de oorspronkelijke hebben. U kunt de naam niet wijzigen als u wilt behouden van dezelfde opslaggroep en de dezelfde MABS-database gebruiken voor het opslaan van gegevens herstelpunten.<br/><br/> U moet een back-up van de MAB-database omdat, moet u deze herstellen.




## <a name="mabs-support-on-azure-stack"></a>MABS-ondersteuning in Azure Stack

U kunt MABS op een virtuele machine van Azure Stack kunt implementeren, zodat u back-up van Azure Stack-VM's en workloads vanaf één locatie beheren kunt.

**Onderdeel** | **Details**
--- | --- 
**MABS op Azure Stack VM** | Ten minste A2-grootte. U wordt aangeraden dat u beginnen met een installatiekopie van Windows Server 2012 R2 of Windows Server 2016 in Azure marketplace.<br/><br/> U mag iets anders niet installeren op de VM MABS.
**MABS-opslag** | Gebruik een afzonderlijk opslagaccount voor MAB-VM. De MARS-agent die wordt uitgevoerd op MABS, heeft tijdelijke opslag voor de locatie van een cache, en voor het opslaan van het herstellen van gegevens vanuit de cloud.
**MABS-opslaggroep** | De grootte van de MAB-opslaggroep wordt bepaald door het aantal en de grootte van schijven die zijn gekoppeld aan de VM MABS. Elke Azure Stack-VM-grootte heeft een maximale aantal schijven. A2 is bijvoorbeeld vier schijven.
**MABS bewaren** | U mag niet back-ups van gegevens op de lokale MABS-schijven meer dan vijf dagen bewaren.
**MABS schalen omhoog** | Als u wilt schalen van uw implementatie, kunt u de grootte van de VM MABS verhogen. Voorbeeld van A naar D-serie.<br/><br/> U kunt er ook voor zorgen bent u gegevens regelmatig offloading met back-up naar Azure, en indien nodig, kunt u extra MABS-servers implementeren.
.NET framework voor MABS | De VM MABS moet .NET Framework 3.3 SP1 of hoger is geïnstalleerd.
**MABS-domein** | De VM MABS moet worden toegevoegd aan een domein. Een gebruiker met beheerdersbevoegdheden moet MABS installeren op de virtuele machine.
**Azure Stack VM gegevensback-up** | U kunt back-up van bestanden, mappen en apps.
**Ondersteunde back-up** | Deze besturingssystemen worden ondersteund voor virtuele machines die u back wilt-up:<br/><br/> Windows Server semi-Annual-kanaal (Enterprise-Datacenter/Standard)<br/><br/> Windows Server 2016/2012 R2/2012/2008 R2.
**SQL-ondersteuning voor Azure Stack-VM 's** | Back-up van SQL Server 2016/2014/2012 SP1.<br/><br/> Back-up en herstel de database.
**SharePoint-ondersteuning voor Azure Stack-VM 's** | SharePoint 2016/2013/2010.<br/><br/> Back-up en herstellen van de farm, database, front-end, webserver.
**Netwerkvereisten voor back-ups van virtuele machines** | Alle virtuele machines in Azure Stack-werkbelasting moeten met hetzelfde VNet en tot hetzelfde abonnement behoren.


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
Verbonden | Actief | Back-up naar schijf voor DPM/MABS<br/><br/> Back-up naar Azure<br/><br/> Herstellen uit een schijf<br/><br/> Herstellen van Azure
Verbonden | Verlopen/de inrichting ongedaan gemaakt | Er is geen back-up op schijf of op Azure.<br/><br/> Als het abonnement is verlopen, kunt u herstellen vanaf schijf of Azure.<br/><br/> Als het abonnement uit bedrijf genomen is kunt u niet terugzetten vanaf schijf of Azure. De Azure-herstelpunten worden verwijderd.
Er is geen verbinding meer dan 15 dagen | Actief | Er is geen back-up op schijf of op Azure.<br/><br/> U kunt herstellen vanaf schijf of Azure.
Er is geen verbinding meer dan 15 dagen | Verlopen/de inrichting ongedaan gemaakt | Er is geen back-up op schijf of op Azure.<br/><br/> Als het abonnement is verlopen, kunt u herstellen vanaf schijf of Azure.<br/><br/> Als het abonnement uit bedrijf genomen is kunt u niet terugzetten vanaf schijf of Azure. De Azure-herstelpunten worden verwijderd.



## <a name="dpmmabs-storage-support"></a>Ondersteuning voor DPM/MABS-opslag

Gegevens back-ups op DPM/MABS worden opgeslagen op de lokale schijfopslag. 

**Storage** | **Details**
--- | ---
**MBS** | Moderne back-upopslag (MB/s) wordt ondersteund vanaf DPM 2016/MABS v2 en hoger. Het is niet beschikbaar voor MABS v1.
**MABS-opslag op virtuele Azure-machine** | Gegevens worden opgeslagen op Azure-schijven die zijn gekoppeld aan de DPM/MABS-VM en beheerd in DPM/MABS. Het aantal schijven dat kan worden gebruikt voor DPM/MABS-opslaggroep wordt beperkt door de grootte van de virtuele machine.<br/><br/> A2 VM: 4-schijven. A3 VM: 8 schijven. A4 VM: 16 schijven, met een maximale grootte van 1 TB voor elke schijf. Hiermee bepaalt u de totale beschikbare back-upopslaggroep.<br/><br/> De hoeveelheid gegevens die u kunt back-up, is afhankelijk van het aantal en de grootte van de gekoppelde schijven.
**MABS het bewaren van gegevens op Azure VM** | U wordt aangeraden gegevens bewaren gedurende één dag op de DPM/MABS-Azure-schijf en back-up van DPM/MABS naar de kluis voor een langere periode. U kunt dus een grotere hoeveelheid gegevens beveiligen door het offloaden van het back-up van Azure.


### <a name="modern-backup-storage-mbs"></a>Moderne back-upopslag (MB/s)
Van DPM 2016/MABS v2 (uitgevoerd op Windows Server 2016) en hoger, kunt u profiteren van modern backup storage (MB/s).

- MB/s back-ups worden opgeslagen op een schijf Resilient File System (ReFS).
- MB/s gebruikt ReFS blokkeren klonen en voor de sneller back-up en efficiënter gebruik van opslagruimte.
- Als u volumes aan de lokale DPM/MABS-opslaggroep toevoegt, kunt u deze geconfigureerd met de stationsletters. Vervolgens kunt u de werkbelasting opslag configureren op verschillende volumes.
- Als u beveiligingsgroepen aan back-ups naar DPM/MABS maakt, selecteert u het station dat u wilt gebruiken. Bijvoorbeeld, kunt u back-ups opslaan voor SQL of andere werkbelastingen met hoge IOPS een station met hoge prestaties en werkbelastingen die minder vaak op een lagere prestaties schijf worden opgeslagen.


## <a name="supported-backups-to-mabs"></a>Ondersteunde back-ups naar MABS

De volgende tabel geeft een overzicht van wat kan worden back-ups op MABS van on-premises machines en virtuele Azure-machines.


**Een back-up maken** | **Versies** | **MABS** | **Details**
--- | --- | --- | ---
**Windows 10, 8.1, 8, 7**<br/><br/>(64-32-bits) | MABS v3, v2, V1 | On-premises | Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund<br/><br/> Volumes moeten ten minste 1 GB en NTFS.
**Windows Server 2016 (Datacenter, Standard, not Nano)**<br/><br/> 32-64-bits | MABS v3, v2 | On-premises/Azure VM.| Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> Ontdubbelde volumes wordt ondersteund.
**Windows Server 2012 R2 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v3, v2, v1 | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VMprotection**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund.
**WindowsServer 2012 met SP1 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v3, v2, v1<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund.
**WindowsServer 2012 (Datacenter en Standard)**<br/><br/> 32-64-bits | MABS v1 | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund.
**Windows 2008 R2 met SP1 (Standard en Enterprise)**<br/><br/> 32-64-bits | Ondersteund door MABS v3, v2, v1.<br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. | On-premises/Azure VM. |   **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund.
**Windows 2008 R2 (Standard en Enterprise)**<br/><br/> 32-64-bits | MABS v1. Voor MABS moet het besturingssysteem v2/v3 SP1 worden uitgevoerd. | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund.
**WindowsServer 2008 met SP2**<br/><br/> 32-64-bits | MABS v1, v2, v3 | Alleen v1 wordt ondersteund bij MABS wordt geïmplementeerd als een on-premises fysieke computer of een Hyper-V-VM.<br/><br/> MABS v1, 2, 3 wordt ondersteund bij MABS wordt geïmplementeerd als een VMware-VM.<br/><br/> Niet ondersteund voor MABS die worden uitgevoerd op virtuele Azure-machine | Volume/share/map/bestand. systeem-status/bare metal.
**Windows Storage Server 2008** | MABS v1, v2, v3 | MABS als on-premises fysieke server/Hyper-V virtuele machine | Niet ondersteund voor MABS die worden uitgevoerd op virtuele Azure-machine. | Volume/share/map/bestand. systeem-status/bare metal.
**SQL Server 2017** | MABS v3 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2016/2016 met SP1** | MABS v3, v2 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | MABS v3, v2, v1 | On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | MABS v3, v2, V1 | On-premises. | Back-up van zelfstandige Exchange server, database onder een DAG<br/><br/> Postvak, postvakdatabase onder een DAG herstellen.<br/><br/> ReFS niet wordt ondersteund.<br/><br/> Back-up niet-gedeelde schijfclusters.<br/><br/> Back-up van Exchange Server die is geconfigureerd voor continue replicatie.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | MABS v3, v2, v1 | On-premises/Azure VM | Back-up-farm, front-endwebserver.<br/><br/> Herstellen-farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, front-endwebserver.<br/><br/> U kunt geen back-up van een farm met behulp van AlwaysOn voor inhoudsdatabases.
**Hyper-V in WindowsServer 2016**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (with SP1)** | MABS v3, v2, v1 | On-premises | **MAB-agent op Hyper-V-host**: Back-up volledige VM's en gegevensbestanden van de host. Back-up van virtuele machines met lokale opslag, virtuele machines in een cluster met CSV-opslag, virtuele machines met SMB file server storage.<br/><br/> **MAB-agent in Gast-VM**: Back-up van workloads die worden uitgevoerd op de virtuele machine. CSV's.<br/><br/> **Recovery**: Virtuele machine, herstel op itemniveau van VHD/volume/mappen/bestanden.<br/><br/> **Virtuele Linux-machines**: Back-up als Hyper-V wordt uitgevoerd op Windows Server 2012 R2 en hoger. Herstel voor virtuele Linux-machines is voor de hele machine.
**Virtuele VMware-machines: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1.<br/><br/> MABS v1 moet updatepakket 1) | On-premises | Back-up van virtuele VMware-machines op CSV's, NFS en SAN-opslag.<br/><br/> Volledige VM herstellen.<br/><br/> Back-up van Windows/Linux.<br/><br/> Herstel op itemniveau van de map/voor Windows-VM's alleen bestanden.<br/><br/> VMware vApps worden niet ondersteund.<br/><br/> Herstel voor virtuele Linux-machines is voor de hele machine.



## <a name="supported-backups-to-dpm"></a>Ondersteunde back-ups van DPM

De volgende tabel geeft een overzicht van wat kan een back-up aan DPM van de on-premises machines en virtuele Azure-machines.



**Een back-up maken** | **DPM** | **Details**
--- | --- | --- 
**Windows 10, 8.1, 8, 7**<br/><br/>(64-32-bits) | Alleen on-premises.<br/><br/> Voor back-ups van Windows 10 met DPM 2012 R2 het beste installeren [Update 11](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager). | Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund<br/><br/> Volumes moeten ten minste 1 GB en NTFS.
**Windows Server 2016 (Datacenter, Standard, not Nano)**<br/><br/> 32-64-bits | On-premises/Azure VM.<br/><br/> DPM 2016.| Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> Ontdubbelde volumes wordt ondersteund.
**Windows Server 2012 R2 (Datacenter en Standard)**<br/><br/> 32-64-bits | On-premises/Azure VM | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund met DPM 2012 R2 en hoger.
**WindowsServer 2012 met SP1 (Datacenter en Standard)**<br/><br/> 32-64-bits | On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund met DPM 2012 R2 en hoger.
**WindowsServer 2012 (Datacenter en Standard)**<br/><br/> 32-64-bits |  On-premises/Azure VM. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand<br/><br/> Ontdubbelde volumes wordt ondersteund met DPM 2012 R2 en hoger.
**Windows 2008 R2 met SP1 (Standard en Enterprise)**<br/><br/> 32-64-bits | On-premises/Azure VM <br/><br/> [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) moet worden geïnstalleerd. |   **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal<br/><br/> **Azure VM-beveiliging**: Volume/share/map/bestand.
**Windows 2008 R2 (Standard en Enterprise)**<br/><br/> 32-64-bits | On-premises.<br/><br/> DPM kan niet worden geïnstalleerd als een VMware-VM.<br/><br/> DPM wordt uitgevoerd op een Azure-VM wordt niet ondersteund. | **On-premises bescherming**: Volume/share/map/bestand. systeem-status/bare-metal
**WindowsServer 2008 met SP2**<br/><br/> 32-64-bits | Alleen on-premises.<br/><br/> DPM wordt ondersteund bij het uitvoeren als een VMware-VM. Service wordt uitgevoerd als een fysieke server of Hyper-V-VM wordt niet ondersteund. | Volume/share/map/bestand. systeem-status/bare metal.
**Windows Storage Server 2008** | DPM on-premises uitgevoerd als een fysieke server of Hyper-V-VM. | Volume/share/map/bestand. systeem-status/bare metal.
**SQL Server 2017** | DPM SAC; DPM 2016 met Update Rollup 5 of hoger<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2016 met SP1** | Niet ondersteund voor DPM 2012 R2; Ondersteund voor DPM SAC DPM 2016 Update Rollup 4 of hoger uitvoeren.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2016** | Niet ondersteund voor DPM 2012 R2. Ondersteund voor DPM SAC DPM 2016 vanaf updatepakket 2 en hoger.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**SQL Server 2014**<br/><br/> **SQL Server 2012/SP1/SP2**<br/><br/> **SQL Server 2008 R2**<br/><br/> **SQL Server 2008** | SQL Server 2014 met DPM 2012 R2 met updatepakket 4 en hoger.<br/><br/> On-premises/Azure VM.| Back-up van SQL Server-database.<br/><br/> SQL Server-cluster back-up wordt ondersteund.<br/><br/>Databases die zijn opgeslagen op CSV's wordt niet ondersteund.
**Exchange 2016**<br/><br/> **Exchange 2013**<br/><br/> **Exchange 2010** | Voor Exchange 2016 moet DPM 2012 R2 updatepakket 9 of hoger.<br/><br/> On-premises | Back-up van zelfstandige Exchange server, database onder een DAG<br/><br/> Postvak, postvakdatabase onder een DAG herstellen.<br/><br/> ReFS niet wordt ondersteund.<br/><br/> Back-up niet-gedeelde schijfclusters.<br/><br/> Back-up van Exchange Server die is geconfigureerd voor continue replicatie.
**SharePoint 2016**<br/><br/> **SharePoint 2013**<br/><br/> **SharePoint 2010** | SharePoint 2016 op DPM 2016 en hoger.<br/><br/>On-premises/Azure VM | Back-up-farm, front-endwebserver.<br/><br/> Herstellen-farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, front-endwebserver.<br/><br/> U kunt geen back-up van een farm met behulp van AlwaysOn voor inhoudsdatabases.
**Hyper-V in WindowsServer 2016**<br/><br/> **Windows Server 2012 R2/2012** (Datacenter/Standard)<br/><br/> **Windows Server 2008 R2 (with SP1)** | Hyper-V op 2016 ondersteund voor DPM 2016 en hoger.<br/><br/> On-premises | **MAB-agent op Hyper-V-host**: Back-up volledige VM's en gegevensbestanden van de host. Back-up van virtuele machines met lokale opslag, virtuele machines in een cluster met CSV-opslag, virtuele machines met SMB file server storage.<br/><br/> **MAB-agent in Gast-VM**: Back-up van workloads die worden uitgevoerd op de virtuele machine. CSV's.<br/><br/> **Recovery**: Virtuele machine, herstel op itemniveau van VHD/volume/mappen/bestanden.<br/><br/> **Virtuele Linux-machines**: Back-up als Hyper-V wordt uitgevoerd op Windows Server 2012 R2 en hoger. Herstel voor virtuele Linux-machines is voor de hele machine.
**Virtuele VMware-machines: vCenter/vSphere ESXi 5.5/6.0/6.5** | MABS v3, v2, v1.<br/><br/> DPM 2012 R2 moet System Center Update Rollup 1) | On-premises | Back-up van virtuele VMware-machines op CSV's, NFS en SAN-opslag.<br/><br/> Volledige VM herstellen.<br/><br/> Back-up van Windows/Linux.<br/><br/> Herstel op itemniveau van de map/voor Windows-VM's alleen bestanden.<br/><br/> VMware vApps worden niet ondersteund.<br/><br/> Herstel voor virtuele Linux-machines is voor de hele machine.


- Houd er rekening mee dat de geclusterde werkbelastingen een back-up door DPM/MABS in hetzelfde domein als de DPM/MABS of in een onderliggende/vertrouwd domein moeten. 
- Verificatie via NTLM/certificaat kunt u back-up van gegevens in niet-vertrouwde domeinen of werkgroepen.



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](backup-architecture.md#architecture-back-up-to-dpmmabs) over de architectuur van MABS.
- [Beoordeling](backup-support-matrix-mars-agent.md) wat wordt ondersteund voor de MARS-agent.
- [Instellen van](backup-azure-microsoft-azure-backup.md) een MABS-server.
- [DPM instellen](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-180)
