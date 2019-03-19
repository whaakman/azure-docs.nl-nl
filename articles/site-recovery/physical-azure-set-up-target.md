---
title: De doelomgeving voor herstel na noodgevallen van on-premises fysieke servers naar Azure instellen | Microsoft-Docs
description: Dit artikel wordt beschreven hoe u voor het instellen van het doel-Azure-omgeving voor herstel na noodgeval voor fysieke servers met Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 41220ccdca945610d7d8ca87af0857114e2cef85
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57866289"
---
# <a name="prepare-target-vmware-to-azure"></a>Doel (VMware naar Azure) voorbereiden

In dit artikel wordt beschreven hoe u uw Azure-omgeving om te beginnen met het repliceren van fysieke servers (x 64) waarop Windows of Linux wordt uitgevoerd in Azure voorbereiden.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat:
- U kunt een Recovery Services-kluis ter bescherming van uw fysieke servers hebt gemaakt. Kunt u een Recovery Services-kluis uit de [Azure-portal](https://portal.azure.com "Azure-portal").
- U hebt [instellen van uw on-premises omgeving](physical-azure-disaster-recovery.md) voor het repliceren van fysieke servers naar Azure.

## <a name="prepare-target"></a>Doel voorbereiden

Na het voltooien van de **stap 1: Selecteer beveiligingsdoel** en **stap 2: bron voorbereiden**, gaat u naar **stap 3: Doel**

![Doel voorbereiden](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonnement:** Selecteer het abonnement dat u wilt repliceren van uw fysieke servers naar in de vervolgkeuzelijst.
2. **Implementatiemodel:** Selecteer het implementatiemodel (klassiek of Resource Manager)

Op basis van het gekozen implementatiemodel, wordt om ervoor te zorgen dat u hebt ten minste één compatibel opslagaccount en virtueel netwerk in het doelabonnement om te repliceren en failover uw fysieke servers naar een validatie uitgevoerd.

Nadat de validatie is voltooid, klikt u op OK om door te gaan met de volgende stap.

Als u geen een compatibele Resource Manager-opslagaccount of een virtueel netwerk hebt, kunt u maken door te klikken op de **+ Opslagaccount** of **+ netwerk** knoppen aan de bovenkant van de pagina.

## <a name="next-steps"></a>Volgende stappen
[Replicatie-instellingen configureren](vmware-azure-set-up-replication.md).
