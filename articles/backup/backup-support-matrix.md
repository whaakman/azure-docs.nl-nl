---
title: Ondersteuningsmatrix voor Azure Backup
description: Bevat een samenvatting van ondersteuningsinstellingen en -beperkingen voor de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: cb3a60995a4edfe5eb00f1a5e88812146816806a
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54883701"
---
# <a name="azure-backup-support-matrix"></a>Ondersteuningsmatrix voor Azure Backup

U kunt de [Azure Backup-service](backup-overview.md) gebruiken om een back-up van gegevens naar de Microsoft Azure-cloud te maken. Dit artikel bevat een overzicht van ondersteuningsinstellingen en -beperkingen voor de Azure Backup-scenario's en implementaties.

## <a name="vault-support"></a>Ondersteuning voor kluizen

Azure Back-up maakt gebruik van Recovery Services-kluizen om back-ups te coördineren en te beheren en om back-upgegevens in op te slaan.

**Instelling** | **Details**
--- | ---
Aantal kluizen | Maximaal 500 Recovery Services-kluizen in één abonnement.
Machines in een kluis | Maximaal 1000 virtuele Azure-machines in een enkele kluis.<br/><br/> Maximaal 50 on-premises machines waarop de Azure Backup-agent (Microsoft Azure Recovery Services-agent (MABS)) wordt uitgevoerd, kunnen in een enkele kluis worden geregistreerd.
Gegevensbron in kluisopslag | Maximaal 54400 GB. Er is geen limiet voor back-ups van virtuele Azure-machines.
Back-ups naar kluis | Azure VM's: één keer per dag; machines die zijn beveiligd met DPM/MABS: twee keer per dag; machines waarvan een back-up direct met MARS-agent wordt gemaakt: drie keer per dag.  
Kluis verplaatsen | Voor het verplaatsen van een Recovery Services-kluis, moet u zich inschrijven voor een beperkte preview. Schrijf naar AskAzureBackupTeam@microsoft.com als u dit wilt uitproberen.
Gegevens verplaatsen tussen kluizen | Het verplaatsen van back-upgegevens tussen kluizen wordt niet ondersteund.
Type opslagreplicatie | U kunt het type opslagreplicatie (GRS/LRS) voor een kluis wijzigen voordat back-ups worden opgeslagen. Nadat een back-ups in de kluis is begonnen, kan het replicatietype niet meer worden gewijzigd.



## <a name="on-premises-backup-support"></a>On-premises ondersteuning voor back-ups

Dit wordt ondersteund als u een back-up wilt maken van on-premises machines.

**Machine** | **Locatie** | **Een back-up maken** | **Functies**
--- | --- | --- | ---
**Windows fysiek/virtueel (geen back-upserver)** | Bestanden, mappen, systeemstatus | Back-up naar Recovery Services-kluis | Back-up drie keer per dag.<br/><br/> Geen app-bewuste back-up.<br/><br/> Bestand, map, volume herstellen.
**Linux fysiek/virtueel (geen back-upserver)** | Back-up niet ondersteund.
**Fysiek/virtueel met DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar DPM (op een schijf die lokaal is aangesloten op de DPM-server, of op tape).<br/><br/> DPM voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.
**Fysiek/virtueel met MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar MABS (op een schijf die lokaal is aangesloten op de MABS-server). Tape wordt niet ondersteund<br/><br/> MABS voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.


## <a name="azure-vms"></a>Azure-VM's

### <a name="azure-vm-limits"></a>Limieten voor Azure VM's

**Limiet** | **Details**
--- | ---
Azure VM-gegevensschijven | Limiet van 16.
Grootte Azure VM-gegevensschijven | Afzonderlijke schijf mag maximaal 4095 GB zijn.


### <a name="azure-vm-backup-options"></a>Azure VM-back-upopties

Dit wordt ondersteund als u een back-up wilt maken van Azure VM's.

**Machine** | **Locatie** | **Een back-up maken** | **Functies**
--- | --- | --- | ---
**Azure VM's (geen back-upserver)** | Bestanden, mappen, systeemstatus | Back-up naar kluis. | Back-up eenmaal per dag.<br/><br/> App-bewuste back-up voor Windows-VM's, bestandsconsistente back-up voor virtuele Linux-machines. U kunt app-consistentie configureren voor Linux-machines met behulp van aangepaste scripts.<br/><br/> Virtuele machine/schijf herstellen.<br/><br/> Kan geen back-up maken op Azure VM naar een on-premises locatie.
**Azure VM met DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar DPM die wordt uitgevoerd in Azure (op een schijf die lokaal is aangesloten op de DPM-server). Tape wordt niet ondersteund.<br/><br/> DPM voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.
**Azure VM met MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar MABS die wordt uitgevoerd in Azure (op een schijf die lokaal is aangesloten op de MABS-server). Tape wordt niet ondersteund<br/><br/> MABS voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.





## <a name="linux-backup-support"></a>Ondersteuning voor Linux-back-ups

Dit wordt ondersteund als u een back-up wilt maken van Linux-machines.

**Een back-up maken** | **Linux (goedgekeurd door Azure)**
--- | ---
**On-premises Linux-machine (zonder DPM of MABS)**. | Nee. De MARS-agent kan alleen worden geïnstalleerd op Windows-machines.
**Azure VM (zonder DPM of MABS)** | App-consistente back-up met behulp van [aangepaste scripts](backup-azure-linux-app-consistent.md).<br/><br/> Herstel op bestandsniveau.<br/><br/> Herstellen door het maken van een VM vanaf een herstelpunt of schijf.
**On-premises machine/Azure VM met DPM** | Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/><br/> Linux-gast-VM's herstellen op Hyper-V- en VMware</br></br> Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines
**On-premises machine/Azure VM met MABS** | Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/><br/> Linux-gast-VM's herstellen op Hyper-V- en VMware</br></br> Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines.

