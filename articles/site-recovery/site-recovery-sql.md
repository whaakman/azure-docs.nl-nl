---
title: Toepassingen met SQL Server en Azure Site Recovery repliceren | Microsoft Docs
description: Dit artikel wordt beschreven hoe u SQL Server met behulp van Azure Site Recovery voor SQL Server-noodherstel mogelijkheden repliceren.
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: pratshar
ms.openlocfilehash: 7981173b419632683a40a54bc07f51f0fccab531
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server met behulp van SQL Server-noodherstel en Azure Site Recovery beveiligen

Dit artikel wordt beschreven hoe u de SQL Server-back-end van een toepassing met een combinatie van SQL Server zakelijke continuïteit en disaster recovery (BCDR)-technologieën, beveiligt en [Azure Site Recovery](site-recovery-overview.md).

Voordat u begint, zorg er dan voor dat u begrijpt dat SQL Server disaster recovery mogelijkheden, zoals Failoverclustering, altijd op beschikbaarheidsgroepen databasespiegeling en back-upfunctie voor logboekbestanden.


## <a name="sql-server-deployments"></a>Implementaties van SQL Server

Veel werkbelastingen SQL Server gebruiken als basis en kunnen worden geïntegreerd met apps, zoals SharePoint, Dynamics en SAP, voor het implementeren van services van gegevens.  SQL Server kan worden geïmplementeerd in een aantal verschillende manieren:

* **Zelfstandige SQL Server**: SQL Server en alle databases worden gehost op een enkele computer (fysiek of een virtuele). Wanneer u een gevirtualiseerde, worden host clustering wordt gebruikt voor lokale beschikbaarheid. Hoge beschikbaarheid gastniveau wordt niet geïmplementeerd.
* **SQL Server Failover Clustering-exemplaren (altijd op FCI)**: twee of meer knooppunten met SQL Server-exemplaar gestart met gedeelde schijven zijn geconfigureerd in een Windows-failovercluster. Als een knooppunt niet actief is, kan het cluster failover SQL Server naar een ander exemplaar. Deze instelling wordt doorgaans gebruikt voor het implementeren van hoge beschikbaarheid op een primaire site. Deze implementatie biedt geen bescherming tegen storingen of onderbreking in de laag gedeelde opslag. Een gedeelde schijf kan worden geïmplementeerd met behulp van iSCSI, Fibre channel of gedeelde vhdx.
* **SQL AlwaysOn-beschikbaarheidsgroepen**: twee of meer knooppunten zijn ingesteld op shared nothing-cluster, met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchrone replicatie en automatische failover.

 In dit artikel maakt gebruik van de volgende systeemeigen SQL disaster recovery technologieën voor het herstellen van databases met een externe site:

* SQL AlwaysOn-beschikbaarheidsgroepen te bieden voor herstel na noodgevallen voor SQL Server 2012 of 2014 Enterprise-edities.
* SQL database mirroring in de modus hoge beveiliging voor SQL Server Standard edition (alle versies), of voor SQL Server 2008 R2.

## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

### <a name="supported-scenarios"></a>Ondersteunde scenario's
Site Recovery kan SQL Server beveiligen, zoals samengevat in de tabel.

**Scenario** | **Een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Ja | Ja
**Azure**|N.v.t.| Ja

### <a name="supported-sql-server-versions"></a>Ondersteunde versies van SQL Server
Deze SQL Server-versies worden ondersteund voor de ondersteunde scenario's:

* SQL Server 2016 Enterprise en Standard
* SQL Server 2014 Enterprise en Standard
* SQL Server 2012 Enterprise en Standard
* SQL Server 2008 R2 Enterprise en Standard

### <a name="supported-sql-server-integration"></a>Ondersteunde SQL Server-integratie

Site Recovery kan worden geïntegreerd met systeemeigen SQL Server BCDR-technologieën samengevat in de tabel, om een herstel na noodgevallen oplossing te bieden.

