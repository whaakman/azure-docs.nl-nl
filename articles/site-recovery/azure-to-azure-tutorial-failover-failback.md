---
title: Failover en mislukken back-Azure VM's die zijn gerepliceerd naar een secundair Azure-regio met Azure Site Recovery (Preview)
description: Meer informatie over het failover en mislukken back replicatie van de virtuele Azure-machines naar een secundair Azure-regio met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 02b709bb8dbab5d10ce9f4cf6155ff26ce229298
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions-preview"></a>Failover en mislukken back Azure VM's tussen Azure-regio's (Preview)

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie wordt beschreven hoe failover van één Azure VM naar een secundair Azure-regio. Nadat u een failover is hebt, kunt u terug naar de primaire regio mislukken wanneer deze beschikbaar is. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Failover van de Azure VM
> * Beveiligt de secundaire virtuele machine van Azure, zodat deze worden gerepliceerd op de primaire regio
> * Failback uit op de secundaire virtuele machine
> * De primaire virtuele machine terug naar de secundaire regio opnieuw beveiligen

## <a name="prerequisites"></a>Vereisten

- Zorg dat u hebt voltooid, een [disaster recovery inzoomen](azure-to-azure-tutorial-dr-drill.md) om te controleren of alles werkt zoals verwacht.
- Controleer de eigenschappen van de virtuele machine voordat u de testfailover uitvoert. De virtuele machine moet voldoen aan [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="run-a-failover-to-the-secondary-region"></a>Een failover uitvoeren naar de secundaire regio

1. In **gerepliceerde items**, selecteert u de virtuele machine die u wilt een failover > **Failover**

   ![Failover](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. In **Failover**, selecteer een **herstelpunt** failover naar. U kunt een van de volgende opties gebruiken:

   * **Meest recente** (standaard): deze optie worden alle gegevens in de Site Recovery-service worden verwerkt en biedt de laagste Recovery Point Objective (RPO).
   * **Meest recente verwerkte**: deze optie wordt de virtuele machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery-service.
   * **Aangepaste**: Gebruik deze optie om de failover naar een bepaald herstelpunt. Deze optie is handig voor het uitvoeren van een testfailover.

3. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt.

4. Volg de voortgang van de failover op de **taken** pagina.

5. Na de failover valideren op de virtuele machine aanmelden bij deze. Als u wilt gaan, een ander herstelpunt voor de virtuele machine, dan kunt u **herstelpunt wijzigen** optie.

6. Wanneer u tevreden met de mislukte bent via de virtuele machine, kunt u **doorvoeren** de failover.
   Vastleggen van verwijdert alle herstelpunten die beschikbaar zijn met de service. De **herstelpunt wijzigen** optie is niet meer beschikbaar.

## <a name="reprotect-the-secondary-vm"></a>De secundaire virtuele machine opnieuw beveiligen

Na een failover van de virtuele machine moet u opnieuw beveiligen zodat terug naar de primaire regio worden gerepliceerd.

1. Zorg ervoor dat de virtuele machine de **Failover doorgevoerd** status en controleer of de primaire regio beschikbaar is, en u kunt maken en toegang tot nieuwe resources in deze.
2. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die failover uitgevoerd is is en selecteer vervolgens **opnieuw beveiligen**.

   ![Met de rechtermuisknop om het opnieuw beveiligen](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. U ziet de richting van beveiliging, ondergeschikt aan een primaire regio is al geselecteerd.
3. Controleer de **resourcegroep, netwerk-, opslag- en beschikbaarheid sets** informatie. Alle resources die zijn gemarkeerd (nieuw) worden gemaakt als onderdeel van de bewerking opnieuw beveiligen.
4. Klik op **OK** voor het activeren van een taak opnieuw beveiligen. Deze taak de seeding van de doelsite met de meest recente gegevens. Vervolgens, worden de delta's naar de primaire regio gerepliceerd. De virtuele machine bevindt zich in een beveiligde status.

## <a name="fail-back-to-the-primary-region"></a>Failback naar de primaire regio

Nadat de virtuele machines opnieuw worden beveiligd, kunt u als nodig is voor failback naar de primaire regio. Volg hiervoor de [failover](#run-a-failover) instructies.
