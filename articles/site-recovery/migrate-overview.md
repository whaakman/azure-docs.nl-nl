---
title: Over de migratie van on-premises machines en Azure VM's Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises en Azure IaaS VM's migreren naar Azure met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: 9ec502f63df23bab311a45972e31e6df13fa9281
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792967"
---
# <a name="about-migration"></a>Info over migratie

Lees dit artikel voor een snel overzicht van hoe de [Azure Site Recovery](site-recovery-overview.md) service helpt u om machines te migreren. 

Dit is wat u kunt migreren met behulp van Site Recovery:

- **Migreren van on-premises naar Azure**: Migreer on-premises Hyper-V-machines, virtuele VMware-machines en fysieke servers naar Azure. Na de migratie worden workloads die op de on-premises machines werden uitgevoerd, op Azure-VM's uitgevoerd. 
- **Migreren binnen Azure**: Virtuele Azure-machines migreren tussen Azure-regio's. 
- **AWS migreren**: AWS Windows-instanties migreren naar Azure IaaS VM's. 


## <a name="what-do-we-mean-by-migration"></a>Wat wordt precies bedoeld met 'migreren’?

Naast het gebruik van Site Recovery voor herstel na noodgevallen van on-premises en Azure-VM's, kunt u de Site Recovery-service om ze te migreren. Wat is het verschil?

- Voor herstel na noodgevallen, kunt u machines regelmatig naar Azure repliceren. Wanneer er een storing optreedt, moet u failover de machines van de primaire site naar de secundaire site voor Azure en ze daar te openen. Als de primaire site weer beschikbaar is, schakelt u van Azure.
- Voor de migratie repliceren u on-premises machines naar Azure of Azure-VM's naar een secundaire regio. Vervolgens u failover de virtuele machine van de primaire site naar de secundaire server en de migratie voltooien. Hierbij is geen failback betrokken.  


## <a name="migration-scenarios"></a>Migratiescenario 's

**Scenario** | **Details**
--- | ---
**Migreren van on-premises naar Azure** | U kunt on-premises VMware-VM's, Hyper-V-machines en fysieke servers naar Azure migreren. U doet dit door voltooien u bijna dezelfde stappen, zoals u zou voor volledige noodherstel doen. U mislukt niet gewoon machines van Azure naar de on-premises site.
**Migreren tussen Azure-regio's** | U kunt virtuele Azure-machines migreren van een Azure-regio naar een andere. Nadat de migratie voltooid is, kunt u herstel na noodgevallen configureren voor de Azure VM's nu in de secundaire regio waarnaar u hebt gemigreerd.
**AWS migreren naar Azure** | U kunt AWS-exemplaren naar Azure-VM's migreren. Site Recovery behandelt AWS-exemplaren als fysieke servers voor migratiedoeleinden. 

## <a name="next-steps"></a>Volgende stappen

- [On-premises machines migreren naar Azure](migrate-tutorial-on-premises-azure.md)
- [VM's van de ene Azure-regio naar de andere migreren](azure-to-azure-tutorial-migrate.md)
- [AWS migreren naar Azure](migrate-tutorial-aws-azure.md)
