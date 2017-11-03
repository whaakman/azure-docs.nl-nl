---
title: Doel (fysiek naar Azure) voorbereiden | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw Azure-omgeving om te repliceren van fysieke servers met Windows of Linux naar Azure voorbereidt.
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
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: aa7a32ace8354f615a8b8cc137f6bdf48fbadf48
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a>Doel (VMware naar Azure) voorbereiden
> [!div class="op_single_selector"]
> * [VMware naar Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fysieke naar Azure](./site-recovery-prepare-target-physical-to-azure.md)

Dit artikel wordt beschreven hoe u uw Azure-omgeving om te repliceren van fysieke servers (x 64), Windows of Linux die worden uitgevoerd in Azure voorbereidt.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan:
- U kunt een Recovery Services-kluis ter bescherming van uw fysieke servers hebt gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](http://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](./site-recovery-set-up-physical-to-azure.md) fysieke servers repliceren naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bereid bron**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Abonnement:** in de vervolgkeuzelijst selecteert u het abonnement dat u wilt uw fysieke servers te repliceren.
2. **Implementatiemodel:** selecteren van het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat er ten minste één compatibel storage-account en het virtuele netwerk in het doelabonnement voor replicatie en failover uw fysieke servers naar.

Zodra de validaties wordt voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen compatibel Resource Manager-opslagaccount of virtueel netwerk hebt of wilt meer toevoegen, kunt u doen door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen boven aan de blade.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](./site-recovery-setup-replication-settings-vmware.md).
