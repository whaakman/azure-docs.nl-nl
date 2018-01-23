---
title: 'Azure Backup: Virtuele machines herstellen met behulp van de Azure portal | Microsoft Docs'
description: Azure een virtuele machine vanaf een herstelpunt herstellen met behulp van de Azure-portal
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: terugzetten van back-up. het herstellen van; herstelpunt;
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 1a1855cc3f83d7fcba749ce94167039feb5bebe1
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>De Azure portal gebruiken voor virtuele machines herstellen
Uw gegevens beschermen door het maken van momentopnamen van uw gegevens op de gedefinieerde intervallen. Deze momentopnamen worden aangeduid als herstelpunten en ze zijn opgeslagen in de Recovery Services-kluizen. Als het nodig zijn om te herstellen of opnieuw opbouwen van een virtuele machine (VM), kunt u de virtuele machine herstellen uit een van de opgeslagen herstelpunten. Wanneer u een herstelpunt herstelt, kunt u het volgende doen:

* Maak een nieuwe virtuele machine, een punt in tijd representatie van een back-up van de VM is.
* Herstellen van schijven, en gebruik de sjabloon die wordt geleverd met het proces voor het aanpassen van de herstelde virtuele machine of doet u het herstel van een afzonderlijk bestand. 

In dit artikel wordt uitgelegd hoe u een virtuele machine naar een nieuwe virtuele machine herstellen of alle schijven van de back-up herstellen. Zie voor herstel van afzonderlijke bestanden, [bestanden terugzetten vanuit een virtuele machine van Azure back-up](backup-azure-restore-files-from-vm.md).

