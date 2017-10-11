---
title: Netwerken voor virtuele machine van Hyper-V-replicatie worden toegewezen aan een secundaire site met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe netwerken toewijzen als Hyper-V-machines repliceren naar een secundaire VMM-site met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a>Stap 7: Netwerken voor virtuele machine van Hyper-V-replicatie worden toegewezen aan een secundaire site


Na het instellen van [bron en doel instellingen](vmm-to-vmm-walkthrough-source-target.md) gebruiken voor het repliceren van Hyper-V virtuele machines (VM's) naar een secundaire site van System Center Virtual Machine Manager (VMM), in dit artikel netwerktoewijzing voor Hyper-V virtuele machine (configureren Replicatie van de virtuele machine) naar een secundaire site, met behulp van [Azure Site Recovery](site-recovery-overview.md).

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.


## <a name="before-you-start"></a>Voordat u begint

- Meer informatie over [netwerktoewijzing](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) voordat u begint.
- Controleren of de virtuele machines op de VMM-servers zijn verbonden met een VM-netwerk.

## <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

1. In **netwerktoewijzing** > **Netwerktoewijzingen**, klikt u op **+ netwerktoewijzing**.
2. In **netwerktoewijzing toevoegen** tabblad, selecteert u de bron en doel van de VMM-servers. De VM-netwerken die zijn gekoppeld aan de VMM-servers worden opgehaald.
3. In **Bronnetwerk**, selecteer het netwerk die u wilt gebruiken in de lijst met VM-netwerken die zijn gekoppeld aan de primaire VMM-server.
4. In **doelnetwerk**, selecteer het netwerk die u wilt gebruiken op de secundaire VMM-server. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Dit is wat er gebeurt wanneer er netwerktoewijzing wordt uitgevoerd:

* Alle bestaande gerepliceerde virtuele machines die met de bron-VM-netwerk overeenkomen wordt worden verbonden met het VM-netwerk van het doel.
* Nieuwe virtuele machines die zijn verbonden met de bron-VM-netwerk, wordt na de replicatie worden verbonden met het doelnetwerk toegewezen.
* Als u een bestaande toewijzing wijzigt met een nieuw netwerk, worden gerepliceerde virtuele machines verbonden met de nieuwe instellingen.
* Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 8: Configureer een beleid voor wachtwoordreplicatie](vmm-to-vmm-walkthrough-replication.md).