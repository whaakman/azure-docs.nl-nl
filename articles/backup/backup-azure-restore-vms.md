---
title: Een virtuele machines herstellen vanuit back-up | Microsoft Docs
description: Informatie over het herstellen van een virtuele machine van Azure vanaf een herstelpunt
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: terugzetten van back-up. het herstellen van; herstelpunt;
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: fc52c909df5e91741ec1fa21fb911487be039fdc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Virtuele machines herstellen in Azure
> [!div class="op_single_selector"]
> * [Herstellen van virtuele machines in Azure-portal](backup-azure-arm-restore-vms.md)
> * [Herstellen van virtuele machines in de klassieke portal](backup-azure-restore-vms.md)
>
>

Een virtuele machine naar een nieuwe virtuele machine herstellen vanuit de back-ups opgeslagen in een Azure Backup-kluis met de volgende stappen.

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Vanaf **15 oktober 2017** kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. <br/> **Vanaf 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

## <a name="restore-workflow"></a>Werkstroom herstellen
### <a name="step-1-choose-an-item-to-restore"></a>Stap 1: Kies een item herstellen
1. Navigeer naar de **beveiligde Items** tabblad en selecteer de virtuele machine die u wilt herstellen naar een nieuwe virtuele machine.

    ![Beveiligde items](./media/backup-azure-restore-vms/protected-items.png)

    De **herstelpunt** kolom in de **beveiligde Items** pagina vertelt u het aantal herstelpunten voor een virtuele machine. De **nieuwste herstelpunt** kolom ziet u de tijd van de meest recente back-up van waaruit een virtuele machine kunnen worden hersteld.
