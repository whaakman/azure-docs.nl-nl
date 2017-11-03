---
title: Migreren van Azure IaaS VM's tussen Azure-regio's | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS virtuele machines migreert vanuit een Azure-regio naar een andere.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: raynew
ms.openlocfilehash: 86806c5dbafc1fd88c434dcee6292683d050cd2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migreren van Azure IaaS virtuele machines tussen Azure-regio's met Azure Site Recovery
## <a name="overview"></a>Overzicht
Welkom bij Azure Site Recovery! Gebruik dit artikel als u wilt migreren van Azure VM's tussen Azure-regio's.
>[!NOTE]
>
> Raadpleeg voor de virtuele Azure-machines repliceren naar een andere regio voor de behoeften migratie en herstel na noodgevallen, [dit document](site-recovery-azure-to-azure.md). Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

Houd rekening met het volgende voordat u begint:

* Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: Azure Resource Manager en het klassieke model. Azure heeft bovendien twee portals: de klassieke Azure Portal, die ondersteuning biedt voor het klassieke implementatiemodel, en Azure Portal, die ondersteuning biedt voor beide implementatiemodellen. De eenvoudige stappen voor migratie zijn hetzelfde of van Site Recovery in de Resource Manager of in de klassieke configureren. De UI-instructies en schermopnamen in dit artikel zijn echter relevant voor de Azure-portal.



U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.

## <a name="prerequisites"></a>Vereisten
Dit is wat u nodig hebt voor deze implementatie:

* **IaaS virtuele machines**: de virtuele machines die u wilt migreren. U migreren deze virtuele machines door ze te behandelen als fysieke machines.

## <a name="deployment-steps"></a>Implementatiestappen
Deze sectie beschrijft de implementatiestappen in de nieuwe Azure portal.

1. [Maak een kluis](site-recovery-azure-to-azure.md#create-a-recovery-services-vault).
2. [Replicatie inschakelen](site-recovery-azure-to-azure.md) voor de virtuele machines die u wilt migreren en Azure als bron kiezen.
  >[!NOTE]
  >
  > Systeemeigen replicatie van Azure VM's met beheerde schijven worden momenteel niet ondersteund. U kunt de optie 'Fysieke naar Azure' in [dit document](site-recovery-vmware-to-azure.md) voor het migreren van virtuele machines met beheerde-schijven.
3. [Een failover uitvoeren](site-recovery-failover.md). Nadat de initiële replicatie is voltooid, kunt u een failover uitvoeren van een Azure-regio naar een andere. U kunt desgewenst een herstelplan maken en uitvoeren van een failover voor het migreren van meerdere virtuele machines tussen regio's. [Meer informatie](site-recovery-create-recovery-plans.md) over plannen voor herstel.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over andere scenario's voor replicatie in [wat is Azure Site Recovery?](site-recovery-overview.md)
