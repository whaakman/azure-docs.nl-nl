---
title: Herstelanalyse uitvoeren voor on-premises machines naar Azure met Azure Site Recovery | Microsoft Docs
description: U leert hier hoe u een herstelanalyse uitvoert van on-premises naar Azure met Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b93fb92c9170f3e0fb7bd6ee754dde5df729e299
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60925751"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Noodherstelanalyse uitvoeren in Azure

In dit artikel wordt beschreven hoe u een Dr-herstelanalyse voor een on-premises machine worden uitgevoerd naar Azure met de [Azure Site Recovery](site-recovery-overview.md) service. Tijdens een dergelijke analyse wordt uw replicatiestrategie gevalideerd zonder dat er kans op gegevensverlies is.


Dit is de vierde zelfstudie in een serie die laat hoe zien het instellen van herstel na noodgevallen naar Azure voor on-premises machines u.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een geïsoleerd netwerk instellen voor de failovertest
> * Voorbereiden om verbinding te maken met de virtuele Azure-machine na de failover
> * Een testfailover voor één machine uitvoeren.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Als u wilt meer informatie over de stappen voor inzoomen herstel na noodgevallen in meer detail [raadpleegt u dit artikel](site-recovery-test-failover-to-azure.md).

## <a name="before-you-start"></a>Voordat u begint

Voltooi de vorige zelfstudies:

1. Zorg ervoor dat u hebt [Azure instellen](tutorial-prepare-azure.md) voor on-premises herstel na noodgeval VMware-VM's, Hyper-V-machines en fysieke machines naar Azure.
2. Voorbereiden van uw on-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md) of [Hyper-V](hyper-v-prepare-on-premises-tutorial.md) omgeving voor herstel na noodgevallen. Als het instellen van herstel na noodgevallen voor fysieke servers, raadpleegt u de [ondersteuningsmatrix](vmware-physical-secondary-support-matrix.md).
3. Instellen van herstel na noodgevallen voor [virtuele VMware-machines](vmware-azure-tutorial.md), [Hyper-V-machines](hyper-v-azure-tutorial.md), of [fysieke machines](physical-azure-disaster-recovery.md).
 

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failovertest uitvoert, controleert u de eigenschappen van de virtuele machine en controleert u of de [Hyper-V VM](hyper-v-azure-support-matrix.md#replicated-vms) of [VMware VM](vmware-physical-azure-support-matrix.md#replicated-machines) voldoet aan de Azure-vereisten.

1. Klik in **Beveiligde items** op **Gerepliceerde items** > en de VM.
2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **Berekening en netwerk** kunt u de Azure-naam, resourcegroep, doelgrootte, beschikbaarheidsset, en instellingen van de beheerde schijf wijzigen.
4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.
5. In **Schijven** ziet u informatie over het besturingssysteem en de gegevensschijven van de VM.

## <a name="create-a-network-for-test-failover"></a>Een netwerk maken voor failovertest

Het wordt aangeraden om voor het testen van failover een netwerk te kiezen dat is geïsoleerd van het herstelnetwerk voor de productiesite dat is opgegeven in de instellingen voor **Berekening en netwerk** voor een VM. De standaardinstelling is dat bij het maken van een virtueel Azure-netwerk dit netwerk wordt geïsoleerd van andere netwerken. Het testnetwerk moet een exacte kopie zijn van uw productienetwerk:

- Het testnetwerk moet hetzelfde aantal subnetten hebben als uw productienetwerk. Subnetten moeten dezelfde naam hebben.
- Het testnetwerk moet hetzelfde IP-adresbereik gebruiken.
- Werk het DNS van het testnetwerk bij met het IP-adres dat voor de DNS-VM is opgegeven in de instellingen voor **Berekening en netwerk**. Lees [Testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations) voor meer informatie.

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
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Als u VM's van Azure wilt verwijderen die tijdens de failovertest zijn gemaakt, klikt u in de VM op **Failovertest opschonen**. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor VMware Linux-computers, VMware-VM's waarop de DHCP-service niet is ingeschakeld, en VMware-VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="connect-after-failover"></a>Verbinden na failover

Als u wilt verbinding maken met virtuele Azure-machines na een failover met behulp van RDP/SSH [voorbereiden om verbinding te](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover). Als u eventuele problemen met de netwerkverbinding na een failover ondervindt, volgt u de [probleemoplossing](site-recovery-failover-to-azure-troubleshoot.md) handleiding.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een failover en failback uitvoeren voor virtuele VMware-machines](vmware-azure-tutorial-failover-failback.md).
> [Een failover en failback uitvoeren voor Hyper-V-machines](hyper-v-azure-failover-failback-tutorial.md).
> [Een failover en failback voor fysieke computers uitvoeren](physical-to-azure-failover-failback.md)
