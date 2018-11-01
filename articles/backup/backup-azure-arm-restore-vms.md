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
ms.openlocfilehash: 7de9d1674860a6369c1dc09462a06def672fbdf2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420520"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>De Azure portal gebruiken voor virtuele machines herstellen
Bescherm uw gegevens door het maken van momentopnamen van uw gegevens met gedefinieerde intervallen. Deze momentopnamen zijn voorzien van herstelpunten en ze zijn opgeslagen in Recovery Services-kluizen. Als het nodig zijn om te herstellen of opnieuw opbouwen van een virtuele machine (VM), kunt u de virtuele machine herstellen uit een van de opgeslagen herstelpunten. Wanneer u een herstelpunt herstelt, kunt u het volgende doen:

* Maak een nieuwe virtuele machine, een point-in-time-weergave van uw VM waarvan een back-up is gemaakt is.
* Herstel de schijven, en gebruik de sjabloon die wordt geleverd met het proces voor het aanpassen van de herstelde virtuele machine, of kan het herstel van een afzonderlijk bestand.

In dit artikel wordt uitgelegd hoe u een virtuele machine herstellen naar een nieuwe virtuele machine of alle schijven van een back-up hersteld. Zie voor herstel van afzonderlijke bestanden, [bestanden herstellen van een Azure-VM back-up](backup-azure-restore-files-from-vm.md).