## <a name="disk-support"></a>Ondersteuning voor schijven

Ondersteuning voor schijfontdubbeling is als volgt:
- Schijfontdubbeling wordt on-premises ondersteund wanneer u DPM of MABS gebruikt voor het maken van een back-up van Hyper-V-VM's waarop Windows wordt uitgevoerd. Windows Server voert gegevensontdubbeling uit (op hostniveau) op virtuele harde schijven (VHD's) die als back-upopslag zijn gekoppeld aan de virtuele machine.
- Ontdubbeling wordt niet ondersteund in Azure voor Backup-onderdelen. Wanneer de System Center DPM en de Backup-server zijn geïmplementeerd in Azure, kunnen de opslagschijven die aan de virtuele machine zijn gekoppeld, niet worden ontdubbeld.


## <a name="securityencryption-support"></a>Ondersteuning voor beveiliging/versleuteling

Azure Backup ondersteunt versleuteling voor in-transit- en inactieve gegevens:

Netwerkverkeer naar Azure:
- Back-upverkeer van servers naar de Recovery Services-kluis wordt versleuteld met Advanced Encryption Standard 256.
- Back-upgegevens worden verzonden via een beveiligde HTTPS-koppeling.
- De back-upgegevens worden versleuteld opgeslagen in de Recovery Services-kluis.
- Alleen u beschikt over de wachtwoordzin waarmee deze gegevens kunnen worden ontgrendeld. De back-upgegevens kunnen niet door Microsoft ontsleuteld.
    > [!WARNING]
    > Nadat u de kluis hebt ingesteld, hebt alleen u toegang tot de versleutelingssleutel. Microsoft bewaart nooit een kopie en heeft geen toegang tot de sleutel. Als de sleutel verkeerd wordt geplaatst, kan Microsoft de back-upgegevens niet herstellen.
Gegevensbeveiliging:
- Als u back-ups van virtuele machines van Azure wilt maken, moet u de versleuteling configureren *binnen* de virtuele machine.
- Azure Backup biedt ondersteuning voor Azure Disk Encryption, dat gebruikmaakt van BitLocker op virtuele Windows-machines en **dm-crypt** op virtuele Linux-machines.
- Op de backend maakt Azure Backup gebruik van [Azure Storage Service-versleuteling](../storage/common/storage-service-encryption.md), waarmee data-at-rest worden beschermd.


**Machine** | **In-transit** | **Inactief**
--- | --- | ---
On-premises Windows-machines zonder DPM/MABS | ![Ja][green] | ![Ja][green]
Azure-VM's | ![Ja][green] | ![Ja][green]
On-premises/Azure VM's met DPM | ![Ja][green] | ![Ja][green]
On-premises/Azure VM's met MABS | ![Ja][green] | ![Ja][green]



## <a name="compression-support"></a>Ondersteuning voor compressie

Backup biedt ondersteuning voor de compressie van back-upverkeer, zoals samengevat in de onderstaande tabel. Houd rekening met het volgende:

- Voor Azure VM's leest de VM-extensie de gegevens rechtstreeks vanuit het Azure Storage-account via het opslagnetwerk. Dit verkeer hoeft daarom niet te worden gecomprimeerd.
- Als u DPM of MABS gebruikt, kunt u de gegevens comprimeren voordat er een back-up van wordt gemaakt in DPM/MABS, om bandbreedte te besparen.

**Machine** | **Comprimeren naar MABS/DPM (TCP)** | **Comprimeren (HTTPS) naar kluis**
--- | --- | ---
On-premises Windows-machines zonder DPM/MABS | N.v.t. | Ja
Azure-VM's | N.v.t. | N.v.t.
On-premises/Azure VM's met DPM | ![Ja][green] | ![Ja][green]
On-premises/Azure VM's met MABS | ![Ja][green] | ![Ja][green]



## <a name="retention-limits"></a>Bewaarlimieten

**Instelling** | **Limieten**
--- | ---
Maximum aantal herstelpunten per beveiligde instantie (machine/workload) | 9999
Maximale verlooptijd voor een herstelpunt | Geen limiet
Maximale back-upfrequentie naar DPM/MABS | Om de 15 minuten voor SQL Server<br/><br/> Eenmaal per uur voor andere workloads.
Maximale back-upfrequentie naar kluis | On-premises Windows-machines/Azure VM's met MARS: drie per dag<br/><br/> DPM/MABS: Twee per dag<br/><br/> Back-up van Azure VM's: Eenmaal per dag
Bewaarperiode van herstelpunt | Dagelijks, wekelijks, maandelijks, jaarlijks.
Maximale bewaarperiode | Afhankelijk van back-upfrequentie.
Herstelpunten op DPM-/MABS-schijf | 64 voor bestandsservers, 448 voor appservers.<br/><br/> Tapeherstelpunten zijn onbeperkt voor on-premises DPM.

## <a name="next-steps"></a>Volgende stappen

- [Back-ups maken van Azure-VM's](backup-azure-arm-vms-prepare.md)
- [Rechtstreeks een back-up maken van Windows-machines](tutorial-backup-windows-server-to-azure.md), zonder een back-upserver.
- [MABS instellen](backup-azure-microsoft-azure-backup.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar MABS maken.
- [DPM instellen](backup-azure-dpm-introduction.md) voor het maken van een back-up naar Azure, en vervolgens een back-up van workloads naar DPM maken.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
