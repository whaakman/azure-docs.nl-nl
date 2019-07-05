---
title: Herstel na noodgevallen instellen voor SQL Server met SQL Server en Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u herstel na noodgevallen instellen voor SQL Server met behulp van SQL Server en Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: sutalasi
ms.openlocfilehash: 1c44b10b54a5f58dff1aecf36c3633cc8ffbd8f0
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491777"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>Herstel na noodgevallen voor SQL Server instellen

In dit artikel wordt beschreven hoe u de SQL Server-back-end van een toepassing met behulp van een combinatie van bedrijfscontinuïteit van SQL Server en disaster recovery (BCDR)-technologieën, beveiligen en [Azure Site Recovery](site-recovery-overview.md).

Voordat u begint, zorg er dan voor dat u bekend bent met het SQL Server herstelfunctionaliteit bij noodgevallen, waaronder Failoverclustering, Always On-beschikbaarheidsgroepen, databasespiegeling, meld u verzending, actieve geo-replicatie en automatische failover-groepen.

## <a name="dr-recommendation-for-integration-of-sql-server-bcdr-technologies-with-site-recovery"></a>Aanbeveling voor herstel na Noodgevallen voor de integratie van SQL Server BCDR-technologieën met Site Recovery

Keuze van een BCDR-technologie recovery SQL-servers moet worden gebaseerd op de behoeften van uw RTO en RPO volgens de onderstaande tabel. Als u deze keuze is gemaakt, kan Site Recovery worden geïntegreerd met de failoverbewerking van technologie voor het indelen van herstel van de gehele toepassing.

**Implementatietype** | **BCDR-technologie** | **Verwachte RTO voor SQL** | **Verwachte RPO voor SQL** |
--- | --- | --- | ---
SQL Server op Azure IaaS-VM of op on-premises| **[Always On Availability Group](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)** | Komt overeen met de benodigde tijd voor het maken van de secundaire replica als primaire | Replicatie asynchrone naar de secundaire replica is, wordt daarom er gegevens verloren gaan.
SQL Server op Azure IaaS-VM of op on-premises| **[Failover clustering (altijd op FCI)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017)** | Komt overeen met de benodigde tijd voor failover tussen de knooppunten | Het maakt gebruik van gedeelde opslag, dus dezelfde weergave van opslag-exemplaar is beschikbaar op de failover.
SQL Server op Azure IaaS-VM of op on-premises| **[Database mirroring (high performance-modus)](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017)** | Komt overeen met de benodigde tijd voor het afdwingen van de service, die gebruikmaakt van de mirrorserver als een warme stand-by-server. | Replicatie is asynchroon. De opdracht ALTER database mogelijk iets achter de hoofddatabase vertraging. De ruimte is meestal klein, howvever, aanzienlijk als de server-principal of mirror-systeem is onder zware belasting kunnen worden.<br></br>Back-upfunctie kan een aanvulling op het spiegelen van de database en een goed alternatief voor asynchrone Databasespiegeling is
SQL as PaaS on Azure<br></br>(Elastische pools, SQL database-servers) | **Actieve Geo-replicatie** | 30 seconden nadat deze is geactiveerd<br></br>Wanneer failover wordt geactiveerd op een van de secundaire databases, worden alle andere secundaire replica's automatisch gekoppeld aan de nieuwe primaire. | RPO van 5 seconden<br></br>Actieve geo-replicatie maakt gebruik van de AlwaysOn-technologie van SQL Server voor het repliceren van asynchroon doorgevoerde transacties op de primaire database naar een secundaire database met behulp van de snapshot-isolatie. <br></br>De secundaire gegevens kan worden gegarandeerd nooit hebben gedeeltelijke transacties.
SQL als PaaS geconfigureerd met actieve geo-replicatie op Azure<br></br>(SQL Database Managed Instance, elastische pools, SQL database-servers) | **Groepen voor automatische failover** | RTO van 1 uur | RPO van 5 seconden<br></br>Automatische failover-groepen bieden de semantiek voor groep boven op de actieve geo-replicatie, maar het dezelfde mechanisme voor asynchrone replicatie wordt gebruikt.
SQL Server op Azure IaaS-VM of op on-premises| **Replicatie met Azure Site Recovery** | Meestal minder dan 15 minuten. [Lees meer](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) voor meer informatie over de SLA van RTO door Azure Site Recovery. | 1 uur voor toepassingsconsistentie en 5 minuten voor crashconsistentie. 

