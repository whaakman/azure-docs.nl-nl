---
title: 'Azure Backup: Virtuele machines herstellen met behulp van Azure portal'
description: Een virtuele Azure-machine herstellen vanaf een herstelpunt met behulp van Azure portal
services: backup
author: geethalakshmig
manager: vijayts
keywords: herstellen van back-up. het herstellen van; herstelpunt;
ms.service: backup
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: geg
ms.openlocfilehash: 2253e729daedc3b130919913c1616449245f9cc1
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315382"
---
# <a name="restore-azure-vms"></a>Azure-VM's herstellen

In dit artikel wordt beschreven hoe u Azure VM-gegevens herstellen vanaf de herstelpunten die zijn opgeslagen in [Azure Backup](backup-overview.md) Recovery Services-kluizen.

Om terug te zetten een virtuele machine Zorg ervoor dat u hebt de vereiste [RBAC](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) machtiging.

> [!NOTE]
> Als u nog geen [RBAC](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) machtiging kunt u uitvoeren [schijf herstellen](backup-azure-arm-restore-vms.md#create-new-restore-disks) en maakt met behulp van virtuele machine [-sjabloon implementeren](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) functie.

### <a name="restore-options"></a>Opties herstellen

Azure Backup biedt een aantal manieren om te herstellen van een virtuele machine.

**Optie herstellen** | **Details**
--- | ---
**Een nieuwe virtuele machine maken** | Snel maakt en een basis-VM te starten vanaf een herstelpunt opgehaald.<br/><br/> U kunt een naam opgeven voor de virtuele machine, selecteert u de resourcegroep en een virtueel netwerk (VNet) waarin deze wordt geplaatst, en een type opgeven.
**Schijf herstellen** | Hiermee herstelt u een VM-schijf die vervolgens kan worden gebruikt om te maken van een nieuwe virtuele machine.<br/><br/> Azure Backup biedt een sjabloon waarmee u kunt aanpassen en een virtuele machine maken. <br/><br/> Deze optie kopieert VHD's naar het opslagaccount dat u opgeeft. De hersteltaak genereert een sjabloon dat u kunt downloaden en gebruiken om aangepaste VM-instellingen te geven, en een virtuele machine maken.<br/><br/> -Het opslagaccount moet zich op dezelfde locatie als de kluis. Maak een storage-account als u dit niet hebt.<br/><br/> Het opslagaccounttype voor replicatie wordt weergegeven. Zone-redundante opslag (ZRS) wordt niet ondersteund.<br/><br/> U kunt ook de schijf koppelen aan een bestaande virtuele machine, of maak een nieuwe virtuele machine met behulp van PowerShell.<br/><br/> Deze optie is handig als u wilt aanpassen van de virtuele machine, configuratie-instellingen die niet er op het moment van back-up waren toevoegen of instellingen op die moet worden geconfigureerd met behulp van de sjabloon of PowerShell.
**Vervang bestaande** | U kunt een schijf herstellen en deze gebruiken om te vervangen door een schijf op de bestaande virtuele machine.<br/><br/> De huidige virtuele machine moet bestaan. Als deze is verwijderd met deze optie kan niet worden gebruikt.<br/><br/> Azure Backup maakt een momentopname van de bestaande virtuele machine voordat de schijf wordt vervangen. De momentopname wordt opgeslagen in de tijdelijke locatie die u opgeeft. Bestaande schijven die zijn verbonden met de virtuele machine vervolgens met behulp van het geselecteerde herstelpunt vervangen. De momentopname die is uitgevoerd, is gekopieerd naar de kluis en behouden in overeenstemming met het opgegeven bewaarbeleid.<br/><br/> Vervangen bestaande wordt ondersteund voor niet-versleutelde beheerde virtuele machines. Het wordt niet ondersteund voor niet-beheerde schijven [gegeneraliseerde VM's](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource), of voor virtuele machines [gemaakt met behulp van aangepaste installatiekopieën](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Als het herstelpunt min of meer schijven dan de huidige virtuele machine is, klikt u vervolgens in het aantal schijven in het herstelpunt alleen de VM-configuratie weer.

> [!NOTE]
> U kunt ook specifieke bestanden en mappen op een Azure-VM herstellen. [Meer informatie](backup-azure-restore-files-from-vm.md).
>
> Als u werkt met de [meest recente versie](backup-instant-restore-capability.md) van Azure Backup voor virtuele Azure-machines (direct herstellen genoemd), de momentopnamen voor maximaal zeven dagen worden bewaard, en kunt u een virtuele machine terugzetten van momentopnamen voordat de back-upgegevens worden verzonden naar de kluis. Als u herstellen van een virtuele machine vanuit een back-up van de afgelopen zeven dagen wilt, is het sneller om te herstellen vanuit de momentopname en niet uit de kluis.


## <a name="select-a-restore-point"></a>Selecteer een herstelpunt

1. In de kluis die zijn gekoppeld aan de virtuele machine die u wilt herstellen, klikt u op **back-up items** > **virtuele Azure-Machine**.
2. Klik op een virtuele machine. Standaard op het dashboard voor VM's, worden herstelpunten van de afgelopen 30 dagen weergegeven. U kunt herstelpunten die ouder zijn dan 30 dagen of filter om te zoeken herstelpunten op basis van datums, tijd bereiken en verschillende soorten momentopname consistentie kunt weergeven.
3. Als u de virtuele machine herstellen, klikt u op **VM terugzetten**.

    ![Herstelpunt](./media/backup-azure-arm-restore-vms/restore-point.png)

4. Selecteer een herstelpunt voor het herstel.

## <a name="choose-a-vm-restore-configuration"></a>Kies de configuratie van een virtuele machine terugzetten

1. In **configuratie terugzetten**, selecteer een optie voor herstellen:
    - **Nieuwe maken**. Gebruik deze optie als u wilt maken van een nieuwe virtuele machine. U kunt een virtuele machine maken met eenvoudige instellingen, of een schijf herstellen en een aangepaste virtuele machine maken.
    - **Vervang bestaande**: Gebruik deze optie als u wilt vervangen door schijven op een bestaande virtuele machine.

        ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/restore-configuration.png)

2. Instellingen opgeven voor de optie voor uw geselecteerde terugzetten.

## <a name="create-new-create-a-vm"></a>Maak nieuwe-een virtuele machine maken

Als een van de [opties voor terugzetten](#restore-options), kunt u snel een virtuele machine maken met de basisinstellingen van een herstelpunt.

1. In **configuratie terugzetten** > **nieuw** > **Type herstellen**, selecteer **maken van een virtuele machine** .
2. In **virtuele-machinenaam**, Geef een virtuele machine bestaat in het abonnement.
3. In **resourcegroep**, selecteer een bestaande resourcegroep voor de nieuwe virtuele machine of een nieuwe maken door een wereldwijd unieke naam. Als u een naam die al bestaat, wijst Azure de groep dezelfde naam als de virtuele machine.
4. In **virtueel netwerk**, selecteert u het VNet waarin de virtuele machine wordt geplaatst. Alle vnet's die zijn gekoppeld aan het abonnement worden weergegeven. Selecteer het subnet. Het eerste subnet is standaard geselecteerd.
5. In **opslaglocatie**, het opslagtype voor de virtuele machine opgeven.

    ![Herstellen van de configuratiewizard](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. In **configuratie terugzetten**, selecteer **OK**. In **herstellen**, klikt u op **herstellen** voor het activeren van de herstelbewerking opnieuw.


## <a name="create-new-restore-disks"></a>Maken van nieuwe-herstel-schijven

Als een van de [opties voor terugzetten](#restore-options), kunt u een schijf van een herstelpunt. Klik met de schijf maakt, kunt u een van de volgende doen:

- Gebruik de sjabloon die wordt gegenereerd tijdens de herstelbewerking op de instellingen aanpassen en het activeren van VM-implementatie. U de standaardinstellingen van de sjabloon bewerken en het verzenden van de sjabloon voor VM-implementatie.
- [Herstelde schijven koppelen](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) aan een bestaande virtuele machine.
- [Maak een nieuwe virtuele machine](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#create-a-vm-from-restored-disks) van de herstelde schijven met behulp van PowerShell.

1. In **configuratie terugzetten** > **nieuw** > **Type herstellen**, selecteer **schijven herstellen**.
2. In **resourcegroep**, selecteer een bestaande resourcegroep voor de herstelde schijven of een nieuwe maken door een wereldwijd unieke naam.
3. In **opslagaccount**, geef het account waarnaar u wilt kopiëren van de VHD's. Zorg ervoor dat het account zich in dezelfde regio bevinden als de kluis.

    ![Herstelconfiguratie van is voltooid](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. In **configuratie terugzetten**, selecteer **OK**. In **herstellen**, klikt u op **herstellen** voor het activeren van de herstelbewerking opnieuw.

### <a name="use-templates-to-customize-a-restored-vm"></a>Sjablonen gebruiken voor het aanpassen van een herstelde VM

Nadat de schijf is hersteld, gebruikt u de sjabloon die is gegenereerd als onderdeel van de herstelbewerking opnieuw aanpassen en een nieuwe virtuele machine maken:

1. Open **taakdetails herstellen** voor de betreffende taak.

2. In **taakdetails herstellen**, selecteer **-sjabloon implementeren** sjabloonimplementatie initiëren.

    ![Taak inzoomen herstellen](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

3. De VM-instelling die is opgegeven in de sjabloon aanpassen, klikt u op **template bewerken**. Als u meer aanpassingen toevoegen wilt, klikt u op **parameters bewerken**.
    - [Meer informatie](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) over het implementeren van resources van een aangepaste sjabloon.
    - [Meer informatie](../azure-resource-manager/resource-group-authoring-templates.md) over het ontwerpen van sjablonen.

   ![Sjabloonimplementatie laden](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Voer de aangepaste waarden in voor de virtuele machine, accepteert u de **voorwaarden en bepalingen** en klikt u op **aankoop**.

   ![Sjabloonimplementatie verzenden](./media/backup-azure-arm-restore-vms/submitting-template1.png)


## <a name="replace-existing-disks"></a>Bestaande schijven vervangen

Als een van de [opties voor terugzetten](#restore-options), kunt u een bestaande VM-schijf vervangen door het geselecteerde herstelpunt. [Beoordeling](#restore-options) alle opties voor terugzetten.

1. In **configuratie terugzetten**, klikt u op **vervangen bestaande**.
2. In **Type herstellen**, selecteer **vervangen schijf/s**. Dit is het herstelpunt dat gebruikt vervangen bestaande VM-schijven is.
3. In **Faseringslocatie**, opgeven waarin momentopnamen van de huidige beheerde schijven moeten worden opgeslagen.

   ![De configuratiewizard vervangen bestaande herstellen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="restore-vms-with-special-configurations"></a>Virtuele machines herstellen met speciale configuraties

Er zijn een aantal algemene scenario's waarin u mogelijk om terug te zetten van virtuele machines.

**Scenario** | **Hulp**
--- | ---
**Virtuele machines herstellen met Hybrid Use Benefit** | Als een Windows-VM gebruikt [Hybrid Use Benefit (HUB)-licentieverlening](../virtual-machines/windows/hybrid-use-benefit-licensing.md), de schijven herstellen en een nieuwe virtuele machine met behulp van de opgegeven sjabloon maken (met **licentietype** ingesteld op **Windows_Server**) , of PowerShell.  Deze instelling kan ook worden toegepast na het maken van de virtuele machine.
**Virtuele machines herstellen tijdens een noodgeval voor Azure-datacenter** | Als de kluis wordt gebruikt voor GRS en het primaire datacenter voor de virtuele machine uitvalt, ondersteunt Azure back-up terugzetten een back-up-VM's tot het gekoppelde datacenter. U selecteert een storage-account in het gekoppelde datacenter en herstellen die normaal werken. Azure Backup maakt gebruik van de compute-service in de gekoppelde locatie om de herstelde VM te maken. [Meer informatie](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md) over datacenter tolerantie.
**Herstellen van één domeincontroller-VM in één domein** | Herstel de virtuele machine, zoals andere VM's. Houd rekening met het volgende:<br/><br/> het perspectief van een Active Directory-rom, de Azure-VM is vergelijkbaar met andere VM's.<br/><br/> Directory Services Restore Mode (DSRM) is ook beschikbaar, zodat alle Active Directory herstelscenario's mogelijk zijn. [Meer informatie](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/virtualized-domain-controllers-hyper-v) over overwegingen voor back-up en herstel voor gevirtualiseerde domeincontrollers.
**Meerdere domeincontroller-VM's in één domein herstellen** | f andere domeincontrollers in hetzelfde domein kunnen worden bereikt via het netwerk, de domeincontroller kan worden hersteld, zoals een virtuele machine. Als het de laatste resterende domeincontroller in het domein, of een herstel in een geïsoleerd netwerk wordt uitgevoerd, gebruikt u een [forest recovery](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery).
**Herstellen van meerdere domeinen in één forest** | Het is raadzaam een [forest recovery](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery).
**Bare metal-herstel** | Het belangrijkste verschil tussen Azure VM's en on-premises hypervisors is dat er geen VM-console beschikbaar zijn in Azure. Een console is vereist voor bepaalde scenario's, zoals het herstellen met behulp van een bare metal recovery (BMR)-back-up. VM herstellen uit de kluis is echter een volledige vervanging voor BMR.
**Virtuele machines herstellen met speciale netwerkconfiguraties** | Speciale netwerkconfiguraties zijn VM's met behulp van de interne of externe load balancing, met behulp van meerdere NIC's of meerdere gereserveerde IP-adressen. U deze virtuele machines herstellen met behulp van de [herstellen-schijfoptie](#create-new-restore-disks). Deze optie maakt een kopie van de VHD's in het opgegeven opslagaccount en vervolgens kunt u een virtuele machine met maken een [interne](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/) of [externe](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/) netwerktaakverdeler, [meerdere NIC's](../virtual-machines/windows/multiple-nics.md), of [meerdere gereserveerde IP-adressen](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md), in overeenstemming met uw configuratie.
**Netwerkbeveiligingsgroep (NSG) op de NIC/Subnet** | Azure VM backup biedt ondersteuning voor back-up en herstellen van NSG-informatie op het vnet, subnet en NIC-niveau.

## <a name="track-the-restore-operation"></a>De herstelbewerking bijhouden
Nadat u de herstelbewerking opnieuw activeert, maakt de Backup-service een taak voor het bijhouden. Azure Backup worden meldingen over de taak in de portal weergegeven. Als ze niet weergegeven worden, klikt u op de **meldingen** symbool te zien.

![Terugzetten is geactiveerd](./media/backup-azure-arm-restore-vms/restore-notification1.png)

 Bijhouden als volgt herstellen:

1. Als u bewerkingen voor de taak, klikt u op de hyperlink meldingen. U kunt ook in de kluis, klikt u op **back-uptaken**, en klik vervolgens op de relevante virtuele machine.

    ![Lijst met virtuele machines in een kluis](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

2. Als u wilt terugzetten voortgang controleren, klikt u op een hersteltaak met de status van **In uitvoering**. U ziet de voortgangsbalk die geeft informatie weer over de voortgang van herstel:

    - **Geschatte tijd van terugzetten**: In eerste instantie biedt de benodigde tijd voor het voltooien van de herstelbewerking opnieuw. Als de bewerking wordt uitgevoerd, is de tijd vermindert en nul wanneer de herstelbewerking is voltooid.
    - **Percentage van de herstelbewerking**. Geeft het percentage van de herstelbewerking die is uitgevoerd.
    - **Aantal overgedragen bytes**: Als u terugzetten wilt door een nieuwe virtuele machine maakt, ziet u de bytes die zijn overgedragen op basis van het totale aantal bytes dat moet worden overgedragen.

## <a name="post-restore-steps"></a>Stappen na herstellen

Er zijn een aantal Let na het herstellen van een virtuele machine:

- Extensies aanwezig zijn tijdens de back-upconfiguratie zijn geïnstalleerd, maar niet is ingeschakeld. Als er een probleem, de extensies installeren.
- Als de VM waarvan een back-up is gemaakt, had een statisch IP-adres, is de herstelde virtuele machine heeft een dynamisch IP-adres om te voorkomen van conflicten. U kunt [statisch IP-adres toevoegen aan de herstelde VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
- Een herstelde virtuele machine beschikt niet over de beschikbaarheid instellen. Als de organisatie-eenheid, gebruik de optie voor terugzetten schijf voor u kunt [Geef een beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) wanneer u een virtuele machine maken van de schijf met de opgegeven sjabloon of PowerShell.
- Als u een cloud-init-op basis van Linux-distributie, zoals Ubuntu, uit veiligheidsoverwegingen is het wachtwoord geblokkeerd na het herstel. De VMAccess-extensie op de herstelde virtuele machine te gebruiken [het wachtwoord opnieuw instellen](../virtual-machines/linux/reset-password.md). Het is raadzaam om met behulp van SSH-sleutels van deze distributies, dus u hoeft het wachtwoord opnieuw instellen na het herstel.


## <a name="backing-up-restored-vms"></a>Back-ups van de herstelde VM 's

- Als u een virtuele machine naar de dezelfde resourcegroep met dezelfde naam als de VM oorspronkelijk een back-up teruggezet, blijft de back-up op de VM na het herstel.
- Als u de virtuele machine hersteld naar een andere resourcegroep of een andere naam voor de herstelde virtuele machine is opgegeven, moet u back-up instellen voor de herstelde virtuele machine.


## <a name="next-steps"></a>Volgende stappen

- Als u problemen tijdens het herstelproces ondervindt [bekijken](backup-azure-vms-troubleshoot.md#restore) veelvoorkomende problemen en fouten.
- Nadat de virtuele machine is hersteld, meer informatie over [beheren van virtuele machines](backup-azure-manage-vms.md)
