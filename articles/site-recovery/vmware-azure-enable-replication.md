---
title: Inschakelen van replicatie van VMware-VM's voor noodherstel van VMware naar Azure met Azure Site Recovery | Microsoft-Docs
description: In dit artikel wordt beschreven hoe u de replicatie van VMware-VM's voor herstel na noodgevallen naar Azure met behulp van Azure Site Recovery inschakelt.
author: mayurigupta13
ms.service: site-recovery
ms.date: 1/29/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 85c5947fa32751bd8381d6a1d59e91ba2026f5f0
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822779"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Schakel replicatie naar Azure voor VMware-VM 's


In dit artikel wordt beschreven hoe u de replicatie van on-premises VMware-machines naar Azure inschakelt.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt:

1.  [Instellen van on-premises bronomgeving](vmware-azure-set-up-source.md).
2.  [Doelomgeving in Azure instellen](vmware-azure-set-up-target.md).


## <a name="before-you-start"></a>Voordat u begint
Bij het repliceren van virtuele VMware-machines:

* Uw Azure-gebruikersaccount moet bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van een nieuwe virtuele machine naar Azure in te schakelen.
* VMware-VM's worden gedetecteerd om de 15 minuten. Het duurt 15 minuten of langer voordat ze worden weergegeven in de Azure-portal na de detectie. Evenzo, detectie kan 15 minuten of langer duren wanneer u een nieuwe vCenter-server of vSphere-host toevoegt.
* Omgevingswijzigingen op de virtuele machine (zoals VMware tools-installatie) duurt 15 minuten of langer in de portal worden bijgewerkt.
* U kunt de laatste detectietijd controleren voor VMware-VM's in de **laatst Contact om** veld voor de vCenter-server/vSphere-host op de **configuratieservers** pagina.
* Als u wilt toevoegen van machines voor replicatie zonder te wachten totdat de geplande detectie, markeert u de configuratieserver (klik er niet op), en klik op de **vernieuwen** knop.
* Wanneer u replicatie inschakelt als de machine wordt voorbereid, installeert de processerver automatisch de Mobility-Service op deze.


## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik op **stap 2: Toepassing repliceren** > **bron**. Wanneer u replicatie voor het eerst inschakelt, klikt u in de kluis op **+Repliceren** om replicatie in te schakelen voor aanvullende machines.
2. In de **bron** pagina > **bron**, selecteer de configuratieserver.
3. In **type Machine**, selecteer **virtuele Machines** of **fysieke Machines**.
4. Selecteer in **vCenter/vSphere-hypervisor** de vCenter-server waarmee de vSphere-host wordt beheerd, of selecteer de host. Deze instelling is niet relevant als u bij het repliceren van fysieke computers.
5. Selecteer de processerver, de naam van de configuratieserver is als u nog een extra processervers gemaakt. Klik vervolgens op **OK**.

    ![Replicatiebron inschakelen](./media/vmware-azure-enable-replication/enable-replication2.png)

6. In **doel**, selecteer het abonnement en de resourcegroep waar u de failover-virtuele machines maken. Kies het implementatiemodel dat u wilt gebruiken in Azure voor de failover-virtuele machines.

7. Selecteer het Azure Storage-account dat u gebruiken wilt voor het repliceren van gegevens. 

    > [!NOTE]

    >   * U kunt een premium- of standard-opslagaccount selecteren. Als u een premium-account selecteert, moet u om op te geven van een extra standard storage-account voor lopende replicatielogboeken. Accounts moeten zich in dezelfde regio als de Recovery Services-kluis.
    >   * Als u gebruiken een ander opslagaccount wilt, kunt u [maakt u er een](../storage/common/storage-create-storage-account.md). Voor het maken van een storage-account met behulp van Resource Manager, klikt u op **nieuw**. 

8. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden geactiveerd. Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren. Als u een netwerk hebt, moet u er een maken. Voor het maken van een netwerk met behulp van Resource Manager, klikt u op **nieuw**. Selecteer een subnet, indien van toepassing, en klik vervolgens op **OK**.

    ![Instelling voor het doel van replicatie inschakelen](./media/vmware-azure-enable-replication/enable-rep3.png)
9. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**. Als u een bepaalde virtuele machine niet kunt weergeven of selecteren, klikt u [hier](https://aka.ms/doc-plugin-VM-not-showing) om het probleem te verhelpen.

    ![Inschakelen van replicatie virtuele machines selecteren](./media/vmware-azure-enable-replication/enable-replication5.png)
10. In **eigenschappen** > **eigenschappen configureren**, selecteert u de account die door de processerver wordt gebruikt voor het installeren van de Mobility-Service automatisch op de machine.  
11. Standaard worden alle schijven worden gerepliceerd. Als u wilt schijven uitsluiten van replicatie, klikt u op **alle schijven** en schakel alle schijven die u niet wilt repliceren.  Klik vervolgens op **OK**. Later kunt u eventueel extra eigenschappen instellen. [Meer informatie](vmware-azure-exclude-disk.md) over het uitsluiten van schijven.

    ![Inschakelen replicatie-eigenschappen configureren](./media/vmware-azure-enable-replication/enable-replication6.png)

12. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**. Kunt u beleidsinstellingen voor replicatie in **instellingen** > **replicatiebeleid** > (beleidsnaam) > **instellingen bewerken**. Wijzigingen die u op een beleid toepast ook van toepassing op machines replicerende en nieuwe.
13. Schakel **Multi-VM-consistentie** als u wilt verzamelen van computers in een replicatiegroep. Geef een naam voor de groep en klik vervolgens op **OK**. 

    > [!NOTE]

    >    * Machines in een replicatiegroep tegelijkertijd gerepliceerd en gedeelde crash-consistente en app-consistente herstelpunten bij failover.
    >    * Verzamelen van virtuele machines en fysieke servers samen zodat ze uw workloads. Inschakelen van multi-VM-consistentie kan invloed hebben op prestaties van de werkbelastingen. Gebruik alleen als machines dezelfde werkbelasting worden uitgevoerd en u consistentie.

    ![Replicatie inschakelen](./media/vmware-azure-enable-replication/enable-replication7.png)
14. Klik op **Replicatie inschakelen**. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.



## <a name="view-and-manage-vm-properties"></a>Eigenschappen van virtuele machines weergeven en beheren

Vervolgens maakt controleren u de eigenschappen van de bronmachine. Houd er rekening mee dat de Azure VM-naam voldoen moet aan de [vereisten voor virtuele Azure-machine](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Klik op **instellingen** > **gerepliceerde items** >, en selecteer vervolgens de machine. De **Essentials** pagina geeft informatie weer over de machine-instellingen en status.
2. In **Eigenschappen** kunt u de replicatie- en failoverinformatie van de virtuele machine weergeven.
3. In **berekening en netwerk** > **Rekeneigenschappen**, kunt u meerdere VM-propoerties wijzigen:
* Virtuele Azure-machine name - de naam om te voldoen aan vereisten voor Azure, indien nodig wijzigen
* Doel-VM-grootte of type - standaard VM-grootte wordt gekozen op basis van de bron-VM-grootte. U kunt een andere VM-grootte op basis van de behoeften van elk gewenst moment voordat de failover selecteren. Houd er rekening mee dat de grootte van de VM-schijf is ook gebaseerd op de grootte van de bronschijf en deze alleen gewijzigd na failover worden kan. Meer informatie over [Standard](../virtual-machines/windows/disks-standard-ssd.md#scalability-and-performance-targets) en [Premium](../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets) schijf grootten en IOPS.

    ![COMPUTE en netwerk-eigenschappen](./media/vmware-azure-enable-replication/vmproperties.png)

*  Resource-Group - kunt u een [resourcegroep](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) uit die een virtuele machine deel uit van een post-failover maakt. U kunt deze instelling op elk gewenst moment voordat de failover. Na een failover, als u de machine naar een andere resourcegroep, de beveiligingsinstellingen voor het einde van deze machine migreert.
* Beschikbaarheidsset - kunt u een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) als uw computer moet deel uitmaken van een post-failover. Terwijl u een beschikbaarheidsset selecteren, houd er rekening mee dat:

    * Alleen beschikbaarheidssets die behoren tot de opgegeven resourcegroep worden weergegeven.  
    * Machines met verschillende virtuele netwerken mag niet een deel van dezelfde beschikbaarheidsset.
    * Alleen virtuele machines van dezelfde grootte kunnen een deel uitmaken van een beschikbaarheidsset.
4. U kunt ook bekijken en informatie over het doelnetwerk, het subnet en het IP-adres toegewezen aan de Azure VM toevoegen.
5. In **schijven**, ziet u het besturingssysteem en gegevensschijven op de virtuele machine worden gerepliceerd.

### <a name="configure-networks-and-ip-addresses"></a>Configureren van netwerken en IP-adressen

- U kunt het doel-IP-adres instellen. Als u geen adres opgeeft, wordt de failover-machine DHCP. Als u een adres dat is niet beschikbaar na een failover hebt ingesteld, wordt de failover niet werkt. Als het adres in het testfailovernetwerk beschikbaar is, kan hetzelfde doel-IP-adres kan worden gebruikt voor test-failover.
- Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:
    - Als het aantal adapters voor netwerkopname op de bronmachine kleiner dan of gelijk zijn aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine is, heeft het doel hetzelfde aantal adapters als de bron.
    - Als het aantal adapters op de virtuele bronmachine groter is dan voor de doelgrootte is toegestaan, wordt het maximum voor de doelgrootte gebruikt.
    Bijvoorbeeld, als een bronmachine twee netwerkadapters heeft en de grootte van de doelmachine vier ondersteunt, heeft de doelmachine twee adapters. Als de bronmachine twee adapters heeft, maar de ondersteunde slechts één doelgrootte, heeft de doelmachine slechts één adapter.
    - Als de virtuele machine meerdere netwerkadapters heeft, wordt deze alle verbonden met hetzelfde netwerk. De eerste architectuur die wordt weergegeven in de lijst wordt ook, de *standaard* netwerkadapter in de virtuele machine van Azure.

### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

Microsoft Software Assurance-klanten kunnen Azure Hybrid Benefit kunnen gebruiken om op te slaan op de licentiekosten voor Windows Server-machines die worden gemigreerd naar Azure, of om Azure te gebruiken voor herstel na noodgevallen. Als u geen in aanmerking voor Azure Hybrid Benefit gebruiken, kunt u opgeven dat de virtuele machine toegewezen van dit voordeel het een dat Azure Site Recovery maakt is als er een failover. Om dit te doen:
- Ga naar de sectie van de eigenschappen van berekening en netwerk van de gerepliceerde virtuele machine.
- De vraag waarin u wordt gevraagd als u een Windows Server-licentie waarmee u in aanmerking voor Azure Hybrid Benefit hebt.
- Schakel het selectievakje in om te bevestigen dat u hebt een in aanmerking komende Windows Server-licentie met Software Assurance, waarmee u kunt Azure Hybrid Benefit van toepassing op de computer die wordt gemaakt bij failover.
- Instellingen opslaan voor de gerepliceerde machine.

Meer informatie over [Azure Hybrid Benefit](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="common-issues"></a>Algemene problemen

* Elke schijf moet minder dan 4 TB groot zijn.
* De besturingssysteemschijf moet een standaardschijf en niet een dynamische schijf.
* De besturingssysteemgroep moet Windows en de opstartschijf moet minder dan 300 GB voor generatie 2/UEFI-compatibele virtuele machines.

## <a name="next-steps"></a>Volgende stappen

Nadat de beveiliging is voltooid en de machine een beveiligde status heeft bereikt, kunt u proberen een [failover](site-recovery-failover.md) om te controleren of uw toepassing weergegeven in Azure of niet wordt.

* Meer informatie over het [opschonen van de instellingen voor registratie en bescherming](site-recovery-manage-registration-and-protection.md) replicatie uitschakelen.
* Meer informatie over het [automatiseren van replicatie voor uw virtuele machines met behulp van Powershell](vmware-azure-disaster-recovery-powershell.md)
