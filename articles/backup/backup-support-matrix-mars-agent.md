---
title: Ondersteuningsmatrix voor de back-up van machines met de Microsoft Azure Recovery Services agent (MARS) met Azure Backup
description: In dit artikel bevat een overzicht van Azure Backup-ondersteuning als u back-up van machines die de Microsoft Azure Recovery Services agent (MARS) worden uitgevoerd.
services: backup
author: rayne-wiselman
ms.service: backup
ms.date: 02/17/2019
ms.topic: conceptual
ms.author: raynew
manager: carmonm
ms.openlocfilehash: 61afefb955914c75606c4fff36ebcc05a4ad0057
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010902"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Ondersteuningsmatrix voor de back-up met de Microsoft Azure Recovery Services agent (MARS)

U kunt de [Azure Backup-service](backup-overview.md) back-up on-premises computers en -apps en virtuele Azure-machines. In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen voor back-ups van virtuele machines met de Microsoft Azure Recovery Services agent (MARS).

## <a name="about-the-mars-agent"></a>Over de MARS-agent

De MARS-agent wordt gebruikt door Azure Backup om maakt u een back-up van gegevens van on-premises machines en virtuele Azure-machines naar een back-Recovery Services-kluis in Azure. De MARS-agent kan als volgt worden gebruikt:
- De agent uitgevoerd op on-premises Windows-computers, zodat ze kunnen back-up rechtstreeks naar een back-Recovery Services-kluis in Azure.
- De agent op Windows Azure Virtual machines uitvoeren, zodat back-up kan rechtstreeks naar een kluis.
- Voer de agent op een Microsoft Azure Backup-Server (MABS) of een System Center Data Protection - server Manager (DPM). In dit scenario, computers en werkbelastingen back-up naar MABS/DPM en klikt u vervolgens MABS/DPM back-up naar een kluis in Azure met behulp van de MARS-agent wordt ondersteund. 

Wat u kunt back-up, is afhankelijk van de waarop de agent is geïnstalleerd.

