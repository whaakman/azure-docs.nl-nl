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
ms.openlocfilehash: 51bd4b935b32bea20d3f5de0b8cda62dfdbf07b8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57898717"
---
# <a name="azure-backup-support-matrix"></a>Ondersteuningsmatrix voor Azure Backup

U kunt [Azure Backup](backup-overview.md) naar back-ups naar de Microsoft Azure-cloud-platform. In dit artikel bevat een overzicht van de instellingen voor de algemene ondersteuning en beperkingen voor Azure Backup-scenario's en implementaties.

Andere matrices ondersteuning zijn beschikbaar:

- Ondersteuningsmatrix voor [back-up van virtuele Azure-machine (VM)](backup-support-matrix-iaas.md)
- Ondersteuningsmatrix voor back-up met behulp van [System Center Data Protection Manager (DPM) / Microsoft Azure Backup Server (MABS)](backup-support-matrix-mabs-dpm.md)
- Ondersteuningsmatrix voor back-up met behulp van de [Microsoft Azure Recovery Services agent (MARS)](backup-support-matrix-mars-agent.md)

## <a name="vault-support"></a>Ondersteuning voor kluizen

Azure Backup maakt gebruik van Recovery Services-kluizen te coördineren en beheren van back-ups. Kluizen worden ook gebruikt voor het opslaan van back-upgegevens. 

De volgende tabel beschrijft de functies van Recovery Services-kluizen:

**Functie** | **Details**
--- | ---
**Kluizen in het abonnement** | Maximaal 500 Recovery Services-kluizen in één abonnement.
**Machines in een kluis** | Maximaal 1000 virtuele Azure-machines in een enkele kluis.<br/><br/> Maximaal 50 MABS kunnen de servers in een enkele kluis worden geregistreerd.
**Gegevensbronnen in de kluis opslag** | Maximale 54,400 GB. Er is geen limiet voor back-ups van virtuele Azure-machines.
**Back-ups naar de kluis** | **Azure VM's:** Eenmaal per dag.<br/><br/>**Machines die zijn beveiligd door DPM/MABS:** Twee keer per dag.<br/><br/> **Machines een back-up direct met behulp van de MARS-agent:** Drie keer per dag. 
**Back-ups tussen kluizen** | Back-up is binnen een regio.<br/><br/> U moet een kluis in elke Azure-regio die u back wilt-up van virtuele machines bevat. U kunt geen back-up naar een andere regio. 
**Verplaatsen van kluizen** | U kunt [verplaatsen van kluizen](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) meerdere abonnementen of tussen resourcegroepen in hetzelfde abonnement.
**Gegevens verplaatsen tussen kluizen** | Een back-up gegevens verplaatsen tussen kluizen wordt niet ondersteund.
**Opslagtype vault wijzigen** | U kunt het replicatietype storage (geografisch redundante opslag of lokaal redundante opslag) voor een kluis wijzigen voordat de back-ups worden opgeslagen. Nadat een back-ups in de kluis is begonnen, kan het replicatietype niet meer worden gewijzigd.

## <a name="on-premises-backup-support"></a>On-premises ondersteuning voor back-ups

Dit is wat wordt ondersteund als u wilt back-up van on-premises computers:

**Machine** | **Wat de back-up** | **Locatie** | **Functies**
--- | --- | --- | ---
**Directe back-ups van Windows-machine met de MARS-agent** | Bestanden, mappen, systeemstatus | Back-up naar de Recovery Services-kluis. | Back-up van drie keer per dag<br/><br/> Geen app-bewuste back-up<br/><br/> Bestand, map, volume herstellen
**Directe back-ups van Linux-machine met de MARS-agent** | Back-up niet ondersteund
**Back-up naar DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens | Back-up naar de lokale DPM-opslag. DPM voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel<br/><br/> Linux ondersteund voor virtuele machines (Hyper-V-/ VMware)<br/><br/> Oracle niet ondersteund
**Back-up naar MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens | Back-up naar de lokale opslag van MABS. MABS voert vervolgens een back-up uit naar de kluis. | App-bewuste momentopnamen<br/><br/> Volledige granulariteit voor back-up en herstel<br/><br/> Linux ondersteund voor virtuele machines (Hyper-V-/ VMware)<br/><br/> Oracle niet ondersteund

## <a name="azure-vm-backup-support"></a>Azure VM-back-upondersteuning

### <a name="azure-vm-limits"></a>Limieten voor Azure VM's

**Limiet** | **Details**
--- | ---
**Azure VM-gegevensschijven** | Limiet van 16
**Azure schijfgrootte van de VM-gegevens** | Afzonderlijke schijven mag maximaal 4095 GB

