---
title: Azure-resources voor de virtuele Hyper-V-machines (met System Center VMM) repliceren naar Azure met Azure Site Recovery voorbereiden | Microsoft Docs
description: Beschrijft wat u moet in de Azure voordat u begint met Hyper-V-machines (met VMM) repliceren naar Azure met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 63b005f37ab5e15e8a1b4645446d65f1529f1bbd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-to-azure"></a>Stap 5: Azure-resources voorbereiden voor Hyper-V-replicatie (met VMM) naar Azure

Nadat u hebt gecontroleerd [vereisten](vmm-to-azure-walkthrough-network.md), volg de instructies in dit artikel voor het voorbereiden van Azure-resources, zodat u kunt lokale Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds repliceren naar Azure met behulp van de [ Azure Site Recovery](site-recovery-overview.md) service.

Na het lezen van dit artikel, eventuele opmerkingen posten onder of technische vragen op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-an-azure-account"></a>Een Azure-account instellen

- Ophalen van een [Microsoft Azure-account](http://azure.microsoft.com/).
- U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- Controleer de ondersteunde regio's voor herstel van sites, onder geografische beschikbaarheid in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).
- Meer informatie over [prijzen voor Site Recovery](site-recovery-faq.md#pricing), en de [prijsinformatie](https://azure.microsoft.com/pricing/details/site-recovery/).
- Zorg ervoor dat uw Azure-account de juiste [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)Azure virtuele machines maken. [Meer informatie](../active-directory/role-based-access-built-in-roles.md) over op rollen gebaseerd toegangsbeheer van Azure.


## <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

- Instellen van een [Azure-netwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md). Virtuele machines in Azure worden geplaatst in dit netwerk wanneer ze zijn gemaakt na een failover.
- Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis
- Site Recovery in de Azure portal kunt instellen netwerken [Resource Manager](../resource-manager-deployment-model.md), of in de klassieke modus.
- U doet er verstandig aan een netwerk in te stellen voordat u begint. Anders moet u dit doen tijdens de implementatie van Site Recovery.
- Meer informatie over [virtueel netwerk prijzen](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen

- Lokale-machines van de site Recovery worden gerepliceerd naar Azure-opslag. Virtuele machines in Azure worden gemaakt van de opslag nadat de failover plaatsvindt.
- Instellen van een standaard/premium [Azure storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor gegevens gerepliceerd naar Azure.
- [Premium-opslag](../storage/common/storage-premium-storage.md) wordt doorgaans gebruikt voor virtuele machines die een consistent hoge i/o-prestaties en lage latentie host i/o-intensieve werkbelastingen nodig.
- Als u een Premium-account wilt gebruiken om gerepliceerde gegevens op te slaan, hebt u ook een Standard-opslagaccount nodig om replicatielogboeken op te slaan waarin de doorlopende wijzigingen in uw on-premises gegevens worden vastgelegd.
- Afhankelijk van het resourcemodel dat u wilt gebruiken voor failover is uitgevoerd op Azure Virtual machines, instellen van een account in [modus Resource Manager](../storage/common/storage-create-storage-account.md), of [klassieke modus](../storage/common/storage-create-storage-account.md).
- Het is raadzaam dat u een opslagaccount instellen voordat u begint. U moet dit doen tijdens de implementatie van Site Recovery. De accounts moeten in dezelfde regio bevinden als de Recovery Services-kluis.
- U kunt de storage-accounts die zijn gebruikt door Site Recovery via resourcegroepen binnen hetzelfde abonnement of via verschillende abonnementen niet verplaatsen.


## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 6: VMM voorbereiden](vmm-to-azure-walkthrough-vmm-hyper-v.md)
