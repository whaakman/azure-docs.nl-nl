---
title: Netwerktoewijzing voor Hyper-V-machines in VMM-clouds repliceren naar Azure met Azure Site Recovery configureren | Microsoft Docs
description: Hierin wordt beschreven hoe netwerktoewijzing configureren wanneer u Hyper-V-machines in VMM-clouds repliceren naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a>Stap 9: Netwerktoewijzing voor Hyper-V-replicatie (met VMM) naar Azure configureren

Na het instellen van de [bron- en replicatie-instellingen](vmm-to-azure-walkthrough-source-target.md), gebruik dit artikel bij netwerktoewijzing om een koppeling tussen on-premises VMM VM-netwerken en Azure-netwerken configureren.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Voordat u begint

- Meer informatie over [netwerktoewijzing](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [VMM voorbereiden op netwerktoewijzing](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Controleer of de virtuele machines op de VMM-server verbonden zijn met een netwerk met virtuele machines en of u ten minste één virtueel Azure-netwerk hebt gemaakt. Aan één Azure-netwerk kunnen meerdere netwerken met virtuele machines worden toegewezen.

## <a name="configure-mapping"></a>Toewijzing configureren

Configureer het toewijzen als volgt:

1. Klik in **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **Netwerktoewijzing** op het pictogram **+Netwerktoewijzing**.

    ![Netwerktoewijzing](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. Selecteer in **Netwerktoewijzing toevoegen** de bron-VMM-server en selecteer **Azure** als doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. Selecteer in **Bronnetwerk** in de lijst die is gekoppeld aan de VMM-server, het on-premises bronnetwerk met virtuele machines dat u wilt toewijzen.
5. Selecteer in **Doelnetwerk** het Azure-netwerk waarin de replica’s (virtuele Azure-machines) moeten worden geplaatst nadat ze zijn gemaakt. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Dit is wat er gebeurt wanneer er netwerktoewijzing wordt uitgevoerd:

* Bestaande virtuele machines in het VM-bronnetwerk zijn verbonden met het netwerk wanneer de toewijzing begint. Nieuwe virtuele machines die zijn verbonden met het bron-VM-netwerk, worden verbonden met het toegewezen Azure-netwerk wanneer replicatie plaatsvindt.
* Als u een bestaande netwerktoewijzing wijzigt, worden gerepliceerde virtuele machines verbonden op basis van de nieuwe instellingen.
* Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet.
* Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 10: een replicatiebeleid maken](vmm-to-azure-walkthrough-replication.md)
