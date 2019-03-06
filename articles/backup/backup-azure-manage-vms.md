---
title: Beheren en controleren van virtuele Azure-machines met behulp van de Azure Backup-service
description: Informatie over het beheren en controleren van virtuele Azure-machines met behulp van de Azure Backup-service.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: 0fa221721471772b066990ec2d33f0cedb960239
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453538"
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

4. Op de **back-Upitems** tegel, selecteer **Azure Virtual Machines**.

    ![Open de tegel back-Upitems](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. Op de **back-Upitems** blade ziet u de laatste back-uptaak voor elk item. In dit voorbeeld de kluis worden beveiligd door één virtuele machine: demovm markgal.  

    ![De blade back-Upitems weergeven](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)


6. Uit de kluis-item-dashboard, kunt u maken of wijzigen van de back-upbeleid, herstelpunten weer een back-up stoppen op aanvraag uitvoeren of hervatten van de beveiliging van virtuele machines, herstelpunten verwijderen en een herstelbewerking worden uitgevoerd.

    ![Het back-Upitems-dashboard en de blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policies"></a>Back-upbeleid beheren

Een back-upbeleid beheren:

1. Op de [item kluisdashboard](#view-vms-in-the-dashboard), selecteer **alle instellingen**.

    ![De instellingenoptie alle](./media/backup-azure-manage-vms/all-settings-button.png)
2. In **instellingen**, selecteer **back-upbeleid**.
3. Op de **back-upbeleid kiezen** menu:

   * Om te schakelen van beleid, selecteert u een ander beleid en selecteer vervolgens **opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.
   * Voor het maken van een beleid selecteert **nieuw**. Zie voor meer informatie, [configureren van een back-upbeleid](backup-azure-arm-vms-prepare.md#configure-a-backup-policy).

     ![Een back-upbeleid kiezen](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren
Nadat u de beveiliging hebt ingesteld, kunt u een on-demand back-up van een virtuele machine uitvoeren. Houd rekening met deze details: 
- Als de eerste back-up in behandeling is, maakt back-up op aanvraag een volledige kopie van de virtuele machine in de Recovery Services-kluis.
- Als de eerste back-up voltooid is, verzendt een on-demand back-up alleen wijzigingen van de vorige momentopname naar de Recovery Services-kluis. Dat wil zeggen, zijn latere back-ups altijd incrementeel.
- De bewaartermijn voor een on-demand back-up is de waarde voor de bewaarperiode dat u opgeeft wanneer u de back-up activeren.

Voor het activeren van een on-demand back-up:

1. Op de [item kluisdashboard](#view-vms-in-the-dashboard)onder **beveiligde Item**, selecteer **back-upitem**.

    ![De back-up nu-optie](./media/backup-azure-manage-vms/backup-now-button.png)

2. Van **Type back-upbeheer**, selecteer **virtuele Azure-Machine**. De **back-upitem (Azure virtuele Machine)** blade wordt weergegeven.
3. Selecteer een virtuele machine en selecteer **nu back-up** een on-demand back-up maken. De **nu back-up** blade wordt weergegeven.
4. In de **bewaren back-up tot** veld, geeft u een datum voor de back-up moet worden bewaard.

    ![De agenda nu back-up](./media/backup-azure-manage-vms/backup-now-check.png)

5. Selecteer **OK** om uit te voeren van de back-uptaak.

Voor het volgen van de voortgang van de taak op het kluisdashboard, selecteer de **back-uptaken** tegel.

## <a name="stop-protecting-a-vm"></a>De beveiliging van een virtuele machine stoppen

Er zijn twee manieren om te stoppen met het beveiligen van een virtuele machine:

- Alle toekomstige back-uptaken stoppen en alle herstelpunten verwijderen. In dit geval kunt u zich niet op de virtuele machine herstellen.
- Alle toekomstige back-uptaken stoppen en de herstelpunten behouden. Hoewel u betaalt voor het behouden van de herstelpunten in de kluis moet, moet u de virtuele machine kunt herstellen indien nodig zijn. Zie voor meer informatie, [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

>[!NOTE]
>Als u een gegevensbron verwijdert zonder back-ups wordt gestopt, worden nieuwe back-ups mislukken. Oude herstelpunten verlopen volgens het beleid, maar één laatste herstelpunt altijd worden bewaard totdat de back-ups stoppen en verwijderen van de gegevens.
>

Beveiliging voor een virtuele machine stoppen:

1. Op de [van item dashboard kluis](#view-vms-in-the-dashboard), selecteer **back-up stoppen**.
2. Kies of u wilt behouden of verwijderen van de back-upgegevens en bevestigt u uw selectie. Voeg een opmerking toe als u wilt. Als u niet zeker weet van de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam van de weer te geven.

    ![Beveiliging stoppen](./media/backup-azure-manage-vms/retain-or-delete-option.png)

     Een melding laat u weten dat de back-uptaken zijn gestopt.

## <a name="resume-protection-of-a-vm"></a>De beveiliging van een virtuele machine hervatten

Als u back-upgegevens behouden wanneer u de virtuele machine stopt, kunt u later beveiliging hervatten. Als u de back-upgegevens verwijdert, kunt u de beveiliging niet hervatten.

Beveiliging voor een virtuele machine hervatten:

1. Op de [van item dashboard kluis](#view-vms-in-the-dashboard), selecteer **back-up hervatten**.

2. Volg de stappen in [back-upbeleid beheren](#manage-backup-policies) om toe te wijzen van het beleid voor de virtuele machine. U hoeft niet te kiezen van de initiële beveiligingsbeleid van de virtuele machine.
3. Nadat u de back-upbeleid op de virtuele machine toepast, ziet u het volgende bericht:

    ![Dat aangeeft dat een is beveiligde virtuele machine](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>back-upgegevens verwijderen

U kunt verwijderen van een virtuele machine back-upgegevens tijdens de **back-up stoppen** taak of nadat de back-uptaak is voltooid. Voordat u back-upgegevens verwijderen, houd rekening met deze details:

- Het is mogelijk een goed idee om te wachten van dagen of weken voordat u de herstelpunten verwijderen.
- In tegenstelling tot het proces voor het herstellen van herstelpunten, wanneer u back-upgegevens verwijdert u kan niet specifieke herstelpunten kiezen om te verwijderen. Als u uw back-upgegevens verwijdert, verwijdert u alle gekoppelde herstelpunten.

Nadat u stoppen of uitschakelen van de back-uptaak van de virtuele machine, kunt u de back-upgegevens verwijderen:


1. Op de [item kluisdashboard](#view-vms-in-the-dashboard), selecteer **back-up verwijderen**.

    ![Back-up verwijderen selecteren](./media/backup-azure-manage-vms/delete-backup-buttom.png)

1. Typ de naam van het back-upitem om te bevestigen dat u wilt verwijderen van de herstelpunten.

    ![Bevestig dat u wilt verwijderen van de herstelpunten](./media/backup-azure-manage-vms/item-verification-box.png)

1. Selecteer wilt verwijderen van de back-upgegevens voor het item **verwijderen**. Een melding laat u weten dat de back-upgegevens zijn verwijderd.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het [maakt u een back-up van virtuele Azure-machines uit de instellingen van de virtuele machine](backup-azure-vms-first-look-arm.md).
- Meer informatie over het [virtuele machines herstellen](backup-azure-arm-restore-vms.md).
- Meer informatie over het [virtuele Azure-machines bewaken](backup-azure-monitor-vms.md).
