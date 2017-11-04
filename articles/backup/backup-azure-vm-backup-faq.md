---
title: Veelgestelde vragen over Azure VM Backup | Microsoft Docs
description: Antwoorden op veelgestelde vragen over de werking van Azure VM Backup, over de beperkingen en over wat er gebeurt wanneer het beleid wordt gewijzigd
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: azure vm backup, virtuele azure-machines herstellen, back-upbeleid
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: d3178413e894c095235dde067b369e3554375aa6
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Vragen over de Azure VM Backup-service
In dit artikel vindt u antwoorden op veelgestelde vragen om u snel een beeld te geven van de verschillende onderdelen van Azure VM Backup. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## <a name="configure-backup"></a>Back-up configureren
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Ondersteunen Recovery Services-kluizen klassieke virtuele machines of virtuele machines op basis van Resource Manager? <br/>
Recovery Services-kluizen ondersteunen beide modellen.  U kunt een back-up naar een Recovery Services-kluis maken van een klassieke virtuele machine die in de klassieke portal is gemaakt of van een virtuele Resource Manager-machine die in Azure Portal is gemaakt.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Welke configuraties worden niet ondersteund door Azure VM backup?
Doorloop [ondersteunde besturingssystemen](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) en [beperkingen van VM-back-up](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Waarom zie ik mijn virtuele machine niet in de wizard Back-up configureren?
In de wizard Back-up configureren, Azure Backup alleen een lijst met virtuele machines die zijn:
  * Nog niet beveiligd kunt u de back-status van een virtuele machine controleren door te gaan naar de blade VM en controleren van de status van de back-up van het Menu instellingen. Meer informatie over [hoe u de back-upstatus van een virtuele machine controleert](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
  * Tot dezelfde regio behoren als de virtuele machine

## <a name="backup"></a>Back-up maken
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Geldt voor back-uptaken op aanvraag hetzelfde bewaarschema als voor geplande back-ups?
Nee. U moet de bewaartermijn voor een back-uptaak op aanvraag opgeven. Standaard wordt deze 30 dagen bewaard wanneer geactiveerd vanuit de portal. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ik heb onlangs Azure Disk Encryption ingeschakeld op een aantal virtuele machines. Worden mijn back-ups gewoon uitgevoerd?
U moet de Azure Backup-service toegang verlenen tot Key Vault. De hiervoor benodigde machtigingen kunt u in PowerShell opgeven door de stappen te volgen die worden beschreven in het gedeelte *Back-up inschakelen* van de documentatie voor [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Ik heb schijven van een virtuele machine gemigreerd naar beheerde schijven. Worden mijn back-ups gewoon uitgevoerd?
Ja, back-ups naadloos werken en u hoeft geen back-up opnieuw configureren. 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Mijn VM is afgesloten. Maakt op aanvraag of een geplande back-up werk?
Ja. Zelfs wanneer een machine is afgesloten back-ups werken en het herstelpunt dat is gemarkeerd als Crash consistent. Voor meer informatie, Zie de sectie van de consistentie gegevens in [in dit artikel](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

## <a name="restore"></a>Herstellen
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Hoe bepaal ik of ik beter afzonderlijke schijven kan herstellen of de virtuele machine als geheel?
Azure volledig VM herstel beschouwen als een optie snel maken. Wijzigingen van VM-optie de namen van containers die worden gebruikt door deze schijven, openbare IP-adressen en netwerkinterfacenamen schijven herstellen. De wijziging is vereist voor het onderhouden van de uniekheid van resources die zijn gemaakt tijdens het maken van de virtuele machine. Maar de virtuele machine wordt niet toevoegen aan beschikbaarheidsset. 

Gebruik de optie voor het herstellen van afzonderlijke schijven voor het volgende:
  * De virtuele machine die wordt gemaakt van een punt in tijd configuratie zoals het wijzigen van de grootte aanpassen
  * Configuraties die niet aanwezig op het moment van back-up toevoegen 
  * De naamgevingsconventie beheren voor de resources die worden gemaakt
  * De virtuele machine toevoegen aan de beschikbaarheidsset
  * Voor andere configuratie die alleen met behulp van PowerShell een declaratief Sjabloondefinitie kan worden bereikt
  
### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Kan ik back-ups van niet-beheerde schijf-VM gebruiken nadat ik mijn schijven te naar beheerde schijven upgraden herstellen?
Ja, kunt u de back-ups gemaakt voordat het migreren van de schijven van niet-begeleid naar beheerde. Standaard wordt de hersteltaak voor de virtuele machine een virtuele machine maken met niet-beheerde schijven. U kunt terugzetten schijven functionaliteit gebruiken bij het herstel van schijven en ze gebruiken voor het maken van een virtuele machine op de beheerde schijven. 

## <a name="manage-vm-backups"></a>Back-ups van uw virtuele machine beheren
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Wat gebeurt er wanneer ik het back-upbeleid voor een of meer virtuele machines wijzig?
Wanneer een nieuw beleid wordt toegepast op VM('s), wordt planning en retentie van het nieuwe beleid gevolgd. Als de bewaarperiode is uitgebreid, worden bestaande herstelpunten zodat ze volgens het beleid voor nieuwe gemarkeerd. Als bewaarperiode wordt beperkt, worden ze gemarkeerd voor verwijderen in de volgende taak opschonen en vervolgens verwijderd. 
