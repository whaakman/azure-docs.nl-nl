---
title: Voorbereiden van de doelomgeving voor VMware-replicatie naar Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving voor VMware-VM-replicatie naar Azure voorbereiden.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: a6f983b08415659b9a989ebed824cddd210396e1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233426"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Voorbereiden van de doelomgeving voor herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar Azure

In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving om te beginnen met het repliceren van virtuele VMware-machines of fysieke servers naar Azure voorbereiden.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis hebt gemaakt op [Azure-portal](http://portal.azure.com "Azure-portal") Bescherm uw bronmachines
- U hebt ingesteld van uw on-premises omgeving voor het repliceren van de bron [virtuele VMware-machines](vmware-azure-set-up-source.md) of [fysieke servers](physical-azure-set-up-source.md) naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bron voorbereiden**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** uit de vervolgkeuzelijst, selecteer het abonnement dat u wilt repliceren van uw virtuele machines of fysieke servers naar.
2. **Implementatiemodel:** Selecteer het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat u ten minste één compatibel opslagaccount en virtueel netwerk in het doelabonnement om te repliceren en failover hebt uw virtuele machine of fysieke server.

Nadat de validatie is voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibele Resource Manager-opslagaccount of een virtueel netwerk hebt, kunt u maken door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
