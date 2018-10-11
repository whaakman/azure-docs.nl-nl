---
title: Over de migratie in Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voor het migreren van on-premises en virtuele Azure-machines met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 329f03c30af167b147e5e45c618e6ec4e58efd3f
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49075996"
---
# <a name="about-migration"></a>Info over migratie

Lees dit artikel voor een snel overzicht van hoe de [Azure Site Recovery](site-recovery-overview.md) service helpt u om machines te migreren. 

Dit is wat u kunt migreren met behulp van Site Recovery:

- **Migreren van on-premises naar Azure**: on-premises Hyper-V-machines, virtuele VMware-machines en fysieke servers naar Azure migreren. Na de migratie worden workloads die op de on-premises machines werden uitgevoerd, op Azure-VM's uitgevoerd. 
- **Migreren binnen Azure**: hierbij worden Azure-VM's tussen Azure-regio's gemigreerd. 
- **AWS migreren**: hierbij worden AWS Windows-exemplaren naar Azure IaaS-VM's gemigreerd. 


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