**Functie** | **Details** | **SQL Server** |
--- | --- | ---
**AlwaysOn-beschikbaarheidsgroep** | Meerdere exemplaren van de zelfstandige versie van SQL Server worden uitgevoerd in een failovercluster met meerdere knooppunten.<br/><br/>Databases kunnen worden gegroepeerd in failover-groepen die kunnen worden gekopieerd (gespiegeld) voor SQL Server-exemplaren zodat er is geen gedeelde opslag nodig is.<br/><br/>Biedt herstel na noodgevallen tussen een primaire site en een of meer secundaire sites. Twee knooppunten kunnen worden ingesteld in een gedeelde niets cluster met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchrone replicatie en automatische failover. | SQL Server 2016, SQL Server 2014 en SQL Server 2012 Enterprise edition
**Failover clustering (altijd op FCI)** | SQL Server maakt gebruik van Windows failover clustering voor hoge beschikbaarheid van de lokale SQL Server-werkbelastingen.<br/><br/>Knooppunten met exemplaren van SQL Server met gedeelde schijven zijn geconfigureerd in een failovercluster. Als een exemplaar niet actief is wordt het cluster overgenomen door naar andere.<br/><br/>Het cluster biedt geen bescherming tegen storingen of storingen in gedeelde opslag. De gedeelde schijf kan worden geïmplementeerd met iSCSI, Fibre channel, of gedeelde vhdx-bestanden. | SQL Server Enterprise-edities<br/><br/>SQL Server Standard edition (maximaal twee knooppunten alleen)
**Databasespiegeling (modus hoge beveiliging)** | Een individuele database naar een enkele secundaire kopie beveiligt. Beschikbaar in beide hoge beveiliging (synchrone) en hoge prestaties (asynchrone) replicatie modi. Geen vereist een failover-cluster. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alle edities
**Zelfstandige SQL Server** | De SQL Server en database gehost op één server (fysiek of virtueel). Host clustering wordt gebruikt voor hoge beschikbaarheid als de server virtueel is. Er is geen gastniveau met hoge beschikbaarheid. | Enterprise of Standard edition

## <a name="deployment-recommendations"></a>Aanbevelingen voor de implementatie

Deze tabel bevat een overzicht van onze aanbevelingen voor het SQL Server BCDR-technologieën te integreren met Site Recovery.

| **Versie** | **Editie** | **Implementatie** | **On-premises naar on-premises** | **On-premises naar Azure** |
| --- | --- | --- | --- | --- |
| SQL Server 2014 of 2012 |Enterprise |Failover clusterexemplaar |Altijd op beschikbaarheidsgroepen |Altijd op beschikbaarheidsgroepen |
|| Enterprise |AlwaysOn-beschikbaarheidsgroepen voor hoge beschikbaarheid |Altijd op beschikbaarheidsgroepen |Altijd op beschikbaarheidsgroepen | |
|| Standard |Failover cluster instance (FCI) |Replicatie van site Recovery met lokale mirror |Replicatie van site Recovery met lokale mirror | |
|| Enterprise of Standard |Zelfstandig |Replicatie van site Recovery |Replicatie van site Recovery | |
| SQL Server 2008 R2 of 2008 |Enterprise of Standard |Failover cluster instance (FCI) |Replicatie van site Recovery met lokale mirror |Replicatie van site Recovery met lokale mirror |
|| Enterprise of Standard |Zelfstandig |Replicatie van site Recovery |Replicatie van site Recovery | |
| SQL Server (alle versies) |Enterprise of Standard |Failover cluster instance - DTC-toepassing |Replicatie van site Recovery |Niet ondersteund |

## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

