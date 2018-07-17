---
title: Herstelanalyse uitvoeren voor on-premises machines naar Azure met Azure Site Recovery | Microsoft Docs
description: U leert hier hoe u een herstelanalyse uitvoert van on-premises naar Azure met Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: af8062fc0134975542c8a5ec420c790f33996154
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920168"
---
# <a name="run-a-disaster-recovery-drill-to-azure"></a>Noodherstelanalyse uitvoeren in Azure

In dit artikel laten we zien hoe u herstelanalyse kunt uitvoeren voor on-premises machines naar Azure, met behulp van een failovertest. Tijdens een dergelijke analyse wordt uw replicatiestrategie gevalideerd zonder dat er kans op gegevensverlies is.

Dit is de vierde zelfstudie in een reeks waarin u ziet hoe u herstel naar Azure na een noodgeval kunt instellen voor on-premises VMware-VM’s of Hyper-V-VM's.

In deze zelfstudie wordt aangenomen dat u de eerste drie zelfstudies hebt voltooid: 
    - In de [eerste zelfstudie](tutorial-prepare-azure.md) hebben we de Azure-onderdelen ingesteld die nodig zijn voor VMware-noodherstel.
    - In de [tweede zelfstudie](vmware-azure-tutorial-prepare-on-premises.md) hebben we on-premises onderdelen voor noodherstel voorbereid en de vereisten bekeken.
    - In de [derde zelfstudie](vmware-azure-tutorial.md) hebben we replicatie ingesteld en ingeschakeld voor de on-premises VMware-VM.
    - In deze zelfstudies ziet u steeds het **eenvoudigste implementatiepad voor een scenario**. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees de [handleiding](site-recovery-test-failover-to-azure.md) als u meer wilt leren over de stappen van een testfailover.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een geïsoleerd netwerk instellen voor de failovertest
> * Voorbereiden om verbinding te maken met de virtuele Azure-machine na de failover
> * Een failovertest uitvoeren voor één machine

Deze zelfstudie

## <a name="verify-vm-properties"></a>VM-eigenschappen verifiëren

Voordat u een failovertest uitvoert, controleert u de eigenschappen van de virtuele machine en controleert u of de [Hyper-V VM](hyper-v-azure-support-matrix.md#replicated-vms) of [VMware VM](vmware-physical-azure-support-matrix.md#replicated-machines) voldoet aan de Azure-vereisten.

1. Klik in **Beveiligde items** op **Gerepliceerde items** > en de VM.
2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **Berekening en netwerk** kunt u de Azure-naam, resourcegroep, doelgrootte, beschikbaarheidsset, en instellingen van de beheerde schijf wijzigen.
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
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Als u VM's van Azure wilt verwijderen die tijdens de failovertest zijn gemaakt, klikt u in de VM op **Failovertest opschonen**. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor VMware Linux-computers, VMware-VM's waarop de DHCP-service niet is ingeschakeld, en VMware-VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een failover en failback uitvoeren voor on-premises VM's met VMware](vmware-azure-tutorial-failover-failback.md).
> [Een failover en failback uitvoeren voor on-premises VM's met Hyper-V](hyper-v-azure-failover-failback-tutorial.md).
