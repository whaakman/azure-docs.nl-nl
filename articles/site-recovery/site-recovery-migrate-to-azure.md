---
title: Over de migratie in Azure Site Recovery | Microsoft Docs
description: In dit artikel beschrijft het migreren van on-premises en Azure VM's met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: 966d532a33626a8fcc3a3b93790d203aadfd81b4
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="about-migration"></a>Over de migratie

Lees dit artikel voor een snel overzicht van hoe de [Azure Site Recovery](site-recovery-overview.md) service helpt u om machines te migreren. 

Dit is wat u kunt migreren met Site Recovery:

- **Migreren van on-premises naar Azure**: lokale Hyper-V-machines, virtuele VMware-machines en fysieke servers migreren naar Azure. Na de migratie worden workloads die op de on-premises machines werden uitgevoerd, op Azure-VM's uitgevoerd. 
- **Migreren binnen Azure**: hierbij worden Azure-VM's tussen Azure-regio's gemigreerd. 
- **AWS migreren**: hierbij worden AWS Windows-exemplaren naar Azure IaaS-VM's gemigreerd. 


## <a name="what-do-we-mean-by-migration"></a>Wat wordt precies bedoeld met 'migreren’?

Naast het gebruik van Site Recovery voor herstel na noodgevallen van on-premises en Azure VM's, kunt u de Site Recovery-service voor het migreren van deze. Wat is het verschil?

- Voor herstel na noodgevallen, kunt u computers regelmatig repliceren naar Azure. Wanneer er een storing optreedt, moet u failover de machines van de primaire site naar de secundaire site Azure en vanaf daar toegang toe hebben. Wanneer de primaire site opnieuw beschikbaar is, u een failback van Azure.
- Voor de migratie, u lokale machines repliceren naar Azure of virtuele Azure-machines met een secundaire regio. Vervolgens failover de virtuele machine van de primaire site naar de secundaire, en de migratie voltooien. Hierbij is geen failback betrokken.  


## <a name="migration-scenarios"></a>Migratiescenario 's

**Scenario** | **Details**
--- | ---
**Migreren van on-premises naar Azure** | U kunt de lokale virtuele VMware-machines, Hyper-V-machines en fysieke servers naar Azure migreren. U doet dit door voltooien u vrijwel dezelfde stappen, zoals u zou voor volledige noodherstel doen. U mislukt niet gewoon machines van Azure naar de lokale site.
**Migreren tussen Azure-regio's** | U kunt virtuele Azure-machines migreren van een Azure-regio naar een andere. Nadat de migratie voltooid is, kunt u herstel na noodgevallen kunt configureren voor de Azure VM's nu in de secundaire regio die u hebt gemigreerd.
**AWS migreren naar Azure** | U kunt AWS-exemplaren naar Azure-VM's migreren. Site Recovery behandelt AWS-exemplaren als fysieke servers voor voor migratiedoeleinden. 

## <a name="next-steps"></a>Volgende stappen

- [On-premises machines migreren naar Azure](tutorial-migrate-on-premises-to-azure.md)
- [VM's van de ene Azure-regio naar de andere migreren](tutorial-migrate-azure-to-azure.md)
- [AWS migreren naar Azure](tutorial-migrate-aws-to-azure.md)