* Een lokale SQL Server-implementatie, met een ondersteunde versie van SQL Server. Normaal gesproken moet u ook Active Directory voor uw SQL-server.
* De vereisten voor het scenario dat u wilt implementeren. Meer informatie over de ondersteuningsvereisten voor voor [replicatie naar Azure](site-recovery-support-matrix-to-azure.md) en [lokale](site-recovery-support-matrix.md), en [implementatievereisten](site-recovery-prereq.md).
* Uitvoeren als u herstel in Azure instelt, de [Azure virtuele Machine Readiness Assessment](http://www.microsoft.com/download/details.aspx?id=40898) hulpprogramma op uw SQL Server-virtuele machines, om ervoor te zorgen dat ze compatibel zijn met de Azure Site Recovery zijn.

## <a name="set-up-active-directory"></a>Active Directory instellen

Active Directory is ingesteld in de herstelsite secundaire voor SQL Server correct kunnen worden uitgevoerd.

* **Kleine onderneming**— met een klein aantal toepassingen en één domeincontroller voor de lokale site, als u failover wilt uitvoeren de hele site, raden wij aan u Site Recovery-replicatie gebruiken om te repliceren van de domeincontroller naar het secundaire datacenter of naar Azure.
* **Middelgrote tot grote onderneming**— als u een groot aantal toepassingen, een Active Directory-forest hebt en u wilt uitvoeren door de toepassing of werkbelasting, raden wij aan instellen van een extra domeincontroller in het secundaire datacenter of in Azure. Als u AlwaysOn-beschikbaarheidsgroepen te herstellen naar een externe site gebruikt, raden wij dat u instellen in een andere extra domeincontroller op de secundaire site of in Azure, moet worden gebruikt voor het herstelde exemplaar van SQL Server.

De instructies in dit artikel wordt ervan uitgegaan dat een domeincontroller beschikbaar op de secundaire locatie is. [Lees meer](site-recovery-active-directory.md) over het beveiligen van Active Directory met Site Recovery.


## <a name="integrate-with-sql-server-always-on-for-replication-to-azure"></a>Integratie met SQL Server Always On voor replicatie naar Azure

Dit is wat u moet doen:

1. Importeer de scripts in uw Azure Automation-account. Hierin zijn de scripts op SQL-beschikbaarheidsgroep failover in een [Resource Manager virtuele machine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) en een [klassieke virtuele machine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR-SQL-FailoverAG als een pre-actie van de eerste groep van het herstelplan toevoegen.

1. Volg de instructies die beschikbaar zijn in het script voor het maken van een automation-variabele de naam van de beschikbaarheidsgroepen opgeven.

### <a name="steps-to-do-a-test-failover"></a>Uit te voeren van een testfailover stappen

Systeemeigen SQL Always On biedt geen ondersteuning voor de testfailover. Daarom raden we het volgende:

1. Instellen van [Azure Backup](../backup/backup-azure-arm-vms.md) op de virtuele machine die als host fungeert voor de beschikbaarheidsreplica van de groep in Azure.

1. Herstellen voordat testfailover van het herstelplan, de virtuele machine van de back-up in de vorige stap.

    ![Azure back-up terugzetten ](./media/site-recovery-sql/restore-from-backup.png)

1. [Een quorum forceren](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) in de virtuele machine teruggezet vanuit een back-up.

1. IP-adres van de listener worden bijgewerkt naar een IP-adres beschikbaar in het testfailovernetwerk.

    ![Bijwerken van de Listener IP](./media/site-recovery-sql/update-listener-ip.png)

1. Listener online brengen.

    ![Listener Online brengen](./media/site-recovery-sql/bring-listener-online.png)

1. Maak een load balancer met een die IP-gemaakt onder frontend-IP-adresgroep overeenkomen met elke beschikbaarheidsgroep-listener en met de SQL-virtuele machine toegevoegd in de back-endpool.

     ![Load Balancer - Frontend IP-adresgroep maken ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Load Balancer - back-endpool maken ](./media/site-recovery-sql/create-load-balancer2.png)

1. Een testfailover uitgevoerd van het herstelplan doen.

### <a name="steps-to-do-a-failover"></a>Uit te voeren van een failover stappen

Zodra u hebt het script toegevoegd in het herstelplan en het herstelplan gevalideerd als volgt een failovertest uitvoert, kunt u de failover van herstelplan kunt doen.


## <a name="integrate-with-sql-server-always-on-for-replication-to-a-secondary-on-premises-site"></a>Integratie met SQL Server Always On voor replicatie naar een secundaire on-premises site

Als de SQL Server-beschikbaarheidsgroepen gebruikt voor hoge beschikbaarheid (of een FCI), wordt u aangeraden beschikbaarheidsgroepen op de herstelsite. Houd er rekening mee dat dit geldt voor apps die geen van gedistribueerde transacties gebruikmaken.

1. [Databases configureren](https://msdn.microsoft.com/library/hh213078.aspx) in beschikbaarheidsgroepen.
1. Maak een virtueel netwerk op de secundaire site.
1. Instellen van een site-naar-site VPN-verbinding tussen het virtuele netwerk en de primaire site.
1. Maak een virtuele machine op de herstelsite en SQL Server installeren op deze.
1. De bestaande altijd op beschikbaarheidsgroepen uitbreiden naar de nieuwe virtuele machine een SQL-Server. Dit exemplaar van SQL Server configureren als een kopie van de asynchrone replica.
1. Een beschikbaarheidsgroep-listener te maken of bijwerken van de bestaande listener om op te nemen van de asynchrone replica virtuele machine.
1. Zorg ervoor dat de toepassing-farm is ingesteld met behulp van de listener. Als deze ingesteld met behulp van de naam van de database is, bijwerken voor het gebruik van de listener, zodat u niet hoeft te configureren na de failover.

Voor toepassingen die gebruikmaken van gedistribueerde transacties, raden wij aan u Site Recovery met implementeert [VMware of fysieke server site-naar-site-replicatie](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Overwegingen voor het plannen van herstel
1. Dit voorbeeldscript aan de VMM-bibliotheek op de primaire en secundaire sites toevoegen.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. Wanneer u een herstelplan voor de toepassing maakt, moet u een pre-actie toevoegen aan groep-1-script stap, die het script voor failover-beschikbaarheidsgroepen wordt aangeroepen.

## <a name="protect-a-standalone-sql-server"></a>Een zelfstandige SQL-Server beveiligen

In dit scenario is het raadzaam dat u Site Recovery-replicatie gebruiken om de SQL Server-machine te beveiligen. De exacte stappen is afhankelijk van of SQL Server een virtuele machine of een fysieke server is, en of u wilt repliceren naar Azure of een secundaire on-premises site. Meer informatie over [Site herstelscenario's](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Beveiligen van een SQL Server-cluster (standard edition/Windows Server 2008 R2)

Voor een cluster met SQL Server Standard edition of SQL Server 2008 R2, wordt u aangeraden dat u Site Recovery-replicatie gebruiken om SQL Server te beveiligen.

### <a name="on-premises-to-on-premises"></a>On-premises naar on-premises

* Als de app gebruikmaakt van gedistribueerde transacties raden wij aan u implementeert [Site Recovery met SAN-replicatie](site-recovery-vmm-san.md) voor een Hyper-V-omgeving of [VMware of fysieke server naar VMware](site-recovery-vmware-to-vmware.md) voor een VMware-omgeving.
* Gebruik de bovenstaande benadering voor het herstellen van het cluster als een zelfstandige server, door gebruik te maken van een lokale hoge beveiliging DB mirror voor niet-DTC-toepassingen.

### <a name="on-premises-to-azure"></a>On-premises naar Azure

Site Recovery biedt niet de Gast clusterondersteuning bij het repliceren naar Azure. SQL Server bieden niet ook een goedkope noodherstel voor Standard-editie. In dit scenario wordt aangeraden de lokale SQL Server-cluster naar een zelfstandige SQL Server beveiligen en herstellen in Azure.

1. Configureer een extra zelfstandige SQL Server-exemplaar op de lokale site.
1. Configureer het exemplaar moeten fungeren als een mirror voor de databases die u wilt beveiligen. Configureer in de modus hoge beveiliging spiegelen.
1. Site Recovery op de lokale site configureren voor ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware-virtuele machines/fysieke servers)](site-recovery-vmware-to-azure-classic.md).
1. Site Recovery-replicatie gebruiken om te repliceren van de nieuwe SQL Server-exemplaar naar Azure. Omdat het een hoge beveiliging mirror-kopie, deze worden vervolgens gesynchroniseerd met de primaire cluster, maar deze wordt gerepliceerd naar Azure met behulp van replicatie van Site Recovery.


![Standard-cluster](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Overwegingen voor failback

Failback na een niet-geplande failover is voor SQL Server Standard clusters vereist een SQL server-back-up en herstel van het mirror-exemplaar voor het oorspronkelijke cluster, met reestablishment van de mirror.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-components.md) over de architectuur van Site Recovery.
