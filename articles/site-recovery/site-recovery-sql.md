---
title: Toepassingen met SQL Server en Azure Site Recovery repliceert | Microsoft Docs
description: In dit artikel wordt beschreven hoe u SQL Server met behulp van Azure Site Recovery voor de mogelijkheden van SQL Server na een noodgeval repliceren.
services: site-recovery
documentationcenter: ''
author: prateek9us
manager: gauravd
editor: ''
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: pratshar
ms.openlocfilehash: c877f4bbc0ed14e859ff39f1d719a9cd0b787118
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920828"
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>SQL Server met behulp van SQL Server-noodherstel en Azure Site Recovery beveiligen

In dit artikel wordt beschreven hoe u de SQL Server-back-end van een toepassing met behulp van een combinatie van bedrijfscontinuïteit van SQL Server en disaster recovery (BCDR)-technologieën, beveiligen en [Azure Site Recovery](site-recovery-overview.md).

Voordat u begint, zorg ervoor dat SQL Server herstelfunctionaliteit bij noodgevallen, met inbegrip van Failoverclustering, Always On-beschikbaarheidsgroepen, databasespiegeling en back-upfunctie.


## <a name="sql-server-deployments"></a>SQL Server-implementaties

Veel werkbelastingen SQL Server gebruiken als basis en deze kan worden geïntegreerd met apps, zoals SharePoint, Dynamics en SAP, voor het implementeren van services van gegevens.  SQL Server kan worden geïmplementeerd op een aantal manieren:

* **Zelfstandige SQL Server**: SQL Server en alle databases worden gehost op een enkele computer (fysieke of een virtueel). Wanneer u een gevirtualiseerde, worden host clusters wordt gebruikt voor lokale hoge beschikbaarheid. Hoge beschikbaarheid op gastniveau wordt niet geïmplementeerd.
* **SQL Server Failover cluster Instances (altijd op FCI)**: twee of meer knooppunten met SQL Server is gestart met gedeelde schijven zijn geconfigureerd in een Windows-failovercluster. Als een knooppunt niet actief is, kan het cluster failover SQL Server naar een andere instantie. Deze instelling wordt meestal gebruikt voor het implementeren van hoge beschikbaarheid op een primaire site. Deze implementatie biedt geen bescherming tegen storingen of onderbreking in de gedeelde opslaglaag. Een gedeelde schijf kan worden geïmplementeerd met behulp van iSCSI, Fibre channel of gedeelde vhdx.
* **SQL Always On Availability Groups**: twee of meer knooppunten zijn ingesteld op shared nothing-cluster, met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchrone replicatie en automatische failover.

 In dit artikel maakt gebruik van de volgende systeemeigen SQL disaster recovery technologieën voor het herstellen van databases met een externe site:

* SQL Always On Availability Groups, om te bieden voor herstel na noodgevallen voor SQL Server 2012 of 2014 Enterprise-edities.
* SQL-database mirroring in hoge veiligheidsmodus voor SQL Server Standard edition (alle versies), of voor SQL Server 2008 R2.

## <a name="site-recovery-support"></a>Ondersteuning voor site Recovery

### <a name="supported-scenarios"></a>Ondersteunde scenario's
Site Recovery kan SQL Server beveiligen, zoals samengevat in de tabel.

**Scenario** | **Naar een secundaire site** | **Naar Azure**
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

Site Recovery kan worden geïntegreerd met systeemeigen SQL Server BCDR-technologieën worden samengevat in de tabel voor een oplossing voor noodherstel.

