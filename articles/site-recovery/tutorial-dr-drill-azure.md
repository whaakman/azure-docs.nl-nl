---
title: Voer een detailanalyse van het herstel na noodgevallen voor lokale machines naar Azure met Azure Site Recovery | Microsoft Docs
description: Informatie over het uitvoeren van disaster recovery detailanalyse van on-premises naar Azure met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: f7dc5e2df95a64685a8b70d25e839c371d4fc2de
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Noodherstelanalyse uitvoeren in Azure

Deze zelfstudie laat zien hoe u een detailanalyse van het herstel na noodgevallen voor een lokale machine uitvoeren naar Azure met behulp van een testfailover. Een detailanalyse valideert uw replicatiestrategie voor zonder gegevensverlies. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een geïsoleerd netwerk voor de testfailover instellen
> * Verbinding maken met de Azure VM na een failover voorbereiden
> * Een testfailover voor één computer uitvoeren

Dit is de vierde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)
3. [Herstel na noodgeval instellen](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>Controleer de eigenschappen van de virtuele machine

Voordat u een failovertest uitvoert, controleert u de VM-eigenschappen en zorg ervoor dat de virtuele machine voldoet aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. In **beveiligde Items**, klikt u op **gerepliceerde Items** > VM.
2. In de **gerepliceerde item** deelvenster er is een overzicht van VM-informatie, gezondheidsstatus, en de meest recente beschikbare herstelpunten. Klik op **eigenschappen** om meer details te bekijken.
3. In **berekening en netwerk**, kunt u de naam van Azure, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en instellingen voor de schijf wordt beheerd.
   
      >[!NOTE]
      Failback naar lokale Hyper-V-machines van Azure VM's met beheerde schijven wordt momenteel niet ondersteund. Gebruik alleen de optie beheerde schijven voor failover als u van plan bent voor het migreren van lokale virtuele machines naar Azure, zonder dat ze weer mislukt.
   
4. U kunt weergeven en wijzigen van de netwerkinstellingen, met inbegrip van de netwerk-en het subnet waarin de virtuele machine in Azure worden opgeslagen na de failover- en het IP-adres dat wordt toegewezen aan deze.
5. In **schijven**, vindt u informatie over het besturingssysteem en gegevensschijven op de virtuele machine.

## <a name="run-a-test-failover-for-a-single-vm"></a>Een testfailover uitvoeren voor een enkele VM

Wanneer u een failovertest uitvoert, gebeurt het volgende:

1. Controle wordt uitgevoerd om te controleren of alle van de voorwaarden voor failover zijn in plaats van een vereisten.
2. Failover verwerkt de gegevens, zodat een virtuele machine van Azure kunnen worden gemaakt. Als deze optie wordt het meest recente herstelpunt, een herstelpunt van de gegevens gemaakt.
3. Een Azure VM is gemaakt met behulp van de gegevens die worden verwerkt in de vorige stap.

Voer de testfailover als volgt:

1. In **instellingen** > **gerepliceerde Items**, klikt u op de virtuele machine > **+ Testfailover**.
2. Selecteer de **meest recente verwerkte** herstelpunt voor deze zelfstudie. Dit niet lukt via de virtuele machine naar het laatste punt in tijd. Het tijdstempel wordt weergegeven. Met deze optie geen tijd besteed aan het verwerken van gegevens, zodat het biedt een laag RTO (beoogde hersteltijd).
3. In **Testfailover**, selecteer de doel-Azure netwerk welke virtuele Azure-machines moet worden verbonden nadat er failover plaatsvindt.
4. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door te klikken op de virtuele machine om de eigenschappen te openen. U kunt ook op de **Testfailover** taak in de kluisnaam > **instellingen** > **taken** >
   **Site Recovery-taken**.
5. Nadat de failover is voltooid, de replica virtuele machine in Azure wordt weergegeven in de Azure portal > **virtuele Machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Als u wilt verwijderen op Azure VM's die zijn gemaakt tijdens de testfailover, klikt u op **testfailover opschonen** op de virtuele machine. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover.

In sommige scenario's moet failover extra verwerking die het duurt ongeveer 8 tot tien minuten is voltooid. Ziet u mogelijk meer failovertijden voor VMware Linux-machines, virtuele VMware-machines waarvoor geen DHCP-service waarmee en VMware-machines die niet de volgende opstartstuurprogramma's testen: storvsc, vmbus, storflt, intelide, atapi.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voer een failover en failback voor de lokale virtuele VMware-machines](tutorial-vmware-to-azure-failover-failback.md).
