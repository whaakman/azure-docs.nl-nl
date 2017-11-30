---
title: De architectuur voor de replicatie van Azure VM's tussen Azure-regio's controleren | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en de architectuur die wordt gebruikt voor het virtuele Azure-machines repliceren tussen Azure-regio's met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/10/2017
ms.author: raynew
ms.openlocfilehash: a449969e627c67660973ccf17123958678dd8844
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-to-azure-replication-architecture"></a>Architectuur van de replicatie van Azure naar Azure


In dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u repliceren, failover en herstellen van Azure virtuele machines (VM's) tussen Azure-regio's, met behulp van de [Azure Site Recovery](../site-recovery-overview.md) service.

>[!NOTE]
>Azure VM-replicatie met Site Recovery-service is momenteel in preview.



## <a name="architectural-components"></a>Architectuuronderdelen

De volgende afbeelding toont een globaal overzicht van een virtuele machine van Azure-omgeving in een specifieke regio (in dit voorbeeld is de locatie VS-Oost). In een virtuele machine van Azure-omgeving:
- Apps kunnen worden uitgevoerd op virtuele machines met schijven die zijn verdeeld over de storage-accounts.
- De virtuele machines kunnen worden opgenomen in een of meer subnetten binnen een virtueel netwerk.


**Replicatie van Azure naar Azure**

![klant-omgeving](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Replicatieproces

### <a name="step-1"></a>Stap 1

Wanneer u replicatie voor virtuele machine van Azure inschakelt, worden de onderstaande resources automatisch gemaakt in de doelregio, op basis van regio broninstellingen. U kunt resources doelinstellingen zo nodig kunt aanpassen. 

![Stap 1 replicatieproces inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Resource** | **Details**
--- | ---
**Doelresourcegroep** | De resourcegroep waartoe gerepliceerde virtuele machines na een failover behoren.
**Doel virtueel netwerk** | Het virtuele netwerk waarin gerepliceerde virtuele machines zich bevinden na een failover. Een netwerktoewijzing wordt gemaakt tussen bron- en virtuele netwerken, en vice versa.
**Cache-opslagaccounts** | Voordat VMs wijzigingen in gegevensbron worden gerepliceerd naar een doelopslagaccount, zijn ze bijgehouden en verzonden naar het storage-account van de cache op de doellocatie. Dit zorgt ervoor minimale gevolgen voor de productie-apps die worden uitgevoerd op de virtuele machine.
**Doel storage-accounts**  | Storage-accounts in de doellocatie waarnaar de gegevens worden gerepliceerd.
**Doel-beschikbaarheidssets**  | Beschikbaarheidssets in die de gerepliceerde virtuele machines zich na een failover.

### <a name="step-2"></a>Stap 2

Als replicatie is ingeschakeld, wordt de Site Recovery-extensie Mobility-service automatisch geïnstalleerd op de virtuele machine:

1. De virtuele machine is geregistreerd met Site Recovery.

2. Continue replicatie is geconfigureerd voor de virtuele machine. Gegevens schrijfbewerkingen op de VM-schijven worden continu overgebracht naar het opslagaccount van de cache op de bronlocatie.

   ![Stap 2 replicatieproces inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

  
 Site Recovery moet nooit binnenkomende verbinding hebben met de virtuele machine. Alleen uitgaande verbinding is Site Recovery-service-URL's of het IP-adressen, Office 365-verificatie URL's of het IP-adressen en IP-adressen van cache storage-account nodig.

### <a name="step-3"></a>Stap 3

Nadat u continue replicatie wordt uitgevoerd, worden schrijfbewerkingen onmiddellijk overgebracht naar het opslagaccount van de cache. Site Recovery de gegevens worden verwerkt en verzonden naar de doel-opslagaccount. Nadat de gegevens zijn verwerkt, worden herstelpunten gegenereerd in de doel-storage-account om de paar minuten.

## <a name="failover-process"></a>Failover-proces

Wanneer u een failover initieert, worden de virtuele machines maken in de doelresourcegroep, virtuele doelnetwerk, Doelsubnet en in de doel-beschikbaarheid instellen. U kunt een willekeurig herstelpunt gebruiken tijdens een failover.

![Failover-proces](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

Ga als volgt de [Quick Start](azure-to-azure-quickstart.md) replicatie van een Azure-VM met een secundaire regio in te schakelen.

