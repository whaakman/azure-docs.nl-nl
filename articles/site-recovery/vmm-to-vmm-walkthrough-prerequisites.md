---
title: Raadpleeg de vereisten voor Hyper-V-replicatie naar een secundaire VMM-site met Azure Site Recovery | Microsoft Docs
description: Beschrijft de vereisten voor Hyper-V-machines repliceren naar een secundaire VMM-site met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 7897a30bf1774609ca8e6037dabcd5fbf4151271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-2-review-the-prerequisites-and-limitations-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>Stap 2: Controleer de vereisten en beperkingen voor replicatie van Hyper-V-machine naar een secundaire VMM-site


Nadat u hebt doorgenomen de [scenarioarchitectuur](vmm-to-vmm-walkthrough-architecture.md), Lees dit artikel om ervoor te zorgen dat u begrijpt de vereisten voor implementatie in System Center Virtual Machine repliceren on-premises Hyper-V virtuele machines (VM's) worden beheerd Clouds Manager (VMM), in een secundaire site met [Azure Site Recovery](site-recovery-overview.md) in de Azure portal.

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.


## <a name="prerequisites-and-limitations"></a>Vereisten en beperkingen

**Vereiste** | **Details**
--- | ---
**Azure** | Een [Microsoft Azure](http://azure.microsoft.com/) abonnement.<br/><br/> U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over prijzen voor Site Recovery.<br/><br/> Controleer de ondersteunde regio's voor herstel van sites, onder geografische beschikbaarheid in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).
**VMM-servers** | U hebt twee VMM-servers, één in de primaire site en een van de secundaire wordt aangeraden.<br/><br/> Repliceren tussen clouds op één VMM-server wordt ondersteund.<br/><br/> VMM-servers waarop ten minste System Center 2012 SP1 met de nieuwste updates.<br/><br/> VMM-servers moeten toegang tot internet.
**VMM-clouds** | Elke VMM-server moet op een of meer clouds, en alle clouds moeten het Hyper-V-Capaciteitsprofiel ingesteld hebben. <br/><br/>Clouds moeten een of meer VMM-hostgroepen bevatten.<br/><br/> Als u slechts één VMM-server hebt, moet deze ten minste twee clouds, om te fungeren als primair en secundair.
**Hyper-V** | Hyper-V-servers moeten ten minste draaien op Windows Server 2012 met de Hyper-V-functie en de meest recente updates hebt geïnstalleerd.<br/><br/> Een Hyper-V-server moet een of meer virtuele machines bevatten.<br/><br/>  Hyper-V-hostservers moeten zich in hostgroepen in de primaire en secundaire VMM-clouds.<br/><br/> Als u op Windows Server 2012 R2 Hyper-V in een cluster uitvoert, installeert u [2961977 bijwerken](https://support.microsoft.com/kb/2961977)<br/><br/> Als u op Windows Server 2012 Hyper-V in een cluster uitvoert, wordt niet clusterbroker automatisch als u een statisch IP-adressen gebaseerde cluster hebt gemaakt. De clusterbroker handmatig configureren. [Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V-servers moeten toegang tot internet.




## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 3: plannen netwerken](vmm-to-vmm-walkthrough-network.md).