![Drie manieren om de VM-back-up terugzetten](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines geïmplementeerd met behulp van de Resource Manager-model.
>
>

Herstellen van een virtuele machine of alle schijven van virtuele machine back-up bestaat uit twee stappen:

* Selecteer een herstelpunt voor herstel.
* Selecteer het type herstel, een nieuwe virtuele machine maken of terugzetten van schijven en geef de vereiste parameters. 

## <a name="select-a-restore-point-for-restore"></a>Selecteer een herstelpunt voor herstel
1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

2. Selecteer in het menu van Azure **Bladeren**. Typ in de lijst met services **Recovery Services**. De lijst met services wordt aangepast aan wat u typt. Wanneer er **Recovery Services-kluizen**, selecteert u deze.

    ![Recovery Services-kluis](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    De lijst met kluizen in het abonnement wordt weergegeven.

    ![Lijst met Recovery Services-kluizen](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Selecteer de kluis die is gekoppeld aan de virtuele machine die u wilt herstellen in de lijst. Wanneer u de kluis selecteert, wordt het dashboard wordt geopend.

    ![Recovery Services-kluis geselecteerd](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Op het kluisdashboard op de **back-Upitems** tegel, selecteer **Azure Virtual Machines**.

    ![kluisdashboard](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    De **back-Upitems** blade wordt geopend en wordt de lijst met virtuele Azure-machines.

    ![lijst met virtuele machines in de kluis](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Selecteer een virtuele machine om het dashboard te openen in de lijst. Hiermee opent u het VM-dashboard aan het gebied bewaking, waarin de **herstelpunten** tegel.

    ![Herstelpunten](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. Selecteer in het menu van het dashboard VM **herstellen**.

    ![Knop herstellen](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    De **herstellen** blade wordt geopend.

    ![Blade herstellen](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. Op de **herstellen** blade Selecteer **herstelpunt**. De **herstelpunt selecteren** blade wordt geopend.

    ![Herstelpunt selecteren](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    In het dialoogvenster wordt standaard weergegeven van alle herstelpunten van de laatste 30 dagen. Gebruik de **Filter** voor het wijzigen van het tijdsbereik van de herstelpunten weergegeven. Standaard worden de herstelpunten van alle consistencies weergegeven. Wijzig de **alle herstelpunten** filtert u de consistentie van een specifieke terugzetten punt. Zie voor meer informatie over elk type van het van herstelpunt [gegevensconsistentie](backup-azure-vms-introduction.md#data-consistency).

    Opties voor consistentiecontrole herstellen:

     * Consistente herstelpunten crash
     * Toepassing consistente herstelpunten
     * Bestandssysteem consistent herstelpunten
     * Alle herstelpunten

8. Kies een herstelpunt en selecteer **OK**.

    ![Opties voor terugzetten](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    De **herstellen** blade ziet u dat het herstelpunt is ingesteld.

9. Als u bent niet al er, gaat u naar de **herstellen** blade. Zorg ervoor dat een [herstelpunt is geselecteerd](#select-restore-point-for-restore), en selecteer **configuratie terugzetten**. De **configuratie terugzetten** blade wordt geopend.

## <a name="choose-a-vm-restore-configuration"></a>Kies een configuratie van de virtuele machine herstellen
Nadat u het herstelpunt selecteert, kiest u de configuratie van een virtuele machine herstellen. U kunt de Azure-portal of PowerShell gebruiken voor het configureren van de herstelde virtuele machine.

1. Als u bent niet al er, gaat u naar de **herstellen** blade. Zorg ervoor dat een [herstelpunt is geselecteerd](#select-restore-point-for-restore), en selecteer **configuratie terugzetten**. De **configuratie terugzetten** blade wordt geopend.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. Op de **configuratie terugzetten** blade hebt u twee mogelijkheden:

   * **Virtuele machine maken**

   * **Herstellen van schijven**

De portal biedt een **snelle invoer** optie voor een herstelde virtuele machine. Gebruik PowerShell of de portal om terug te zetten back-up-schijven voor het aanpassen van de VM-configuratie of de namen van de resources die zijn gemaakt als onderdeel van het maken van een nieuwe VM-keuze. PowerShell-opdrachten gebruiken voor deze koppelt aan uw keuze van VM-configuratie. Of u kunt de sjabloon die wordt geleverd met een herstelde schijven voor het aanpassen van de herstelde virtuele machine. Zie voor meer informatie over het herstellen van een virtuele machine die meerdere NIC's of onder een load balancer is [herstellen van een virtuele machine met speciale netwerkconfiguraties](#restore-a vm-with-special-network-configurations). Als uw virtuele machine van Windows gebruikt [HUB licentieverlening](../virtual-machines/windows/hybrid-use-benefit-licensing.md), het herstel van schijven en gebruik PowerShell/sjabloon zoals opgegeven in dit artikel te maken van de virtuele machine. Zorg ervoor dat u opgeeft de **licentietype** als 'Windows_Server' bij het maken van de virtuele machine als u wilt gebruikmaken van HUB voordelen op de herstelde virtuele machine. 
 
## <a name="create-a-new-vm-from-a-restore-point"></a>Een nieuwe virtuele machine van een herstelpunt maken
1. Als u niet al daar [Selecteer een herstelpunt](#restore-a vm-with-special-network-configurations) voordat u begint met een nieuwe virtuele machine van een herstelpunt maken. Nadat u een herstelpunt selecteert op de **configuratie terugzetten** blade invoeren of selecteren van waarden voor elk van de volgende velden:

    a. **Type herstellen**. Maak een virtuele machine.

    b. **Naam van virtuele machine**. Geef een naam voor de virtuele machine. De naam moet uniek zijn voor de resourcegroep (voor een Azure Resource Manager geïmplementeerde VM) of de cloudservice (voor een klassieke VM). U kunt de virtuele machine niet vervangen als deze al in het abonnement bestaat.

    c. **Resourcegroep**. Gebruik een bestaande resourcegroep of maak een nieuwe. Als u een klassieke virtuele machine terugzetten wilt, moet u dit veld gebruiken om op te geven van de naam van een nieuwe cloudservice. Als u een nieuwe resource-group/cloudservice maakt, is de naam moet globaal uniek zijn. Normaal gesproken de naam van de cloudservice is gekoppeld aan een openbare URL: bijvoorbeeld [cloudservice]. cloudapp.net. Als u probeert te gebruiken, een naam voor de cloud/cloudservice voor een groep resource al in gebruik is, wijst Azure de bron/het cloudservice dezelfde naam als de virtuele machine. Azure weergeven resource groepen/cloudservices en virtuele machines die niet zijn gekoppeld aan een affiniteitsgroepen Zie voor meer informatie [van affiniteitsgroepen migreren naar een regionaal virtueel netwerk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Virtueel netwerk**. Selecteer het virtuele netwerk bij het maken van de virtuele machine. Het veld bevat alle virtuele netwerken die zijn gekoppeld aan het abonnement. De resourcegroep van de virtuele machine wordt weergegeven tussen haakjes.

    e. **Subnet**. Als het virtuele netwerk subnetten heeft, wordt het eerste subnet is standaard ingeschakeld. Als er extra subnetten, selecteer de gewenste subnet.

    f. **Storage-Account**. Dit menu geeft een lijst van de storage-accounts op dezelfde locatie als de Recovery Services-kluis. Storage-accounts die zoneredundant zijn worden niet ondersteund. Als er geen opslagaccounts met dezelfde locatie als de Recovery Services-kluis, moet u een maken voordat u de herstelbewerking start. Type van de replicatie van de storage-account wordt weergegeven tussen haakjes.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

    > [!NOTE]
    > * Als u een Resource Manager geïmplementeerde VM herstellen, moet u een virtueel netwerk te identificeren. Een virtueel netwerk is optioneel voor een klassieke virtuele machine.

    > * Als u virtuele machines met beheerde schijven herstellen, zorg er dan voor dat het geselecteerde opslagaccount is niet ingeschakeld voor Azure Storage-Service: versleuteling in de levensduur.

    > * Op basis van het type van het geselecteerde opslagaccount (premium of standaard), alle schijven teruggezet worden premium of standaardschijven. Er wordt een gemengde modus van schijven momenteel niet ondersteund bij het herstellen.
    >
    >

2. Op de **configuratie terugzetten** blade Selecteer **OK** voor het voltooien van de configuratie herstellen. Op de **herstellen** blade Selecteer **herstellen** voor het activeren van de herstelbewerking opnieuw.

## <a name="restore-backed-up-disks"></a>Terugzetten van back-up-schijven
Voor het aanpassen van de virtuele machine die u maken van back-up schijven verschilt wilt van wat is aanwezig in de **configuratie terugzetten** blade Selecteer **schijven herstellen** als de waarde voor **herstellen Type**. Deze keuze vraagt om een opslagaccount waar schijven uit de back-ups zullen worden gekopieerd. Wanneer u een opslagaccount kiest, selecteert u een account dat u, dezelfde locatie als de Recovery Services-kluis deelt. Storage-accounts die zoneredundant zijn worden niet ondersteund. Als er geen opslagaccounts met dezelfde locatie als de Recovery Services-kluis, moet u een maken voordat u de herstelbewerking start. Type van de replicatie van de storage-account wordt weergegeven tussen haakjes.

Nadat de herstelbewerking is voltooid, kunt u:

* [Gebruik de sjabloon voor het aanpassen van de herstelde virtuele machine](#use-templates-to-customize-restore-vm)
* [De herstelde schijven gebruiken om te koppelen aan een bestaande virtuele machine](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Een nieuwe virtuele machine maken met behulp van PowerShell van herstelde schijven](./backup-azure-vms-automation.md#restore-an-azure-vm)

Op de **configuratie terugzetten** blade Selecteer **OK** voor het voltooien van de configuratie herstellen. Op de **herstellen** blade Selecteer **herstellen** voor het activeren van de herstelbewerking opnieuw.

![De herstelconfiguratie voltooid](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-the-restore-operation"></a>De herstelbewerking bijhouden
Nadat u de herstelbewerking opnieuw activeert, maakt de Backup-service een taak voor het bijhouden van de herstelbewerking opnieuw. De Backup-service maakt ook en tijdelijk geeft de melding in de **meldingen** gebied van de portal. Als u de melding niet ziet, selecteert u de **meldingen** symbool om uw meldingen weer te geven.

![Herstel is geactiveerd](./media/backup-azure-arm-restore-vms/restore-notification.png)

De bewerking uit te zien wanneer het verwerken van of deze weer te geven wanneer deze voltooid, opent u de **back-uptaken** lijst.

1. Selecteer in het menu van Azure **Bladeren**, en typt u in de lijst met services **Recovery Services**. De lijst met services wordt aangepast aan wat u typt. Wanneer er **Recovery Services-kluizen**, selecteert u deze.

    ![Recovery Services-kluis openen](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    De lijst met kluizen in het abonnement wordt weergegeven.

    ![Lijst met Recovery Services-kluizen](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Selecteer de kluis die is gekoppeld aan de virtuele machine die u teruggezet in de lijst. Wanneer u de kluis selecteert, wordt het dashboard wordt geopend.

3. Op het kluisdashboard op de **back-uptaken** tegel, selecteer **virtuele Azure-machines** om weer te geven van de taken die zijn gekoppeld aan de kluis.

    ![kluisdashboard](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    De **back-uptaken** blade wordt geopend en wordt de lijst met taken.

    ![Lijst met VM's in een kluis](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-to-customize-a-restored-vm"></a>Sjablonen gebruiken voor het aanpassen van een herstelde virtuele machine
Na de [schijven herstelbewerking](#Track-the-restore-operation), de sjabloon die is gegenereerd als onderdeel van de herstelbewerking opnieuw gebruiken voor het maken van een nieuwe virtuele machine met een configuratie verschilt van de back-upconfiguratie. Ook kunt u het aanpassen van de namen van bronnen die zijn gemaakt tijdens het maken van een nieuwe virtuele machine van een herstelpunt. 

> [!NOTE]
> Sjablonen worden toegevoegd als onderdeel van de schijven voor de herstelpunten die zijn gemaakt na 1 maart 2017 terugzetten. Ze zijn van toepassing op niet-beheerde schijf virtuele machines. Ondersteuning voor beheerde schijf virtuele machines wordt binnenkort in toekomstige releases. 
>
>

De sjabloon die is gegenereerd als onderdeel van de optie voor terugzetten schijven ophalen:

1. Ga naar de restore-taakdetails overeenkomt met de taak.

2. Op de **taakdetails herstellen** Schakel in het scherm **sjabloon implementeren** sjabloonimplementatie van de te starten. 

     ![taak inzoomen herstellen](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
3. Op de **implementeren sjabloon** blade voor aangepaste implementatie, gebruik sjabloonimplementatie naar [bewerken en implementeren van de sjabloon](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) of toevoeg meer aanpassingen door [ontwerpen van een sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) voordat u implementeert. 

   ![Sjabloonimplementatie laden](./media/backup-azure-arm-restore-vms/loading-template.png)
   
4. Nadat u de vereiste waarden hebt ingevoerd, accepteer de **voorwaarden en bepalingen** en selecteer **aankoop**.

   ![Sjabloonimplementatie verzenden](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Stappen na herstellen
* Als u een init-Linux clouddistributiepunt, zoals Ubuntu, uit veiligheidsoverwegingen, het wachtwoord is geblokkeerd boeken terugzetten. De VMAccess-extensie op de herstelde virtuele machine te gebruiken [wachtwoordherstel](../virtual-machines/linux/reset-password.md). U wordt aangeraden met behulp van SSH-sleutels op deze verdelingen om te voorkomen dat het herstellen van de post wachtwoord opnieuw instellen.
* Uitbreidingen die aanwezig zijn tijdens de back-upconfiguratie zijn geïnstalleerd, maar niet ingeschakeld. Als er een probleem, installeer de uitbreidingen. 
* Als de back-up VM statische IP-post terugzetten heeft, heeft de herstelde virtuele machine een dynamische IP-adres om conflicten te voorkomen bij het maken van een herstelde virtuele machine. Meer informatie over hoe u kunt [een statische IP-adres toevoegen aan een herstelde virtuele machine](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Een teruggezette VM beschikt niet over een beschikbaarheid waarde ingesteld. Het is raadzaam de optie voor terugzetten schijven te gebruiken [toevoegen van een beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) wanneer u een virtuele machine maakt vanuit PowerShell of sjablonen met behulp van schijven teruggezet. 


## <a name="backup-for-restored-vms"></a>Back-up voor de herstelde virtuele machines
Als u een virtuele machine op dezelfde resourcegroep met dezelfde naam als de VM oorspronkelijk back-up is teruggezet, blijft de back-up op de VM post terugzetten. Als u de virtuele machine hersteld naar een andere resourcegroep of een andere naam voor de herstelde virtuele machine is opgegeven, wordt de virtuele machine behandeld alsof het een nieuwe virtuele machine. U moet back-up instellen voor de herstelde virtuele machine.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Herstellen van een virtuele machine tijdens een Azure-datacenter na noodgevallen
Azure Backup kunt back-up virtuele machines herstellen naar het datacenter gekoppeld als het primaire datacenter waarin de VM's worden uitgevoerd er een ramp optreedt en u de back-upkluis geografisch redundante worden geconfigureerd. Tijdens deze scenario's, selecteert u een opslagaccount aanwezig in een gekoppeld datacenter is. De rest van het herstelproces blijft hetzelfde. Back-up maakt gebruik van de compute-service van de gekoppelde geo voor het maken van de herstelde virtuele machine. Zie voor meer informatie [Azure-datacenter tolerantie](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restore-domain-controller-vms"></a>Domeincontroller virtuele machines herstellen
Back-up van de domeincontroller (DC) is een ondersteunde scenario met back-up van virtuele machines. Echter, moet u zorgvuldig tijdens het herstelproces. Het juiste herstelproces is afhankelijk van de structuur van het domein. In het meest eenvoudige geval hebt u een enkele DC in één domein. Meer vaak voor productie is geladen hebben u één domein met meerdere DC's, bijvoorbeeld met een aantal DC's on-premises. Ten slotte kunt u een forest met meerdere domeinen hebben. 

Vanuit het perspectief van een Active Directory, wordt de Azure VM is net als elke andere virtuele machine op een moderne ondersteunde hypervisor. Het belangrijkste verschil met lokale hypervisors is dat er geen VM-console beschikbaar zijn in Azure. Een console is vereist voor bepaalde scenario's, zoals het herstellen met behulp van een bare metal recovery (BMR)-back-up. VM-herstel van de back-upkluis is echter een volledige vervanging voor BMR. Directory Services Restore Mode (DSRM) is ook beschikbaar is, zodat alle Active Directory herstelscenario's mogelijk of gewenst zijn is. Zie voor meer informatie [overwegingen voor back-up en herstel voor gevirtualiseerde domeincontrollers](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) en [plannen voor herstel van Active Directory-forest](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Enkele DC in één domein
De virtuele machine kan worden hersteld (zoals VM) van de Azure-portal of met behulp van PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Meerdere domeincontrollers in één domein
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC zoals een VM worden hersteld. Als dit het laatste resterende DC in het domein is of een herstel in een geïsoleerd netwerk wordt uitgevoerd, kan een herstelprocedure forest moet worden gevolgd.

### <a name="multiple-domains-in-one-forest"></a>Meerdere domeinen in een forest
Wanneer andere DC's van hetzelfde domein kunnen worden bereikt via het netwerk, kan de DC zoals een VM worden hersteld. In andere gevallen wordt aangeraden een forestherstel.

## <a name="restore-vms-with-special-network-configurations"></a>Herstellen van virtuele machines met speciale netwerkconfiguraties
Het is mogelijk back-up en herstellen van virtuele machines met de volgende speciale netwerkconfiguraties. Deze configuraties vereisen echter een aantal letten tijdens het doorlopen van het herstelproces:

* VM's onder netwerktaakverdelers (intern en extern)
* Virtuele machines met meerdere gereserveerde IP 's
* Virtuele machines met meerdere NIC 's

> [!IMPORTANT]
> Wanneer u de speciale configuratie voor virtuele machines maakt, moet u PowerShell gebruiken voor het maken van virtuele machines van de herstelde schijven.
>
>

Om volledig opnieuw te maken de VM's na het herstellen naar de schijf, de volgende stappen uit:

1. Herstellen met behulp van de schijven van een Recovery Services-kluis [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

2. De VM-configuratie vereist voor de load balancer maken / meerdere NIC/meerdere gereserveerde IP-adres met behulp van de PowerShell-cmdlets. Gebruik het maken van de virtuele machine met de gewenste configuratie:

   a. Een virtuele machine maken in de cloudservice met een [interne load balancer](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Maken van een virtuele machine verbinding maken met een [internet gerichte load balancer](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/).

   c. Maak een VM met [meerdere NIC's](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/).

   d. Maak een VM met [meerdere IP-adressen gereserveerd](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/).

## <a name="next-steps"></a>Volgende stappen
Nu u uw virtuele machines kunt herstellen, gaat u naar artikel voor informatie over probleemoplossing voor veelvoorkomende problemen met virtuele machines. Controleer ook het artikel over het beheren van taken met uw virtuele machines.

* [Het oplossen van problemen](backup-azure-vms-troubleshoot.md#restore)
* [Virtuele machines beheren](backup-azure-manage-vms.md)
