---
title: De doelomgeving voor herstel na noodgevallen van on-premises fysieke servers naar Azure instellen | Microsoft-Docs
description: Dit artikel wordt beschreven hoe u voor het instellen van het doel-Azure-omgeving voor herstel na noodgeval voor fysieke servers met Azure Site Recovery.
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: b89d04a6e2fd11a61de8b56690664f6204c208ad
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209289"
---
# <a name="prepare-target-vmware-to-azure"></a>Doel (VMware naar Azure) voorbereiden

In dit artikel wordt beschreven hoe u uw Azure-omgeving om te beginnen met het repliceren van fysieke servers (x 64) waarop Windows of Linux wordt uitgevoerd in Azure voorbereiden.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis ter bescherming van uw fysieke servers hebt gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](http://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](physical-azure-disaster-recovery.md) voor het repliceren van fysieke servers naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bron voorbereiden**, gaat u naar **stap 3: doel**

![Doel voorbereiden](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonnement:** uit de vervolgkeuzelijst, selecteer het abonnement dat u wilt uw fysieke servers te repliceren.
2. **Implementatiemodel:** Selecteer het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt om ervoor te zorgen dat u hebt ten minste één compatibel opslagaccount en virtueel netwerk in het doelabonnement om te repliceren en failover uw fysieke servers naar een validatie uitgevoerd.

Nadat de validatie is voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibele Resource Manager-opslagaccount of een virtueel netwerk hebt, kunt u maken door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
