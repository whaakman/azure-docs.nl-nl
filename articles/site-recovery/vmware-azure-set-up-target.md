---
title: Voorbereiden van de doelomgeving voor VMware-replicatie naar Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving voor VMware-VM-replicatie naar Azure voorbereiden.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: 238e7a26be67fcfd2a0b79a87409e5c0d57e0cbf
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338362"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Voorbereiden van de doelomgeving voor herstel na noodgevallen van virtuele VMware-machines of fysieke servers naar Azure

In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving om te beginnen met het repliceren van virtuele VMware-machines of fysieke servers naar Azure voorbereiden.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis hebt gemaakt op [Azure-portal](http://portal.azure.com "Azure-portal") Bescherm uw bronmachines
- U hebt ingesteld van uw on-premises omgeving voor het repliceren van de bron [virtuele VMware-machines](vmware-azure-set-up-source.md) of [fysieke servers](physical-azure-set-up-source.md) naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: Bron voorbereiden**, gaat u naar **stap 3: Doel**

![Doel voorbereiden](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** Selecteer het abonnement dat u wilt repliceren van uw virtuele machines of fysieke servers naar in de vervolgkeuzelijst.
2. **Implementatiemodel:** Selecteer het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat u ten minste één virtueel netwerk in het doelabonnement te repliceren en failover, uw virtuele machine of fysieke server hebt.

Nadat de validatie is voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u een virtueel netwerk hebt, kunt u maken door te klikken op de **+ netwerk** knop aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
