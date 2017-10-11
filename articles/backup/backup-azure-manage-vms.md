---
title: "Resource Manager geïmplementeerde virtuele machine back-ups beheren | Microsoft Docs"
description: "Meer informatie over het beheren en controleren van de back-ups van virtuele machine geïmplementeerd met Resource Manager"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 35a21cb99ca4bad124a9f764cef9da453e1fe47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-virtual-machine-backups"></a>Back-ups van een virtuele Azure-machine beheren
> [!div class="op_single_selector"]
> * [Back-ups van virtuele machine in Azure beheren](backup-azure-manage-vms.md)
> * [Klassieke VM-back-ups beheren](backup-azure-manage-vms-classic.md)
>
>

Dit artikel biedt richtlijnen voor het beheren van VM-back-ups en wordt uitgelegd van de back-waarschuwingen informatie die beschikbaar zijn in het portal-dashboard. De instructies in dit artikel is van toepassing op het gebruik van virtuele machines met Recovery Services-kluizen. Dit artikel wordt niet beschreven voor het maken van virtuele machines worden uitgevoerd en wordt uitgelegd hoe u virtuele machines beveiligt. Zie voor een primer over het beveiligen van Azure Resource Manager geïmplementeerde VM's in Azure met een Recovery Services-kluis [eerste kennismaking: Maak een Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Kluizen en beveiligde virtuele machines beheren
In de Azure portal biedt de Recovery Services-kluisdashboard toegang tot informatie over de kluis, waaronder:

* de meest recente back-upmomentopname, dat ook de meest recente herstelpunt < br\>
* het back-upbeleid < br\>
* totale grootte van alle back-upmomentopnamen < br\>
* aantal virtuele machines die zijn beveiligd met de kluis < br\>

Veel beheertaken voor een virtuele machine back-up begint met het openen van de kluis in het dashboard. Omdat kluizen kunnen worden gebruikt voor het beveiligen van meerdere items (of meerdere virtuele machines), om te bekijken over een bepaalde virtuele machine, opent u het item kluisdashboard. De volgende procedure laat zien hoe opent u de *kluisdashboard* en ga verder naar de *kluisdashboard item*. Er zijn 'tips' in beide procedures die wijzen op het toevoegen van de kluis en item naar het dashboard van Azure met behulp van de vastmaken aan dashboard opdracht-kluis. Vastmaken aan dashboard is een manier voor het maken van een snelkoppeling naar de kluis of het item. Ook kunt u veelgebruikte opdrachten uitvoeren vanuit de snelkoppeling.

> [!TIP]
> Als er meerdere dashboards en blades geopend, gebruikt u de schuifregelaar donker blauw aan de onderkant van het venster naar de Azure-dashboard heen en weer dia.
>
>