![Drie manieren om te herstellen vanuit back-up van virtuele machine](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager en klassiek](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines geïmplementeerd met behulp van de Resource Manager-model.
>
>

Herstellen van een virtuele machine of alle schijven van virtuele machine back-up bestaat uit twee stappen:

* Selecteer een herstelpunt voor herstel.
* Selecteer het type herstel, maak een nieuwe virtuele machine of herstel de schijven en geef de vereiste parameters.

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

    (a) Klik met de rechtermuisknop op het herstelpunt dat in deze blade (minder dan 30 dagen) en initiëren **VM terugzetten**.

    (b) om het herstel van gegevenspunten groter is dan 30 dagen, klikt u op het hier opgegeven in de blade kan worden gebruikt.

    c) **VM terugzetten** in het menu header bevat een optie voor het weergeven en filteren van de virtuele machines in aangepaste datums als voorkeur.

    Gebruik het Filter om te wijzigen van het tijdsbereik van de herstelpunten weergegeven. Herstelpunten van alle consistenties worden standaard weergegeven. Het filter met alle terugzetten punten om te selecteren van een specifieke restore point consistentie wijzigen. Zie voor meer informatie over elk type herstelpunt [gegevensconsistentie](backup-azure-vms-introduction.md#data-consistency).

    Opties voor consistentiecontrole herstellen:
    - Crashconsistente herstelpunten
    - Toepassingsconsistente herstelpunten
    - Bestandssysteem toepassingsconsistente herstelpunten
    - Alle herstelpunten

  ![Herstelpunten](./media/backup-azure-arm-restore-vms/vm-blade1.png)

    >  [!NOTE]
    > Hersteltype vertegenwoordigen als deze zich binnen het opslagaccount van de klant, in de kluis of beide. Meer informatie over [Instant herstelpunt](https://azure.microsoft.com/blog/large-disk-support/).

8. Op de **herstellen** Selecteer **herstelpunt**.

    ![Herstelpunt selecteren](./media/backup-azure-arm-restore-vms/select-recovery-point1.png)

    De **herstellen** blade ziet u dat het herstelpunt is ingesteld op te klikken op **OK**.
9. Als u bent nog niet is, gaat u naar de **herstellen** blade. Zorg ervoor dat een [herstelpunt is geselecteerd](#select-a-restore-point-for-restore), en selecteer **configuratie terugzetten**. De **configuratie terugzetten** blade wordt geopend.

## <a name="choose-a-vm-restore-configuration"></a>Kies de configuratie van een virtuele machine terugzetten
Nadat u het herstelpunt geselecteerd, kiest u een VM-configuratie voor herstel. Voor het configureren van de herstelde virtuele machine, kunt u de Azure portal of PowerShell.

1. Als u bent nog niet is, gaat u naar de **herstellen** blade. Zorg ervoor dat een [herstelpunt is geselecteerd](#select-a-restore-point-for-restore), en selecteer **configuratie terugzetten**. De **configuratie terugzetten** blade wordt geopend.
2. Deze blade heeft momenteel twee opties voor één wordt **nieuw** is standaard en de andere is **vervangen bestaande** is in-place herstellen ter vervanging van de schijven alleen behouden de bestaande configuraties en -extensies.

> [!NOTE]
> Er wordt gewerkt aan het vervangen van de hele virtuele machine met de volgende schijf of schijven, netwerkinstellingen, configuraties en -extensies in de komende maanden.
>
>

 In de **nieuw** optie die wordt hersteld naar de gegevens naar de nieuwe virtuele machine of nieuwe schijven, hebt u twee opties:

 ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/restore-configuration-create-new.png)

 - **Virtuele machine maken**
 - **Schijven herstellen**

![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)

De portal biedt een **snelle invoer** een herstelde virtuele machine. Voor het aanpassen van de VM-configuratie of de namen van de resources die worden gemaakt als onderdeel van het maken van de keuze van een nieuwe virtuele machine, moet u PowerShell of de portal gebruiken voor het herstel de schijven waarvan een back-up is gemaakt. PowerShell-opdrachten gebruiken voor deze koppelt aan uw eigen keuze aan VM-configuratie. Of u kunt de sjabloon die wordt geleverd met herstelde schijven om aan te passen van de herstelde VM. Zie voor meer informatie over het herstellen van een virtuele machine die meerdere NIC's heeft of onder een load balancer [een virtuele machine terugzetten met speciale netwerkconfiguraties](#restore-vms-with-special-network-configurations). Als uw Windows-VM gebruikmaakt [HUB licentieverlening](../virtual-machines/windows/hybrid-use-benefit-licensing.md), herstel de schijven en PowerShell/sjabloon zoals opgegeven in dit artikel gebruiken om te maken van de virtuele machine. Zorg ervoor dat u opgeeft de **licentietype** als "Windows_Server" terwijl u de virtuele machine als u gebruik van de HUB voordelen op de herstelde virtuele machine wilt maken. Houd er rekening mee dat kunt dit doen na het maken van virtuele machine ook later opnieuw.

## <a name="create-a-new-vm-from-a-restore-point"></a>Een nieuwe virtuele machine maken vanaf een herstelpunt
1. Op de **configuratie terugzetten** blade vermeld in de vóór sectie, invoeren of selecteren van waarden voor elk van de volgende velden:

    a. **Type herstellen**. Hiermee maakt u een virtuele machine.

    b. **Naam van virtuele machine**. Voer de naam van de virtuele machine die niet bestaat in het abonnement.

    c. **Resourcegroep**. Gebruik een bestaande resourcegroep of maak een nieuwe. Als u een klassieke virtuele machine terugzetten bent, moet u dit veld gebruiken om op te geven van de naam van een nieuwe cloudservice. Als u een nieuwe resource-group/cloudservice maakt, is de naam moet globaal uniek zijn. Normaal gesproken de naam van de cloudservice is gekoppeld aan een openbare URL: bijvoorbeeld [cloudservice]. cloudapp.net. Als u probeert te gebruiken, een naam op voor de cloud resource group/cloud service al in gebruik is, wijst Azure resource group/cloud service dezelfde naam als de virtuele machine. Azure weergeven resource groepen/cloud services en virtuele machines die niet zijn gekoppeld aan een affiniteitsgroepen Zie voor meer informatie, [van affiniteitsgroepen migreren naar een regionaal virtueel netwerk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Virtueel netwerk**. Het virtuele netwerk selecteert wanneer u een virtuele machine maakt. Het veld bevat alle virtuele netwerken die zijn gekoppeld aan het abonnement. De resourcegroep van de virtuele machine wordt weergegeven tussen haakjes.

    e. **Subnet**. Als het virtuele netwerk subnetten bevat, wordt het eerste subnet is standaard geselecteerd. Als er extra subnetten, selecteert u het subnet dat u wilt.

    f. **Opslaglocatie**. Storage-accounts zijn de faseringslocatie. Dit menu bevat de storage-accounts op dezelfde locatie als de Recovery Services-kluis. Storage-accounts die zone-redundant zijn worden niet ondersteund. Als er geen opslagaccounts met dezelfde locatie als de Recovery Services-kluis zijn, moet u een maken voordat u begint met de herstelbewerking opnieuw. Type van de replicatie van de storage-account wordt weergegeven tussen haakjes.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

    > [!NOTE]
    > * Een virtueel netwerk is optioneel voor een klassieke virtuele machine en verplicht voor Resource Manager geïmplementeerde virtuele machine.

    > * Opslagtype opgegeven in de opslag (premium of standard)-account in de faseringslocatie besluit het type schijfopslag terugzetten. Op dit moment ondersteund niet een gemengde modus van schijven bij het herstellen.
    >
    >

2. Op de **configuratie terugzetten** Selecteer **OK** voor het voltooien van de configuratie herstellen. Op de **herstellen** Selecteer **herstellen** voor het activeren van de herstelbewerking opnieuw.

## <a name="restore-backed-up-disks"></a>Herstel de schijven waarvan een back-up is gemaakt
Typewaarde herstellen **schijf voor het terugzetten** in **configuratie terugzetten** blade kunt u gegevens kunt maken van een virtuele machine met aangepaste configuraties. Tijdens het herstellen van schijven, moet de Storage-account te selecteren in dezelfde locatie als de Recovery services-kluis. Het is verplicht om een opslagaccount te maken als er geen opslagaccounts met dezelfde locatie als de Recovery Services-kluis zijn. ZRS-opslagaccounts worden niet ondersteund. Type van de replicatie van Storage-Account wordt weergegeven tussen haakjes.

Na de herstelbewerking, gebruikt u hieronder:
* [Gebruik de sjabloon voor het aanpassen van de herstelde VM](#use-templates-to-customize-restore-vm)
* [De herstelde schijven gebruiken om te koppelen aan een bestaande virtuele machine](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Een nieuwe virtuele machine maken met behulp van PowerShell van herstelde schijven](./backup-azure-vms-automation.md#restore-an-azure-vm)

Op de **configuratie terugzetten** Selecteer **OK** voor het voltooien van de configuratie herstellen. Op de **herstellen** Selecteer **herstellen** voor het activeren van de herstelbewerking opnieuw.

![Herstelconfiguratie van is voltooid](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

In **plaats herstellen** wordt loopt via het tabblad **vervangen bestaande**.

## <a name="replace-existing-disks-from-a-restore-point"></a>Vervang de bestaande schijven vanaf een herstelpunt
**Vervang bestaande** optie helpt bij het vervangen van bestaande schijven in de huidige virtuele machine met het geselecteerde herstelpunt. Deze bewerking kan alleen worden uitgevoerd als de huidige VM zich bevindt. Als deze zijn verwijderd vanwege een redenen, kan deze bewerking kan niet worden uitgevoerd; u kunt ook, raden we u om te doen **nieuw** VM of schijven om door te gaan met het terugzetten. Tijdens bewerkingen voor schijven, bestaande als voorzorgsmaatregel vervangen we back-up van de gegevens voordat bewerkingen voor het initiëren van de vervangen-schijven. Als het herstelpunt schijven die meer/kleiner dan de huidige virtuele machine, wordt het aantal schijven in het herstelpunt alleen in de virtuele machine weergegeven wordt heeft. **Vervang bestaande** optie is momenteel niet ondersteund voor niet-beheerde schijven en versleutelde virtuele machines. Dit wordt ook niet ondersteund voor [gegeneraliseerde VM's](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) en voor virtuele machines die zijn gemaakt met behulp van [aangepaste installatiekopieën](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).  

 Op de **configuratie herstellen** blade, de enige invoer die moet worden geselecteerd is **Faseringslocatie**.

   ![De configuratiewizard vervangen bestaande herstellen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

 a. **Type herstellen**. Vervang of meer schijven die aangeeft dat het geselecteerde herstelpunt wordt vervangen door de schijven in de bestaande virtuele machine.

 b. **Faseringslocatie**. Storage-accounts zijn de faseringslocatie voor beheerde schijven. Dit menu bevat de storage-accounts op dezelfde locatie als de Recovery Services-kluis. Storage-accounts die zone-redundant zijn worden niet ondersteund. Als er geen opslagaccounts met dezelfde locatie als de Recovery Services-kluis zijn, moet u een maken voordat u begint met de herstelbewerking opnieuw. Type van de replicatie van de storage-account wordt weergegeven tussen haakjes.

## <a name="track-the-restore-operation"></a>De herstelbewerking bijhouden
Nadat u de herstelbewerking opnieuw activeert, maakt de Backup-service een taak voor het bijhouden van de herstelbewerking opnieuw. De Backup-service wordt ook gemaakt en tijdelijk wordt weergegeven de melding in de **meldingen** gebied van de portal. Als u de melding niet ziet, selecteert u de **meldingen** symbool om uw meldingen weer te geven.

![Terugzetten is geactiveerd](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Klik op de hyperlink van de meldingen naar **BackupJobs** lijst. De details van de bewerkingen voor een bepaalde taak is beschikbaar in de **BackupJobs** bevat. Gaat u naar **BackupJobs** van het kluisdashboard door te klikken op de back-uptaken tegel, selecteert u **virtuele Azure-machine** om weer te geven van de taken die zijn gekoppeld aan de kluis.

De **back-uptaken** blade wordt geopend en toont de lijst met taken.

![Lijst met virtuele machines in een kluis](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

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
