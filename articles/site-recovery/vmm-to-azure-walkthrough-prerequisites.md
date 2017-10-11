---
title: De vereisten voor Hyper-V-replicatie (met System Center VMM) met Azure Site Recovery | Microsoft Docs
description: Beschrijft de vereisten voor het instellen van replicatie, failover en herstel van on-premises Hyper-V-machines in VMM-clouds naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: 47c178c66ec98fe5d333edd725b64465026e73ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-with-vmm-to-azure-replication"></a>Stap 2: Controleer de vereisten voor Hyper-V-replicatie (met VMM) naar Azure

Lees nadat u de [scenarioarchitectuur](vmm-to-azure-walkthrough-architecture.md) hebt doorgenomen dit artikel om ervoor te zorgen dat u de vereisten voor implementatie begrijpt. 

## <a name="prerequisites-and-limitations"></a>Vereisten en beperkingen

**Vereiste** | **Details**
--- | ---
**Azure-account** | U hebt een [Microsoft Azure-account](http://azure.microsoft.com/) nodig.
**Azure Storage** | U hebt een Azure Storage-account nodig om gerepliceerde gegevens op te slaan.<br/><br/> Het opslagaccount moet zich in dezelfde regio bevinden als de Azure Recovery Services-kluis.<br/><br/>U kunt [geografisch redundante opslag](../storage/common/storage-redundancy.md#geo-redundant-storage) of lokaal redundante opslag gebruiken. Wij raden geografisch redundante opslag aan. Met geografisch redundante opslag zijn gegevens flexibel te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld.<br/><br/> U kunt een standaard Azure-opslagaccount gebruiken of kunt u Azure [Premium-Storage](../storage/common/storage-premium-storage.md) gebruiken. Premium Storage kan I/O-intensieve werkbelastingen hosten en wordt meestal gebruikt voor virtuele machines die consistent hoge I/O-prestaties en lage latentie nodig hebben. Als u Premium Storage gebruikt voor gerepliceerde gegevens, hebt u ook een standaardopslagaccount nodig. In een standaardopslagaccount worden replicatielogboeken opgeslagen die de doorlopende wijzigingen in de on-premises gegevens vastleggen.
**Azure-netwerk** | U hebt een [Azure-netwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md) nodig waarmee de virtuele Azure-machines die na een failover worden gemaakt, verbinding maken. Het Azure-netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
**On-premises VMM-servers** | U hebt een of meer VMM-servers nodig met System Center 2012 R2 of hoger.<br/><br/> Elke VMM-server moet een of meer privéclouds hebben. Elke cloud heeft een of meer hostgroepen nodig.<br/><br/> De VMM-server heeft internettoegang nodig.
**On-premises Hyper-V** | Op Hyper-V-servers moet ten minste Windows Server 2012 R2 met de Hyper-V-rol of Microsoft Hyper-V Server 2012 R2 worden uitgevoerd. De meest recente updates moeten zijn geïnstalleerd.<br/><br/> De Hyper-V-host moet zich in een VMM-hostgroep bevinden (in een VMM-cloud).<br/><br/> Een host moet een of meer virtuele machines hebben die u wilt repliceren.<br/><br/> Hyper-V-hosts moeten verbinding hebben met internet voor replicatie naar Azure, rechtstreeks of met een proxy. Hyper-V-servers moeten beschikken over de correcties die worden beschreven in artikel [2961977](https://support.microsoft.com/kb/2961977).
**On-premises Hyper-V-VM's** | VM's die u wilt repliceren, moeten worden uitgevoerd op een [ondersteund besturingssysteem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) en voldoen aan de [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). De naam van de VM kan worden gewijzigd nadat de replicatie is ingeschakeld. 




## <a name="next-steps"></a>Volgende stappen

[Stap 3: Capaciteit plannen](vmm-to-azure-walkthrough-capacity.md)
