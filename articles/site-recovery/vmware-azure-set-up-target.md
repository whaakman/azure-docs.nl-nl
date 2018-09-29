---
title: Voorbereiden van de doelomgeving (VMware/fysiek naar Azure) | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving voorbereiden voor VMware VM-replicatie en replicatie van fysieke Servers naar Azure.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/28/2018
ms.author: bsiva
ms.openlocfilehash: 948812f05697362978ad041566d22977efec92a1
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434633"
---
# <a name="prepare-the-target-environment-vmwarephysical-to-azure"></a>Voorbereiden van de doelomgeving (VMware/fysiek naar Azure)

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
