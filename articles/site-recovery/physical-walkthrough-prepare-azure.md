---
title: Azure-resources voor het repliceren van fysieke on-premises servers naar Azure met Azure Site Recovery voorbereiden | Microsoft Docs
description: Beschrijft wat u moet in de Azure voordat u begint met het lokale servers repliceren naar Azure met behulp van de Azure Site Recovery-service
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>Stap 5: Azure-resources voorbereiden voor replicatie van de fysieke server naar Azure


Volg de instructies in dit artikel voor het voorbereiden van Azure-resources, zodat u lokale servers naar Azure worden verkregen met repliceren kunt de [Azure Site Recovery](site-recovery-overview.md) service.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat u hebt gelezen de [vereisten](physical-walkthrough-prerequisites.md).

## <a name="set-up-an-azure-account"></a>Een Azure-account instellen

- Ophalen van een [Microsoft Azure-account](http://azure.microsoft.com/).
- U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- Controleren van de ondersteunde regio's voor herstel van sites onder **geografische beschikbaarheid** in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).
- Meer informatie over [prijzen voor Site Recovery](site-recovery-faq.md#pricing), en de [prijsinformatie](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

- Een Azure-netwerk instellen. Virtuele machines in Azure worden geplaatst in dit netwerk wanneer ze zijn gemaakt na een failover.
- Site Recovery in de Azure portal kunt instellen netwerken [Resource Manager](../resource-manager-deployment-model.md), of in de klassieke modus.
- Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
- Meer informatie over [virtueel netwerk prijzen](https://azure.microsoft.com/pricing/details/virtual-network/).
- Meer informatie over [Azure VM-connectiviteit](physical-walkthrough-network.md) na een failover.


## <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen

- Lokale servers repliceert siteherstel naar Azure storage. Virtuele machines in Azure worden gemaakt van de opslag nadat de failover plaatsvindt.
- Instellen van een [Azure storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor gerepliceerde gegevens.
- Site Recovery in de Azure portal kunt instellen in Resource Manager, of in de klassieke modus storage-accounts gebruiken.
- Het opslagaccount kan worden standaard of [premium](../storage/common/storage-premium-storage.md).
- Als u een premium-account hebt ingesteld, moet u ook een extra standaardaccount voor logboekgegevens.


## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 6: een kluis instellen](physical-walkthrough-create-vault.md)
