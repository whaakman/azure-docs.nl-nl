---
title: Een kluis voor Hyper-V-replicatie naar een secundaire site maken met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe een kluis maken bij het Hyper-V-machines repliceren naar een secundaire site van System Center VMM met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>Stap 5: Een kluis voor Hyper-V-replicatie naar een secundaire site maken

Na het voorbereiden van de lokale [System Center Virtual Machine Manager (VMM)-servers en Hyper-V-hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) voor Hyper-V-replicatie in een secundaire site met [Azure Site Recovery](site-recovery-overview.md), kunt u een Recovery Services-kluis en selecteer het replicatiescenario voor.

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Kies een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u naar wilt repliceren.

1. Klik op **siteherstel** > **stap 1: infrastructuur voorbereiden** > **beveiligingsdoel**.
2. Selecteer **met site recovery**, en selecteer **Ja, met Hyper-V**.
3. Selecteer **Ja** om aan te geven u VMM voor het beheren van Hyper-V-hosts.
4. Selecteer **Ja** hebt u een secundaire VMM-server. Als u replicatie tussen clouds op één VMM-server implementeert, klikt u op **Nee**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 6: instellen van de replicatiebron en de doelserver](vmm-to-vmm-walkthrough-source-target.md).