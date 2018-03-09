---
title: Voorbereiden van de doelomgeving voor VMware-replicatie naar Azure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u het doel-Azure-omgeving voor replicatie naar Azure VMware VM voorbereidt.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: bsiva
ms.openlocfilehash: 5f14bbed7ae59737f62fb736591775cb7ba495c5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="prepare-the-target-environment-for-vmware-replication-to-azure"></a>Voorbereiden van de doelomgeving voor VMware-replicatie naar Azure

Dit artikel wordt beschreven hoe u het doel-Azure-omgeving om te virtuele VMware-machines repliceren naar Azure voorbereidt.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis ter bescherming van uw virtuele VMware-machines hebt gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](http://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](vmware-azure-set-up-source.md) virtuele VMware-machines repliceren naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bereid bron**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** uit de vervolgkeuzelijst selecteert u het abonnement dat u wilt uw virtuele machines te repliceren.
2. **Implementatiemodel:** selecteren van het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat er ten minste één compatibel storage-account en het virtuele netwerk in het doelabonnement voor replicatie en failover uw virtuele machine.

Zodra de validaties wordt voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibel Resource Manager storage-account of een virtueel netwerk hebt, kunt u op door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
