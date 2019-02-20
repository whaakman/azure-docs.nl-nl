---
title: Ondersteuningsmatrix voor Azure Backup
description: Bevat een samenvatting van ondersteuningsinstellingen en -beperkingen voor de Azure Backup-service.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: ff4ee1d88bd13e647d0f6218d7e9c9b2c57a5a01
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429567"
---
# <a name="azure-backup-support-matrix"></a>Ondersteuningsmatrix voor Azure Backup

U kunt de [Azure Backup-service](backup-overview.md) gebruiken om een back-up van gegevens naar de Microsoft Azure-cloud te maken. In dit artikel bevat een overzicht van instellingen voor de algemene ondersteuning en beperkingen voor Azure Backup-scenario's en implementaties.

Andere matrices ondersteuning zijn beschikbaar:

[Ondersteuningsmatrix voor](backup-support-matrix-iaas.md) voor Azure VM backup [ondersteuningsmatrix](backup-support-matrix-mabs-dpm.md) voor back-up met behulp van System Center DPM/Microsoft Azure Backup server (MABS) [ondersteuningsmatrix](backup-support-matrix-mars-agent.md) voor back-ups maken met behulp van Microsoft Azure Recovery Services (MARS)-agent

## <a name="vault-support"></a>Ondersteuning voor kluizen

Azure Back-up maakt gebruik van Recovery Services-kluizen om back-ups te coördineren en te beheren en om back-upgegevens in op te slaan.

**Instelling** | **Details**
--- | ---
**Kluizen in het abonnement** | Maximaal 500 Recovery Services-kluizen in één abonnement.
**Machines in een kluis** | Maximaal 1000 virtuele Azure-machines in een enkele kluis.<br/><br/> Maximaal 50 MABS kunnen de servers in een enkele kluis worden geregistreerd.
**Gegevensbronnen in de kluis opslag** | Maximaal 54400 GB. Er is geen limiet voor back-ups van virtuele Azure-machines.
**Back-ups naar de kluis** | Azure VM's: één keer per dag<br/><br/>Machines die zijn beveiligd door DPM/MABS: twee keer per dag<br/><br/> Machines een back-up direct met MARS-agent: drie keer per dag. 
**Back-ups tussen kluizen** | Back-up is binnen een regio.<br/><br/> U moet een kluis in elke Azure-regio die u back wilt-up van virtuele machines bevat. U kunt geen back-up naar een andere regio. 
**Kluis verplaatsen** | U kunt [verplaatsen van kluizen](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) voor abonnementen, of tussen resourcegroepen in hetzelfde abonnement.
**Gegevens verplaatsen tussen kluizen** | Het verplaatsen van back-upgegevens tussen kluizen wordt niet ondersteund.
**Opslagtype vault wijzigen** | U kunt het type opslagreplicatie (GRS/LRS) voor een kluis wijzigen voordat back-ups worden opgeslagen. Nadat een back-ups in de kluis is begonnen, kan het replicatietype niet meer worden gewijzigd.



## <a name="on-premises-backup-support"></a>On-premises ondersteuning voor back-ups

Dit wordt ondersteund als u een back-up wilt maken van on-premises machines.

**Machine** | **Een back-up** | **Locatie** | **Functies**
--- | --- | --- | ---
**Directe back-ups van Windows-machine met de MARS-agent** | Bestanden, mappen, systeemstatus | Back-up naar de Recovery services-kluis | Back-up drie keer per dag.<br/><br/> Geen app-bewuste back-up.<br/><br/> Bestand, map, volume herstellen.
**Directe back-ups van Linux-machine met de MARS-agent** | Back-up niet ondersteund.
**Back-up naar DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar de lokale DPM-opslag. DPM voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.
**Terug naar MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar de lokale opslag van MABS. MABS voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.


## <a name="azure-vm-backup-support"></a>Azure VM-back-upondersteuning

### <a name="azure-vm-limits"></a>Limieten voor Azure VM's

**Limiet** | **Details**
--- | ---
Azure VM-gegevensschijven | Limiet van 16.
Grootte Azure VM-gegevensschijven | Afzonderlijke schijf mag maximaal 4095 GB zijn.


### <a name="azure-vm-backup-options"></a>Azure VM-back-upopties