### <a name="azure-vm-backup-options"></a>Azure VM-back-upopties

Dit is wat wordt ondersteund als u wilt back-up van virtuele Azure-machines:

**Machine** | **Wat de back-up** | **Locatie** | **Functies**
--- | --- | --- | ---
**Azure VM backup met behulp van de VM-extensie** | Hele virtuele machine | Back-up naar de kluis. | De extensie wordt geïnstalleerd wanneer u een back-up voor een virtuele machine inschakelen.<br/><br/> Back-up eenmaal per dag.<br/><br/> App-bewuste back-up voor virtuele Windows-machines; bestandsconsistente back-up voor virtuele Linux-machines. U kunt app-consistentie voor Linux-machines configureren met behulp van aangepaste scripts.<br/><br/> VM of schijf herstellen.<br/><br/> Kan geen back-up van een Azure-VM naar een on-premises locatie.
**Azure VM backup met behulp van de MARS-agent** | Bestanden en mappen | Back-up naar de kluis. | Back-up drie keer per dag.<br/><br/> Als u back-up van specifieke bestanden of mappen in plaats van de hele virtuele machine wilt, worden de MARS-agent kunt uitvoeren samen met de VM-extensie.
**Azure VM met DPM** | Bestanden, mappen, volumes, systeemstatus, app-gegevens | Back-up naar de lokale opslag van Azure-VM die door DPM wordt uitgevoerd. DPM voert vervolgens een back-up uit naar de kluis. | App-gerichte momentopnamen.<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/> Oracle wordt niet ondersteund.
**Azure VM met MABS** | Bestanden, mappen, volumes, systeemstatus, app-gegevens | Back-up naar de lokale opslag van Azure-VM met MABS. MABS voert vervolgens een back-up uit naar de kluis. | App-gerichte momentopnamen.<br/><br/> Volledige granulariteit voor back-up en herstel.<br/><br/> Linux wordt ondersteund voor virtuele machines (Hyper-V-/VMware).<br/><br/> Oracle wordt niet ondersteund.

## <a name="linux-backup-support"></a>Ondersteuning voor Linux-back-ups

Dit is wat wordt ondersteund als u wilt back-up van Linux-machines:

**Type back-up** | **Linux (goedgekeurd door Azure)**
--- | ---
**Directe back-ups van on-premises machine waarop Linux wordt uitgevoerd** | Wordt niet ondersteund. De MARS-agent kan alleen worden geïnstalleerd op Windows-machines.
**Met behulp van de agent-extensie voor back-up van virtuele Azure-machine waarop Linux wordt uitgevoerd** | App-consistente back-up met behulp van [aangepaste scripts](backup-azure-linux-app-consistent.md).<br/><br/> Herstel op bestandsniveau.<br/><br/> Herstellen door het maken van een VM vanaf een herstelpunt of schijf.
**Met behulp van DPM back-up on-premises of virtuele Azure-machine waarop Linux wordt uitgevoerd** | Bestandsconsistente back-up van Linux Gast-VM's op Hyper-V en VMWare.<br/><br/> Herstel van de virtuele machine van Hyper-V en VMWare Linux Gast-VM's.<br/><br/> Bestandsconsistente back-up niet beschikbaar voor virtuele Azure-machine.
**Met behulp van MABS back-up on-premises computer of virtuele Azure-machine waarop Linux wordt uitgevoerd** | Bestandsconsistente back-up van Linux Gast-VM's op Hyper-V en VMWare.<br/><br/> Herstel van de virtuele machine van Hyper-V en VMWare Linux Gast-VM's.<br/><br/> Bestandsconsistente back-up is niet beschikbaar voor virtuele Azure-machines.

## <a name="daylight-saving-time-support"></a>Ondersteuning voor zomer-en wintertijd

Azure Backup biedt geen ondersteuning voor automatische clock correctie voor zomer-/ wintertijd voor back-ups van virtuele Azure-machine. Back-upbeleid handmatig zo nodig wijzigen.

## <a name="disk-deduplication-support"></a>Ondersteuning voor Ontdubbeling van schijf

Ondersteuning voor schijfontdubbeling is als volgt:

