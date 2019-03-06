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
ms.openlocfilehash: e12ca7561181412318fa594093b047cd95e4e6bc
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57448030"
---
# <a name="support-matrix-for-backup-with-the-microsoft-azure-recovery-services-mars-agent"></a>Ondersteuningsmatrix voor de back-up met de Microsoft Azure Recovery Services agent (MARS)

U kunt de [Azure Backup-service](backup-overview.md) back-up on-premises machines en apps en back-up van virtuele Azure-machines (VM's). In dit artikel bevat een overzicht van instellingen voor de ondersteuning en beperkingen wanneer u de Microsoft Azure Recovery Services agent (MARS) voor back-up van machines.

## <a name="the-mars-agent"></a>De MARS-agent

Azure Backup gebruikt de MARS-agent om back-ups van on-premises machines en virtuele Azure-machines naar een back-Recovery Services-kluis in Azure. De MARS-agent kunt doen:
- Uitgevoerd op on-premises Windows-computers zodat ze kunnen back-up rechtstreeks naar een back-Recovery Services-kluis in Azure.
- Uitgevoerd op Windows-VM's zodat ze kunnen back-up rechtstreeks naar een kluis.
- Uitgevoerd op Microsoft Azure Backup-Server (MABS) of een server met System Center Data Protection Manager (DPM). In dit scenario back computers en werkbelastingen-up naar MABS of naar de DPM-server. De MARS-agent en de back-up van deze server naar een kluis in Azure. 

Uw back-upopties, is afhankelijk van de waarop de agent is geïnstalleerd. Zie voor meer informatie, [Azure Backup-architectuur met behulp van de MARS-agent](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders). Zie voor meer informatie over de architectuur van MABS en DPM back-up [back-up op de DPM- of MABS](backup-architecture.md#architecture-back-up-to-dpmmabs). Zie ook [vereisten](backup-support-matrix-mabs-dpm.md) voor de back-architectuur.

## <a name="supported-installations"></a>Ondersteunde installaties

**Installatie** | **Details**
--- | ---
Download de nieuwste MARS-agent | U kunt de meest recente versie van de agent downloaden uit de kluis of [rechtstreeks downloaden](https://aka.ms/azurebackup_agent).
Rechtstreeks op een computer installeren | U kunt de MARS-agent installeren rechtstreeks op een on-premises Windows server of op een Windows-VM die wordt uitgevoerd een van de [ondersteunde besturingssystemen](https://docs.microsoft.com/en-us/azure/backup/backup-support-matrix-mabs-dpm#supported-mabs-and-dpm-operating-systems).
Installeren op een back-upserver | Bij het instellen van DPM- of MABS back-up naar Azure, kunt u downloaden en installeren van de MARS-agent op de server. U kunt de agent installeren op [ondersteunde besturingssystemen](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems) in de ondersteuningsmatrix voor back-upserver.

> [!NOTE]
> Standaard zijn Azure-VM's die zijn ingeschakeld voor back-up een installatie van de Azure Backup-extensie. Deze extensie een back-up van de hele virtuele machine. U kunt installeren en de MARS-agent op een Azure-VM samen met de extensie wordt uitgevoerd als u wilt back-up van specifieke mappen en bestanden, in plaats van de volledige virtuele machine.
> Wanneer u de MARS-agent op een Azure-VM uitvoert, wordt een back-up van bestanden en mappen die zich in de tijdelijke opslag op de virtuele machine. Back-ups mislukken als de bestanden of mappen worden verwijderd uit de tijdelijke opslag, of als de tijdelijke opslag wordt verwijderd.


## <a name="cache-folder-support"></a>Ondersteuning voor cache-map

Wanneer u de MARS-agent gebruikt voor back-up van gegevens, wordt de agent een momentopname van de gegevens en slaat ze op in een lokale map voordat het de gegevens naar Azure verzendt. De cachemap (volledig) heeft verschillende vereisten:

**Cache** | **Details**
--- | ---
Grootte |  Vrije ruimte in de cachemap moet ten minste 5 tot 10 procent van de totale grootte van uw back-upgegevens. 
Locatie | De cachemap moet lokaal worden opgeslagen op de computer die de back-up en moet online zijn. De cachemap mag niet op een netwerkshare bevindt, op verwisselbare media of op een offline-volume. 
Map | De cachemap moet worden versleuteld op een ontdubbeld volume of in een map die gecomprimeerd, die wordt verspreid, of waarvoor een reparsepunt.
Locatiewijzigingen | U kunt de cachelocatie wijzigen door de engine voor het back-up stoppen (`net stop bengine`) en de cachemap kopiëren naar een nieuw station. (Zorg ervoor dat het nieuwe station voldoende ruimte heeft.) Werk vervolgens twee registervermeldingen onder **HKLM\SOFTWARE\Microsoft\Windows Azure Backup** (**Config/ScratchLocation** en **CloudBackupProvider-Config-ScratchLocation**) naar de nieuwe locatie en de engine voor het opnieuw.

## <a name="networking-and-access-support"></a>Ondersteuning voor netwerk- en toegangsbeheer

### <a name="url-access"></a>URL-toegang

De MARS-agent moet toegang hebben tot deze URL's:

- http://www.msftncsi.com/ncsi.txt
- *.Microsoft.com
- *.WindowsAzure.com
- *.MicrosoftOnline.com
- *.Windows.net

### <a name="throttling-support"></a>Ondersteuning voor bandbreedteregeling

**Functie** | **Details**
--- | ---
Bandbreedteregeling | Ondersteund. In de MARS-agent gebruiken **eigenschappen wijzigen** bandbreedte aanpassen.
Netwerkbeperking | Niet beschikbaar voor een back-up-machines waarop Windows Server 2008 R2, Windows Server 2008 SP2 of Windows 7 wordt uitgevoerd.

## <a name="support-for-direct-backups"></a>Ondersteuning voor directe back-ups

De MARS-agent kunt u back-up rechtstreeks naar Azure op sommige besturingssystemen die worden uitgevoerd op on-premises machines en virtuele Azure-machines. Het besturingssysteem moet 64-bits en moet worden uitgevoerd van de meest recente servicepacks en updates. De volgende tabel geeft een overzicht van deze besturingssystemen:

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
Windows Storage Server 2016/2012 R2/2012 (Standard, Workgroup) | Ja | Nee

Zie voor meer informatie, [besturingssystemen ondersteund MABS en DPM](backup-support-matrix-mabs-dpm.md#supported-mabs-and-dpm-operating-systems).

## <a name="backup-limits"></a>Limieten voor back-up

Azure Backup beperkt de grootte van een bestand of map gegevensbron die u kunt een back-up. De items die u op één volume back-up niet langer zijn dan de grootten die in deze tabel wordt samengevat:

**Besturingssysteem** | **Maximale grootte**
--- | ---
Windows Server 2012 of hoger |  54.400 GB
Windows Server 2008 R2 SP1 |    1700 GB
Windows Server 2008 SP2 | 1700 GB
Windows 8 of hoger  | 54.400 GB
Windows 7   | 1700 GB


## <a name="supported-file-types-for-backup"></a>Ondersteunde bestandstypen voor back-up

**Type** | **Ondersteuning** 
--- | --- 
Versleuteld   | Ondersteund. 
Gecomprimeerd | Ondersteund.
Sparse | Ondersteund.
Gecomprimeerd en sparse | Ondersteund.
Vaste koppelingen  | Wordt niet ondersteund. Overgeslagen.
Reparsepunt   | Wordt niet ondersteund. Overgeslagen.
Versleuteld en sparse |  Wordt niet ondersteund. Overgeslagen.
Gecomprimeerde stream   | Wordt niet ondersteund. Overgeslagen.
Sparse stream   | Wordt niet ondersteund. Overgeslagen.
OneDrive (gesynchroniseerde bestanden zijn sparse streams)  | Wordt niet ondersteund. 

## <a name="supported-drives-or-volumes-for-backup"></a>Ondersteunde stations of volumes voor back-up

**Station of het volume** | **Ondersteuning** | **Details**
--- | --- | ---
Alleen-lezen-volumes   | Niet ondersteund | Volume Copy Shadow Service (VSS) werkt alleen als het volume schrijfbaar is.
Offlinevolumes | Niet ondersteund |   VSS werkt alleen als het volume online is.
Netwerkshare   | Niet ondersteund |   Het volume moet lokaal op de server.
Met BitLocker beveiligde volumes | Niet ondersteund |   Het volume moet worden ontgrendeld voordat de back-up wordt gestart.
Bestandssysteemidentificatie  | Niet ondersteund |   Alleen NTFS wordt ondersteund.
Verwisselbare media | Niet ondersteund |   Alle back-upitem bronnen ze beschikken over een *vaste* status.
Ontdubbelde schijven | Ondersteund | Azure Backup converteert de ontdubbelde gegevens naar normale gegevens. Deze optimaliseert, worden versleuteld opgeslagen en verzendt die gegevens naar de kluis.

## <a name="support-for-initial-offline-backup"></a>Ondersteuning voor de eerste offline back-up

Azure Backup ondersteunt *offline seeding* om over te dragen van eerste back-upgegevens naar Azure met behulp van schijven. Deze ondersteuning is handig als uw eerste back-up is waarschijnlijk in het bereik van de grootte van terabytes (TB). Offline back-up wordt ondersteund voor:

- Directe back-ups van bestanden en mappen op de on-premises computers waarop de MARS-agent.
- Back-up van workloads en bestanden van een DPM-server of MABS.

Offline back-up kan niet worden gebruikt voor systeemstatusbestanden.


## <a name="support-for-data-restoration"></a>Ondersteuning voor gegevens worden hersteld

Met behulp van de [direct herstellen](backup-instant-restore-capability.md) functie van Azure Backup, kunt u gegevens terugzetten voordat deze wordt gekopieerd naar de kluis. De computer die u een back-up moet worden uitgevoerd op .NET Framework 4.5.2 of hoger.

Back-ups kunnen niet worden hersteld naar een doel-VM die een eerdere versie van het besturingssysteem wordt uitgevoerd. Bijvoorbeeld, kan een back-up van een computer met Windows 7 worden hersteld op Windows 8 of hoger. Maar een back-up van een computer met Windows 8 kan niet worden hersteld op een computer waarop Windows 7 wordt uitgevoerd.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [back-up van de architectuur die gebruikmaakt van de MARS-agent](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-machinesazure-vm-filesfolders).
- Informatie over wat er is ondersteund wanneer u [de MARS-agent worden uitgevoerd op MABS of een DPM-server](backup-support-matrix-mabs-dpm.md).