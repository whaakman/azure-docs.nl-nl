---
title: Replicatie van Azure naar Azure-architectuur in Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en architectuur die worden gebruikt bij het instellen van herstel na noodgevallen tussen Azure-regio's voor Azure-VM's met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: bb67051ae969497b9e191c0ceb6c0271375e094e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53787663"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Azure naar Azure disaster recovery-architectuur


In dit artikel beschrijft de architectuur die wordt gebruikt bij het implementeren van herstel na noodgeval met replicatie, failover en herstel van virtuele Azure-machines (VM's) tussen Azure-regio's, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service.




## <a name="architectural-components"></a>Architectuuronderdelen

De volgende afbeelding toont een globaal overzicht van een virtuele machine van Azure-omgeving in een bepaalde regio (in dit voorbeeld is de locatie VS-Oost). In een omgeving met virtuele Azure-machine:
- Apps kunnen worden uitgevoerd op virtuele machines met beheerde schijven of niet-beheerde schijven die zijn verdeeld over de storage-accounts.
- De virtuele machines kunnen worden opgenomen in een of meer subnetten binnen een virtueel netwerk.


**Replicatie van Azure naar Azure**

![klant-omgeving](./media/concepts-azure-to-azure-architecture/source-environment.png)

## <a name="replication-process"></a>Replicatieproces

### <a name="step-1"></a>Stap 1

Wanneer u Azure VM-replicatie inschakelt, worden de volgende bronnen automatisch gemaakt in de doelregio op basis van de regio-instellingen voor gegevensbron. U kunt resources doelinstellingen behoefte kunt aanpassen.

![Inschakelen van replicatieproces, stap 1](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

**Resource** | **Details**
--- | ---
**Doelresourcegroep** | De resourcegroep waartoe gerepliceerde VM's na een failover behoren. De locatie van deze resourcegroep kan zich in andere Azure-regio's met uitzondering van de Azure-regio waarin de virtuele bronmachines worden gehost.
**Virtueel doelnetwerk** | Het virtuele netwerk waarin gerepliceerde VM's zich na een failover. Een netwerktoewijzing wordt gemaakt tussen de bron- en virtuele netwerken, en vice versa.
**Cacheopslagaccounts** | Voordat wijzigingen in gegevensbron VM worden gerepliceerd naar een doelopslagaccount, zijn ze bijgehouden en verzonden naar het cacheopslagaccount op de bronlocatie. Deze stap zorgt ervoor dat minimale gevolgen voor de productie-Apps die worden uitgevoerd op de virtuele machine.
**Doelopslagaccounts (als de bron-VM maakt geen gebruik van beheerde schijven)**  | Storage-accounts op de doellocatie waaraan de gegevens worden gerepliceerd.
** Beheerde replicaschijven (als de bron-VM is op beheerde schijven) **  | Beheerde schijven op de doellocatie waarop gegevens worden gerepliceerd.
**Doelbeschikbaarheidssets**  | Beschikbaarheidssets dat de gerepliceerde VM's zich na een failover.

### <a name="step-2"></a>Stap 2

Als replicatie is ingeschakeld, wordt de extensie voor Site Recovery mobiliteitsservice automatisch geïnstalleerd op de virtuele machine:

1. De virtuele machine is geregistreerd bij Site Recovery.

2. Continue replicatie is geconfigureerd voor de virtuele machine. Schrijven van gegevens op de VM-schijven worden continu overgebracht naar de cache-opslagaccount in de bronlocatie.

   ![Replicatieproces, stap 2 inschakelen](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)


 Site Recovery moet nooit binnenkomende verbindingen aan de virtuele machine. Alleen uitgaande connectiviteit is nodig voor het volgende.

 - Site Recovery-service-URL's / IP-adressen
 - Office 365-verificatie-URL's / IP-adressen
 - IP-adressen van cache storage-account

Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd.

> [!IMPORTANT]
Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.

### <a name="step-3"></a>Stap 3

Nadat de continue replicatie wordt uitgevoerd, worden schrijfbewerkingen onmiddellijk overgebracht naar de cache-opslagaccount. Site Recovery verwerkt de gegevens en verzendt ze naar de doel-opslagaccount of replica beheerde schijven. Nadat de gegevens zijn verwerkt, worden herstelpunten gegenereerd in de doel-opslagaccount om de paar minuten.

## <a name="failover-process"></a>Failover-proces

Wanneer u een failover hebt gestart, worden de virtuele machines gemaakt in de doelresourcegroep, het virtuele doelnetwerk, het doelsubnet en in de doel-beschikbaarheid instellen. U kunt eender welk herstelpunt gebruiken tijdens een failover.

![Failover-proces](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

[Snel repliceren](azure-to-azure-quickstart.md) een Azure-VM naar een secundaire regio.
