---
title: Instellen van een kluis voor de virtuele machine van Azure repliction tussen regio's met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de stappen die u nodig hebt voor het instellen van een kluis voor Azure replicatie tussen Azure-regio's met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a>Stap 4: Een kluis voor replicatie van Azure naar Azure instellen

Na [netwerken plannen](azure-to-azure-walkthrough-network.md), gebruik dit artikel bij het instellen van een kluis, voor Azure virtual machines (VM's) repliceren naar een andere Azure-regio, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

- Wanneer u klaar bent met het artikel, moet u een Recovery Services-kluis instellen hebben.
- Eventuele opmerkingen kunt u onderaan dit artikel plaatsen of vragen stellen op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



>[!NOTE]
>
> Replicatie van Azure VM is momenteel in preview.




## <a name="create-a-vault"></a>Een kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> U wordt aangeraden dat u de Recovery Services-kluis in de locatie waar u uw virtuele machines maken worden gerepliceerd. Bijvoorbeeld, als uw doellocatie is de centraal VS, maken de kluis in **VS-midden**.


## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 5: replicatie inschakelen](azure-to-azure-walkthrough-enable-replication.md)