![Volledige weergave met een schuifregelaar](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Open een Recovery Services-kluis in het dashboard:
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.

    ![Lijst met Recovery Services-kluizen ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > Als u een kluis naar het Azure-Dashboard vastmaken, is deze kluis direct toegankelijk wanneer u de Azure-portal opent. Als u wilt vastmaken een kluis naar het dashboard in de lijst van de kluis, met de rechtermuisknop op de kluis en selecteer **vastmaken aan dashboard**.
   >
   >
3. Selecteer in de lijst met kluizen de kluis om het dashboard te openen. Wanneer u de kluis, het kluisdashboard selecteert en de **instellingen** blade geopend. In de volgende afbeelding, de **Contoso kluis** dashboard is gemarkeerd.

    ![Opent kluisdashboard en de blade instellingen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Een kluisdashboard item openen
In de vorige procedure moet u het kluisdashboard geopend. Hiermee opent u het kluisdashboard item:

1. Op het kluisdashboard op de **back-Upitems** tegel, klikt u op **Azure Virtual Machines**.

    ![Tegel back-items openen](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    De **back-ups** blade geeft een lijst van de laatste back-uptaak voor elk item. In dit voorbeeld is er één virtuele machine, demovm-markgal beveiligd door deze kluis.  

    ![Tegel back-items](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > U kunt een kluis item naar het Azure-Dashboard vastmaken voor eenvoudige toegang. Als u wilt een kluis-item in de lijst van de kluis item vastmaken, met de rechtermuisknop op het item en selecteer **vastmaken aan dashboard**.
   >
   >
2. In de **back-Upitems** blade, klikt u op het item om het kluisdashboard item openen.

    ![Tegel back-items](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Het item kluisdashboard en de bijbehorende **instellingen** blade geopend.

    ![Dashboard van de back-ups met de blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Van het kluisdashboard item, kunt u veel Sleutelbeheer taken, zoals uitvoeren:

   * beleidsregels wijzigen of maak een nieuwe back-upbeleid < br\>
   * herstelpunten weergeven en hun status consistentie Zie < br\>
   * op aanvraag back-up van een virtuele machine < br\>
   * Stop de beveiliging van virtuele machines < br\>
   * beveiliging van een virtuele machine hervatten < br\>
   * verwijderen van een back-upgegevens (of herstelpunt) < br\>
   * [terugzetten van back-schijven](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\>

Het startpunt is voor de volgende procedures het kluisdashboard item.

## <a name="manage-backup-policies"></a>Back-upbeleid beheren
1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **alle instellingen** openen de **instellingen** blade.

    ![De blade back-upbeleid](./media/backup-azure-manage-vms/all-settings-button.png)
2. Op de **instellingen** blade, klikt u op **back-up maken van beleid** om deze blade te openen.

    Op de blade worden back-upfrequentie en bewaartermijn bereik details weergegeven.

    ![De blade back-upbeleid](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Van de **back-upbeleid kiezen** menu:

   * Selecteer een ander beleid als beleid wilt wijzigen, en klik op **opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast. < br\>
   * Voor het maken van een beleid selecteert **nieuw**.

     ![Back-up van virtuele machine](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Zie voor instructies over het maken van een back-upbeleid [een back-upbeleid definiëren](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Bij het beheren van back-upbeleid, zorg ervoor dat u de [aanbevolen procedures](backup-azure-vms-introduction.md#best-practices) voor optimale prestaties van de back-up
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>Op aanvraag back-up van een virtuele machine
U kunt op aanvraag back-up van een virtuele machine uitvoeren zodra deze is geconfigureerd voor beveiliging. Als de eerste back-up in behandeling is, maakt back-up op aanvraag een volledige kopie van de virtuele machine in de Recovery Services-kluis. Als de eerste back-up is voltooid, verzendt een back-up van op aanvraag alleen wijzigingen van de vorige momentopname naar de Recovery Services-kluis. Dat wil zeggen, zijn volgende back-ups altijd incrementeel.

> [!NOTE]
> De bewaartermijn voor een op aanvraag back-up is de retentie-waarde opgegeven voor het dagelijkse back-punt in het beleid. Als er geen dagelijkse back-uppunt is ingeschakeld, wordt de wekelijkse back-uppunt gebruikt.
>
>

Voor het activeren van een op aanvraag back-up van een virtuele machine:

* Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up nu**.

    ![Back-up nu knop](./media/backup-azure-manage-vms/backup-now-button.png)

    De portal zorgt ervoor dat u wilt starten van een back-uptaak op aanvraag. Klik op **Ja** starten van de back-uptaak.

    ![Back-up nu knop](./media/backup-azure-manage-vms/backup-now-check.png)

    De back-uptaak maakt een herstelpunt. De bewaartermijn van het herstelpunt is hetzelfde als de bewaartermijn is opgegeven in het beleid dat is gekoppeld aan de virtuele machine. Klik om de voortgang voor de taak, op het kluisdashboard te volgen op de **back-uptaken** tegel.  

## <a name="stop-protecting-virtual-machines"></a>Stop de beveiliging van virtuele machines
Als u de beveiliging van een virtuele machine stopt, wordt u gevraagd als u wilt de herstelpunten bewaren. Er zijn twee manieren om te stoppen met het beveiligen van virtuele machines:

* alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten, of
* alle toekomstige back-uptaken stoppen, maar laat de herstelpunten <br/>

Er is een kosten in verband met het verlaten van de herstelpunten die in de opslag. Het voordeel van het verlaten van de herstelpunten is echter dat kunt u later de virtuele machine herstellen indien gewenst. Zie voor informatie over de kosten van het verlaten van de herstelpunten, de [prijsinformatie](https://azure.microsoft.com/pricing/details/backup/). Als u alle herstelpunten verwijderd wilt, kunt u de virtuele machine niet herstellen.

Beveiliging voor een virtuele machine stoppen:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up stoppen**.

    ![Back-knop stoppen](./media/backup-azure-manage-vms/stop-backup-button.png)

    De blade back-up stoppen wordt geopend.

    ![Back-blade stoppen](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Op de **back-up stoppen** blade, kies of u wilt behouden of verwijderen van de back-upgegevens. Het gegevens bevat informatie over uw keuze.

    ![Stop de beveiliging](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Als u kiest voor het bewaren van de back-upgegevens, gaat u verder met stap 4. Als u wilt verwijderen van de back-upgegevens, u wilt bevestigen dat de back-uptaken stoppen en verwijderen van herstelpunten - Typ de naam van het item.

    ![Controle stoppen](./media/backup-azure-manage-vms/item-verification-box.png)

    Als u niet zeker weet wat de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam weer te geven. De naam van het item is ook onder **back-up stoppen** boven aan de blade.
4. Geef optioneel een **reden** of **Opmerking**.
5. Als u wilt stoppen van de back-uptaak voor het huidige item, klikt u op ![back-knop stoppen](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Een melding laat u weten dat de back-uptaken zijn gestopt.

    ![Stop de beveiliging bevestigen](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>Beveiliging van een virtuele machine hervatten
Als de **back-up gegevens behouden** optie hebt gekozen wanneer beveiliging voor de virtuele machine is gestopt, dan is het mogelijk beveiliging hervatten. Als de **back-up-gegevens verwijderen** optie hebt gekozen, en vervolgens beveiliging voor de virtuele machine niet hervatten.

Beveiliging voor de virtuele machine hervatten

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up hervatten**.

    ![Beveiliging hervatten](./media/backup-azure-manage-vms/resume-backup-button.png)

    De blade back-upbeleid wordt geopend.

   > [!NOTE]
   > Wanneer de virtuele machine opnieuw te beveiligen, kunt u een ander beleid dan het beleid waarmee virtuele machine in eerste instantie is beveiligd.
   >
   >
2. Volg de stappen in [beheren van back-upbeleid](backup-azure-manage-vms.md#manage-backup-policies) toewijzen van het beleid voor de virtuele machine.

    Nadat de back-upbeleid wordt toegepast op de virtuele machine, ziet u het volgende bericht.

    ![Met succes beveiligde VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Back-up van gegevens verwijderen
U kunt de back-upgegevens die zijn gekoppeld aan een virtuele machine tijdens het verwijderen de **back-up stoppen** taak, of op elk gewenst moment na de back-up taak is voltooid. Kan zelfs het nuttig zijn moet worden gewacht dagen of weken voordat de herstelpunten worden verwijderd. In tegenstelling tot het herstellen van herstelpunten, wanneer het verwijderen van de back-upgegevens, kunt u specifieke herstelpunten verwijderen niet kiezen. Als u ervoor kiest om uw back-gegevens te verwijderen, verwijdert u alle herstelpunten die zijn gekoppeld aan het item.

De volgende procedure wordt ervan uitgegaan dat de back-uptaak voor de virtuele machine is gestopt of uitgeschakeld. Zodra de back-uptaak is uitgeschakeld, de **back-up hervatten** en **back-up verwijderen** opties zijn beschikbaar in het kluisdashboard item.

![Hervat en knoppen verwijderen](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Verwijderen van de back-upgegevens op een virtuele machine met de *back-up uitgeschakeld*:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **verwijderen back-up**.

    ![VM-Type](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    De **back-up-gegevens verwijderen** blade wordt geopend.

    ![VM-Type](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Typ de naam van het artikel om te bevestigen dat u wilt verwijderen van de herstelpunten.

    ![Controle stoppen](./media/backup-azure-manage-vms/item-verification-box.png)

    Als u niet zeker weet wat de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam weer te geven. De naam van het item is ook onder **back-up-gegevens verwijderen** boven aan de blade.
3. Geef optioneel een **reden** of **Opmerking**.
4. Als u wilt verwijderen van de back-upgegevens voor het huidige item, klikt u op ![back-knop stoppen](./media/backup-azure-manage-vms/delete-button.png)

    Een melding laat u weten dat de back-upgegevens is verwijderd.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over het opnieuw maken van een virtuele machine vanaf een herstelpunt [Azure Virtual machines herstellen](backup-azure-restore-vms.md). Als u informatie over het beveiligen van uw virtuele machines, Zie [eerste kennismaking: Maak een Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md). Zie voor informatie over de controle van gebeurtenissen, [waarschuwingen voor back-ups van virtuele machine van Azure controleren](backup-azure-monitor-vms.md).
