
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
ms.topic: get-started-article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 8c06a90f58cf56ebb4e75e7567e237de7414a300
ms.contentlocale: nl-nl
ms.lasthandoff: 07/20/2017

---
# <a name="questions-about-the-azure-vm-backup-service"></a>Vragen over de Azure VM Backup-service
In dit artikel vindt u antwoorden op veelgestelde vragen om u snel een beeld te geven van de verschillende onderdelen van Azure VM Backup. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## <a name="configure-backup"></a>Back-up configureren
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Ondersteunen Recovery Services-kluizen klassieke virtuele machines of virtuele machines op basis van Resource Manager? <br/>
Recovery Services-kluizen ondersteunen beide modellen.  U kunt een back-up naar een Recovery Services-kluis maken van een klassieke virtuele machine die in de klassieke portal is gemaakt of van een virtuele Resource Manager-machine die in Azure Portal is gemaakt.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Welke configuraties worden niet ondersteund door Azure VM Backup?
Lees de informatie over [ondersteunde besturingssystemen](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) en [beperkingen van Azure VM Backup](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Waarom zie ik mijn virtuele machine niet in de wizard Back-up configureren?
In de wizard Back-up configureren geeft Azure Backup alleen virtuele machines weer die:
* Nog niet zijn beveiligd. U kunt de back-upstatus van een virtuele machine controleren door naar de blade van de betreffende virtuele machine te gaan en in het menu Instellingen op de blade de back-upstatus te bekijken. Meer informatie over [hoe u de back-upstatus van een virtuele machine controleert](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
* Tot dezelfde regio behoren als de virtuele machine

## <a name="backup"></a>Back-up maken
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Geldt voor back-uptaken op aanvraag hetzelfde bewaarschema als voor geplande back-ups?
Nee. U moet de bewaartermijn voor een back-uptaak op aanvraag afzonderlijk opgeven. Bij activering vanuit de portal wordt een dergelijke taak standaard 30 dagen bewaard. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ik heb onlangs Azure Disk Encryption ingeschakeld op een aantal virtuele machines. Worden mijn back-ups gewoon uitgevoerd?
U moet de Azure Backup-service toegang verlenen tot Key Vault. De hiervoor benodigde machtigingen kunt u in PowerShell opgeven door de stappen te volgen die worden beschreven in het gedeelte *Back-up inschakelen* van de documentatie voor [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Ik heb schijven van een virtuele machine gemigreerd naar beheerde schijven. Worden mijn back-ups gewoon uitgevoerd?
Ja, uw back-ups blijven normaal werken. U hoeft ze niet opnieuw te configureren. 

## <a name="restore"></a>Herstellen
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Hoe bepaal ik of ik beter afzonderlijke schijven kan herstellen of de virtuele machine als geheel?
Met de Azure-functie voor het herstellen van de virtuele machine als geheel hebt u snel de beschikking over een herstelde virtuele machine. Bij gebruik van deze optie worden echter wel de namen van schijven, de door de schijven gebruikte containers, de openbare IP-adressen en de netwerkinterfacenamen van de gemaakte resources gewijzigd. Bovendien wordt de virtuele machine niet toegevoegd aan de beschikbaarheidsset. 

Gebruik de optie voor het herstellen van afzonderlijke schijven voor het volgende:
* De virtuele machine die vanuit de punt-in-tijd-configuratie wordt gemaakt, aanpassen door bijvoorbeeld de grootte te wijzigen die in de back-upconfiguratie is vastgelegd
* Configuraties toevoegen die op het moment waarop de back-up werd gemaakt, niet aanwezig waren 
* De naamgevingsconventie beheren voor de resources die worden gemaakt
* De virtuele machine toevoegen aan de beschikbaarheidsset
* U hebt een configuratie die alleen kan worden uitgevoerd met PowerShell of een declaratieve sjabloondefinitie

## <a name="manage-vm-backups"></a>Back-ups van uw virtuele machine beheren
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Wat gebeurt er wanneer ik het back-upbeleid voor een of meer virtuele machines wijzig?
Wanneer er op een of meer virtuele machines een nieuw beleid wordt toegepast, worden het schema en de retentie van het nieuwe beleid gevolgd. Als de retentie is uitgebreid, worden de bestaande herstelpunten gemarkeerd voor bewaring conform het nieuwe beleid. Als de retentie is beperkt, worden de bestaande herstelpunten gemarkeerd voor verwijdering in de eerstvolgende opschoontaak. Bij uitvoering van deze taak worden ze verwijderd. 

