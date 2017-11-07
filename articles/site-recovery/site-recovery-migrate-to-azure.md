---
title: On-premises VM's en fysieke servers naar Azure migreren met Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een on-premises VM en fysieke servers naar Azure migreert met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/30/2017
ms.author: raynew
ms.openlocfilehash: 423a1727efb0e1fd54eb0f8d5971ace3f8efc6cb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-to-azure-with-site-recovery"></a>Migreren naar Azure met Site Recovery

Lees dit artikel voor meer informatie over het gebruik van de service [Azure Site Recovery](site-recovery-overview.md) voor het migreren van virtuele on-premises machines (VM's) en fysieke servers naar Azure-VM's.

## <a name="before-you-start"></a>Voordat u begint

Bekijk deze video voor een kort overzicht van de vereiste stappen voor een migratie naar Azure.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]


## <a name="what-do-we-mean-by-migration"></a>Wat wordt precies bedoeld met 'migreren’?

U kunt Site Recovery implementeren voor het repliceren en migreren van on-premises VM's en fysieke servers.

- Bij de replicatie configureert u on-premises machines zodanig dat ze regelmatig naar Azure worden gerepliceerd. Mocht er een storing optreden, voert u een failover uit van de on-premises site naar Azure, zodat u er vanaf Azure toegang toe hebt. Als de on-premises site weer toegankelijk is, voert u een failback uit vanaf Azure.
- Als u Site Recovery voor migratie gebruikt, repliceert u on-premises machines naar Azure. Vervolgens voert u er een failover voor uit vanaf uw on-premises site naar Azure en voltooit u het migratieproces. Hierbij is geen failback betrokken.  

## <a name="what-can-site-recovery-migrate"></a>Wat kan er met Site Recovery worden gemigreerd?

U kunt:

- **Migreren vanaf on-premises machines**: hierbij worden on-premises Hyper-V-machines, VMware-VM's en fysieke servers naar Azure gemigreerd. Na de migratie worden workloads die op de on-premises machines werden uitgevoerd, op Azure-VM's uitgevoerd. 
- **Migreren binnen Azure**: hierbij worden Azure-VM's tussen Azure-regio's gemigreerd. 
- **AWS migreren**: hierbij worden AWS Windows-exemplaren naar Azure IaaS-VM's gemigreerd. 

## <a name="migrate-from-on-premises-to-azure"></a>Migreren vanaf on-premises machines naar Azure

Als u on-premises VMware-VM's, Hyper-V-VM's en fysieke servers wilt migreren, volgt u bijna dezelfde stappen als bij volledige replicatie. 


## <a name="migrate-between-azure-regions"></a>Migreren tussen Azure-regio's

Als u Azure-VM's tussen regio's wilt migreren, volgt u bijna dezelfde stappen als bij volledige replicatie.

1. U [schakelt migratie in](azure-to-azure-tutorial-enable-replication.md) voor de machines die u wilt migreren.
2. U [voert een snelle testfailover](azure-to-azure-tutorial-dr-drill.md) uit om te controleren of alles werkt.
3. Daarna [voert u een niet-geplande failover](azure-to-azure-tutorial-failover-failback.md) uit met de optie **Volledige migratie**.
4. Nadat u de migratie hebt voltooid, kunt u [replicatie instellen voor herstel na noodgevallen](site-recovery-azure-to-azure-after-migration.md) vanaf de Azure-regio waarnaar u hebt gemigreerd, naar een secundaire regio.



## <a name="migrate-aws-to-azure"></a>AWS migreren naar Azure

U kunt AWS-exemplaren naar Azure-VM's migreren.
- In dit scenario wordt alleen migratie ondersteund. Met andere woorden, u kunt de Azure-exemplaren repliceren en een failover naar Azure uitvoeren, maar u kunt geen failback uitvoeren.
- Voor migratiedoeleinden worden AWS-exemplaren op dezelfde manier afgehandeld als fysieke servers. U stelt een Recovery Services-kluis in en implementeert een on-premises configuratieserver om de replicatie te beheren. Daarna voegt u deze toe aan de kluis en geeft u replicatie-instellingen op.
- U schakelt replicatie in voor de machines die u wilt migreren en voert een snelle testfailover uit. Daarna voert u een niet-geplande failover uit met de optie **Volledige migratie**.






## <a name="next-steps"></a>Volgende stappen

- [On-premises machines migreren naar Azure](tutorial-migrate-on-premises-to-azure.md)
- [VM's van de ene Azure-regio naar de andere migreren](site-recovery-migrate-azure-to-azure.md)
- [AWS migreren naar Azure](tutorial-migrate-aws-to-azure.md)