- [Meer informatie](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) over de architectuur van back-up met behulp van de MARS-agent.
- Meer informatie over MABS/DPM [back-up van architectuur](backup-architecture.md#architecture-back-up-to-dpmmabs)en [vereisten](backup-support-matrix-mabs-dpm.md).


## <a name="supported-installation"></a>Ondersteunde-installatie

**Installeren** | **Details**
--- | ---
**Meest recente MARS-agent downloaden** | U kunt de meest recente versie van de agent downloaden uit de kluis of [rechtstreeks downloaden](https://aka.ms/azurebackup_agent).
**Rechtstreeks op een computer installeren** | U kunt de MARS-agent installeren op een on-premises Windows server of Windows Azure-VM met een van de [ondersteunde besturingssystemen](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).
**Installeren op een back-upserver** | Bij het instellen van DPM- of MABS back-up naar Azure, kunt u downloaden en installeren van de MARS-agent op de server. De agent kan worden geïnstalleerd in overeenstemming met de [ondersteunde besturingssystemen](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) in de ondersteuningsmatrix voor back-upserver.

> [!NOTE]
> Standaard Azure VM's zijn ingeschakeld voor back-up beschikt over een installatie van de Azure Backup-extensie. Deze extensie een back-up van de hele virtuele machine. U kunt installeren en de MARS-agent op een Azure-VM samen met de extensie wordt uitgevoerd als u wilt back-up van specifieke mappen en bestanden, in plaats van de volledige virtuele machine.
> Wanneer u de MARS-agent op een Azure-VM uitvoert, wordt een back-up van bestanden/mappen in de tijdelijke opslag op de virtuele machine. Back-ups mislukken als de bestanden/mappen worden verwijderd uit de tijdelijke opslag, of als de tijdelijke opslag wordt verwijderd.


## <a name="cache-folder-support"></a>Ondersteuning voor cache-map

Wanneer u een back-up maakt met de MARS-agent, wordt de agent een momentopname van de gegevens en slaat ze op in een lokale cachemap voordat deze naar Azure verzonden. De cachemap (volledig) heeft een aantal vereisten.

**Cache** | **Details**
--- | ---
**Cachegrootte** |  Vrije ruimte op de grootte cachemap moet ten minste 5-10% van de totale grootte van uw back-upgegevens. 
**Locatie van de cache** | De cachemap moet zich lokaal op de computer een back-wordt gemaakt en moet online zijn.<br/><br/> De cachemap mag niet worden gevonden op een netwerkshare bevindt, op verwisselbare media of op een offline-volume. 
**Cachemap** | De cachemap moet worden versleuteld, op een ontdubbeld volume of op een map die is gecomprimeerd/sparse /-reparsepunt
**Locatie van de cache wijzigen** | U kunt de cachelocatie wijzigen door te stoppen van de back-engine (net stop bengine) en de cachemap kopiëren naar een nieuw station (Zorg ervoor dat er voldoende ruimte heeft). Vervolgens u twee registervermeldingen onder HKLM\SOFTWARE\Microsoft\Windows Azure back-up (Config/ScratchLocation en configuratie/CloudBackupProvider/ScratchLocation) bijwerken naar de nieuwe locatie en de engine voor het opnieuw.

## <a name="networking-and-access-support"></a>Ondersteuning voor netwerk- en toegangsbeheer

### <a name="url-access"></a>URL-toegang

De MARS-agent moet toegang hebben tot deze URL's:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- *.windows.net

### <a name="throttling-support"></a>Ondersteuning voor bandbreedteregeling

**Functie** | **Details**
--- | ---
Bandbreedteregeling | Ondersteund<br/><br/> Gebruik **eigenschappen wijzigen** in de MARS-agent om aan te passen van bandbreedte.
Netwerkbeperking | Niet beschikbaar voor back-ups van virtuele machines met Windows Server 2008 R2, Windows Server 2008 SP2 of Windows 7.

## <a name="support-for-direct-backups"></a>Ondersteuning voor directe back-ups

De volgende tabel geeft een overzicht van welke besturingssystemen die worden uitgevoerd op on-premises machines en virtuele Azure-machines kunt een back-up rechtstreeks naar Azure met de MARS-agent.

- Alle besturingssystemen zijn 64-bits.
- Alle besturingssystemen moet worden uitgevoerd, de meest recente servicepacks en updates.
- Raadpleeg voor meer informatie over DPM en MABS servers kunnen een back-up met de MARS-agent [deze tabel](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

**Besturingssysteem** | **Bestanden/mappen** | **Systeemstatus**
--- | --- | ---
Windows 10 (Enterprise, Pro, Home) | Ja | Nee
Windows 8.1 (Enterprise, Pro)| Ja |Nee
Windows 8 (Enterprise, Pro) | Ja | Nee
Windows 7 (Ultimate, Enterprise, Pro, Home Premium/Basic, Starter) | Ja | Nee
WindowsServer 2016 (Standard, Datacenter, Essentials) | Ja | Ja
Windows Server 2012 R2 (Standard, Datacenter, Foundation, Essentials) | Ja | Ja
Windows Server 2012 (Standard, Datacenter, Foundation) | Ja | Ja
Windows Server 2008 R2 (Standard, Enterprise, Datacenter, Foundation) | Ja | Ja
Windows Server 2008 SP2 (Standard, Datacenter, Foundation) | Ja | Nee
Windows Storage Server 2016/2012 R2/2012(Standard, Workgroup | Ja | Nee


## <a name="backup-limits"></a>Limieten voor back-up

Azure Backup plaatst een limiet op de grootte van een bestand/map-gegevensbron die u kunt een back-up. De grootte van de items zijn geselecteerd voor back-up van een enkel volume kan niet groter zijn dan de grootten die in de tabel wordt samengevat.

**Besturingssysteem** | **Maximale grootte**
--- | ---
Windows Server 2012 of hoger |  54.400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 of hoger  | 54.400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Ondersteunde bestandstypen voor back-up

**Type** | **Ondersteund** 
--- | --- 
Versleuteld   | Ja 
Gecomprimeerd | Ja
Sparse | Ja 
Gecomprimeerd en sparse | Ja
Vaste koppelingen  | Niet ondersteund<br/><br/> Overgeslagen
Reparsepunt   | Niet ondersteund<br/><br/> Overgeslagen
Versleuteld en sparse |  Niet ondersteund<br/><br/> Overgeslagen
Gecomprimeerde stream   | Niet ondersteund<br/><br/> Overgeslagen
Sparse stream   | Niet ondersteund<br/><br/> Overgeslagen
Een station (bestanden gesynchroniseerd zijn sparse streams)    | Niet ondersteund 

## <a name="supported-drivesvolumes-for-backup"></a>Ondersteunde stations/volumes voor back-up

**Station of het volume** | **Ondersteund** | **Details**
--- | --- | ---
Alleen-lezen-volumes   | Niet ondersteund | Het volume moet beschrijfbaar zijn voor de VSS om te werken.
Offlinevolumes | Niet ondersteund |   Het volume moet online zijn voor de VSS om te werken.
Netwerkshare   | Niet ondersteund |   Het volume moet zich lokaal op de server voor back-up worden.
Met BitLocker beveiligde volumes | Niet ondersteund |   Het volume moet worden ontgrendeld voordat voor back-up om te werken.
Bestandssysteemidentificatie  | Niet ondersteund |   Uitsluitend NTFS.
Verwisselbare media | Niet ondersteund |   Status van vaste hebben alle back-upitem bronnen.
Ontdubbelde schijven | Ondersteund.<br/><br/> Azure Backup converteert de ontdubbelde gegevens naar normale gegevens. Deze de optimaliseert de gegevens, versleutelt deze, worden opgeslagen en verzendt ze naar de kluis.

## <a name="support-for-initial-offline-backup"></a>Ondersteuning voor de eerste offline back-up

Azure Backup biedt ondersteuning voor 'offline-seeding' om over te dragen van eerste back-upgegevens naar Azure met behulp van schijven. Dit is handig als uw eerste back-up is waarschijnlijk in het bereik van terabytes (TB). Offline back-up wordt ondersteund voor:

- Directe back-ups van bestanden en mappen op de on-premises computers waarop de MARS-agent wordt uitgevoerd.
- Back-up van workloads en bestanden van een DPM-server of MABS.
- Offline back-up kan niet worden gebruikt voor systeemstatusbestanden.


## <a name="support-for-restore"></a>Ondersteuning voor herstel

- De nieuwe [instant restore](backup-instant-restore-capability.md) versie van Azure Backup kunt u voor het terugzetten van gegevens voordat deze is gekopieerd naar de kluis.<br/><br/> Deze functie wilt gebruiken de machine back-up moet worden uitgevoerd .NET Framework 4.5.2 of hoger.
- Back-ups kunnen niet worden hersteld naar een doel-VM met een eerdere versie van het besturingssysteem. Bijvoorbeeld, kan een back-up van een computer met Windows 7 worden hersteld op Windows 8 of hoger. Echter, een back-up van een computer met Windows 8 kan niet worden hersteld naar een computer met Windows 7.


## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders) over de architectuur van de back-up met de MARS-agent.
- [Informatie over](backup-support-matrix-mabs-dpm.md) wat wordt ondersteund wanneer u de MARS-agent op Microsoft Azure Backup-Server (MABS) of System Center DPM uitvoert.


