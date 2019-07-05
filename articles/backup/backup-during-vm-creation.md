---
title: Back-up inschakelen wanneer u een Azure-VM met Azure Backup maken
description: Beschrijft hoe u back-up in te schakelen wanneer u een Azure-VM met Azure Backup maken.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: raynew
ms.openlocfilehash: a19653f7ae3900fd7999f347ef4d3ef710be1430
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436341"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Back-up inschakelen wanneer u een Azure-VM maakt

De Azure Backup-service gebruiken voor back-up van virtuele Azure-machines (VM's). Virtuele machines back-ups volgens een schema dat is opgegeven in een back-upbeleid en herstelpunten worden gemaakt op basis van de back-ups. Herstelpunten worden opgeslagen in Recovery Services-kluizen.

Dit artikel wordt uitgelegd hoe u back-up inschakelen wanneer u een virtuele machine (VM) maakt in Azure portal.  

## <a name="before-you-start"></a>Voordat u begint

- [Controleer](backup-support-matrix-iaas.md#supported-backup-actions) welke besturingssystemen worden ondersteund als u back-up inschakelen wanneer u een virtuele machine maakt.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u nog niet hebt aangemeld bij uw account, aanmelden bij de [Azure-portal](https://portal.azure.com).

## <a name="create-a-vm-with-backup-configured"></a>Een virtuele machine maken met back-up geconfigureerd

1. Klik in Azure portal op **een resource maken**.

2. Klik in de Azure Marketplace, **Compute**, en selecteer vervolgens een VM-installatiekopie.

3. Instellen van de virtuele machine in overeenstemming met de [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) of [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) instructies.

4. Op de **Management** tabblad, in **back-up inschakelen**, klikt u op **op**.
5. Azure Backup back-ups naar een Recovery Services-kluis. Klik op **nieuw** als u een bestaande kluis hebt.
6. Accepteer de voorgestelde kluisnaam of geef uw eigen.
7. Geef op of maak een resourcegroep waarin de kluis zich bevindt. De resource-group-kluis kan afwijken van de VM-resourcegroep.

    ![Back-up voor een virtuele machine inschakelen](./media/backup-during-vm-creation/enable-backup.png)

8. Accepteer het standaardbeleid voor back-up of wijzig de instellingen.
    - Een back-upbeleid geeft aan hoe vaak momentopnamen back-up van de virtuele machine, en hoe lang de back-ups.
    - Het standaardbeleid voor back-ups van de virtuele machine eenmaal per dag.
    - U kunt uw eigen back-upbeleid voor een Azure-VM voor het maken van back-ups dagelijks of wekelijks.
    - [Meer informatie](backup-azure-vms-introduction.md#backup-and-restore-considerations) over back-overwegingen voor Azure VM's.
    - [Meer informatie](backup-instant-restore-capability.md) over de instant functionaliteit te herstellen.

      ![Standaard back-upbeleid](./media/backup-during-vm-creation/daily-policy.png)


> [!NOTE]
> Azure Backup-service wordt een afzonderlijke resourcegroep (met uitzondering van de VM-resourcegroep) voor het opslaan van de momentopname, met de naamgevingsindeling **AzureBackupRG_geography_number** (voorbeeld: AzureBackupRG_northeurope_1). De gegevens die in deze resourcegroep bewaard voor de duur in dagen zoals opgegeven in *behouden met instant herstelmomentopname* sectie van de virtuele Machine back-up van Azure-beleid.  Toepassen van een vergrendeling op deze resourcegroep kan leiden tot mislukte back-ups.<br>
Deze resourcegroep moet ook worden uitgesloten van de naam/tag beperkingen als een restrictiebeleid voor het maken van de punt van de Resource-verzamelingen in deze opnieuw back-upfouten veroorzaakt blokkeren.


## <a name="start-a-backup-after-creating-the-vm"></a>Een back-up na het maken van de virtuele machine starten

De virtuele machine back-up wordt uitgevoerd in overeenstemming met uw back-upbeleid. We raden echter aan een eerste back-up uit te voeren.

Nadat de virtuele machine is gemaakt, het volgende doen:

1. Klik in de VM-eigenschappen op **back-up**. De status van de virtuele machine is eerste back-up in behandeling totdat de eerste back-up wordt uitgevoerd
2. Klik op **nu Back-up** om uit te voeren van een on-demand back-up.

    ![Een on-demand back-up uitvoeren](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Resource Manager-sjabloon gebruiken om te implementeren van een beveiligde virtuele machine

De vorige stappen wordt uitgelegd hoe de Azure portal gebruiken voor een virtuele machine maken en deze in een Recovery Services-kluis beveiligen. Voor het snel een of meer virtuele machines implementeren en ze beschermen in een Recovery Services-kluis, ziet u de sjabloon [een Windows-machine implementeert en back-up inschakelen](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).



## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt uw virtuele machine hebt beveiligd, informatie over het beheren en deze terugzetten.

- [Beheren en controleren van virtuele machines](backup-azure-manage-vms.md)
- [VM herstellen](backup-azure-arm-restore-vms.md)

Als u problemen ondervindt, [bekijken](backup-azure-vms-troubleshoot.md) de gids voor probleemoplossing.
