---
title: Doel (VMware naar Azure) voorbereiden | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw Azure-omgeving om te virtuele VMware-machines repliceren naar Azure voorbereidt.
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: ec1322e95431d5fd38d8e05a66322239d3184ac0
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="prepare-target-vmware-to-azure"></a>Doel (VMware naar Azure) voorbereiden
> [!div class="op_single_selector"]
> * [VMware naar Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fysieke naar Azure](./site-recovery-prepare-target-physical-to-azure.md)

Dit artikel wordt beschreven hoe u uw Azure-omgeving om te virtuele VMware-machines repliceren naar Azure voorbereidt.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis ter bescherming van uw virtuele VMware-machines hebt gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](http://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](./site-recovery-set-up-vmware-to-azure.md) virtuele VMware-machines repliceren naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bereid bron**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. **Abonnement:** uit de vervolgkeuzelijst selecteert u het abonnement dat u wilt uw virtuele machines te repliceren.
2. **Implementatiemodel:** selecteren van het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat er ten minste één compatibel storage-account en het virtuele netwerk in het doelabonnement voor replicatie en failover uw virtuele machine.

Zodra de validaties wordt voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibel Resource Manager storage-account of een virtueel netwerk hebt, kunt u op door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](./site-recovery-setup-replication-settings-vmware.md).
