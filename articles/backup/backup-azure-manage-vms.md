---
title: Beheren en controleren van virtuele Azure-machines met behulp van de Azure Backup-service
description: Informatie over het beheren en controleren van virtuele Azure-machines met behulp van de Azure Backup-service.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sogup
ms.openlocfilehash: add2c72535b5be0edcbc00c077dfe20a6deaa3e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434198"
---
# <a name="manage-azure-vm-backups"></a>Back-ups van Azure-VM's beheren

In dit artikel wordt beschreven hoe u voor het beheren van virtuele Azure-machines (VM's) die worden ondersteund met behulp van de [Azure Backup-service](backup-overview.md). Het artikel bevat ook een overzicht van de back-informatie die kunt u vinden op het kluisdashboard.


In de Azure-portal, het dashboard van de Recovery Services-kluis biedt toegang tot kluis informatie, met inbegrip van:

* De meest recente back-up, dit ook de meest recente herstelpunt is.
* Het back-upbeleid.
* De totale grootte van alle back-momentopnamen.
* Het aantal virtuele machines die zijn ingeschakeld voor back-ups.

U kunt back-ups beheren met behulp van het dashboard en verkrijgen van toegang tot afzonderlijke virtuele machines. Als u wilt back-ups van de computer, open de kluis op het dashboard.

![Volledige dashboardweergave met schuifregelaar](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Virtuele machines op het dashboard weergeven

Virtuele machines op het kluisdashboard weergeven:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer in het menu Hub **Bladeren**. Typ in de lijst met resources **Recovery Services**. Terwijl u typt, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Recovery Services-kluizen**.

    ![Een Recovery Services-kluis maken](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Voor gebruiksgemak, met de rechtermuisknop op de kluis en selecteer **vastmaken aan dashboard**.
4. Open het dashboard van de kluis.

    ![Open het dashboard van de kluis en de blade instellingen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. Op de **back-Upitems** tegel, selecteer **Azure Virtual Machines**.

    ![Open de tegel back-Upitems](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. Op de **back-Upitems** blade vindt u de lijst van beveiligde VM's. In dit voorbeeld de kluis worden beveiligd door één virtuele machine: demobackup.  

    ![De blade back-Upitems weergeven](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. In de kluis-item-dashboard wijzigen van de back-upbeleid, uitvoeren van een on-demand back-up stoppen of hervatten van de beveiliging van virtuele machines, back-upgegevens verwijderen, herstelpunten weergeven en uitvoeren van een herstelpunt.

    ![Het back-Upitems-dashboard en de blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Back-upbeleid voor een virtuele machine beheren

Een back-upbeleid beheren:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/). Open het dashboard van de kluis.
2. Op de **back-Upitems** tegel, selecteer **Azure Virtual Machines**.

    ![Open de tegel back-Upitems](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. Op de **back-Upitems** blade kunt u de lijst van beveiligde virtuele machines en de status van laatste back-up met de meest recente punten hersteltijd weergeven.

    ![De blade back-Upitems weergeven](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. U kunt een back-upbeleid selecteren in de kluis-item-dashboard.

   * Om te schakelen van beleid, selecteert u een ander beleid en selecteer vervolgens **opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.

     ![Een back-upbeleid kiezen](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren
Nadat u de beveiliging hebt ingesteld, kunt u een on-demand back-up van een virtuele machine uitvoeren. Houd rekening met deze details:

- Als de eerste back-up in behandeling is, maakt back-up op aanvraag een volledige kopie van de virtuele machine in de Recovery Services-kluis.
- Als de eerste back-up voltooid is, verzendt een on-demand back-up alleen wijzigingen van de vorige momentopname naar de Recovery Services-kluis. Dat wil zeggen, zijn latere back-ups altijd incrementeel.
- De bewaartermijn voor een on-demand back-up is de waarde voor de bewaarperiode dat u opgeeft wanneer u de back-up activeren.

Voor het activeren van een on-demand back-up:

1. Op de [item kluisdashboard](#view-vms-on-the-dashboard)onder **beveiligde Item**, selecteer **back-upitem**.

    ![De back-up nu-optie](./media/backup-azure-manage-vms/backup-now-button.png)

2. Van **Type back-upbeheer**, selecteer **virtuele Azure-Machine**. De **back-upitem (Azure virtuele Machine)** blade wordt weergegeven.
3. Selecteer een virtuele machine en selecteer **nu back-up** een on-demand back-up maken. De **nu back-up** blade wordt weergegeven.
4. In de **bewaren back-up tot** veld, geeft u een datum voor de back-up moet worden bewaard.

    ![De agenda nu back-up](./media/backup-azure-manage-vms/backup-now-check.png)

5. Selecteer **OK** om uit te voeren van de back-uptaak.

Voor het volgen van de voortgang van de taak op het kluisdashboard, selecteer de **back-uptaken** tegel.

## <a name="stop-protecting-a-vm"></a>De beveiliging van een virtuele machine stoppen

Er zijn twee manieren om te stoppen met het beveiligen van een virtuele machine:

* **Stop de beveiliging en back-upgegevens behouden**. Deze optie worden alle toekomstige back-uptaken voor het beveiligen van uw virtuele machine; stoppen Azure Backup-service, maar de herstelpunten die back-ups behouden.  U moet betalen voor het behouden van de herstelpunten in de kluis (Zie [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/) voor meer informatie). U zult de virtuele machine kunt herstellen indien nodig. Als u wilt doorgaan met VM-beveiliging, dan kunt u *back-up hervatten* optie.
* **Stop de beveiliging en back-upgegevens verwijderen**. Deze optie worden alle toekomstige back-uptaken voor het beveiligen van uw virtuele machine stoppen en verwijderen van alle herstelpunten. Kunt u zich niet op de virtuele machine herstellen en niet gebruiken *back-up hervatten* optie.

>[!NOTE]
>Als u een gegevensbron verwijdert zonder back-ups wordt gestopt, worden nieuwe back-ups mislukken. Oude herstelpunten verlopen volgens het beleid, maar één laatste herstelpunt altijd worden bewaard totdat de back-ups stoppen en verwijderen van de gegevens.
>

### <a name="stop-protection-and-retain-backup-data"></a>Stop de beveiliging en back-upgegevens behouden

Stop de beveiliging en behoud van gegevens van een virtuele machine:

1. Op de [van item dashboard kluis](#view-vms-on-the-dashboard), selecteer **back-up stoppen**.
2. Kies **back-upgegevens behouden**, en bevestigt u uw selectie. Voeg een opmerking toe als u wilt. Als u niet zeker weet van de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam van de weer te geven.

    ![Back-upgegevens behouden](./media/backup-azure-manage-vms/retain-backup-data.png)

Een melding laat u weten dat de back-uptaken zijn gestopt.

### <a name="stop-protection-and-delete-backup-data"></a>Stop de beveiliging en back-upgegevens verwijderen

Beveiliging stoppen en verwijderen van gegevens van een virtuele machine:

1. Op de [van item dashboard kluis](#view-vms-on-the-dashboard), selecteer **back-up stoppen**.
2. Kies **back-upgegevens verwijderen**, en bevestigt u uw selectie. Voer de naam van het back-upitem en voeg een opmerking toe als u wilt.

    ![Back-upgegevens verwijderen](./media/backup-azure-manage-vms/delete-backup-data1.png)

## <a name="resume-protection-of-a-vm"></a>De beveiliging van een virtuele machine hervatten

Als u had gekozen [Stop de beveiliging en back-upgegevens behouden](#stop-protection-and-retain-backup-data) tijdens de beveiliging van de virtuele machine stop te zetten, kunt u de optie **back-up hervatten**. Deze optie is niet beschikbaar als u ervoor kiest [Stop de beveiliging en back-upgegevens verwijderen](#stop-protection-and-delete-backup-data) optie of [back-upgegevens verwijderen](#delete-backup-data).

Beveiliging voor een virtuele machine hervatten:

1. Op de [van item dashboard kluis](#view-vms-on-the-dashboard), selecteer **back-up hervatten**.

2. Volg de stappen in [back-upbeleid beheren](#manage-backup-policy-for-a-vm) om toe te wijzen van het beleid voor de virtuele machine. U hoeft niet te kiezen van de initiële beveiligingsbeleid van de virtuele machine.
3. Nadat u de back-upbeleid op de virtuele machine toepast, ziet u het volgende bericht:

    ![Dat aangeeft dat een is beveiligde virtuele machine](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Back-upgegevens verwijderen

Er zijn twee manieren om de back-gegevens van een virtuele machine te verwijderen:

- Het kluisdashboard item, selecteer back-up stoppen en volg de instructies voor [Stop de beveiliging en back-upgegevens verwijderen](#stop-protection-and-delete-backup-data) optie.

  ![Back-up stoppen selecteren](./media/backup-azure-manage-vms/stop-backup-buttom.png)

- Selecteer in het item kluisdashboard back-upgegevens verwijderen. Deze optie is ingeschakeld als u had gekozen voor [Stop de beveiliging en back-upgegevens behouden](#stop-protection-and-retain-backup-data) optie tijdens de beveiliging van de virtuele machine stop te zetten

  ![Back-up verwijderen selecteren](./media/backup-azure-manage-vms/delete-backup-buttom.png)

  - Op de [item kluisdashboard](#view-vms-on-the-dashboard), selecteer **back-upgegevens verwijderen**.
  - Typ de naam van het back-upitem om te bevestigen dat u wilt verwijderen van de herstelpunten.

    ![Back-upgegevens verwijderen](./media/backup-azure-manage-vms/delete-backup-data1.png)

  - Selecteer wilt verwijderen van de back-upgegevens voor het item **verwijderen**. Een melding laat u weten dat de back-upgegevens zijn verwijderd.

  > [!NOTE]
  > Wanneer u back-upgegevens verwijdert verwijderen u alle gekoppelde herstelpunten. U kunt specifieke herstelpunten te verwijderen niet kiezen.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [maakt u een back-up van virtuele Azure-machines uit de instellingen van de virtuele machine](backup-azure-vms-first-look-arm.md).
- Meer informatie over het [virtuele machines herstellen](backup-azure-arm-restore-vms.md).
- Meer informatie over het [virtuele Azure-machines bewaken](backup-azure-monitor-vms.md).
