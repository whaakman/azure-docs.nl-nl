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
ms.openlocfilehash: 805582d89ee906e21fbe588e895ce0fe3a926a66
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migreren van Azure IaaS virtuele machines tussen Azure-regio's met Azure Site Recovery

Gebruik dit artikel als u wilt virtuele Azure-machines (VM's) migreren tussen Azure-regio's, met behulp van de [siteherstel](../site-recovery-overview.md) service.

## <a name="prerequisites"></a>Vereisten

U moet IaaS VM's die u wilt migreren.

## <a name="deployment-steps"></a>Implementatiestappen

1. [Maak een kluis](azure-to-azure-tutorial-enable-replication.md#create-a-vault).
2. [Replicatie inschakelen](azure-to-azure-tutorial-enable-replication.md#enable-replication) voor de virtuele machines die u wilt migreren, en kies Azure als de bron. Op dit moment wordt niet systeemeigen replicatie van Azure VM's met beheerde schijven ondersteund.
3. [Een failover uitvoeren](../site-recovery-failover.md). Nadat de initiële replicatie is voltooid, kunt u een failover uitvoeren van een Azure-regio naar een andere. U kunt desgewenst een herstelplan maken en uitvoeren van een failover voor het migreren van meerdere virtuele machines tussen regio's. [Meer informatie](../site-recovery-create-recovery-plans.md) over plannen voor herstel.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over andere scenario's voor replicatie in [wat is Azure Site Recovery?](../site-recovery-overview.md)