**Functie** | **Details** | **SQL Server** |
--- | --- | ---
**AlwaysOn-beschikbaarheidsgroep** | Meerdere exemplaren van de zelfstandige versie van SQL Server wordt uitgevoerd in een failovercluster met meerdere knooppunten.<br/><br/>Databases kunnen worden gegroepeerd in failover-groepen die kunnen worden gekopieerd (gespiegeld) op SQL Server-exemplaren zodat er is geen gedeelde opslag nodig is.<br/><br/>Biedt herstel na noodgevallen tussen een primaire site en een of meer secundaire sites. Twee knooppunten kunnen worden ingesteld in een gedeelde niets cluster met SQL Server-databases die zijn geconfigureerd in een beschikbaarheidsgroep met synchrone replicatie en automatische failover. | SQL Server 2016, SQL Server 2014 en SQL Server 2012 Enterprise edition
**Failover clustering (altijd op FCI)** | SQL Server maakt gebruik van Windows failover clustering voor hoge beschikbaarheid van on-premises SQL Server-workloads.<br/><br/>Knooppunten met de exemplaren van SQL Server met gedeelde schijven worden geconfigureerd in een failover-cluster. Als een exemplaar niet actief is, is het cluster wordt overgenomen naar ander account.<br/><br/>Het cluster biedt geen bescherming tegen storingen of onderbrekingen in gedeelde opslag. De gedeelde schijf kan worden geïmplementeerd met iSCSI, Fibre channel, of gedeelde vhdx-bestanden. | SQL Server Enterprise-edities<br/><br/>SQL Server Standard edition (beperkt tot alleen twee knooppunten)
**Database mirroring (hoge veiligheidsmodus)** | Beschermt een één database naar een secundair één exemplaar. Beschikbaar in beide hoge beveiliging (synchrone) en hoge prestaties (asynchrone) replicatiemodi. Er zijn een failover-cluster vereist. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alle edities
**Zelfstandige SQL Server** | De SQL Server en database worden gehost op één server (fysiek of virtueel). Host clusters wordt gebruikt voor hoge beschikbaarheid als de server virtueel is. Er is geen hoge beschikbaarheid op gastniveau. | Enterprise- of Standard-editie

## <a name="deployment-recommendations"></a>Aanbevelingen voor de implementatie

Deze tabel bevat een overzicht van onze aanbevelingen voor het integreren van SQL Server BCDR-technologieën met Site Recovery.

| **Versie** | **Editie** | **Implementatie** | **On-premises naar on-premises** | **On-premises naar Azure** |
| --- | --- | --- | --- | --- |
| SQL Server 2014 of 2012 |Enterprise |Failover-clusterexemplaar |AlwaysOn-beschikbaarheidsgroepen |AlwaysOn-beschikbaarheidsgroepen |
|| Enterprise |AlwaysOn-beschikbaarheidsgroepen voor hoge beschikbaarheid |AlwaysOn-beschikbaarheidsgroepen |AlwaysOn-beschikbaarheidsgroepen | |
|| Standard |Failover clusterexemplaar (FCI) |Replicatie van site Recovery met lokale mirror |Replicatie van site Recovery met lokale mirror | |
|| Enterprise of Standard |Zelfstandig |Replicatie van site Recovery |Replicatie van site Recovery | |
| SQL Server 2008 R2 of 2008 |Enterprise of Standard |Failover clusterexemplaar (FCI) |Replicatie van site Recovery met lokale mirror |Replicatie van site Recovery met lokale mirror |
|| Enterprise of Standard |Zelfstandig |Replicatie van site Recovery |Replicatie van site Recovery | |
| SQL Server (alle versies) |Enterprise of Standard |Failover cluster instance - DTC-toepassing |Replicatie van site Recovery |Niet ondersteund |

## <a name="deployment-prerequisites"></a>Vereisten voor implementatie