- Schijfontdubbeling is ondersteunde on-premises wanneer u DPM- of MABs voor back-up van Hyper-V-machines met Windows. Windows Server voert gegevensontdubbeling (op hostniveau) op virtuele harde schijven (VHD's) die zijn gekoppeld aan de virtuele machine als back-upopslag.
- Ontdubbeling wordt niet ondersteund in Azure voor Backup-onderdelen. Wanneer DPM en MABS zijn geïmplementeerd in Azure, kan het storage-schijven die zijn gekoppeld aan de virtuele machine kunnen niet worden ontdubbeld.

## <a name="security-and-encryption-support"></a>Ondersteuning voor beveiliging en versleuteling

Azure Backup ondersteunt versleuteling voor in-transit en inactieve gegevens.

### <a name="network-traffic-to-azure"></a>Netwerkverkeer naar Azure

- Back-upverkeer van servers naar de Recovery Services-kluis wordt versleuteld met Advanced Encryption Standard 256.
- Back-upgegevens worden verzonden via een beveiligde HTTPS-koppeling.
- Back-upgegevens is opgeslagen in de Recovery Services-kluis in een versleutelde vorm.
- Alleen u beschikt over de wachtwoordzin waarmee deze gegevens kunnen worden ontgrendeld. De back-upgegevens kunnen niet door Microsoft ontsleuteld.

    > [!WARNING]
    > Nadat u de kluis hebt ingesteld, hebt alleen u toegang tot de versleutelingssleutel. Microsoft bewaart nooit een kopie en heeft geen toegang tot de sleutel. Als de sleutel verkeerd wordt geplaatst, kan Microsoft de back-upgegevens niet herstellen.

### <a name="data-security"></a>Gegevensbeveiliging

- Wanneer u een back-up van virtuele Azure-machines, moet u het instellen van versleuteling *binnen* de virtuele machine.
- Azure Backup biedt ondersteuning voor Azure Disk Encryption, dat gebruikmaakt van BitLocker op virtuele Windows-machines en **dm-crypt** op virtuele Linux-machines.
- Op de back-end Azure Backup gebruikt [Azure Storage-Serviceversleuteling](../storage/common/storage-service-encryption.md), data-at-rest te beschermen.

**Machine** | **In-transit** | **Inactief**
--- | --- | ---
**On-premises Windows-computers zonder DPM/MABS** | ![Ja][green] | ![Ja][green]
**Azure VM's** | ![Ja][green] | ![Ja][green]
**On-premises Windows-machines of virtuele Azure-machines met DPM** | ![Ja][green] | ![Ja][green]
**On-premises Windows-machines of virtuele Azure-machines met MABS** | ![Ja][green] | ![Ja][green]

## <a name="compression-support"></a>Ondersteuning voor compressie

Backup biedt ondersteuning voor de compressie van het back-verkeer, zoals samengevat in de volgende tabel.

- Voor Azure VM's leest de VM-extensie de gegevens rechtstreeks vanuit de Azure storage-account via het opslagnetwerk, dus het is niet nodig voor het comprimeren van dit verkeer.
- Als u DPM- of MABS gebruikt, kunt u bandbreedte besparen door wanneer de gegevens worden gecomprimeerd voordat er een back-wordt gemaakt.

**Machine** | **Comprimeren naar MABS/DPM (TCP)** | **Comprimeren naar de kluis (HTTPS)**
--- | --- | ---
**Directe back-ups van on-premises Windows-machines** | N.v.t. | ![Ja][green]
**Back-up van virtuele Azure-machines met behulp van de VM-extensie** | N.v.t. | N.v.t.
**Back-up op de on-premises/Azure-machines met behulp van MABS/DPM** | ![Ja][green] | ![Ja][green]

## <a name="retention-limits"></a>Bewaarlimieten

**Instelling** | **Limieten**
--- | ---
**Maximum aantal herstelpunten per beveiligd exemplaar (machine of workload)** | 9,999
**Max verlooptijd voor een herstelpunt** | Geen limiet
**Maximum aantal back-upfrequentie naar DPM/MABS** | Om de 15 minuten voor SQL Server<br/><br/> Eenmaal per uur voor andere werkbelastingen
**Maximale frequentie van back-up naar de kluis** | **On-premises Windows-machines of virtuele Azure-machines MARS uitgevoerd:** Drie per dag<br/><br/> **DPM/MABS:** Twee per dag<br/><br/> **Azure VM backup:** Een per dag
**Bewaarperiode voor herstelpunten** | Dagelijks, wekelijks, maandelijks, jaarlijks
**Maximale bewaarperiode** | Afhankelijk van back-upfrequentie
**Herstelpunten op DPM/MABS-schijf** | 64 voor bestandsservers; 448 voor appservers <br/><br/>Onbeperkte tapeherstelpunten die voor on-premises DPM

## <a name="next-steps"></a>Volgende stappen

- [Ondersteuningsmatrix voor revisie](backup-support-matrix-iaas.md) voor Azure VM backup.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
