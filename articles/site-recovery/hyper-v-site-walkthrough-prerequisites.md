---
title: Raadpleeg de vereisten voor Hyper-V naar Azure replicatie (zonder de System Center VMM) met Azure Site Recovery | Microsoft Docs
description: Beschrijft de vereisten voor het instellen van replicatie, failovers en herstel van lokale Hyper-V-machines naar Azure met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: cbb5d3598ef91512991d7d1e9f854eb12980752b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-2-review-the-prerequisites-for-hyper-v-without-vmm-to-azure-replication"></a>Stap 2: Controleer de vereisten voor Hyper-V (zonder VMM) naar Azure replicatie

De vereisten worden samengevat in de tabel.


**Vereiste** | **Details** 
--- | --- 
**Azure** | Meer informatie over [Azure-vereisten](site-recovery-prereq.md#azure-requirements).
**On-premises servers** | [Meer informatie](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm) over de vereisten voor de lokale Hyper-V-hosts.
**On-premises Hyper-V-VM's** | VM's die u wilt repliceren, moeten worden uitgevoerd op een [ondersteund besturingssysteem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) en voldoen aan de [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL's** | Hyper-V-hosts moeten toegang hebben tot deze URL's:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Als u op IP-adressen gebaseerde firewallregels gebruikt, controleert u of de regels communicatie met Azure toestaan.<br/></br> Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.<br/></br> Sta de IP-adresbereiken voor de Azure-regio van uw abonnement en voor de regio VS - west toe (deze worden gebruikt voor toegangs- en identiteitsbeheer).



## <a name="next-steps"></a>Volgende stappen

- Als u een volledige implementatie uitvoert, gaat u naar [stap 3: plannen van capaciteit](hyper-v-site-walkthrough-capacity.md)
- Als u een eenvoudige testimplementatie uitvoert, gaat u naar [stap 4: Plan netwerken](hyper-v-site-walkthrough-network.md).
