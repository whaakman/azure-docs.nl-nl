---
title: Schakel replicatie naar Azure voor virtuele VMware-machines met Azure Site Recovery | Microsoft-Docs
description: Dit artikel wordt beschreven hoe u de replicatie van virtuele VMware-machines naar Azure met Azure Site Recovery instelt.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: asgang
ms.openlocfilehash: 48d84f59a80ea14c2b40821d886fc410c7e3f2c3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Schakel replicatie naar Azure voor virtuele VMware-machines


In dit artikel wordt beschreven hoe de replicatie van de lokale virtuele VMware-machines naar Azure in te schakelen.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt:

1.  [Instellen van on-premises gegevensbron omgeving](vmware-azure-set-up-source.md).
2.  [Doelomgeving in Azure instellen](vmware-azure-set-up-target.md).


## <a name="before-you-start"></a>Voordat u begint
Bij het repliceren van virtuele VMware-machines:

* Uw Azure gebruikersaccount moet zijn bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van een nieuwe virtuele machine naar Azure in te schakelen.
* Virtuele VMware-machines worden gedetecteerd om de 15 minuten. Het duurt 15 minuten of langer voordat ze worden weergegeven in de Azure-portal na de detectie. Evenzo detectie kunt 15 minuten of langer duren wanneer u een nieuwe vCenter-server of vSphere host toevoegt.
* Omgevingswijzigingen op de virtuele machine (zoals VMware tools-installatie) kunnen duren voordat de 15 minuten of langer worden bijgewerkt in de portal.
* U kunt de laatste keer dat gedetecteerde controleren op virtuele VMware-machines in de **laatste Contact op** veld voor de vCenter-server/vSphere-host op de **configuratieservers** pagina.
* Als u wilt toevoegen machines voor replicatie zonder te wachten op de geplande detectie, markeer de configuratieserver (Klik niet op deze), en klik op de **vernieuwen** knop.
* Wanneer u replicatie inschakelt als de machine is voorbereid, installeert de processerver automatisch de Mobility-Service op deze.


## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik op **Stap 2: toepassing repliceren** > **Bron**. Wanneer u replicatie voor het eerst inschakelt, klikt u in de kluis op **+Repliceren** om replicatie in te schakelen voor aanvullende machines.
2. In de **bron** pagina > **bron**, selecteer de configuratieserver.
3. In **type Machine**, selecteer **virtuele Machines** of **fysieke Machines**.
4. Selecteer in **vCenter/vSphere-hypervisor** de vCenter-server waarmee de vSphere-host wordt beheerd, of selecteer de host. Deze instelling niet relevant als u fysieke machines repliceert.
5. Selecteer de processerver, de naam van de configuratieserver worden wordt als u geen processervers extra hebt gemaakt. Klik vervolgens op **OK**.

    ![Replicatiebron inschakelen](./media/vmware-azure-enable-replication/enable-replication2.png)

6. In **doel**, selecteer het abonnement en de resourcegroep waar u de failover virtuele machines maken. Kies het implementatiemodel dat u wilt gebruiken in Azure voor de failover virtuele machines.

7. Selecteer het Azure Storage-account dat u gebruiken wilt voor het repliceren van gegevens. 

    > [!NOTE]

    >   * U kunt een premium- of standard-opslagaccount selecteren. Als u een premium-account selecteren, moet u een extra standard storage-account voor lopende replicatielogboeken opgeven. Accounts moeten in dezelfde regio bevinden als de Recovery Services-kluis.
    >   * Als u gebruiken een ander opslagaccount gebruikt wilt, kunt u [maken van een](../storage/common/storage-create-storage-account.md). Klik op om een opslagaccount met Resource Manager **nieuw**. 

8. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden geactiveerd. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren. Als u een netwerk hebt, moet u [maken van een](#set-up-an-azure-network). Klik op om een netwerk met Resource Manager **nieuw**. Selecteer een subnet, indien van toepassing en klik vervolgens op **OK**.

    ![Schakel replicatie doel instelling](./media/vmware-azure-enable-replication/enable-rep3.png)
9. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie Selecteer virtuele machines inschakelen](./media/vmware-azure-enable-replication/enable-replication5.png)
10. In **eigenschappen** > **eigenschappen configureren**, selecteert u het account dat wordt gebruikt door de processerver voor het installeren van de Mobility-Service automatisch op de machine.  
11. Standaard zijn alle schijven worden gerepliceerd. Als u wilt schijven uitsluiten van replicatie, klikt u op **alle schijven** en wis alle schijven die u niet wilt repliceren.  Klik vervolgens op **OK**. Later kunt u eventueel extra eigenschappen instellen. [Meer informatie](vmware-azure-exclude-disk.md) over het uitsluiten van schijven.

    ![Schakel replicatie eigenschappen configureren](./media/vmware-azure-enable-replication/enable-replication6.png)

12. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**. U kunt replicatie-beleidsinstellingen in wijzigen **instellingen** > **replicatiebeleid** > (naam van beleid) > **instellingen bewerken**. Wijzigingen die u aan een beleid toepast ook van toepassing op replicerende en nieuwe machines.
13. Schakel **consistentie tussen meerdere VM's** als u wilt verzamelen machines in een replicatiegroep. Geef een naam voor de groep en klik vervolgens op **OK**. 

    > [!NOTE]

    >    * Computers in een replicatiegroep samen repliceren en gedeelde crashconsistent en toepassingsconsistente herstelpunten wanneer ze een failover.
    >    * Verzamelen virtuele machines en fysieke servers zodat ze uw werkbelastingen. Inschakelen van de consistentie tussen meerdere VM's kan invloed hebben op prestaties van de werkbelasting. Alleen gebruiken als machines dezelfde werkbelasting worden uitgevoerd en moet u de consistentie.

    ![Replicatie inschakelen](./media/vmware-azure-enable-replication/enable-replication7.png)
14. Klik op **Replicatie inschakelen**. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.

> [!NOTE]
> Als de machine is voorbereid voor de push-installatie, wordt het onderdeel van de Mobility-Service geïnstalleerd wanneer beveiliging is ingeschakeld. Nadat het onderdeel is geïnstalleerd op de computer, wordt een beveiligingstaak wordt gestart en is mislukt. Na de fout moet u elke computer handmatig opnieuw te starten. Na het opnieuw opstarten wordt de beveiligingstaak opnieuw begint en initiële replicatie plaatsvindt.
>
>

## <a name="view-and-manage-vm-properties"></a>Eigenschappen van virtuele machines weergeven en beheren

Vervolgens moet u controleren of de eigenschappen van de bronmachine. Houd er rekening mee dat de naam van de virtuele machine van Azure om te voldoen aan moet [vereisten van de virtuele machine van Azure](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Klik op **instellingen** > **gerepliceerde items** >, en selecteer vervolgens de machine. De **Essentials** pagina bevat informatie over machine-instellingen en status.
2. In **Eigenschappen** kunt u de replicatie- en failoverinformatie van de virtuele machine weergeven.
3. In **Berekening en netwerk** > **Eigenschappen berekenen** kunt u de naam van de virtuele Azure-machine opgeven, evenals de doelgrootte. De naam om te voldoen aan vereisten voor Azure, indien nodig wijzigen.

    ![Reken- en eigenschappen van het netwerk](./media/vmware-azure-enable-replication/vmproperties.png)

4.  Kunt u een [resourcegroep](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) uit die een machine deel uit van een post-failover maakt. U kunt deze instelling op elk gewenst moment voordat failover wordt uitgevoerd. Na de failover, als u de machine naar een andere resourcegroep in de beveiligingsinstellingen voor het einde van deze machine migreert.
5. Kunt u een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) als de computer moet deel uitmaken van een post-failover. Terwijl u een beschikbaarheidsset selecteert, houd er rekening mee dat:

    * Alleen beschikbaarheidssets die horen bij de opgegeven resourcegroep worden weergegeven.  
    * Computers met verschillende virtuele netwerken niet deel uit van dezelfde beschikbaarheidsset.
    * Alleen virtuele machines met dezelfde grootte kan een deel uitmaken van een beschikbaarheidsset.
