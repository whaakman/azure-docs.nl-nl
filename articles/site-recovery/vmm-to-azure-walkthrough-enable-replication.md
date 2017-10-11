---
title: Schakel replicatie naar Azure voor Hyper-V-machines in VMM-clouds met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe u de replicatie naar Azure voor Hyper-V-machines in VMM-clouds met de Azure Site Recovery-service inschakelen
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 96a817e43a830e836f2faa4603fc88ed9c0b1828
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-enable-replication-to-azure-for-hyper-v-vms-in-vmm-clouds"></a>Stap 11: Schakel replicatie naar Azure voor Hyper-V-machines in VMM-clouds

Nadat u een beleid voor wachtwoordreplicatie hebt ingesteld, gebruik dit artikel bij replicatie inschakelen voor on-premises Hyper-V virtuele machines (VM's) in System Center Virtual Machine Manager (VMM)-clouds worden beheerd), naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) de service in de Azure portal.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat uw Azure-account de juiste [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)Azure virtuele machines maken. [Meer informatie](../active-directory/role-based-access-built-in-roles.md) over op rollen gebaseerd toegangsbeheer van Azure.

## <a name="exclude-disks-from-replication"></a>Schijven uitsluiten van replicatie

Standaard worden alle schijven op een machine gerepliceerd. U kunt schijven uitsluiten van replicatie. Bijvoorbeeld u mogelijk niet naar wilt repliceren schijven met tijdelijke gegevens of gegevens die telkens wanneer een machine is bijgewerkt of de toepassing opnieuw wordt gestart (bijvoorbeeld pagefile.sys of SQL Server tempdb). [Meer informatie](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Virtuele machines repliceren

Replicatie voor virtuele machines als volgt inschakelen:  

1. Klik op **Stap 2: toepassing repliceren** > **Bron**. Wanneer u replicatie voor het eerst inschakelt, klikt u in de kluis op **+Repliceren** om replicatie in te schakelen voor aanvullende machines.

    ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. Selecteer op de blade **Bron** de VMM-server en de cloud waarin de Hyper-V-hosts zich bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. Selecteer in **Doel** het abonnement, het implementatiemodel voor gebruik na een failover, en het opslagaccount dat u voor gerepliceerde gegevens gebruikt.

    ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Selecteer het opslagaccount dat u wilt gebruiken. Als u een ander opslagaccount wilt gebruiken dan de accounts die u al hebt, kunt u er [een maken](#set-up-an-azure-storage-account). Als u een Premium-opslagaccount gebruikt voor gerepliceerde gegevens, moet u een extra Standard-opslagaccount selecteren om de replicatielogboeken op te slaan waarin de doorlopende wijzigingen in uw on-premises gegevens worden vastgelegd. Als u een opslagaccount wilt maken op basis van het Resource Manager-model, klikt u op **Nieuw**. Als u een opslagaccount wilt maken op basis van het klassieke model, doet u dat [in Azure Portal](../storage/common/storage-create-storage-account.md). Klik vervolgens op **OK**.
5. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren. Als u een ander netwerk wilt gebruiken dan de netwerken die u al hebt, kunt u er [een maken](#set-up-an-azure-network). Als u een opslagaccount wilt maken op basis van het Resource Manager-model, klikt u op **Nieuw**. Als u een netwerk wilt maken op basis van het klassieke model, doet u dat [in Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer indien van toepassing een subnet. Klik vervolgens op **OK**.
6. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het besturingssysteem voor de geselecteerde virtuele machines, evenals de schijf met het besturingssysteem.

    - Controleer of de naam van de Azure-VM (doelnaam) voldoet aan de [vereisten voor virtuele Azure-machines](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Standaard worden alle schijven van de virtuele machine geselecteerd voor replicatie, maar u kunt schijven wissen als u deze wilt uitsluiten.

        - Mogelijk wilt u schijven uitsluiten om de bandbreedte voor de replicatie te beperken. Misschien wilt u bijvoorbeeld schijven met tijdelijke gegevens of gegevens die telkens worden vernieuwd wanneer een machine of app opnieuw wordt gestart (zoals pagefile.sys of Microsoft SQL Server tempdb), van de replicatie uitsluiten. U kunt de schijf van replicatie uitsluiten door de selectie van de schijf op te heffen.
        - Alleen standaardschijven kunnen worden uitgesloten. U kunt geen besturingssysteemschijven uitsluiten.
        - We raden u aan geen dynamische schijven uit te sluiten. Met Site Recovery kan niet worden vastgesteld of een virtuele harde schijf in een gast-VM een basisschijf of een dynamische schijf is. Als niet alle afhankelijke, dynamische volumeschijven worden uitgesloten, wordt de beveiligde dynamische schijf weergegeven als een niet-werkende schijf wanneer een VM-failover optreedt. De gegevens op de schijf zijn dan niet toegankelijk.
        - Nadat de replicatie is ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de virtuele machine uitschakelen en vervolgens weer inschakelen.
        - Voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en u er twee rechtstreeks op de virtuele Azure-machine maakt, wordt slechts voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd van Azure naar Hyper-V. U kunt schijven die handmatig zijn gemaakt niet opnemen in de failback of in de omgekeerde replicatie van Hyper-V naar Azure.
        - Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt Azure Automation ook integreren in een herstelplan om de schijf tijdens failover van de machine te maken.

    Klik op **OK** om de wijzigingen op te slaan. Later kunt u eventueel extra eigenschappen instellen.

    ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. Selecteer in **Replicatie-instellingen** > **Replicatie-instellingen configureren** het replicatiebeleid dat u op de beveiligde virtuele machines wilt toepassen. Klik vervolgens op **OK**. U kunt het replicatiebeleid wijzigen via **Replicatiebeleid** > beleidsnaam > **Instellingen bewerken**. De wijzigingen die u toepast, worden zowel gebruikt voor machines die al worden gerepliceerd, als voor nieuwe machines.

   ![Replicatie inschakelen](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 12: een testfailover uitvoeren](vmm-to-azure-walkthrough-test-failover.md)
