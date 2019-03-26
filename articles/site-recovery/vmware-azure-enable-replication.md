---
title: Schakel replicatie van VMware-VM's voor herstel na noodgevallen naar Azure met Azure Site Recovery | Microsoft-Docs
description: In dit artikel wordt beschreven hoe u virtuele VMware-machines inschakelen voor replicatie naar Azure voor herstel na noodgevallen met behulp van Azure Site Recovery.
author: mayurigupta13
ms.service: site-recovery
ms.date: 3/6/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 472ff7810852bd03ef322cd5eb647c3d61f09b01
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418105"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Schakel replicatie naar Azure voor VMware-VM 's

In dit artikel wordt beschreven hoe u de replicatie van on-premises VMware-machines naar Azure inschakelt.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt:

- [Instellen van uw on-premises bronomgeving](vmware-azure-set-up-source.md).
- [Uw doelomgeving in Azure instellen](vmware-azure-set-up-target.md).

## <a name="before-you-start"></a>Voordat u begint
Als u virtuele VMware-machines repliceert, houd rekening met deze informatie:

* Uw Azure-gebruikersaccount moet bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van een nieuwe virtuele machine naar Azure in te schakelen.
* VMware-VM's worden gedetecteerd om de 15 minuten. Duurt 15 minuten of langer voor virtuele machines in Azure portal worden weergegeven na de detectie. Detectie kan ook 15 minuten duren of langer wanneer u een nieuwe vCenter-server of vSphere-host toevoegt.
* Duurt 15 minuten of langer voordat de wijzigingen op de virtuele machine (zoals VMware tools-installatie) in de portal worden bijgewerkt.
* U kunt de tijd laatst gedetecteerd voor VMware-VM's controleren: Zie de **laatst Contact om** op de **configuratieservers** -pagina voor de vCenter-server/vSphere-host.
* Als u wilt toevoegen van virtuele machines voor replicatie zonder te wachten totdat de geplande detectie, markeert u de configuratieserver (maar klik er niet op), en selecteer **vernieuwen**.
* Wanneer u replicatie inschakelt als de virtuele machine wordt voorbereid, installeert de processerver automatisch de Azure Site Recovery Mobility-service op deze.

## <a name="enable-replication"></a>Replicatie inschakelen

Voordat u de stappen in deze sectie, houd rekening met de volgende informatie:
* Azure Site Recovery repliceert nu rechtstreeks naar beheerde schijven voor alle nieuwe replicaties. De processerver schrijft de replicatielogboeken naar een cache-opslagaccount in de doelregio. Deze logboeken worden gebruikt voor het maken van herstelpunten op beheerde replicaschijven.
* Op het moment van failover, wordt het herstelpunt dat u gebruikt om de doel-beheerde schijf te maken.
* Virtuele machines die eerder zijn geconfigureerd als u wilt repliceren naar doel storage-accounts worden niet beïnvloed.
* Replicatie naar storage-accounts voor een nieuwe virtuele machine is alleen beschikbaar via een Representational State Transfer (REST) API en Powershell. Azure REST API versie 2016-08-10- of 2018-01-10 gebruiken voor het repliceren naar storage-accounts.

1. Ga naar **stap 2: Toepassing repliceren** > **bron**. Nadat u replicatie voor de eerste keer inschakelt, selecteert u **+ repliceren** in de kluis replicatie extra virtuele machines in te schakelen.
1. In de **bron** pagina > **bron**, selecteer de configuratieserver.
1. Voor **type Machine**, selecteer **virtuele Machines** of **fysieke Machines**.
1. Selecteer in **vCenter/vSphere-hypervisor** de vCenter-server waarmee de vSphere-host wordt beheerd, of selecteer de host. Deze instelling is niet relevant als u bij het repliceren van fysieke computers.
1. Selecteer de processerver, die de configuratieserver als u een extra processervers dat nog niet hebt gemaakt. Selecteer vervolgens **OK**.

    ![Replicatie bron venster inschakelen](./media/vmware-azure-enable-replication/enable-replication2.png)

1. Voor **doel**, selecteer het abonnement en resource waar u de failover-virtuele machines maken. Kies het implementatiemodel dat u wilt gebruiken in Azure voor de failover-VM's.

1. Selecteer het Azure-netwerk en subnet die de Azure VM's verbinding met na een failover maken. Het netwerk moet zich in dezelfde regio als de kluis van Site Recovery-service.

   Selecteer **nu configureren voor geselecteerde machines** de netwerk-instellingen toepassen op alle virtuele machines die u hebt geselecteerd voor beveiliging. Selecteer **later configureren** om te selecteren van het Azure-netwerk per virtuele machine. Als u een netwerk hebt, moet u er een maken. Voor het maken van een netwerk met behulp van Azure Resource Manager selecteert **nieuw**. Selecteer een subnet, indien van toepassing, en selecteer vervolgens **OK**.
   
   ![Het doelvenster replicatie inschakelen](./media/vmware-azure-enable-replication/enable-rep3.png)

