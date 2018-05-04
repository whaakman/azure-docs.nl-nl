---
title: Replicatie van Azure naar Azure-architectuur in Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en de architectuur die wordt gebruikt voor het virtuele Azure-machines repliceren tussen Azure-regio's met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: ffa60e24b93caaaefcab70c99fa2c76065d97233
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-to-azure-replication-architecture"></a>Architectuur van de replicatie van Azure naar Azure


In dit artikel beschrijft de architectuur die wordt gebruikt wanneer u replicatie, failover en Azure virtuele machines (VM's) tussen Azure-regio's herstellen, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.

>[!NOTE]
>Azure VM-replicatie met Site Recovery-service is momenteel in preview.



## <a name="architectural-components"></a>Architectuuronderdelen

De volgende afbeelding toont een globaal overzicht van een virtuele machine van Azure-omgeving in een specifieke regio (in dit voorbeeld is de locatie VS-Oost). In een virtuele machine van Azure-omgeving:
- Apps kunnen worden uitgevoerd op virtuele machines met beheerde schijven of niet-beheerde schijven verdeeld over de storage-accounts.
- De virtuele machines kunnen worden opgenomen in een of meer subnetten binnen een virtueel netwerk.


**Replicatie van Azure naar Azure**

![klant-omgeving](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Replicatieproces

### <a name="step-1"></a>Stap 1

Wanneer u replicatie voor virtuele machine van Azure inschakelt, worden de volgende bronnen automatisch gemaakt in de doelregio, op basis van de instellingen voor de regio. U kunt resources doelinstellingen zo nodig kunt aanpassen.

![Stap 1 replicatieproces inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Resource** | **Details**
--- | ---
**Doelresourcegroep** | De resourcegroep waartoe gerepliceerde virtuele machines na een failover behoren. De locatie van deze resourcegroep kan zich in een Azure-regio met uitzondering van de Azure-regio waarin u de bron virtuele machines worden gehost.
**Doel virtueel netwerk** | Het virtuele netwerk waarin gerepliceerde virtuele machines zich bevinden na een failover. Een netwerktoewijzing wordt gemaakt tussen bron- en virtuele netwerken, en vice versa.
**Cache-opslagaccounts** | Voordat u wijzigingen in gegevensbron VM worden gerepliceerd naar een doelopslagaccount, zijn ze bijgehouden en verzonden naar de storage-account van de cache in de bronlocatie. Deze stap zorgt ervoor dat minimale gevolgen voor de productietoepassingen die worden uitgevoerd op de virtuele machine.
**Storage-accounts als doel (als de bron-VM maakt geen gebruik van schijven die worden beheerd)**  | Storage-accounts in de doellocatie waarnaar de gegevens worden gerepliceerd.
** Replica beheerde schijven (als de bron de VM bevindt zich op schijven die worden beheerd) **  | Schijven in de doellocatie waarmee gegevens worden gerepliceerd die worden beheerd.
**Doel-beschikbaarheidssets**  | Beschikbaarheidssets in die de gerepliceerde virtuele machines zich na een failover.

### <a name="step-2"></a>Stap 2

Als replicatie is ingeschakeld, wordt de Site Recovery-extensie Mobility-service automatisch geïnstalleerd op de virtuele machine:

1. De virtuele machine is geregistreerd met Site Recovery.

2. Continue replicatie is geconfigureerd voor de virtuele machine. Gegevens schrijfbewerkingen op de VM-schijven worden continu overgebracht naar het opslagaccount van de cache op de bronlocatie.

   ![Stap 2 replicatieproces inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery moet nooit binnenkomende verbinding hebben met de virtuele machine. Alleen uitgaande verbinding nodig is voor het volgende.

 - Site Recovery-service-URL's of het IP-adressen
 - Authenticatie van Office 365-URL's of het IP-adressen
 - Cache storage account IP-adressen

Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd.

> [!IMPORTANT]
Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.

### <a name="step-3"></a>Stap 3

Nadat u continue replicatie wordt uitgevoerd, worden schrijfbewerkingen onmiddellijk overgebracht naar het opslagaccount van de cache. Site Recovery de gegevens worden verwerkt en verzonden naar het doel storage-account of replica schijven die worden beheerd. Nadat de gegevens zijn verwerkt, worden herstelpunten gegenereerd in de doel-storage-account om de paar minuten.

## <a name="failover-process"></a>Failover-proces

Wanneer u een failover initieert, worden de virtuele machines maken in de doelresourcegroep, virtuele doelnetwerk, Doelsubnet en in de doel-beschikbaarheid instellen. U kunt een willekeurig herstelpunt gebruiken tijdens een failover.

![Failover-proces](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM met een secundaire regio.
