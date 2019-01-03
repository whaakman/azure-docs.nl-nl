---
title: 'Azure Backup: Virtuele machines herstellen met behulp van Azure portal'
description: Een virtuele Azure-machine herstellen vanaf een herstelpunt met behulp van Azure portal
services: backup
author: geethalakshmig
manager: vijayts
keywords: herstellen van back-up. het herstellen van; herstelpunt;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: ac0c727e41ad9b361ea9f558a97f16446c12ef0e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793373"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>De Azure portal gebruiken voor virtuele machines herstellen
Bescherm uw gegevens door het maken van momentopnamen van uw gegevens met gedefinieerde intervallen. Deze momentopnamen zijn voorzien van herstelpunten en ze zijn opgeslagen in Recovery Services-kluizen. Als het nodig zijn om te herstellen of opnieuw opbouwen van een virtuele machine (VM), kunt u de virtuele machine herstellen uit een van de opgeslagen herstelpunten. Wanneer u een herstelpunt herstelt, kunt u het volgende doen:

* Maak een nieuwe virtuele machine: Vanaf een point-in-time herstelpunt van de VM waarvan een back-up is gemaakt.
* Herstel de schijven: Gebruik de sjabloon die wordt geleverd met het proces voor het aanpassen van de herstelde virtuele machine, of herstel van een afzonderlijk bestand doen.

In dit artikel wordt uitgelegd hoe u een virtuele machine herstellen naar een nieuwe virtuele machine of alle schijven van een back-up hersteld. Zie voor herstel van afzonderlijke bestanden, [bestanden herstellen van een Azure-VM back-up](backup-azure-restore-files-from-vm.md).

![Drie manieren om te herstellen vanuit back-up van virtuele machine](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)


Herstellen van een virtuele machine of alle schijven van virtuele machine back-up bestaat uit twee stappen:

* Selecteer een herstelpunt voor herstel.
* Het type herstel selecteren
    - Optie 1: Een nieuwe virtuele machine maken
    - Optie 2: Herstel de schijven.

## <a name="select-a-restore-point-for-restore"></a>Selecteer een herstelpunt voor herstel
1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

2. Selecteer op het menu van Azure, **alle services**. Typ in de lijst met services, **Recovery Services** of Ga naar **opslag** waar de **Recovery Services-kluizen** wordt weergegeven, selecteert u deze.

    ![Recovery Services-kluis](./media/backup-azure-arm-restore-vms/open-recovery-services-vault1.png)

3. De lijst met kluizen in het abonnement wordt weergegeven.

    ![Lijst met Recovery Services-kluizen](./media/backup-azure-arm-restore-vms/list-of-rs-vaults1.png)

4. Selecteer in de lijst met Recovery Services-kluizen, de kluis die zijn gekoppeld aan de virtuele machine die u wilt herstellen. Wanneer u de kluis selecteert, wordt het dashboard wordt geopend.

    ![Recovery Services-kluis geselecteerd](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade1.png)

5. In het kluisdashboard op de **back-Upitems** tegel, selecteer **virtuele Azure-Machine**.

    ![Dashboard van de kluis](./media/backup-azure-arm-restore-vms/vault-dashboard1.png)

6. De **back-Upitems** blade met de lijst met virtuele Azure-machines wordt geopend.

    ![Lijst met virtuele machines in de kluis](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault1.png)

7. Selecteer een virtuele machine om het dashboard te openen in de lijst. Het dashboard voor VM's wordt geopend op het gebied bewaking, waarin de **herstelpunten**. Alle bewerkingen voor VM-niveau, zoals **back-up nu**, **-herstel**, **back-up stoppen** vanaf deze blade kan worden uitgevoerd.
Herstel kan worden uitgevoerd op veel manieren vanaf deze blade. Houd er rekening mee dat deze blade alleen de laatste 30 dagen herstelpunten bevat.

    - Als u wilt herstellen met behulp van een herstelpunt van de afgelopen 30 dagen, met de rechtermuisknop op de virtuele machine > **VM terugzetten**.
    - Als u wilt herstellen met behulp van een herstelpunt ouder dan 30 dagen, klikt u op de koppeling onder **herstelpunten**.
    - Als u wilt weergeven en filteren van virtuele machines met aangepaste datums, klikt u op **VM terugzetten** in het menu. Gebruik het filter om te wijzigen van het tijdsbereik voor herstelpunten weergegeven. U kunt ook filteren op verschillende typen gegevensconsistentie.