1. Voor **virtuele machines** > **virtuele machines selecteren**, selecteert u elke virtuele machine die u wilt repliceren. U kunt alleen virtuele machines waarvoor replicatie kan worden ingeschakeld selecteren. Selecteer vervolgens **OK**. Als u niet zien of een bepaalde virtuele machine selecteren, Zie [bronmachine niet wordt weergegeven in de Azure-portal](https://aka.ms/doc-plugin-VM-not-showing) om het probleem te verhelpen.

    ![Virtuele machines selecteren-venster replicatie inschakelen](./media/vmware-azure-enable-replication/enable-replication5.png)

1. Voor **eigenschappen** > **eigenschappen configureren**, selecteer het account dat de processerver wordt gebruikt voor het installeren van de Site Recovery Mobility service automatisch op de virtuele machine. Kies ook het type beheerde schijf om te repliceren naar op basis van uw gegevens verloop patronen.
10. Standaard worden alle schijven van een virtuele bronmachine worden gerepliceerd. Als u wilt schijven uitsluiten van replicatie, schakelt u de **opnemen** selectievakje in voor alle schijven die u niet wilt repliceren. Selecteer vervolgens **OK**. Later kunt u eventueel extra eigenschappen instellen. Meer informatie over [schijven uitsluiten](vmware-azure-exclude-disk.md).

    ![Inschakelen replicatie eigenschappenvenster configureren](./media/vmware-azure-enable-replication/enable-replication6.png)

1. Op **replicatie-instellingen** > **replicatie-instellingen configureren**, controleert u of het juiste replicatiebeleid is geselecteerd. U kunt de instellingen van replicatiebeleid op wijzigen **instellingen** > **replicatiebeleid** > ***beleidsnaam***  >  **Instellingen bewerken**. Wijzigingen die u aan een beleid van toepassing zijn ook van toepassing op replicerende en nieuwe virtual machines.
1. Schakel **Multi-VM-consistentie** als u wilt verzamelen van virtuele machines in een replicatiegroep. Geef een naam voor de groep, en selecteer vervolgens **OK**.

    > [!NOTE]
    >    * Virtuele machines in een replicatiegroep tegelijkertijd gerepliceerd en gedeelde crash-consistente en app-consistente herstelpunten bij failover.
    >    * Verzamelen van virtuele machines en fysieke servers samen zodat ze uw workloads. Inschakelen van multi-VM-consistentie kan van invloed zijn op de prestaties van de werkbelastingen. Doe dit alleen als de virtuele machines zijn dezelfde werkbelasting uitvoeren, en u consistentie.

    ![Venster replicatie inschakelen](./media/vmware-azure-enable-replication/enable-replication7.png)
    
1. Selecteer **Replicatie inschakelen**. U kunt de voortgang van volgen de **beveiliging inschakelen** taak op **instellingen** > **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak wordt uitgevoerd, wordt de virtuele machine is gereed voor failover.

## <a name="view-and-manage-vm-properties"></a>Eigenschappen van virtuele machines weergeven en beheren

Controleer vervolgens of de eigenschappen van de virtuele bronmachine. Houd er rekening mee dat de Azure VM-naam voldoen moet aan de [vereisten voor virtuele Azure-machine](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Ga naar **instellingen** > **gerepliceerde items**, en selecteer vervolgens de virtuele machine. De **Essentials** pagina bevat informatie over de instellingen en status van de virtuele machine.
1. In **Eigenschappen** kunt u de replicatie- en failoverinformatie van de virtuele machine weergeven.
1. In **berekening en netwerk** > **Rekeneigenschappen**, kunt u meerdere VM-eigenschappen. 

    ![Venster met eigenschappen van berekening en netwerk](./media/vmware-azure-enable-replication/vmproperties.png)

    * Azure VM-naam: Wijzig de naam om te voldoen aan vereisten voor Azure, indien nodig.
    * Doel-VM-grootte of VM-type: De standaard-VM-grootte wordt gekozen op basis van het VM-grootte van de gegevensbron. U kunt een andere VM-grootte op basis van uw behoeften op elk gewenst moment voordat de failover selecteren. Houd er rekening mee dat de grootte van de VM-schijf is ook gebaseerd op de grootte van de bronschijf en kan alleen worden gewijzigd na een failover. Meer informatie over schijven en IOPS-tarieven op [schaalbaarheids- en prestatiedoelen voor VM-schijven op Windows](../virtual-machines/windows/disk-scalability-targets.md).

    *  Resourcegroep: U kunt een [resourcegroep](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines), uit die een virtuele machine een onderdeel van een post-failover wordt. U kunt deze instelling op elk gewenst moment voordat de failover te wijzigen. Na een failover, als u de virtuele machine naar een andere resourcegroep migreren, de beveiligingsinstellingen voor die virtuele machine worden verbroken.
    * Beschikbaarheidsset: U kunt een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) als uw virtuele machine moet een onderdeel van een post-failover. Wanneer u een beschikbaarheidsset selecteert, houd rekening met de volgende informatie:

        * Alleen beschikbaarheidssets die deel uitmaken van de opgegeven resourcegroep worden weergegeven.  
        * Virtuele machines die zich op verschillende virtuele netwerken mag niet een deel van dezelfde beschikbaarheidsset.
        * Alleen virtuele machines van dezelfde grootte kunnen een deel uitmaken van een beschikbaarheidsset.
1. U kunt ook informatie over het doelnetwerk, subnet en IP-adres dat toegewezen aan de Azure-VM toevoegen.
2. In **schijven**, ziet u het besturingssysteem en gegevensschijven op de virtuele machine die wordt gerepliceerd.

### <a name="configure-networks-and-ip-addresses"></a>Configureren van netwerken en IP-adressen

U kunt het doel-IP-adres instellen. Als u geen adres opgeeft, betekent dit dat de virtuele failover-machine DHCP gebruikt. Als u een adres dat is niet beschikbaar na een failover hebt ingesteld, wordt de failover niet werkt. Als het adres in het testfailovernetwerk beschikbaar is, kunt u hetzelfde doel-IP-adres voor test-failover.

Het aantal netwerkadapters wordt bepaald door de grootte die u voor de doel-VM als volgt opgeeft:

- Als het aantal adapters voor netwerkopname op de virtuele bronmachine kleiner dan of gelijk zijn aan het aantal adapters die zijn toegestaan voor de doel-VM-grootte is, heeft het doel hetzelfde aantal adapters als de bron.
- Als het aantal adapters voor de virtuele bronmachine groter is dan het getal dat toegestaan voor de doel-VM-grootte, wordt het maximum gebruikt. Bijvoorbeeld, als een virtuele bronmachine twee netwerkadapters heeft en de doel-VM-grootte vier ondersteunt, heeft de virtuele doelmachine twee adapters. Als de bron-VM twee adapters heeft, maar de doelgrootte slechts één, heeft de doel-VM slechts één adapter.
- Als de virtuele machine meerdere netwerkadapters heeft, wordt deze alle verbonden met hetzelfde netwerk. De eerste netwerkadapter die wordt weergegeven in de lijst wordt ook, de *standaard* netwerkadapter in de virtuele machine van Azure. 

### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

Microsoft Software Assurance-klanten kunnen Azure Hybrid Benefit gebruiken om op te slaan op de kosten voor softwarelicenties voor Windows Server-computers die zijn gemigreerd naar Azure. Het voordeel is ook van toepassing op Azure-noodherstel. Als u geen in aanmerking komt, kunt u het voordeel toewijzen aan de virtuele machine die Site Recovery maakt als er een failover. Voer hiervoor de volgende stappen uit:
1. Ga naar de **eigenschappen van de Computer en het netwerk** van de gerepliceerde virtuele machine.
2. Wanneer u wordt gevraagd als u een Windows Server-licentie waarmee u in aanmerking voor Azure Hybrid Benefit hebt beantwoord.
3. Bevestig dat u hebt een in aanmerking komende Windows Server-licentie met Software Assurance kunt u het voordeel van toepassing op de virtuele machine die na een failover worden gemaakt.
4. Sla de instellingen voor de gerepliceerde virtuele machine.

Meer informatie over [Azure Hybrid Benefit](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="resolve-common-issues"></a>Oplossen van veelvoorkomende problemen

* Elke schijf moet kleiner zijn dan 4 TB.
* De besturingssysteemschijf moet een standaardschijf, niet een dynamische schijf.
* De besturingssysteemgroep moet Windows en de opstartschijf moet kleiner zijn dan 300 GB voor generatie 2/UEFI-compatibele virtuele machines.

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine een beveiligde status bereikt, probeert u een [failover](site-recovery-failover.md) om te controleren of uw toepassing wordt weergegeven in Azure.

* Meer informatie over het [opschonen van de instellingen voor registratie en bescherming](site-recovery-manage-registration-and-protection.md) replicatie uitschakelen.
* Meer informatie over het [replicatie voor uw virtuele machines te automatiseren met behulp van Powershell](vmware-azure-disaster-recovery-powershell.md).