> [!NOTE]
> Enkele belangrijke overwegingen bij het beveiligen van SQL-workloads met Azure Site Recovery:
> * Azure Site Recovery is neutraal toepassing en daarom kan een willekeurige versie van SQL server die is geïmplementeerd op een ondersteund besturingssysteem kan worden beveiligd door Azure Site Recovery. [Meer informatie](vmware-physical-azure-support-matrix.md#replicated-machines).
> * U kunt Site Recovery gebruiken voor een implementatie op Azure, Hyper-V, VMware of fysieke infrastructuur. Volg de [richtlijnen](site-recovery-sql.md#how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2) aan het einde van het document over het beveiligen van SQL Server-Cluster met Azure Site Recovery.
> * Zorg ervoor dat de veranderingssnelheid van gegevens (geschreven bytes per seconde) waargenomen op de computer zich binnen [limieten van Site Recovery](vmware-physical-azure-support-matrix.md#churn-limits). Voor windows-machines, kunt u dit onder het tabblad prestaties op Taakbeheer weergeven. Bekijk schrijven snelheid voor elke schijf.
> * Azure Site Recovery biedt ondersteuning voor replicatie van Failover-Clusterexemplaren op Storage Spaces Direct. [Meer informatie](azure-to-azure-how-to-enable-replication-s2d-vms.md).
 

## <a name="disaster-recovery-of-application"></a>Herstel na noodgevallen van toepassing

**Azure Site Recovery coördineert de test-failover en failover van de gehele toepassing met behulp van herstelplannen.** 

Er zijn enkele vereisten om te controleren of dat het herstelplan kan volledig worden aangepast aan de hand van uw behoeften. Een SQL Server-implementatie moet doorgaans een Active Directory. Er is bovendien verbinding nodig voor uw toepassingslaag.

### <a name="step-1-set-up-active-directory"></a>Stap 1: Active Directory instellen

Instellen van Active Directory in de secundaire herstelsite voor SQL Server correct kunnen worden uitgevoerd.

* **Kleine onderneming**, met een klein aantal toepassingen en één domeincontroller voor de on-premises site, als u wilt voor failover van de hele site, raden wij u Site Recovery-replicatie gebruiken om te repliceren van de domeincontroller naar de secundaire server Datacenter, of naar Azure.
* **Middelgrote tot grote onderneming**— als u een groot aantal toepassingen, een Active Directory-forest hebt en u wilt Failover-overschakeling toepassing of werkbelasting uitvoeren, het beste instellen van een extra domeincontroller in het secundaire datacenter of in Azure. Als u wilt herstellen naar een externe site AlwaysOn-beschikbaarheidsgroepen gebruikt, raden wij dat u nog een extra domeincontroller op de secundaire site of in Azure, ingesteld om te gebruiken voor het herstelde exemplaar van SQL Server.

De instructies in dit artikel wordt ervan uitgegaan dat een domeincontroller beschikbaar op de secundaire locatie is. [Lees meer](site-recovery-active-directory.md) over het beveiligen van Active Directory met Site Recovery.

### <a name="step-2-ensure-connectivity-with-other-application-tiers-and-web-tier"></a>Stap 2: Zorg voor verbindingen met andere toepassing laag of lagen toe en de weblaag

Zorg ervoor dat zodra de databaselaag actief en werkend in Azure-doelregio is, u verbinding met de toepassing en de weblaag hebben. Stappen die nodig zijn moeten vooraf worden genomen voor het valideren van verbindingen met test-failover.

Begrijpen hoe u kunt toepassingen ontwerpen voor overwegingen voor connectiviteit met een aantal voorbeelden:
* [Een toepassing ontwerpen voor noodherstel voor cloud](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Strategieën voor herstel na noodgevallen van elastische pool](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-integrate-with-always-on-active-geo-replication-or-auto-failover-groups-for-application-failover"></a>Stap 3: Integreren met Always On, actieve Geo-replicatie of automatische failover-groepen voor failover van de toepassing

BCDR-technologieën Always On, actieve Geo-replicatie en automatische failover-groepen hebt secundaire replica's van SQL server wordt uitgevoerd in Azure-doelregio. Daarom is de eerste stap voor de failover van uw toepassing te maken van deze replica als primaire (ervan uitgaande dat er al een domeincontroller in secundaire). Deze stap is mogelijk niet nodig als u wilt een automatische failover. Pas nadat de databasefailover is voltooid, moet u failover de lagen van uw web- of toepassing.

> [!NOTE] 
> Als u de SQL-machines met Azure Site Recovery hebt beveiligd, moet u alleen een groep voor gegevensherstel van deze machines maken en toevoegen van de failover van het herstelplan te gaan.

[Maken van een herstelplan](site-recovery-create-recovery-plans.md) met toepassings- en web-laag virtuele machines. Volg de onderstaande stappen om toe te voegen, failover van database-laag:

1. Scripts in uw Azure Automation-account importeren. Hierin zijn de scripts voor de SQL-beschikbaarheidsgroep failover in een [virtuele machine van Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) en een [klassieke virtuele machine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR-SQL-FailoverAG toevoegen als een pre-actie van de eerste groep van het herstelplan te gaan.

1. Volg de instructies die beschikbaar zijn in het script voor het maken van een automation-variabele voor de naam van de beschikbaarheidsgroepen.

### <a name="step-4-conduct-a-test-failover"></a>Stap 4: Een failovertest uitvoeren

Sommige BCDR-technologieën, zoals SQL Always On ondersteuning geen systeemeigen voor test-failover. Daarom raden wij de volgende benadering **alleen bij het integreren met dergelijke technologieën**:

1. Instellen van [Azure Backup](../backup/backup-azure-arm-vms.md) op de virtuele machine die als host fungeert voor de beschikbaarheidsreplica van de groep in Azure.

1. Voordat u activeert de failovertest van het herstelplan te gaan, de virtuele machine van de back-up die zijn uitgevoerd in de vorige stap te herstellen.

    ![Herstellen vanaf Azure Backup](./media/site-recovery-sql/restore-from-backup.png)

1. [Afdwingen dat een quorum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) in de virtuele machine teruggezet vanuit een back-up.

1. IP-adres van de listener bijwerken naar een IP-adres in het testfailovernetwerk.

    ![Bijwerken van de Listener IP](./media/site-recovery-sql/update-listener-ip.png)

1. Listener online brengen.

    ![Listener Online brengen](./media/site-recovery-sql/bring-listener-online.png)

1. Een load balancer maken met een die IP gemaakt op basis van front-end-IP-adresgroep overeenkomt met elke beschikbaarheidsgroep-listener en met de SQL-virtuele machine in de back-endpool toegevoegd.

     ![Load Balancer - Frontend-IP-adresgroep maken](./media/site-recovery-sql/create-load-balancer1.png)

    ![Load Balancer - back-endpool maken](./media/site-recovery-sql/create-load-balancer2.png)

1. Failover van uw toepassingslaag, gevolgd door de weblaag in dit herstelplan in latere recovery groepen toevoegen. 
1. Voer een failovertest uit van het herstelplan te gaan voor het testen van failover van de end-to-end van toepassing.

## <a name="steps-to-do-a-failover"></a>Stappen voor het doen van een failover

Nadat u het script in het herstelplan te gaan in stap 3 hebt toegevoegd en gevalideerd door het uitvoeren van een testfailover met een gespecialiseerde benadering in stap 4, kunt u de failover van het herstelplan te gaan in stap 3 hebt gemaakt kunt doen.

Houd er rekening mee dat de failover-stappen voor de web-lagen en toepassing met het in test-failover en failover-herstelplannen overeenkomen moeten.

## <a name="how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2"></a>Het beveiligen van een SQL Server-cluster (standard edition/SQL Server 2008 R2)

Voor een cluster met SQL Server Standard edition of SQL Server 2008 R2, wordt u aangeraden dat u Site Recovery-replicatie gebruiken om SQL Server te beveiligen.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure naar Azure en On-premises naar Azure

Site Recovery biedt geen guest cluster ondersteuning bij het repliceren naar een Azure-regio. SQL Server ook biedt geen oplossing voor noodherstel van de lage kosten voor de Standard edition. In dit scenario wordt aangeraden u beveiligen van de SQL Server-cluster naar een zelfstandige SQL Server in de primaire locatie en het herstellen van de secundaire.

1. Configureer een extra zelfstandige SQL Server-exemplaar op de primaire Azure-regio of op on-premises site.
1. Configureer de instantie moet fungeren als een mirror voor de databases die u wilt beveiligen. Configureer mirroring in hoge veiligheidsmodus.
1. Site Recovery op de primaire site configureert ([Azure](azure-to-azure-tutorial-enable-replication.md), [Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware-machines/fysieke servers)](site-recovery-vmware-to-azure-classic.md).
1. Site Recovery-replicatie gebruiken om te repliceren van de nieuwe SQL Server-exemplaar naar secundaire site. Omdat dit een hoge beveiliging mirror-kopie, deze wordt gesynchroniseerd met de primaire-cluster, maar zal worden gerepliceerd met behulp van Site Recovery-replicatie.


![Standard-cluster](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Overwegingen voor failback

Failback na een niet-geplande failover is voor SQL Server Standard-clusters vereist een SQL server-back-up en herstellen van de mirror-instantie naar het oorspronkelijke cluster, met reestablishment van de mirror.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-does-sql-get-licensed-when-protected-with-azure-site-recovery"></a>Hoe SQL ophalen in licentie gegeven als beveiligd met Azure Site Recovery?
Replicatie van Azure Site Recovery voor SQL Server wordt gedekt onder Software Assurance Disaster Recovery. Dit geldt voor alle Azure Site Recovery-scenario's (on-premises naar Azure-noodherstel of Azure IaaS-noodherstel over meerdere regio's). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/)

### <a name="will-azure-site-recovery-support-my-sql-version"></a>Azure Site Recovery biedt ondersteuning voor mijn SQL-versie?
Azure Site Recovery is neutraal van toepassing. Daarom kan kan een willekeurige versie van SQL server die is geïmplementeerd op een ondersteund besturingssysteem worden beveiligd door Azure Site Recovery. [Meer informatie](vmware-physical-azure-support-matrix.md#replicated-machines)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie](site-recovery-components.md) over Site Recovery-architectuur.
* Meer informatie voor SQL-Servers in Azure, over [hoge beschikbaarheidsoplossingen](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) voor herstel in secundaire Azure-regio.
* Meer informatie voor SQL-Database in Azure over de [bedrijfscontinuïteit](../sql-database/sql-database-business-continuity.md) en [hoge beschikbaarheid](../sql-database/sql-database-high-availability.md) opties voor herstel in secundaire Azure-regio.
* Voor SQL server-machines op de on-premises [meer](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) over de opties voor hoge beschikbaarheid voor herstel in Azure Virtual Machines.
