---
title: Beheren en controleren van de back-ups van virtuele Azure-machine met de Azure Backup-service
description: Informatie over het beheren en controleren van de back-up van virtuele Azure-machine met de Azure Backup-service.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: da686e53f711877642a33806719a0d71050e721b
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56669377"
---
# <a name="manage-azure-vm-backups"></a>Back-ups van Azure-VM's beheren

In dit artikel wordt beschreven hoe u voor het beheren van virtuele Azure-machines met back-ups van de [Azure Backup-service](backup-overview.md) back-ups, en bevat een overzicht van back-upwaarschuwingen informatie beschikbaar in de portal-dashboard.


In de Azure-portal bevat het dashboard van de Recovery Services-kluis toegang tot informatie over de kluis, waaronder:

* De meest recente back-up, dit ook de meest recente herstelpunt is.
* het back-upbeleid
* De totale grootte van alle back-momentopnamen
* Het aantal VM's zijn ingeschakeld voor back-up

U kunt back-ups met behulp van het dashboard, beheren en te verkrijgen van toegang tot afzonderlijke virtuele machines. back-up achine beginnen met het openen van de kluis in het dashboard. 

![Volledige weergave met schuifregelaar](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-in-the-dashboard"></a>Virtuele machines weergeven in het dashboard

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**. 

    ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Voor gebruiksgemak, met de rechtermuisknop op de kluis in de lijst met kluizen > **vastmaken aan dashboard**.
4. Open het dashboard van de kluis. 
    ![Open het dashboard van de kluis en de blade instellingen](./media/backup-azure-manage-vms/full-view-rs-vault.png)

4. Op de **back-Upitems** tegel, klikt u op **Azure Virtual Machines**.

    ![Back-upitems van Open-tegel](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. In **back-Upitems** blade ziet u de laatste back-uptaak voor elk item. In dit voorbeeld is er één virtuele machine, demovm-markgal, beveiligd door deze kluis.  

    ![Back-upitems van tegel](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

 
6. Van het kluisdashboard item, kunt u maken of wijzigen van de back-upbeleid, herstelpunten weergeven uitvoeren van een on-demand back-up stoppen en hervatten van de beveiliging van virtuele machines, herstelpunten verwijderen en een herstelbewerking worden uitgevoerd.

    ![Back-upitems dashboard met de blade instellingen](./media/backup-azure-manage-vms/item-dashboard-settings.png)



## <a name="manage-backup-policies"></a>Back-upbeleid beheren
1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **alle instellingen** .

    ![Blade back-upbeleid](./media/backup-azure-manage-vms/all-settings-button.png)
2. In **instellingen**, klikt u op**back-upbeleid**e.
3. Uit de **back-upbeleid kiezen** menu:

   * Selecteer een ander beleid als beleid wilt wijzigen, en klikt u op **opslaan**. Het nieuwe beleid wordt onmiddellijk op de kluis toegepast.
   * Voor het maken van een beleid selecteert **nieuw**. [Meer informatie](backup-azure-arm-vms-prepare.md#configure-a-backup-policy)

     ![Back-up van virtuele machine](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren
U kunt een on-demand back-up van een virtuele machine nemen nadat deze is geconfigureerd voor beveiliging.
- Als de eerste back-up in behandeling is, maakt back-up op aanvraag een volledige kopie van de virtuele machine in de Recovery Services-kluis.
- Als de eerste back-up is voltooid, verzendt een on-demand back-up alleen wijzigingen van de vorige momentopname naar de Recovery Services-kluis. Dat wil zeggen, zijn volgende back-ups altijd incrementeel.
- De bewaartermijn voor een on-demand back-up is de waarde voor de bewaarperiode opgegeven voor de dagelijkse back-uppunt in het beleid. Als er geen dagelijkse back-uppunt is geselecteerd, wordt de wekelijkse back-uppunt gebruikt.


Voor het activeren van een on-demand back-up:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up nu**.

    ![Knop Nu back-up](./media/backup-azure-manage-vms/backup-now-button.png)

 2. Klik op **Ja** starten van de back-uptaak.

    ![Knop Nu back-up](./media/backup-azure-manage-vms/backup-now-check.png)

 
 Met de back-uptaak wordt een herstelpunt gemaakt. De bewaartermijn van het herstelpunt is hetzelfde als de bewaartermijn is opgegeven in het beleid dat is gekoppeld aan de virtuele machine. Klik om de voortgang voor de taak, op het kluisdashboard te volgen op de **back-uptaken** tegel.  

## <a name="stop-protecting-a-vm"></a>De beveiliging van een virtuele machine stoppen

Er zijn twee manieren om te stoppen met het beveiligen van virtuele machines:

- Alle toekomstige back-uptaken stoppen en alle herstelpunten verwijderen. Kunt u zich niet in dit geval de virtuele machine herstellen.
- Alle toekomstige back-uptaken stoppen maar laat u de herstelpunten. Er is een kosten die gepaard gaan met het verlaten van de herstelpunten in de opslag. Het voordeel van het bewaren van de herstelpunten is echter dat kunt u de virtuele machine herstellen indien nodig. [Meer informatie](https://azure.microsoft.com/pricing/details/backup/) over prijzen.

Houd rekening met het volgende:

- Als u de herstelpunten laat, wordt de punten worden opgeschoond in overeenstemming met de back-upbeleid. U betaalt kosten voor het beveiligde exemplaar en de verbruikte opslag totdat alle herstelpunten worden opgeschoond. [Meer informatie](https://azure.microsoft.com/pricing/details/backup/) over prijzen.
- Als u herstelpunten behouden, laat hoewel ze zijn verlopen volgens het bewaarbeleid, houdt Azure Backup altijd een laatste herstelpunt totdat u expliciet back-upgegevens verwijdert.
- Als u een gegevensbron zonder back-up stoppen verwijdert, nieuwe back-ups zullen mislukken. Nogmaals, de oude herstelpunten verlopen volgens het beleid, maar één laatste herstelpunt altijd bewaard totdat u back-up stoppen en verwijderen van de gegevens.



Beveiliging voor een virtuele machine stoppen:

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up stoppen**.
2. Kies of u wilt behouden of verwijderen van de back-upgegevens en controleer indien nodig. Bevestig als vereist, en geef eventueel een opmerking. Als u niet zeker weet van de naam van het item, Beweeg de muisaanwijzer over de uitroepteken om de naam van de weer te geven.

    ![Beveiliging stoppen](./media/backup-azure-manage-vms/retain-or-delete-option.png)

 Een melding laat u weten dat de back-uptaken zijn gestopt.


## <a name="resume-protection-of-a-vm"></a>De beveiliging van een virtuele machine hervatten

Als u back-upgegevens behouden wanneer de virtuele machine is gestopt, kunt u de beveiliging te hervatten. Als u back-upgegevens verwijderd en u kunt niet hervatten.

Te

1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up hervatten**.

2. Volg de stappen in [back-upbeleid beheren](backup-azure-manage-vms.md#manage-backup-policies) om toe te wijzen van het beleid voor de virtuele machine. u kunt een ander beleid dan het beleid waarmee virtuele machine in eerste instantie is beveiligd.
3. Nadat de back-upbeleid wordt toegepast op de virtuele machine, ziet u het volgende bericht weergegeven.

    ![Is beveiligde virtuele machine](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Back-upgegevens verwijderen

U kunt de back-upgegevens die zijn gekoppeld aan een virtuele machine tijdens het verwijderen de **back-up stoppen** taak, of op elk gewenst moment na de back-up taak is voltooid.

- Het kan zelfs handig zijn om een aantal dagen of weken te wachten voordat u de herstelpunten verwijdert.
- Wanneer u back-upgegevens verwijdert, kunt u niet specifieke herstelpunten kiezen om te verwijderen, in tegenstelling tot bij het herstellen van herstelpunten. Als uw back-gegevens verwijdert, verwijdert u alle herstelpunten die aan het item zijn gekoppeld.

Deze procedure wordt ervan uitgegaan dat de back-uptaak voor de virtuele machine is gestopt of uitgeschakeld.


1. Op de [item kluisdashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikt u op **back-up verwijderen**.

    ![VM-type](./media/backup-azure-manage-vms/delete-backup-buttom.png)

2. Typ de naam van het item om te bevestigen dat u wilt verwijderen van de herstelpunten.

    ![Controle stoppen](./media/backup-azure-manage-vms/item-verification-box.png)

4. Als u wilt verwijderen van de back-upgegevens voor het huidige item, klikt u op ![back-knop stoppen](./media/backup-azure-manage-vms/delete-button.png)

    Een melding laat u weten dat de back-upgegevens is verwijderd.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over](backup-azure-vms-first-look-arm.md) back-ups van virtuele Azure-machines uit de VM-instellingen.
- [Meer informatie over](backup-azure-arm-restore-vms.md) herstellen van virtuele machines. 
- [Meer informatie over](backup-azure-monitor-vms.md) virtuele Azure-machines bewaken.
 