2. Klik op **herstellen** openen de **een Item terugzetten** wizard.

    ![Een item terugzetten](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>Stap 2: Kies een herstelpunt
1. In de **Selecteer een herstelpunt** scherm kunt u herstellen vanaf het meest recente herstelpunt, of van een eerder punt in tijd. De standaardoptie is geselecteerd bij de wizard wordt geopend is *nieuwste herstelpunt*.

    ![Selecteer een herstelpunt](./media/backup-azure-restore-vms/select-recovery-point.png)
2. Als u wilt een eerder punt in tijd kiest, kies de **Selecteer datum** optie in de vervolgkeuzelijst en selecteer een datum in de kalender door te klikken op de **pictogram Agenda**. In het besturingselement alle datums waarvoor herstelpunten worden gevuld met een lichte grijze tint en kunnen worden geselecteerd door de gebruiker.

    ![Selecteer een datum](./media/backup-azure-restore-vms/select-date.png)

    Nadat u op een datum in de kalender, worden de herstelpunten beschikbaar op dat de datum in recovery punten in de volgende tabel worden weergegeven. De **tijd** kolom geeft de tijd waarop de momentopname werd gemaakt. De **Type** kolom ziet u de [consistentie](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) van het herstelpunt. De tabelkop toont het aantal herstelpunten beschikbaar op die dag tussen haakjes.

    ![Herstelpunten](./media/backup-azure-restore-vms/recovery-points.png)
3. Selecteer het herstelpunt uit de **herstelpunten** tabel en klik op de pijl Volgende om door te gaan naar het volgende scherm.

### <a name="step-3-specify-a-destination-location"></a>Stap 3: Geef een bestemmingslocatie
1. In de **Selecteer terugzetten exemplaar** scherm Geef details op van de locatie waar de virtuele machine te herstellen.

   * Geef de naam van de virtuele machine: In een bepaalde cloud-service, de naam van de virtuele machine moet uniek zijn. Bestaande VM te veel schrijven ondersteund niet.
   * Selecteer een cloudservice voor de virtuele machine: dit is verplicht voor het maken van een virtuele machine. U kunt gebruik van een bestaande cloudservice of maak een nieuwe cloudservice.

        De naam van de cloudservice wordt gekozen moet globaal uniek zijn. Normaal gesproken de naam van de cloud-service gekoppeld aan een openbare URL in de vorm van [cloudservice] opgehaald. cloudapp.net. Azure kunt niet u een nieuwe cloudservice maken als de naam is al gebruikt. Als u maken van een nieuwe cloudservice wilt, krijgt deze dezelfde naam als de virtuele machine – die geval verzameld van de VM-naam uniek moet moeten worden toegepast op de gekoppelde cloud-service.

        Wordt alleen weergegeven voor cloudservices en virtuele netwerken die niet gekoppeld aan een affiniteitsgroepen in de details van de restore-exemplaar zijn. [Meer informatie](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Selecteer een opslagaccount voor de virtuele machine: dit is verplicht voor het maken van de virtuele machine. U kunt kiezen uit bestaande opslagaccounts in dezelfde regio bevinden als de Azure Backup-kluis. Storage-accounts die Zone redundant of van het type Premium-opslag zijn ondersteund niet.

    Als er geen opslagaccounts met een ondersteunde configuratie, maakt u een opslagaccount van de ondersteunde configuratie voordat u start de restore-bewerking.

    ![Een virtueel netwerk selecteren](./media/backup-azure-restore-vms/restore-sa.png)
3. Een virtueel netwerk selecteert: het virtuele netwerk (VNET) voor de virtuele machine moet worden geselecteerd op het moment van de virtuele machine maken. Het terugzetten van gebruikersinterface toont de VNETs binnen dit abonnement die kunnen worden gebruikt. Dit is niet verplicht om te selecteren van een VNET voor de herstelde virtuele machine: u wordt mogelijk verbinding met de herstelde virtuele machine via het internet, zelfs als het VNET is niet toegepast.

    Als de geselecteerde cloudservice gekoppeld aan een virtueel netwerk is, kunt u het virtuele netwerk niet wijzigen.

    ![Een virtueel netwerk selecteren](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Selecteer een subnet: als het VNET over subnetten, standaard het eerste subnet wordt geselecteerd. Kies het subnet van uw keuze uit de vervolgkeuzelijst-opties. Voor meer informatie het subnet, gaat u naar netwerken met de extensie in de [portal startpagina](https://manage.windowsazure.com/), gaat u naar **virtuele netwerken** en selecteer het virtuele netwerk en inzoomen op configureren om subnet details te bekijken.

    ![Een subnet selecteren](./media/backup-azure-restore-vms/select-subnet.png)
5. Klik op de **indienen** pictogram in de wizard verzenden de gegevens en maken van een hersteltaak.

## <a name="track-the-restore-operation"></a>De herstelbewerking bijhouden
Zodra u hebt de informatie in de wizard terugzetten invoer en deze ingediend probeert Azure back-up maken van een taak voor het bijhouden van de herstelbewerking opnieuw.

![Er wordt een taak terugzetten](./media/backup-azure-restore-vms/create-restore-job.png)

Als het maken van de taak voltooid is, ziet u een toast-melding die aangeeft dat de taak is gemaakt. U kunt meer informatie krijgen door te klikken op de **taak weergeven** knop waarmee u naar gaat **taken** tabblad.

![Herstellen van de taak is gemaakt](./media/backup-azure-restore-vms/restore-job-created.png)

Zodra de herstelbewerking is voltooid, wordt gemarkeerd als voltooid in de **taken** tabblad.

![Herstellen van de taak is voltooid](./media/backup-azure-restore-vms/restore-job-complete.png)

Na het terugzetten van de virtuele machine wellicht moet u de bestaande op de oorspronkelijke VM-extensies opnieuw te installeren en [wijzigen van de eindpunten](../virtual-machines/windows/classic/setup-endpoints.md) voor de virtuele machine in de Azure portal.

## <a name="post-restore-steps"></a>Stappen na herstellen
Als u van een cloud-init op basis van Linux-distributie zoals Ubuntu, uit veiligheidsoverwegingen gebruikmaakt, wachtwoord geblokkeerd boeken terugzetten. Gebruik VMAccess-extensie op de herstelde virtuele machine naar [wachtwoordherstel](../virtual-machines/linux/classic/reset-access.md). U wordt aangeraden met behulp van SSH-sleutels op deze verdelingen om te voorkomen dat opnieuw instellen van wachtwoord post terugzetten.

## <a name="backup-for-restored-vms"></a>Back-up voor de herstelde virtuele machines
Als u VM voor dezelfde cloudservice met dezelfde naam als het oorspronkelijk een back-up VM hersteld, blijven back-up bij het herstellen van de post VM. Als u hebt hersteld van de VM naar een andere cloudservice of een andere naam voor de herstelde virtuele machine wordt opgegeven, wordt dit beschouwd als een nieuwe virtuele machine en u moet setup back-up voor de herstelde virtuele machine.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Herstellen van een virtuele machine tijdens Azure DataCenter na noodgevallen
Azure Backup kunt terugzetten van back-up VM's in het gekoppelde Datacenter voor het geval waarin VM's worden uitgevoerd na noodgevallen ervaringen en u Backup-kluis geografisch redundante worden geconfigureerd voor de primaire data center. Tijdens de dergelijke scenario's, moet u een opslagaccount die aanwezig is in een gekoppeld Datacenter selecteren en de rest van het herstelproces blijft hetzelfde. Azure Backup gebruikt Compute-service van het gekoppelde geo voor de herstelde virtuele machine maken. Meer informatie over [Azure Data center tolerantie](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Domain Controller virtuele machines herstellen
Back-up van de domeincontroller (DC) virtuele machines is een ondersteund scenario met Azure Backup. Maar wees voorzichtig tijdens het herstelproces. Het juiste herstelproces is afhankelijk van de structuur van het domein. In het meest eenvoudige geval hebt u een enkele DC in één domein. Meer vaak voor productie-belastingen hebt u één domein met meerdere DC's, mogelijk met sommige domeincontrollers on-premises. Mogelijk hebt u ten slotte een forest met meerdere domeinen.

Vanuit het perspectief van een Active Directory is de Azure VM zoals elke andere virtuele machine op een moderne ondersteunde hypervisor. Het belangrijkste verschil met lokale hypervisors is dat er geen VM-console beschikbaar zijn in Azure. Er is een console vereist voor bepaalde scenario's zoals herstellen met behulp van een Bare Metal Recovery (BMR) type back-up. VM-herstel van de back-upkluis is echter een volledige vervanging voor BMR. Active Directory Restore Mode (DSRM) is ook beschikbaar is, zodat alle Active Directory herstelscenario's mogelijk of gewenst zijn is. Zie voor meer achtergrondinformatie [back-up en herstellen van de overwegingen voor gevirtualiseerde domeincontrollers](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) en [plannen voor herstel van Active Directory-Forest](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Enkele DC in één domein
De virtuele machine kan worden hersteld (zoals VM) van de Azure portal of met behulp van PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Meerdere domeincontrollers in één domein
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC zoals een VM worden hersteld. Als dit het laatste resterende DC in het domein is of een herstel in een geïsoleerd netwerk wordt uitgevoerd, kan een herstelprocedure forest moet worden gevolgd.

### <a name="multiple-domains-in-one-forest"></a>Meerdere domeinen in een forest
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC zoals een VM worden hersteld. In alle andere gevallen wordt een forestherstel echter aanbevolen.

<!--- WK: this following original supportability statement is incorrect, taking it out.
The challenge arises because DSRM mode is not present in Azure. So to restore such a VM, you cannot use the Azure portal. The only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use the Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about the [USN rollback problem](https://technet.microsoft.com/library/dd363553) and the strategies suggested to fix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Herstellen van virtuele machines met speciale netwerkconfiguraties
Azure Backup ondersteunt back-up voor speciale configuraties van virtuele machines te volgen.

* VM's onder de load balancer (intern en extern)
* Virtuele machines met meerdere gereserveerde IP 's
* Virtuele machines met meerdere NIC 's

Deze configuraties verplichten na overwegingen bij het terugzetten.

> [!TIP]
> Gebruik PowerShell gebaseerd herstel stroom opnieuw maken van de speciale configuratie van virtuele machines post terugzetten.
>
>

### <a name="restoring-from-the-ui"></a>Terugzetten van de gebruikersinterface:
Tijdens het terugzetten van de gebruikersinterface, **Kies altijd een nieuwe cloudservice**. Houd er rekening mee dat omdat portal slechts verplichte duurt parameters tijdens herstel flow, virtuele machines die zijn teruggezet met behulp van de gebruikersinterface, verliest de speciale netwerkconfiguratie die ze beschikken. Terugzetten van virtuele machines worden met andere woorden, normale virtuele machines zonder configuratie van load balancer of meerdere NIC of meerdere gereserveerde IP-adres.

### <a name="restoring-from-powershell"></a>Terugzetten vanuit PowerShell:
PowerShell heeft de mogelijkheid de VM-schijven alleen back-up terugzetten en de virtuele machine niet maken. Dit is handig bij het herstellen van virtuele machines waarvoor speciale netwerkconfiguraties hierboven vermeld.

Om volledig opnieuw te maken van de virtuele machine herstellen post-schijven, de volgende stappen uit:

1. De schijven herstellen vanuit back-upkluis met [Azure back-PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Maken van de VM-configuratie vereist voor de load balancer-meerdere NIC/meerdere gereserveerde IP-adres met behulp van de PowerShell-cmdlets en het gebruik te maken van de virtuele machine van de gewenste configuratie.

   * Virtuele machine maken in een cloudservice met [interne Load balancer](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Verbinding maken met virtuele-machine maken [Internet gerichte load balancer](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Met virtuele machine maken [meerdere NIC's](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Met virtuele machine maken [meerdere gereserveerde IP-adressen](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Volgende stappen
* [Het oplossen van problemen](backup-azure-vms-troubleshoot.md#restore)
* [Virtuele machines beheren](backup-azure-manage-vms.md)
