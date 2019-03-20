---
title: Replicatie configureren voor opslagruimten (S2d) virtuele machines in Azure Site Recovery direct | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van replicatie voor virtuele machines met S2D, van een Azure-regio naar de andere met behulp van Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/29/2019
ms.author: asgang
ms.openlocfilehash: 6c639d4503b170660abed5767e3571c8a2bf24b9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112769"
---
# <a name="replicate-azure-virtual-machines-using-storage-spaces-direct-to-another-azure-region"></a>Azure virtuele machines repliceren met behulp van storage spaces direct naar een andere Azure-regio

Dit artikel wordt beschreven hoe u herstel na noodgevallen van virtuele Azure-machines uitgevoerd opslagruimten direct inschakelt.

>[!NOTE]
>Alleen crash consistente herstelpunten die worden ondersteund voor storage spaces direct-clusters.
>

## <a name="introduction"></a>Inleiding 
[Opslagruimten direct (S2D)](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct) is een met software gedefinieerde opslag, waarmee u een manier om te maken [gastclusters](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure) op Azure.  Een gastcluster in Microsoft Azure is dat een Failover-Cluster bestaat uit de IaaS-VM's. Hierdoor kan gehoste VM-workloads voor failover tussen de gastclusters hogere mate van beschikbaarheid SLA voor toepassingen dan één Azure-VM kan bieden. Dit is handig in scenario's waar virtuele machine die als host fungeert voor een essentiële toepassing, zoals SQL- of Scale out bestandsserver enzovoort.

## <a name="disaster-recovery-of-azure-virtual-machines-using-storage-spaces-direct"></a>Disaster Recovery virtuele Machines van Azure met behulp van opslagruimten direct
In een typisch scenario hebt u mogelijk gastcluster virtuele machines op Azure voor hogere tolerantie van uw toepassing, zoals Scale out bestandsserver. Hoewel dit uw toepassing een hogere beschikbaarheid bieden kan, wilt u deze toepassingen met behulp van Site Recovery voor het niveau regio niet beveiligen. Site Recovery repliceert de gegevens van de ene regio naar een andere Azure-regio en zorgt voor de Dr-regio in geval van failover-cluster.

De onderstaande diagram ziet u de grafisch weergegeven van de twee virtuele machines van Azure failover-cluster met behulp van storage spaces direct.

![storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacedirect.png)

 
- Twee Azure virtuele machines in een Windows-failovercluster en elke virtuele machine hebt twee of meer gegevensschijven.
- S2D synchroniseert de gegevens op de gegevensschijf en geeft de gesynchroniseerde opslag als een opslaggroep.
- De opslaggroep weergegeven als een gedeeld clustervolume (CSV) aan het failovercluster.
- Het failovercluster maakt gebruik van de CSV voor de gegevensstations.

**Overwegingen voor herstel na noodgevallen**

1. Als u instelt [cloud witness](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp) behouden voor het cluster witness in de regio voor herstel na noodgevallen.
2. Als u de virtuele machines met het subnet op de DR-regio die van de bronregio afwijkt failover moet cluster-IP-adres worden gewijzigd na een failover.  Wijzigen van IP-adres van het cluster dat u wilt gebruiken voor automatisch Systeemherstel [herstelplanscript.](https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation)</br>
[Voorbeeldscript](https://github.com/krnese/azure-quickstart-templates/blob/master/asr-automation-recovery/scripts/ASR-Wordpress-ChangeMysqlConfig.ps1) opdracht binnen virtuele machine met behulp van de extensie voor aangepaste scripts uit te voeren 

### <a name="enabling-site-recovery-for-s2d-cluster"></a>U kunt Site Recovery inschakelt voor S2D-cluster:

1. In de recovery services-kluis, klikt u op ' + repliceren '
1. Selecteer alle knooppunten in het cluster en deze deel uitmaakt van een [consistentiegroep Multi-VM](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-common-questions#multi-vm-consistency)
1. Selecteer replicatiebeleid met toepassingsconsistentie uit * (alleen crash-consistentie-ondersteuning beschikbaar is)
1. De replicatie inschakelen

   ![storagespacesdirect beveiliging](./media/azure-to-azure-how-to-enable-replication-s2d-vms/multivmgroup.png)

2. Ga naar gerepliceerde items en ziet u zowel de status van de virtuele machine. 
3. De virtuele machines zijn ophalen beveiligd en worden ook weergegeven als onderdeel van de groep van multi-VM-consistentie.

   ![storagespacesdirect beveiliging](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacesdirectgroup.PNG)

## <a name="creating-a-recovery-plan"></a>Het maken van een herstelplan
Een herstelplan ondersteunt de sequentiëren van de verschillende lagen in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een plan voor herstel voor een webtoepassing met meerdere lagen maakt, voltooid de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>VM's toevoegt aan de failover-groepen

1.  Maak een plan voor herstel door toe te voegen van de virtuele machines.
2.  Klik op 'Aanpassen' aan de groep van de virtuele machines. Standaard worden alle virtuele machines deel uit van 'Groep 1'.


### <a name="add-scripts-to-the-recovery-plan"></a>Voeg scripts toe aan het herstelplan te gaan
Voor uw toepassingen goed te laten functioneren, moet u bepaalde bewerkingen op de virtuele Azure-machines uitvoeren na de failover of tijdens een test-failover. U kunt bepaalde bewerkingen na een failover automatiseren. Bijvoorbeeld, hier we zijn bezig met koppelen van Load Balancer en wijzigen van cluster-IP.


### <a name="failover-of-the-virtual-machines"></a>Failover van de virtuele machines 
Beide knooppunten van de virtuele machines moet worden uitgevoerd met behulp van de [ASR herstelplan](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) 

![storagespacesdirect beveiliging](./media/azure-to-azure-how-to-enable-replication-s2d-vms/recoveryplan.PNG)

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren
1.  Selecteer uw Recovery Services-kluis in de Azure-portal.
2.  Selecteer het herstelplan te gaan die u hebt gemaakt.
3.  Selecteer **Failover testen**.
4.  Selecteer het herstelpunt en de Azure-netwerk voor het starten van de test-failover-proces.
5.  Wanneer de secundaire-omgeving is, validatieprocedures.
6.  Wanneer validaties voltooid zijn, voor het opschonen van de failover-omgeving, selecteer **failovertest**.

Zie voor meer informatie, [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Een failover uitvoeren

1.  Selecteer uw Recovery Services-kluis in de Azure-portal.
2.  Selecteer het herstelplan te gaan die u hebt gemaakt voor SAP-toepassingen.
3.  Selecteer **Failover**.
4.  Selecteer het herstelpunt dat voor het starten van het failoverproces.

Zie voor meer informatie, [Failover in Site Recovery](site-recovery-failover.md).
## <a name="next-steps"></a>Volgende stappen

[Meer informatie](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) over het uitvoeren van failback.