5. U kunt ook bekijken en informatie over het doelnetwerk, subnet en IP-adres is toegewezen aan de Azure VM toevoegen.
6. In **schijven**, ziet u het besturingssysteem en gegevensschijven op de virtuele machine worden gerepliceerd.

### <a name="configure-networks-and-ip-addresses"></a>Configureren van netwerken en IP-adressen

- U kunt het doel-IP-adres instellen. Als u een adres niet opgeeft, gebruikt de failover-machine DHCP. Als u een adres dat is niet beschikbaar bij een failover, werkt de failover niet. Als het adres in het testfailovernetwerk beschikbaar is, kan de hetzelfde doel-IP-adres worden gebruikt voor de testfailover.
- Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:
    - Als het aantal netwerkadapters op de bronmachine kleiner dan of gelijk aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine is, heeft het doel hetzelfde aantal adapters als de bron.
    - Als het aantal adapters op de virtuele bronmachine groter is dan voor de doelgrootte is toegestaan, wordt het maximum voor de doelgrootte gebruikt.
    Als een bronmachine twee netwerkadapters heeft en de grootte van de doelmachine vier ondersteunt, heeft de doelmachine twee adapters. Als de bronmachine twee adapters heeft, maar de ondersteunde doelgrootte slechts één ondersteunt, heeft de doelmachine slechts één adapter.
    - Als de virtuele machine meerdere netwerkadapters heeft, ze alle verbinding maken met hetzelfde netwerk. De eerste die wordt weergegeven in de lijst wordt ook het geval is, de *standaard* netwerkadapter in de virtuele machine van Azure.

### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

Klanten van Microsoft Software Assurance kunnen Azure hybride voordeel op te slaan op licentiekosten voor Windows Server-machines die worden gemigreerd naar Azure of Azure gebruiken voor herstel na noodgevallen gebruiken. Als u in aanmerking voor de Azure hybride profiteren, kunt u opgeven dat de virtuele machine toegewezen voordeel een die Azure Site Recovery wordt gemaakt is als er een failover. Om dit te doen:
- Ga naar het gedeelte berekenings- en eigenschappen van de gerepliceerde virtuele machine.
- Beantwoord de vraag waarin u wordt gevraagd of u hebt een Windows Server-licentie waarmee u in aanmerking voor hybride Azure.
- Schakel het selectievakje in om te bevestigen dat u een in aanmerking komende Windows Server-licentie met Software Assurance, kunt u het voordeel van de hybride Azure toepassen op de machine die wordt gemaakt op failover.
- Instellingen voor de gerepliceerde machine opslaan.

Meer informatie over [Azure hybride voordeel](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="common-issues"></a>Algemene problemen

* Elke schijf moet minder dan 1 TB groot zijn.
* De besturingssysteemschijf moet op een standaardschijf zijn en niet een dynamische schijf.
* De familie besturingssysteem moet Windows en de opstartschijf moet minder dan 300 GB voor generatie 2, UEFI-functionaliteit virtuele machines.

## <a name="next-steps"></a>Volgende stappen

Nadat de beveiliging is voltooid en de machine is een beveiligde status bereikt, kunt u proberen een [failover](site-recovery-failover.md) om te controleren of uw toepassing weergegeven in Azure of niet wordt.

Als u uitschakelen, beveiliging wilt, meer te weten hoe [registratie en protection-instellingen wilt opschonen](site-recovery-manage-registration-and-protection.md).