Dit wordt ondersteund als u een back-up wilt maken van Azure VM's.

**Machine** | **Een back-up** | **Locatie** | **Functies**
--- | --- | --- | ---
**Azure VM backup VM-extensie gebruiken** | Hele virtuele machine | Back-up naar de kluis | De extensie wordt geïnstalleerd wanneer u een back-up voor een virtuele machine inschakelen.<br/><br/> Back-up eenmaal per dag.<br/><br/> App-bewuste back-up voor Windows-VM's, bestandsconsistente back-up voor virtuele Linux-machines. U kunt app-consistentie configureren voor Linux-machines met behulp van aangepaste scripts.<br/><br/> Virtuele machine/schijf herstellen.<br/><br/> Kan geen back-up maken op Azure VM naar een on-premises locatie.
**Azure VM backup met behulp van de MARS-agent** | Bestanden en mappen | Back-up naar de kluis | Back-up drie keer per dag.<br/><br/> De MARS-agent kunt uitvoeren samen met de VM-extensie als u wilt back-up van bepaalde bestanden/mappen in plaats van de hele virtuele machine.
**Azure VM met DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-up naar de lokale opslag van virtuele Azure-machine met DPM. DPM voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/>. Oracle wordt niet ondersteund.
**Azure VM met MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens. | Back-ups op lokale opslag van virtuele machine van Azure MABS. MABS voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/> Oracle wordt niet ondersteund.





## <a name="linux-backup-support"></a>Ondersteuning voor Linux-back-ups

Dit wordt ondersteund als u een back-up wilt maken van Linux-machines.

**Een back-up maken** | **Linux (goedgekeurd door Azure)**
--- | ---
**Directe back-ups van on-premises machine waarop Linux wordt uitgevoerd**. | Nee. De MARS-agent kan alleen worden geïnstalleerd op Windows-machines.
**Back-up van virtuele Azure-machine waarop Linux wordt uitgevoerd (agent-extensie)** | App-consistente back-up met behulp van [aangepaste scripts](backup-azure-linux-app-consistent.md).<br/><br/> Herstel op bestandsniveau.<br/><br/> Herstellen door het maken van een VM vanaf een herstelpunt of schijf.
**Back-up van on-premises of virtuele Azure-machine waarop Linux met behulp van DPM wordt uitgevoerd** | Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/><br/> Linux-gast-VM's herstellen op Hyper-V- en VMware</br></br> Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines
**Maak een back-up van on-premises machine/Azure virtuele machine waarop Linux MABS wordt uitgevoerd** | Bestandsconsistente back-up van Linux-gast-VM's op Hyper-V en VMware<br/><br/> Linux-gast-VM's herstellen op Hyper-V- en VMware</br></br> Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines.

## <a name="daylight-saving-support"></a>Zomertijd en ondersteuning

Azure Backup biedt geen ondersteuning voor automatische clock correctie voor zomer-en wintertijd wijzigingen voor back-ups van virtuele Azure-machine. Back-upbeleid handmatig zo nodig wijzigen.


## <a name="disk-deduplication-support"></a>Ondersteuning voor Ontdubbeling van schijf

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

Backup biedt ondersteuning voor de compressie van back-upverkeer, zoals samengevat in de onderstaande tabel. 

- Voor Azure VM's leest de VM-extensie de gegevens rechtstreeks vanuit het Azure Storage-account via het opslagnetwerk. Dit verkeer hoeft daarom niet te worden gecomprimeerd.
- Als u DPM of MABS gebruikt, kunt u de gegevens comprimeren voordat er een back-up van wordt gemaakt in DPM/MABS, om bandbreedte te besparen.

**Machine** | **Comprimeren naar MABS/DPM (TCP)** | **Comprimeren (HTTPS) naar kluis**
--- | --- | ---
**Directe back-ups van on-premises Windows-machines** | N.v.t. | Ja
**Back-up van virtuele Azure-machines met VM-extensie** | N.v.t. | N.v.t.
** Back-up op on-premises/Azure-machines met behulp van MABS/DPM | ![Ja][green] | ![Ja][green]



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

- [Ondersteuningsmatrix voor revisie](backup-support-matrix-iaas.md) voor Azure VM backup.


[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
