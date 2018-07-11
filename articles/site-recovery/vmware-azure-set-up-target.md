---
title: Voorbereiden van de doelomgeving voor VMware-replicatie naar Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving voor VMware-VM-replicatie naar Azure voorbereiden.
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 4b428dff1cebf353cc081696649494e6e4ec9b92
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921107"
---
# <a name="prepare-the-target-environment-for-vmware-replication-to-azure"></a>De doelomgeving voor VMware-replicatie naar Azure voorbereiden

In dit artikel wordt beschreven hoe u uw doel-Azure-omgeving om te beginnen met het repliceren van virtuele VMware-machines naar Azure voorbereiden.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U hebt een Recovery Services-kluis ter bescherming van uw virtuele VMware-machines gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](http://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](vmware-azure-set-up-source.md) virtuele VMware-machines repliceren naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bron voorbereiden**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** uit de vervolgkeuzelijst, selecteer het abonnement die u wilt uw virtuele machines te repliceren.
2. **Implementatiemodel:** Selecteer het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt een validatie uitgevoerd om ervoor te zorgen dat u hebt ten minste één compatibel opslagaccount en virtueel netwerk in het doelabonnement om te repliceren en failover uw virtuele machine.

Nadat de validatie is voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibele Resource Manager-opslagaccount of een virtueel netwerk hebt, kunt u maken op door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
