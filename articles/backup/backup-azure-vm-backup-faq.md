---
title: Veelgestelde vragen over back-ups van virtuele Azure-machines met Azure Backup
description: Antwoorden op veelgestelde vragen over back-ups van virtuele Azure-machines met Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: sogup
ms.openlocfilehash: 9f233af316bd6022b93a7208bf3fae37e913e6af
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885261"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Veelgestelde vragen-Back-up van virtuele Azure-machines

In dit artikel vindt u antwoorden op veelgestelde vragen over back-ups van virtuele Azure-machines met de [Azure Backup](backup-introduction-to-azure-backup.md) service.


## <a name="backup"></a>Backup

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>Welke VM-installatiekopieën kunnen worden ingeschakeld voor back-up wanneer ik ze maken?
Wanneer u een virtuele machine maakt, kunt u back-up inschakelen voor virtuele machines met [ondersteunde besturingssystemen](backup-support-matrix-iaas.md#supported-backup-actions)
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>Zijn de back-up kosten opgenomen in de VM-kosten? 

Nee. Back-kosten zijn gescheiden van de kosten van een virtuele machine. Meer informatie over [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>Welke machtigingen zijn vereist voor het inschakelen van back-up voor een virtuele machine? 

Als u een VM-Inzender bent, kunt u back-up op de virtuele machine inschakelen. Als u een aangepaste rol, moet u de volgende machtigingen voor de back-up op de virtuele machine inschakelen: 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Als uw Recovery Services-kluis en de virtuele machine hebt verschillende resourcegroepen bevinden, zorg er dan voor dat u hebt schrijfmachtigingen in de resourcegroep voor de Recovery Services-kluis.  


### <a name="what-azure-vms-can-you-back-up-using-azure-backup"></a>Welke Azure-VM's kunt u back-up maken met Azure Backup?

Controleer de [ondersteuningsmatrix](backup-support-matrix-iaas.md) voor details van bestandsondersteuning en beperkingen.

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>Maakt een on-demand back-uptaak het hetzelfde bewaarschema als geplande back-ups gebruik?
Nee. Geef de bewaartermijn voor een back-uptaak op aanvraag. Standaard wel 30 dagen worden bewaard wanneer ze worden geactiveerd via de portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ik heb onlangs Azure Disk Encryption ingeschakeld op een aantal virtuele machines. Worden mijn back-ups gewoon uitgevoerd?
Machtigingen voor Azure Backup toegang tot Key Vault opgeven. De machtigingen opgeven in PowerShell, zoals beschreven in de **back-up inschakelen** sectie de [Azure Backup PowerShell](backup-azure-vms-automation.md) documentatie.

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Ik VM-schijven gemigreerd naar managed disks. Worden mijn back-ups gewoon uitgevoerd?
Ja, back-ups werken naadloos. Er is niet nodig om alles opnieuw te configureren.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Waarom zie ik niet mijn virtuele machine in de wizard Back-up configureren?
De wizard worden alleen virtuele machines in dezelfde regio bevinden als de kluis en die niet worden al back-up.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Mijn virtuele machine wordt afgesloten. Maakt een on-demand of een geplande back-werk?
Ja. Back-ups uitgevoerd wanneer een virtuele machine wordt afgesloten. Het herstelpunt dat is gemarkeerd als crash consistent.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Kan ik een back-uptaak in uitvoering annuleren?
Ja. U kunt back-uptaak in annuleren een **duurt momentopname** staat. U kunt een taak niet annuleren als de overdracht van gegevens vanuit de momentopname uitgevoerd wordt.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprggeonumber-will-my-backups-continue-to-work"></a>Ik ingeschakeld vergrendelen op de resourcegroep hebt gemaakt met Azure Backup-Service (zoals) `AzureBackupRG_<geo>_<number>`), blijven mijn back-ups werken?
Wanneer u de resourcegroep hebt gemaakt met Azure Backup-Service, back-ups niet gestart omdat er een maximumlimiet van 18 herstelpunten.

Gebruiker moet Verwijder de vergrendeling en schakelt u de verzameling voor herstelpunt in die resourcegroep zodat de toekomstige back-ups lukt, [als volgt te werk](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) de herstelpuntverzameling verwijderen.

### <a name="does-the-backup-policy-consider-daylight-saving-time-dst"></a>Het back-upbeleid wordt rekening gehouden met zomertijd (Zomertijd)?
Nee. De datum en tijd op uw lokale computer is lokaal met huidige zomertijd toegepast. De tijd die is ingesteld voor de geplande back-ups kan verschillen van de lokale tijd vanwege de Zomertijd.

### <a name="how-many-data-disks-can-i-attach-to-a-vm-backed-up-by-azure-backup"></a>Het aantal gegevensschijven kan ik koppelen aan een virtuele machine back-up gemaakt door Azure Backup?
Azure Backup kunt back-up van virtuele machines met maximaal 16 schijven. Ondersteuning voor 16 schijven wordt aangeboden in de [direct herstellen](backup-instant-restore-capability.md).

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Wordt Azure back-upondersteuning SSD beheerde standaardschijven?
Azure Backup ondersteunt [standard-SSD-beheerde schijven](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). SSD-beheerde schijven bieden een nieuw type duurzame opslag voor virtuele Azure-machines. Ondersteuning voor SSD beheerde schijven wordt aangeboden in de [direct herstellen](backup-instant-restore-capability.md).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Kunnen we back-up van een virtuele machine met een schijf Write Accelerator WA ingeschakeld?
Momentopnamen kunnen niet worden uitgevoerd op de schijf WA ingeschakeld. De Azure Backup-service kunt echter de WA ingeschakeld schijf uitsluiten van back-up. Uitsluiting van de schijf voor virtuele machines met schijven WA-functionaliteit wordt alleen ondersteund voor abonnementen die zijn bijgewerkt naar direct herstellen.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Een virtuele machine met Write Accelerator (WA) schijven, en SAP HANA zijn geïnstalleerd. Hoe maak ik een back-up?
Azure Backup kan geen back-up van de schijf WA is ingeschakeld, maar kunt uitsluiten van back-up. De back-up wordt niet evenwel consistentie van de database omdat de informatie op de schijf WA-functionaliteit is niet een back-up. U kunt back-up van schijven met deze configuratie als u wilt dat de besturingssysteemschijf back-ups en back-ups van schijven die niet WA ingeschakeld.

We uitvoert beperkte preview voor back-up van een SAP HANA met een RPO van 15 minuten. Het gebouwd op een soortgelijke manier SQL DB back-up maken en de backInt-interface wordt gebruikt voor oplossingen van derden gecertificeerd door SAP HANA. Als u geïnteresseerd bent, een e-mail verzenden naar `AskAzureBackupTeam@microsoft.com` met het onderwerp **zich aanmelden voor een beperkte preview voor back-up van SAP HANA in virtuele Azure-machines**.


## <a name="restore"></a>Herstellen

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Hoe bepaal ik of alleen schijven herstellen of een volledige virtuele machine?
Zien van een virtuele machine als geheel als de optie voor een snelle versie maken voor een Azure-VM. Deze optie wijzigt de namen van de schijf, containers die worden gebruikt door de schijven, het openbare IP-adressen en de netwerkinterfacenamen. De wijziging houdt unieke bronnen wanneer een virtuele machine wordt gemaakt. De virtuele machine is niet toegevoegd aan een beschikbaarheidsset.

Als u wilt, kunt u de optie voor terugzetten schijf gebruiken:
  * Aanpassen van de virtuele machine die wordt gemaakt. Bijvoorbeeld, de grootte wijzigen.
  * Configuratieinstellingen die niet er op het moment van back-up waren toevoegen.
  * Beheer de naamgevingsconventie voor resources die zijn gemaakt.
  * De virtuele machine toevoegen aan een beschikbaarheidsset.
  * Alle andere instellingen die moet worden geconfigureerd met behulp van PowerShell of een sjabloon toevoegen.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Kan ik back-ups van niet-beheerde VM-schijven herstellen nadat ik een upgrade naar managed disks uitvoeren?
Ja, kunt u back-ups uitgevoerd voordat de schijven zijn gemigreerd vanuit niet-beheerde beheerd.
- Een hersteltaak van de virtuele machine maakt standaard een niet-beheerde virtuele machine.
- U kunt echter herstel de schijven en gebruiken voor het maken van een beheerde virtuele machine.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>Hoe kan ik een virtuele machine herstellen naar een herstelpunt voordat de virtuele machine is gemigreerd naar managed disks?
Een hersteltaak van de virtuele machine maakt standaard een virtuele machine met niet-beheerde schijven. Een virtuele machine maken met beheerde schijven:
1. [Herstellen naar niet-beheerde schijven](tutorial-restore-disk.md#restore-a-vm-disk).
2. [De herstelde schijven converteren naar managed disks](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk).
3. [Een virtuele machine maken met beheerde schijven](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

[Meer informatie](backup-azure-vms-automation.md#restore-an-azure-vm) hierover in PowerShell.

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>Kan ik de virtuele machine die wordt verwijderd herstellen?
Ja. Zelfs als u de virtuele machine hebt verwijderd, gaat u naar de bijbehorende back-up item in de kluis en deze herstellen vanaf een herstelpunt.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>Hiermee stelt u een virtuele machine herstellen op de beschikbaarheid van dezelfde?
Door op te geven van een optie in de sjabloon tijdens het herstellen van beheerde schijven is terugzetten naar de beschikbaarheidssets voor beheerde schijf Azure VM ingeschakeld. Deze sjabloon bevat de invoerparameter **beschikbaarheidssets**.

### <a name="how-do-we-get-faster-restore-performances"></a>Hoe krijgen we de prestaties voor sneller herstellen?
Voor nog betere prestaties voor herstel, zijn we verplaatsen naar [direct herstellen](backup-instant-restore-capability.md) mogelijkheid.

## <a name="manage-vm-backups"></a>Back-ups van uw virtuele machine beheren

### <a name="what-happens-if-i-modify-a-backup-policy"></a>Wat gebeurt er als ik een back-upbeleid wijzigen?
De VM een back-up met behulp van het schema en de retentie-instellingen in het gewijzigde of nieuwe beleid.

- Als de retentie is uitgebreid, worden de bestaande herstelpunten gemarkeerd en blijven in overeenstemming met het nieuwe beleid.
- Als de retentie is beperkt, worden herstelpunten gemarkeerd voor verwijdering in de eerstvolgende opschoontaak en vervolgens verwijderd.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>Hoe kan ik een virtuele machine back-ups van door Azure Backup naar een andere resourcegroep verplaatsen?

1. Tijdelijk de back-up stoppen en back-upgegevens behouden.
2. Verplaats de virtuele machine naar de doelresourcegroep.
3. Opnieuw worden ingeschakeld back-up in de dezelfde of een nieuwe kluis.

U kunt de virtuele machine herstellen met beschikbare herstelpunten die zijn gemaakt voordat de bewerking voor verplaatsen.
