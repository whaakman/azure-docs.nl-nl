---
title: Schakel Hyper-V-replicatie naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u replicatie inschakelen voor Hyper-V-machines repliceren naar een secundaire site van System Center VMM met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a>Stap 9: Schakel replicatie naar een secundaire site voor Hyper-V-machines


Nadat het instellen van een beleid voor wachtwoordreplicatie gebruiken in dit artikel replicatie naar een secundaire site van System Center Virtual Machine Manager (VMM) voor on-premises Hyper-V virtuele machines (VM) in te schakelen, gebruik [Azure Site Recovery](site-recovery-overview.md).

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.



## <a name="select-vms-to-replicate"></a>Selecteer de virtuele machines repliceren

1. Klik op **Stap 2: toepassing repliceren** > **Bron**. 

    ![Replicatie inschakelen](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. In **bron**, selecteert u de VMM-server en de cloud waarin u wilt repliceren Hyper-V-hosts zich bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. In **doel**, controleert u de secundaire VMM-server en de cloud.
4. In **virtuele machines**, selecteert u de virtuele machines die u wilt beveiligen in de lijst.

    ![Beveiliging van de virtuele machine inschakelen](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

Opmerking:

- U kunt ook beveiliging voor virtuele machines in de VMM-console inschakelen. Klik op **beveiliging inschakelen** op de werkbalk van de eigenschappen van de virtuele machine > **Azure Site Recovery** tabblad.
- Nadat u replicatie hebt ingeschakeld, kunt u de eigenschappen weergeven voor de virtuele machine in **gerepliceerde Items**. Op de **Essentials** dashboard ziet u informatie over het replicatiebeleid voor de virtuele machine en de status ervan. Klik op **eigenschappen** voor meer informatie.

## <a name="onboard-existing-vms"></a>Ingebouwde bestaande virtuele machines

Als u een bestaande virtuele machines in VMM die worden gerepliceerd met Hyper-V Replica hebt, kunt u vrijgeven ze voor Azure Site Recovery replicatie als volgt:

1. Zorg ervoor dat de Hyper-V-server die als host fungeert voor de bestaande virtuele machine zich bevinden in de primaire cloud en dat de Hyper-V-server die als host fungeert voor de replica virtuele machine bevindt zich in de secundaire cloud.
2. Zorg ervoor dat een beleid voor wachtwoordreplicatie is geconfigureerd voor de primaire VMM-cloud.
3. Replicatie inschakelen voor de primaire virtuele machine. Azure Site Recovery en VMM Zorg ervoor dat de dezelfde replicatiehost en de virtuele machine wordt gedetecteerd en Azure Site Recovery wordt opnieuw gebruikt en herstellen van replicatie met de opgegeven instellingen.


## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 10: een testfailover uitvoeren](vmm-to-vmm-walkthrough-test-failover.md).