8. Controleer de instellingen voor het terugzetten:
    - Gegevens consistentie toont de [type consistentie](backup-azure-vms-introduction.md#consistency-types) in het herstelpunt.
    - De **hersteltype** laat zien waar het punt wordt opgeslagen (in een opslagaccount in de kluis of beide. [Meer informatie](https://azure.microsoft.com/blog/large-disk-support/) over directe herstelpunten.

  ![Herstelpunten](./media/backup-azure-arm-restore-vms/vm-blade1.png)
9. Selecteer een herstelpunt.

10. Selecteer **configuratie terugzetten**. De **configuratie terugzetten** blade wordt geopend.

## <a name="choose-a-vm-restore-configuration"></a>Kies de configuratie van een virtuele machine terugzetten
Nadat u het herstelpunt geselecteerd, kiest u een VM-configuratie voor herstel. Voor het configureren van de herstelde virtuele machine, kunt u de Azure portal of PowerShell.

### <a name="restore-options"></a>Opties herstellen

**Optie** | **Details**
--- | ---
**Maak nieuwe-virtuele machine maken** | Komt overeen met het snel maken van een virtuele machine. Hiermee haalt u een basis-VM te starten vanaf een herstelpunt.<br/><br/> De instellingen van de virtuele machine kunnen worden gewijzigd als onderdeel van het herstelproces.
**Nieuwe herstellen schijf maken** | Hiermee maakt u een virtuele machine die u als onderdeel van het herstelproces kunt aanpassen.<br/><br/> Deze optie kopieert VHD's naar het opslagaccount dat u opgeeft.<br/><br/> -Het opslagaccount moet zich op dezelfde locatie als de kluis.<br/><br/> Als u geen account de juiste opslag moet u een te maken.<br/><br/> Het opslagaccounttype voor replicatie wordt tussen haakjes weergegeven. Zone-redundante opslag (ZRS) wordt niet ondersteund.<br/><br/> Van het opslagaccount kunt u de herstelde schijven koppelen aan een bestaande virtuele machine of een nieuwe virtuele machine maken van de herstelde schijven met behulp van PowerShell.
**Vervang bestaande** | Met deze optie moet u niet een virtuele machine maken.<br/><br/> De huidige virtuele machine moet bestaan. Als deze is verwijderd met deze optie kan niet worden gebruikt.<br/><br/> Azure Backup maakt een momentopname van de bestaande virtuele machine en slaat ze op in de faseringslocatie opgegeven. De bestaande schijven die zijn verbonden met de virtuele machine vervolgens vervangen door het geselecteerde herstelpunt. Momentopname van het eerder gemaakte wordt gekopieerd naar de kluis en opgeslagen als een herstelpunt in overeenstemming met uw back-upbewaarbeleid.<br/><br/> Vervangen bestaande wordt ondersteund voor niet-versleutelde beheerde virtuele machines. Het wordt niet ondersteund voor niet-beheerde schijven [gegeneraliseerde VM's](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource), of voor virtuele machines [gemaakt met behulp van aangepaste installatiekopieën](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Als het herstelpunt min of meer schijven dan de huidige virtuele machine is, klikt u vervolgens in het aantal schijven in het herstelpunt alleen de virtuele machine weer.

> [!NOTE]
> Als u herstellen van een virtuele machine met geavanceerde instellingen voor netwerken, bijvoorbeeld wilt als ze worden beheerd door een interne of externe load balancer, of meerdere NIC's of meerdere gereserveerde IP-adressen, kunt u herstellen met PowerShell. [Meer informatie](#restore-vms-with-special-network-configurations).
> Als een Windows-VM gebruikt [HUB licentieverlening](../virtual-machines/windows/hybrid-use-benefit-licensing.md), gebruiken de **maken nieuwe herstellen schijf** optie en klikt u vervolgens PowerShell of de sjabloon terugzetten gebruiken voor het maken van de virtuele machine met **licentietype** ingesteld op **Windows_Server**. Deze instelling kan ook worden toegepast na het maken van de virtuele machine.


Geef het terugzetten als volgt instellen:

1. In **herstellen**, selecteer een [herstelpunt](#select-a-restore-point-for-restore) > **configuratie terugzetten**.
2. In **configuratie terugzetten**, selecteer de manier waarop u wilt herstellen van de virtuele machine in overeenstemming met de instellingen in de bovenstaande tabel.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)


## <a name="create-new-create-a-vm"></a>Maak nieuwe-een virtuele machine maken

1. In **configuratie terugzetten** > **nieuw** > **Type herstellen**, selecteer **maken van een virtuele machine** .
2. In **virtuele-machinenaam**, Geef een virtuele machine bestaat in het abonnement.
3. In **resourcegroep**, selecteer een bestaande resourcegroep voor de nieuwe virtuele machine of een nieuwe maken door een wereldwijd unieke naam. Als u een naam die al bestaan toewijzen, wijst Azure de groep dezelfde naam als de virtuele machine.
4. In **virtueel netwerk**, selecteert u het VNet waarin de virtuele machine wordt geplaatst. Alle vnet's die zijn gekoppeld aan het abonnement worden weergegeven. Selecteer het subnet. Het eerste subnet is standaard geselecteerd.
5. In **opslaglocatie**, geef het opslagtype gebruikt voor de virtuele machine.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. In **configuratie terugzetten**, selecteer **OK**. In **herstellen**, klikt u op **herstellen** voor het activeren van de herstelbewerking opnieuw.



## <a name="create-new-restore-disks"></a>Maken van nieuwe-herstel-schijven

1. In **configuratie terugzetten** > **nieuw** > **Type herstellen**, selecteer **schijven herstellen**.
2. In **resourcegroep**, selecteer een bestaande resourcegroep voor de herstelde schijven of een nieuwe maken door een wereldwijd unieke naam.
3. In **opslagaccount**, geef het account waarnaar u wilt kopiëren van de VHD's. Zorg ervoor dat het account zich in dezelfde regio bevinden als de kluis.

    ![Herstelconfiguratie van is voltooid](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. In **configuratie terugzetten**, selecteer **OK**. In **herstellen**, klikt u op **herstellen** voor het activeren van de herstelbewerking opnieuw.
5. Nadat de schijf is hersteld, kunt u een van de volgende mogelijkheden voor het voltooien van de VM-herstelbewerking kunt doen.

    - Gebruik de sjabloon die is gegenereerd als onderdeel van de herstelbewerking opnieuw naar de instellingen aanpassen en het activeren van VM-implementatie. U de standaardinstellingen van de sjabloon bewerken en het verzenden van de sjabloon voor VM-implementatie.
    - U kunt [Koppel de herstelde schijven](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps) aan een bestaande virtuele machine.
    - U kunt [maken van een nieuwe virtuele machine](backup-azure-vms-automation.md#restore-an-azure-vm) van de herstelde schijven met behulp van PowerShell.


## <a name="replace-existing-disks"></a>Bestaande schijven vervangen

Gebruik deze optie om te vervangen bestaande schijven in de huidige virtuele machine met het geselecteerde herstelpunt.

1. In **configuratie terugzetten**, klikt u op **vervangen bestaande**.
2. In **Type herstellen**, selecteer **vervangen schijf/s** (het herstelpunt dat de bestaande VM-schijf of schijven vervangt).
3. In **Faseringslocatie**, opgeven waarin momentopnamen van de huidige beheerde schijven moeten worden opgeslagen.

   ![De configuratiewizard vervangen bestaande herstellen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="track-the-restore-operation"></a>De herstelbewerking bijhouden
Nadat u de herstelbewerking opnieuw activeert, maakt de Backup-service een taak voor het bijhouden van de herstelbewerking opnieuw. De Backup-service wordt ook gemaakt en tijdelijk wordt weergegeven de melding in de **meldingen** gebied van de portal. Als u de melding niet ziet, selecteert u de **meldingen** symbool om uw meldingen weer te geven.

![Terugzetten is geactiveerd](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Klik op de hyperlink van de meldingen naar **BackupJobs** lijst. De details van de bewerkingen voor een bepaalde taak is beschikbaar in de **BackupJobs** bevat. Gaat u naar **BackupJobs** van het kluisdashboard door te klikken op de back-uptaken tegel, selecteert u **virtuele Azure-machine** om weer te geven van de taken die zijn gekoppeld aan de kluis.

De **back-uptaken** blade wordt geopend en toont de lijst met taken.

![Lijst met virtuele machines in een kluis](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

**Voortgangsbalk** is nu beschikbaar in de **herstellen Details** blade. De **herstellen Details** blade kan worden geopend door te klikken op een hersteltaak die de status is **in uitvoering**. De **voortgangsbalk** is beschikbaar in alle varianten van herstelbewerkingen zoals **nieuw**, **schijf herstellen** en **vervangen bestaande**. De gegevens die worden uitgevoerd door de voortgangsbalk herstellen zijn **geschatte tijd van terugzetten**, **percentage terugzetten** en **aantal bytes dat wordt overgebracht**.

Herstellen van de voortgangsbalk details worden hieronder vermeld:

- **Geschatte tijd van terugzetten** in eerste instantie biedt de benodigde tijd voor het voltooien van de herstelbewerking opnieuw. Als de bewerking wordt uitgevoerd, is de tijd vermindert en 0 keer bereikt wordt de herstelbewerking is voltooid.
- **Percentage van de herstelbewerking** biedt u de gegevens welk percentage van de herstelbewerking is voltooid.
- **Aantal overgedragen bytes** is beschikbaar in het uitvoeren van subtaak wanneer herstel vindt plaats via de nieuwe virtuele machine maken. Dit biedt dat de details van het aantal cijfers van bytes zijn overgedragen op basis van het totale aantal bytes dat moet worden overgedragen.


## <a name="use-templates-to-customize-a-restored-vm"></a>Sjablonen gebruiken voor het aanpassen van een herstelde VM
Na de [schijven herstelbewerking is voltooid](#Track-the-restore-operation), gebruikt u de sjabloon die is gegenereerd als onderdeel van de herstelbewerking opnieuw een nieuwe virtuele machine maken met een configuratie verschilt van de back-upconfiguratie. Ook kunt u het aanpassen van de namen van resources die zijn gemaakt tijdens het proces voor het maken van een nieuwe virtuele machine vanaf een herstelpunt.

![Taak inzoomen herstellen](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

Aan de sjabloon die is gegenereerd als onderdeel van de optie voor terugzetten schijven:

1. Ga naar de **taakdetails herstellen** overeenkomt met de taak.

2. Op de **taakdetails herstellen** scherm, selecteer **-sjabloon implementeren** sjabloonimplementatie initiëren.

3. Op de **implementeren sjabloon** blade voor aangepaste implementatie, gebruik de sjabloonimplementatie naar [bewerken en implementeer de sjabloon](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) of toevoegen van meer aanpassingen door [ontwerpen van een sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) voordat u implementeert.

  ![Sjabloonimplementatie laden](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Nadat u de vereiste waarden invoert, accepteert de **voorwaarden en bepalingen** en selecteer **aankoop**.

  ![Sjabloonimplementatie verzenden](./media/backup-azure-arm-restore-vms/submitting-template1.png)

## <a name="post-restore-steps"></a>Stappen na herstellen
* Als u een cloud-init-op basis van Linux-distributie, zoals Ubuntu, uit veiligheidsoverwegingen, het wachtwoord is geblokkeerd terugzetten plaatsen. De VMAccess-extensie op de herstelde virtuele machine te gebruiken [het wachtwoord opnieuw instellen](../virtual-machines/linux/reset-password.md). U wordt aangeraden met behulp van SSH-sleutels van deze distributies om te voorkomen dat het terugzetten van de post wachtwoord opnieuw in te stellen.
* Extensies aanwezig zijn tijdens de back-upconfiguratie zijn geïnstalleerd, maar ze niet ingeschakeld. Als er een probleem, de extensies installeren.
* Als de VM een back-up statische IP-post terugzetten, heeft de herstelde virtuele machine met een dynamische IP-adres om te voorkomen van conflicten bij het maken van een herstelde VM. Meer informatie over hoe u kunt [een statisch IP-adres toevoegen aan een herstelde VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Een herstelde virtuele machine beschikt niet over een beschikbaarheid van waarde ingesteld. Het is raadzaam de optie voor terugzetten schijven te gebruiken [toevoegen van een beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) wanneer u een virtuele machine maken vanuit PowerShell of sjablonen met behulp van schijven hersteld.


## <a name="backup-for-restored-vms"></a>Back-up voor de herstelde VM 's
Als u een virtuele machine naar de dezelfde resourcegroep met dezelfde naam als de VM oorspronkelijk een back-up teruggezet, blijft de back-up op de VM na herstel. Als u de virtuele machine hersteld naar een andere resourcegroep of een andere naam voor de herstelde virtuele machine is opgegeven, wordt de virtuele machine wordt behandeld alsof het een nieuwe virtuele machine. U moet back-up instellen voor de herstelde virtuele machine.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Herstellen van een virtuele machine tijdens een noodgeval voor Azure-datacenter
Azure Backup kunt een back-up VM's herstellen naar het gekoppelde datacenter als het primaire datacenter waar de virtuele machines worden uitgevoerd na een noodgeval voordoet en u de back-upkluis geografisch redundante worden geconfigureerd. Selecteer een opslagaccount, die aanwezig is in een gekoppeld datacenter tijdens dergelijke scenario's. De rest van het herstelproces blijft hetzelfde. Back-up maakt gebruik van de compute-service van de gekoppelde geografische regio te maken van de herstelde VM. Zie voor meer informatie, [Azure-datacenter tolerantie](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restore-domain-controller-vms"></a>Domeincontroller virtuele machines herstellen
Back-up van de domeincontroller (DC) virtuele machines is een ondersteund scenario met back-up. Echter, moet u zorgvuldig tijdens het herstelproces. Het juiste herstelproces is afhankelijk van de structuur van het domein. In het meest eenvoudige geval hebt u een enkele DC in één domein. Vaker voor productie-belastingen hebt u één domein met meerdere DC's, mogelijk met bepaalde DC's on-premises. Ten slotte kunt u een forest met meerdere domeinen hebben.

Vanuit het perspectief van een Active Directory, wordt de virtuele machine van Azure is net als andere VM's op een moderne ondersteunde hypervisor. Het belangrijkste verschil met on-premises hypervisors is dat er geen VM-console beschikbaar zijn in Azure. Een console is vereist voor bepaalde scenario's, zoals het herstellen met behulp van een bare metal recovery (BMR)-back-up. VM herstellen vanuit de back-upkluis is echter een volledige vervanging voor BMR. Directory Services Restore Mode (DSRM) is ook beschikbaar, zodat alle Active Directory herstelscenario's mogelijk zijn. Zie voor meer informatie, [overwegingen voor back-up en herstel voor gevirtualiseerde domeincontrollers](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) en [plannen voor herstel van Active Directory-forest](https://technet.microsoft.com/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Enkele DC in één domein
De virtuele machine kan worden hersteld (zoals andere VM's) vanuit de Azure-portal of met behulp van PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Meerdere domeincontrollers in één domein
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC, zoals een virtuele machine worden hersteld. Als dit de laatste resterende DC in het domein, of een herstel in een geïsoleerd netwerk wordt uitgevoerd, kan een herstelprocedure forest moet worden gevolgd.

### <a name="multiple-domains-in-one-forest"></a>Meerdere domeinen in één forest
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC, zoals een virtuele machine worden hersteld. In alle andere gevallen wordt aangeraden een voor forestherstel.

## <a name="restore-vms-with-special-network-configurations"></a>Virtuele machines herstellen met speciale netwerkconfiguraties
Het is mogelijk om back-up maken en herstellen van virtuele machines met de volgende speciale netwerkconfiguraties. Deze configuraties vereisen echter bepaalde letten tijdens het doorlopen van het herstelproces:

* Virtuele machines onder load balancers (intern en extern)
* Virtuele machines met meerdere gereserveerde IP-adressen
* Virtuele machines met meerdere NIC 's

> [!IMPORTANT]
> Wanneer u de speciale netwerkconfiguratie voor virtuele machines maakt, moet u PowerShell gebruiken voor het maken van virtuele machines van de herstelde schijven.
>
>

Voor het volledig opnieuw maken van de VM's na het herstellen naar de schijf, de volgende stappen uit:

1. Herstel de schijven van een Recovery Services-kluis met behulp van [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

1. De VM-configuratie vereist voor load balancer maken / meerdere NIC/meerdere gereserveerd IP met behulp van de PowerShell-cmdlets. Gebruiken om u te maken van de virtuele machine met de gewenste configuratie:

   a. Een virtuele machine maken in de cloudservice met een [interne load balancer](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Maak een virtuele machine verbinding maken met een [internetgerichte load balancer](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/).

   c. Maak een VM met [meerdere NIC's](../virtual-machines/windows/multiple-nics.md).

   d. Maak een VM met [meerdere gereserveerde IP-adressen](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md).

## <a name="next-steps"></a>Volgende stappen
Nu kunt u uw virtuele machines herstellen, Zie het artikel over probleemoplossing voor informatie over veelvoorkomende problemen met virtuele machines. Controleer ook het artikel over het beheren van taken met uw VM's.

* [Het oplossen van problemen](backup-azure-vms-troubleshoot.md#restore)
* [Virtuele machines beheren](backup-azure-manage-vms.md)
