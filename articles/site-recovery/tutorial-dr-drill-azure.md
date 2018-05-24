---
title: Herstelanalyse uitvoeren voor on-premises machines naar Azure met Azure Site Recovery | Microsoft Docs
description: U leert hier hoe u een herstelanalyse uitvoert van on-premises naar Azure met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/16/2018
ms.author: raynew
ms.openlocfilehash: 724144e8f2f2f76c4ad98b4c5cad84e69dadadbb
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209722"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Noodherstelanalyse uitvoeren in Azure

Deze zelfstudie laat zien hoe u een herstelanalyse kunt uitvoeren voor een on-premises machine naar Azure, met behulp van een failovertest. Tijdens een dergelijke analyse wordt uw replicatiestrategie gevalideerd zonder dat er kans op gegevensverlies is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een geïsoleerd netwerk instellen voor de failovertest
> * Voorbereiden om verbinding te maken met de virtuele Azure-machine na de failover
> * Een failovertest uitvoeren voor één machine

Dit is de vierde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid.

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. [On-premises VMware voorbereiden](tutorial-prepare-on-premises-vmware.md)
3. [Herstel na noodgeval instellen](tutorial-vmware-to-azure.md)

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failovertest uitvoert, controleert u de eigenschappen van de virtuele machine en zorgt u ervoor dat de VM met Hyper-V[hyper-v-azure-support-matrix.md#replicated-vms], [de VM met VMware of de fysieke server](vmware-physical-azure-support-matrix.md#replicated-machines) voldoet aan de Azure-vereisten.

1. Klik in **Beveiligde items** op **Gerepliceerde items** > VM.
2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) en instellingen van de beheerde schijf wijzigen
   
      >[!NOTE]
      Failback naar on-premises Hyper-V-machines vanaf virtuele machines in Azure met beheerde schijven wordt momenteel niet ondersteund. Gebruik de optie met beheerde schijven alleen voor failover als u van plan bent om on-premises VM's te migreren naar Azure, zonder failback te configureren.
   
4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.
5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="run-a-test-failover-for-a-single-vm"></a>Een testfailover uitvoeren voor één VM

Wanneer u een testfailover uitvoert, gebeurt het volgende:

1. Er wordt een controle uitgevoerd om na te gaan of aan alle vereiste voorwaarden voor failover wordt voldaan.
2. De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
3. Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

Voer de failovertest als volgt uit:

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM **+Failover testen**.
2. Selecteer voor deze zelfstudie het **meest recente verwerkte** herstelpunt. Hierdoor wordt de virtuele machine hersteld naar het laatst beschikbare tijdstip. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is.
3. Selecteer in **Failover testen** het Azure-doelnetwerk waarmee de virtuele Azure-machines moeten worden verbonden nadat de failover heeft plaatsgevonden.
4. Klik op **OK** om te beginnen met de failover. Als u de voortgang wilt volgen, klikt u op de virtuele machine en opent u de eigenschappen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** >
   **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Als u VM's van Azure wilt verwijderen die tijdens de failovertest zijn gemaakt, klikt u in de VM op **Failovertest opschonen**. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor VMware Linux-computers, VMware-VM's waarop de DHCP-service niet is ingeschakeld, en VMware-VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een failover en failback uitvoeren voor on-premises VM's met VMware](vmware-azure-tutorial-failover-failback.md).
