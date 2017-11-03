---
title: Hoe werkt Azure virtuele machinereplicatie tussen Azure-regio's in Azure Site Recovery?  | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en de architectuur die wordt gebruikt voor het virtuele Azure-machines repliceren tussen Azure-regio's met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 7a3e1eb129db530295cbf28bbce1dbe48575d50d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Hoe werkt Azure VM-replicatie in Site Recovery?


Dit artikel wordt beschreven voor de onderdelen en -processen betrokken bij repliceren en herstellen van de virtuele Azure-machines (VM's) van de ene regio naar de andere met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

>[!NOTE]
>Azure VM-replicatie met Site Recovery-service is momenteel in preview.

Eventuele opmerkingen kunt u onderaan dit artikel plaatsen of vragen stellen op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architectural-components"></a>Architectuuronderdelen

Het volgende diagram toont een globaal overzicht van een virtuele machine van Azure-omgeving in een specifieke regio (in dit voorbeeld is de locatie VS-Oost). In een virtuele machine van Azure-omgeving:
- Apps kunnen worden uitgevoerd op virtuele machines met schijven die zijn verdeeld over de storage-accounts.
- De virtuele machines kunnen worden opgenomen in een of meer subnetten binnen een virtueel netwerk.

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Meer informatie over de vereisten voor implementatie en de vereisten in de [ondersteuningsmatrix](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Replicatieproces

### <a name="step-1"></a>Stap 1

Wanneer u replicatie voor virtuele Azure-machine in de Azure portal inschakelt, worden de resources die wordt weergegeven in het volgende diagram en de tabel automatisch gemaakt in de doelregio. Resources worden gemaakt op basis van regio broninstellingen standaard. U kunt de doelinstellingen zo nodig kunt aanpassen. [Meer informatie](site-recovery-replicate-azure-to-azure.md).

![Stap 1 replicatieproces inschakelen](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

**Resource** | **Details**
--- | ---
**Doelresourcegroep** | De resourcegroep waartoe gerepliceerde virtuele machines na een failover behoren.
**Doel virtueel netwerk** | Het virtuele netwerk waarin gerepliceerde virtuele machines zich bevinden na een failover. Een netwerktoewijzing wordt gemaakt tussen bron- en virtuele netwerken, en vice versa.
**Cache-opslagaccounts** | Voordat u wijzigingen op de bron-VM's worden gerepliceerd naar de doel-opslagaccount, zijn ze bijgehouden en verzonden naar het storage-account van de cache op de doellocatie. Dit zorgt ervoor minimale gevolgen voor de productie-apps die worden uitgevoerd op de virtuele machine.
**Doel storage-accounts**  | Storage-accounts in de doellocatie waarnaar de gegevens worden gerepliceerd.
**Doel-beschikbaarheidssets**  | Beschikbaarheidssets in die de gerepliceerde virtuele machines zich na een failover.

### <a name="step-2"></a>Stap 2

Als replicatie is ingeschakeld, wordt de Site Recovery-extensie Mobility-service wordt automatisch geïnstalleerd op de virtuele machine. Het volgende gebeurt:

1. De virtuele machine is geregistreerd met Site Recovery.

2. Continue replicatie is geconfigureerd voor de virtuele machine. Gegevens van schrijfbewerkingen op de VM-schijven worden continu overgebracht naar het storage-account van de cache op de bronlocatie.

   ![Stap 2 replicatieproces inschakelen](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Site Recovery moet nooit binnenkomende verbinding hebben met de virtuele machine. De virtuele machine moet alleen uitgaande verbinding met Site Recovery-service-URL's of het IP-adressen, Office 365-verificatie URL's of het IP-adressen en IP-adressen van cache storage-account. Zie voor meer informatie de [netwerken richtlijnen voor het repliceren van virtuele machines in Azure](site-recovery-azure-to-azure-networking-guidance.md) artikel.

## <a name="continuous-replication-process"></a>Continue replicatie in proces

Nadat u continue replicatie werkt, worden schrijfbewerkingen onmiddellijk overgebracht naar het opslagaccount van de cache. Site Recovery de gegevens worden verwerkt en verzonden naar de doel-opslagaccount. Nadat de gegevens zijn verwerkt, worden herstelpunten gegenereerd in de doel-storage-account om de paar minuten.

## <a name="failover-process"></a>Failover-proces

Wanneer u een failover initieert, de virtuele machines worden gemaakt in de doelresourcegroep, doel virtueel netwerk, Doelsubnet, en de beschikbaarheid van het doel is ingesteld. U kunt een willekeurig herstelpunt gebruiken tijdens een failover.

![Failover-proces](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [netwerken](site-recovery-azure-to-azure-networking-guidance.md) voor replicatie van de virtuele machine van Azure.
- Ga als volgt een overzicht te [virtuele Azure-machines repliceren.](site-recovery-azure-to-azure.md)