* Een on-premises SQL Server-implementatie, waarop een ondersteunde versie van SQL Server wordt uitgevoerd. Normaal gesproken moet u ook Active Directory voor uw SQL-server.
* De vereisten voor het scenario dat u wilt implementeren. Meer informatie over de vereisten van het ondersteuningsteam voor [replicatie naar Azure](site-recovery-support-matrix-to-azure.md) en [on-premises](site-recovery-support-matrix.md), en [implementatievereisten](site-recovery-prereq.md).
* Als u herstel in Azure instelt, worden uitgevoerd de [Azure Virtual Machine Readiness Assessment](http://www.microsoft.com/download/details.aspx?id=40898) hulpprogramma op uw SQL Server-machines om te controleren of ze compatibel zijn met Azure en Site Recovery.

## <a name="set-up-active-directory"></a>Active Directory instellen

Instellen van Active Directory in de secundaire herstelsite voor SQL Server correct kunnen worden uitgevoerd.

* **Kleine onderneming**, met een klein aantal toepassingen en één domeincontroller voor de on-premises site, als u wilt voor failover van de hele site, raden wij u Site Recovery-replicatie gebruiken om te repliceren van de domeincontroller naar de secundaire server Datacenter, of naar Azure.
* **Middelgrote tot grote onderneming**— als u een groot aantal toepassingen, een Active Directory-forest hebt en u wilt Failover-overschakeling toepassing of werkbelasting uitvoeren, het beste instellen van een extra domeincontroller in het secundaire datacenter of in Azure. Als u wilt herstellen naar een externe site AlwaysOn-beschikbaarheidsgroepen gebruikt, raden wij dat u nog een extra domeincontroller op de secundaire site of in Azure, ingesteld om te gebruiken voor het herstelde exemplaar van SQL Server.

De instructies in dit artikel wordt ervan uitgegaan dat een domeincontroller beschikbaar op de secundaire locatie is. [Lees meer](site-recovery-active-directory.md) over het beveiligen van Active Directory met Site Recovery.


## <a name="integrate-with-sql-server-always-on-for-replication-to-azure"></a>Integreren met SQL Server Always On voor replicatie naar Azure

Dit is wat u moet doen:

1. Scripts in uw Azure Automation-account importeren. Hierin zijn de scripts voor de SQL-beschikbaarheidsgroep failover in een [virtuele machine van Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) en een [klassieke virtuele machine](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Implementeren in Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. ASR-SQL-FailoverAG toevoegen als een pre-actie van de eerste groep van het herstelplan te gaan.

1. Volg de instructies die beschikbaar zijn in het script voor het maken van een automation-variabele voor de naam van de beschikbaarheidsgroepen.

### <a name="steps-to-do-a-test-failover"></a>Stappen voor het doen van een test-failover

Systeemeigen SQL Always On biedt geen ondersteuning voor test-failover. Daarom raden we het volgende:

1. Instellen van [Azure Backup](../backup/backup-azure-arm-vms.md) op de virtuele machine die als host fungeert voor de beschikbaarheidsreplica van de groep in Azure.

1. Voordat u activeert de failovertest van het herstelplan te gaan, de virtuele machine van de back-up die zijn uitgevoerd in de vorige stap te herstellen.

    ![Herstellen vanaf Azure Backup ](./media/site-recovery-sql/restore-from-backup.png)

1. [Afdwingen dat een quorum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) in de virtuele machine teruggezet vanuit een back-up.

1. IP-adres van de listener bijwerken naar een IP-adres in het testfailovernetwerk.

    ![Bijwerken van de Listener IP](./media/site-recovery-sql/update-listener-ip.png)

1. Listener online brengen.

    ![Listener Online brengen](./media/site-recovery-sql/bring-listener-online.png)

1. Een load balancer maken met een die IP gemaakt op basis van front-end-IP-adresgroep overeenkomt met elke beschikbaarheidsgroep-listener en met de SQL-virtuele machine in de back-endpool toegevoegd.

     ![Load Balancer - Frontend-IP-adresgroep maken ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Load Balancer - back-endpool maken ](./media/site-recovery-sql/create-load-balancer2.png)

1. Voer een failovertest uit van het herstelplan te gaan.

### <a name="steps-to-do-a-failover"></a>Stappen voor het doen van een failover

Nadat u het script toegevoegd in het herstelplan te gaan en het herstelplan gevalideerd door een testfailover uitvoert, kunt u de failover van het herstelplan te gaan doen.


## <a name="integrate-with-sql-server-always-on-for-replication-to-a-secondary-on-premises-site"></a>Integreren met SQL Server Always On voor replicatie naar een secundaire on-premises site

Als de SQL Server-beschikbaarheidsgroepen gebruikt voor hoge beschikbaarheid (of een FCI), wordt u aangeraden beschikbaarheidsgroepen op de site recovery ook. Houd er rekening mee dat dit geldt voor apps die geen van gedistribueerde transacties gebruikmaken.

1. [Databases configureren](https://msdn.microsoft.com/library/hh213078.aspx) in beschikbaarheidsgroepen.
1. Maak een virtueel netwerk op de secundaire site.
1. Instellen van een site-naar-site VPN-verbinding tussen het virtuele netwerk en de primaire site.
1. Een virtuele machine maken op de site recovery en SQL Server op deze installeren.
1. De bestaande AlwaysOn-beschikbaarheidsgroepen uitbreiden naar de nieuwe SQL Server-VM. Dit exemplaar van SQL Server configureren als een kopie van de asynchrone replica.
1. Een beschikbaarheidsgroep-listener maken of bijwerken van de bestaande listener om op te nemen van de asynchrone replica virtuele machine.
1. Zorg ervoor dat de toepassing-farm met behulp van de listener is ingesteld. Als deze ingesteld met behulp van de naam van de database-server is, bijgewerkt voor het gebruik van de listener, dus u hoeft deze na de failover opnieuw te configureren.

Voor toepassingen die gebruikmaken van gedistribueerde transacties, raden we u bij het implementeren van Site Recovery met [site-naar-site-replicatie van VMware/fysieke server](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Overwegingen voor het plannen van herstel
1. Met dit voorbeeldscript toevoegen aan de VMM-bibliotheek op de primaire en secundaire sites.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. Wanneer u een herstelplan voor de toepassing maakt, moet u een pre-actie toevoegen aan groep-1 in een script vastgelegd stap, die het script voor failover van beschikbaarheidsgroepen aanroept.

## <a name="protect-a-standalone-sql-server"></a>Een zelfstandige SQL Server beveiligen

In dit scenario raden we u Site Recovery-replicatie gebruiken om de SQL Server-machine te beveiligen. De exacte stappen, hangt af van of SQL Server een virtuele machine of een fysieke server is, en of u wilt repliceren naar Azure of een secundaire on-premises site. Meer informatie over [scenario's voor Site Recovery](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Beveiligen van een SQL Server-cluster (standard edition/Windows Server 2008 R2)

Voor een cluster met SQL Server Standard edition of SQL Server 2008 R2, wordt u aangeraden dat u Site Recovery-replicatie gebruiken om SQL Server te beveiligen.

### <a name="on-premises-to-on-premises"></a>On-premises naar on-premises

* Als de app gebruikmaakt van gedistribueerde transacties raden we u implementeren [Site Recovery met SAN-replicatie](site-recovery-vmm-san.md) voor een Hyper-V-omgeving of [VMware/fysieke server naar VMware](site-recovery-vmware-to-vmware.md) voor een VMware-omgeving.
* Voor niet-DTC-toepassingen, bovenstaande benadering te gebruiken voor het herstellen van het cluster als een zelfstandige server, door gebruik te maken van een lokale hoge beveiliging DB mirror.

### <a name="on-premises-to-azure"></a>On-premises naar Azure

Site Recovery biedt geen guest cluster ondersteuning bij het repliceren naar Azure. SQL Server ook biedt geen oplossing voor noodherstel van de lage kosten voor de Standard edition. In dit scenario wordt aangeraden de on-premises SQL Server-cluster naar een zelfstandige SQL Server beveiligen en herstellen in Azure.

1. Configureer een extra zelfstandige SQL Server-exemplaar op de on-premises site.
1. Configureer de instantie moet fungeren als een mirror voor de databases die u wilt beveiligen. Configureer mirroring in hoge veiligheidsmodus.
1. Site Recovery op de on-premises-site configureren voor ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) of [VMware-machines/fysieke servers)](site-recovery-vmware-to-azure-classic.md).
1. Gebruik Site Recovery-replicatie voor het repliceren van de nieuwe SQL Server-exemplaar naar Azure. Omdat dit een hoge beveiliging mirror-kopie, deze wordt gesynchroniseerd met de primaire-cluster, maar deze moet worden gerepliceerd naar Azure met behulp van Site Recovery-replicatie.


![Standard-cluster](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Overwegingen voor failback

Failback na een niet-geplande failover is voor SQL Server Standard-clusters vereist een SQL server-back-up en herstellen van de mirror-instantie naar het oorspronkelijke cluster, met reestablishment van de mirror.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie](site-recovery-components.md) over Site Recovery-architectuur.
