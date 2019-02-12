---
title: Resource Manager geïmplementeerde virtuele machine back-ups beheren
description: Meer informatie over het beheren en controleren van de back-ups van virtuele machine geïmplementeerd met Resource Manager
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 11/28/2016
ms.author: sogup
ms.openlocfilehash: d0fac3a075923b000c453480edbf18599f5fed3d
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55994863"
---
# <a name="manage-azure-virtual-machine-backups"></a>Back-ups van een virtuele Azure-machine beheren

Dit artikel bevat richtlijnen over het beheren van back-ups van virtuele machine en wordt uitgelegd van de back-upwaarschuwingen-informatie die beschikbaar zijn in de portal-dashboard. De instructies in dit artikel is van toepassing op het gebruik van virtuele machines met Recovery Services-kluizen. In dit artikel wordt niet beschreven voor het maken van virtuele machines, en ook wordt uitgelegd hoe u virtuele machines beveiligt. Zie voor informatie over het beveiligen van Azure Resource Manager geïmplementeerde virtuele machines in Azure met een Recovery Services-kluis, [eerste blik: Maak een back-up van VM's naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Kluizen en beveiligde virtuele machines beheren
In de Azure-portal bevat het dashboard van de Recovery Services-kluis toegang tot informatie over de kluis, waaronder:

* de meest recente back-upmomentopname, dit ook de meest recente herstelpunt is
* het back-upbeleid
* totale grootte van alle back-momentopnamen
* aantal virtuele machines die zijn beveiligd met de kluis

Veel beheertaken met een virtuele machine back-up begint bij het openen van de kluis in het dashboard. Omdat kluizen kunnen worden gebruikt voor het beveiligen van meerdere items (of meerdere virtuele machines), om meer informatie over een bepaalde virtuele machine weer te geven, opent u het item kluisdashboard. De volgende procedure laat zien hoe u opent de *kluisdashboard* en ga vervolgens door met de *item kluisdashboard*. Er zijn "tips" in beide procedures die wijzen op het toevoegen van de kluis en item kluis naar het Azure-dashboard met behulp van de vastmaken aan dashboard-opdracht. Vastmaken aan dashboard is een manier om een snelkoppeling naar de kluis of het item te maken. Ook kunt u veelgebruikte opdrachten uitvoeren vanuit de snelkoppeling.

> [!TIP]
> Als u meerdere dashboards hebt en opent u blades, gebruikt u de schuifregelaar donkerblauw aan de onderkant van het venster om het Azure-dashboard heen en weer.
>
>

![Volledige weergave met schuifregelaar](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Open een Recovery Services-kluis in het dashboard:
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

    De lijst met Recovery Services-kluizen wordt weergegeven.

    ![Lijst met Recovery Services-kluizen ](./media/backup-azure-manage-vms/list-o-vaults.png)

   > [!TIP]
   > Als u een kluis naar het Azure-Dashboard vastmaakt, wordt die vault onmiddellijk toegankelijk wanneer u de Azure-portal opent. Als u wilt vastmaken een kluis naar het dashboard in de kluislijst, met de rechtermuisknop op de kluis en selecteer **vastmaken aan dashboard**.
   >
   >
3. Selecteer in de lijst met kluizen de kluis om het dashboard te openen. Wanneer u de kluis, het dashboard van de kluis selecteert en de **instellingen** blade geopend. In de volgende afbeelding, de **Contoso-kluis** dashboard is gemarkeerd.

    ![Open het dashboard van de kluis en de blade instellingen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Open een dashboard van de kluis item
In de vorige procedure moet u het dashboard van de kluis geopend. Het kluisdashboard item openen:

1. In het kluisdashboard op de **back-Upitems** tegel, klikt u op **Azure Virtual Machines**.

    ![Back-upitems van Open-tegel](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    De **back-Upitems** blade geeft een lijst van de laatste back-uptaak voor elk item. In dit voorbeeld is er één virtuele machine, demovm-markgal, beveiligd door deze kluis.  

    ![Back-upitems van tegel](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Voor betere toegankelijkheid, kunt u een item kluis naar het Azure-Dashboard vastmaken. Als u wilt een kluis-item in de lijst van de kluis item vastmaken, met de rechtermuisknop op het item en selecteer **vastmaken aan dashboard**.
   >
   >
2. In de **back-Upitems** blade, klikt u op het item om het openen van het kluisdashboard item.

    ![Back-upitems van tegel](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Het item kluisdashboard en de bijbehorende **instellingen** blade geopend.

    ![Back-upitems dashboard met de blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    U kunt veel belangrijke taken, zoals uitvoeren vanuit het kluisdashboard item:

   * wijzigen van beleid of een nieuwe back-upbeleid maken
   * bekijken van herstelpunten en hun status consistentie bekijken
   * on-demand back-up van een virtuele machine
   * de beveiliging van virtuele machines stoppen
   * de beveiliging van een virtuele machine hervatten
   * verwijderen van een back-upgegevens (of herstelpunt)
   * [Herstel de back-schijven](backup-azure-arm-restore-vms.md#create-new-restore-disks)

Het beginpunt is voor de volgende procedures het kluisdashboard item.

## <a name="manage-backup-policies"></a>Back-upbeleid beheren
1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **alle instellingen** openen de **instellingen** blade.

    ![Blade back-upbeleid](./media/backup-azure-manage-vms/all-settings-button.png)
2. Op de **instellingen** blade, klikt u op **back-upbeleid** om deze blade te openen.

    Op de blade worden de back-upfrequentie en bewaartermijn bereik details weergegeven.

    ![Blade back-upbeleid](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Uit de **back-upbeleid kiezen** menu:

   * Selecteer een ander beleid als beleid wilt wijzigen, en klikt u op **opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.
   * Voor het maken van een beleid selecteert **nieuw**.

     ![Back-up van virtuele machine](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Zie voor instructies over het maken van een back-upbeleid [een back-upbeleid definiëren](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Zorg ervoor dat u tijdens het back-upbeleid beheren, de [aanbevolen procedures](backup-azure-vms-introduction.md#best-practices) voor optimale back-upprestaties
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>on-demand back-up van een virtuele machine
U kunt een on-demand back-up van een virtuele machine nemen nadat deze is geconfigureerd voor beveiliging. Als de eerste back-up in behandeling is, maakt back-up op aanvraag een volledige kopie van de virtuele machine in de Recovery Services-kluis. Als de eerste back-up is voltooid, verzendt een on-demand back-up alleen wijzigingen van de vorige momentopname naar de Recovery Services-kluis. Dat wil zeggen, zijn volgende back-ups altijd incrementeel.

> [!NOTE]
> De bewaartermijn voor een on-demand back-up is de waarde voor de bewaarperiode opgegeven voor de dagelijkse back-uppunt in het beleid. Als er geen dagelijkse back-uppunt is geselecteerd, wordt de wekelijkse back-uppunt gebruikt.
>
>

Voor het activeren van een on-demand back-up van een virtuele machine:

* Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up nu**.

    ![Knop Nu back-up](./media/backup-azure-manage-vms/backup-now-button.png)

    De portal zorgt ervoor dat u wilt een on-demand back-uptaak starten. Klik op **Ja** starten van de back-uptaak.

    ![Knop Nu back-up](./media/backup-azure-manage-vms/backup-now-check.png)

    Met de back-uptaak wordt een herstelpunt gemaakt. De bewaartermijn van het herstelpunt is hetzelfde als de bewaartermijn is opgegeven in het beleid dat is gekoppeld aan de virtuele machine. Klik om de voortgang voor de taak, op het kluisdashboard te volgen op de **back-uptaken** tegel.  

## <a name="stop-protecting-virtual-machines"></a>de beveiliging van virtuele machines stoppen
Als u stoppen met het beveiligen van een virtuele machine wilt, wordt u gevraagd of u wilt bewaren van de herstelpunten. Er zijn twee manieren om te stoppen met het beveiligen van virtuele machines:

* Alle toekomstige back-uptaken stoppen en alle herstelpunten verwijderen
* Alle toekomstige back-uptaken stoppen maar de herstelpunten behouden

Er is een kosten die gepaard gaan met het verlaten van de herstelpunten in de opslag. Het voordeel van het bewaren van de herstelpunten is echter dat kunt u de virtuele machine later herstellen indien gewenst. Zie voor meer informatie over de kosten voor het behouden van de herstelpunten van de [prijsinformatie](https://azure.microsoft.com/pricing/details/backup/). Als u verwijderen van alle herstelpunten wilt, kunt u de virtuele machine niet terugzetten.

Het herstelpunt wordt altijd bewaard totdat de back-upitem is beveiligd met een bewaarbeleid of StopProtection met verwijderen van gegevens. In het geval van opnieuw beveiligen bepaalt het nieuwe beleid dat is gekoppeld kunnen ophalen van de retentie van herstelpunten. Als u een gegevensbron verwijderen zonder het uitvoeren van back-up stoppen, nieuwe back-ups mislukken en oude herstelpunten verlopen volgens het beleid voor het bewaren, maar een laatste herstelpunt wordt altijd worden bewaard zullen totdat u back-up stoppen met uitvoeren op dezelfde manier gegevens verwijderen.

Beveiliging voor een virtuele machine stoppen:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up stoppen**.

    ![Back-knop Stop](./media/backup-azure-manage-vms/stop-backup-button.png)

    De blade back-up stoppen wordt geopend.

    ![Blade back-up stoppen](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Op de **back-up stoppen** blade, kies of u wilt behouden of verwijderen van de back-upgegevens. In het gegevens bevat informatie over uw keuze.

    ![Beveiliging stoppen](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Als u wilt de back-upgegevens behouden, gaat u naar stap 4. Als u wilt back-upgegevens verwijderen, bevestigt u dat u wilt de back-uptaken stoppen en verwijderen van de herstelpunten: Typ de naam van het item.

    ![Controle stoppen](./media/backup-azure-manage-vms/item-verification-box.png)

    Als u niet zeker weet van de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam van de weer te geven. De naam van het item is ook onder **back-up stoppen** aan de bovenkant van de blade.
4. Geef eventueel een **reden** of **Opmerking**.
5. Als u wilt stoppen met de back-uptaak voor het huidige item, klikt u op ![back-knop stoppen](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Een melding laat u weten dat de back-uptaken zijn gestopt.

    ![Stop de beveiliging bevestigen](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>de beveiliging van een virtuele machine hervatten
Als de **back-upgegevens behouden** optie hebt gekozen bij het beveiliging voor de virtuele machine is gestopt, en vervolgens kunt u de beveiliging hervatten. Als de **back-upgegevens verwijderen** optie hebt gekozen, en vervolgens beveiliging voor de virtuele machine niet hervatten.

Beveiliging voor de virtuele machine hervatten

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up hervatten**.

    ![Beveiliging hervatten](./media/backup-azure-manage-vms/resume-backup-button.png)

    De blade back-upbeleid wordt geopend.

   > [!NOTE]
   > Wanneer de virtuele machine opnieuw te beveiligen, kunt u een ander beleid dan het beleid waarmee virtuele machine in eerste instantie is beveiligd.
   >
   >
2. Volg de stappen in [back-upbeleid beheren](backup-azure-manage-vms.md#manage-backup-policies) om toe te wijzen van het beleid voor de virtuele machine.

    Zodra de back-upbeleid is toegepast op de virtuele machine, ziet u het volgende bericht weergegeven.

    ![Is beveiligde virtuele machine](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Back-upgegevens verwijderen
U kunt de back-upgegevens die zijn gekoppeld aan een virtuele machine tijdens het verwijderen de **back-up stoppen** taak, of op elk gewenst moment na de back-up taak is voltooid. Het kan zelfs handig zijn om een aantal dagen of weken te wachten voordat u de herstelpunten verwijdert. Wanneer u back-upgegevens verwijdert, kunt u niet specifieke herstelpunten kiezen om te verwijderen, in tegenstelling tot bij het herstellen van herstelpunten. Als uw back-gegevens verwijdert, verwijdert u alle herstelpunten die aan het item zijn gekoppeld.

De volgende procedure wordt ervan uitgegaan dat de back-uptaak voor de virtuele machine is gestopt of uitgeschakeld. Zodra de back-uptaak is uitgeschakeld, de **back-up hervatten** en **back-up verwijderen** opties zijn beschikbaar in het dashboard van de kluis item.

![Knoppen hervatten en verwijderen](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Verwijderen van back-upgegevens op een virtuele machine met de *back-up uitgeschakeld*:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up verwijderen**.

    ![VM-type](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    De **back-upgegevens verwijderen** blade wordt geopend.

    ![VM-type](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Typ de naam van het item om te bevestigen dat u wilt verwijderen van de herstelpunten.

    ![Controle stoppen](./media/backup-azure-manage-vms/item-verification-box.png)

    Als u niet zeker weet van de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam van de weer te geven. De naam van het item is ook onder **back-upgegevens verwijderen** aan de bovenkant van de blade.
3. Geef eventueel een **reden** of **Opmerking**.
4. Als u wilt verwijderen van de back-upgegevens voor het huidige item, klikt u op ![back-knop stoppen](./media/backup-azure-manage-vms/delete-button.png)

    Een melding laat u weten dat de back-upgegevens is verwijderd.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over het opnieuw maken van een virtuele machine vanaf een herstelpunt [Azure-VM's herstellen](backup-azure-arm-restore-vms.md). Als u informatie over het beveiligen van uw virtuele machines nodig hebt, raadpleegt u [eerste blik: Maak een back-up van VM's naar een Recovery Services-kluis](backup-azure-vms-first-look-arm.md). Zie voor meer informatie over het bewaken van gebeurtenissen [waarschuwingen voor back-ups van virtuele machine van Azure controleren](backup-azure-monitor-vms.md).
