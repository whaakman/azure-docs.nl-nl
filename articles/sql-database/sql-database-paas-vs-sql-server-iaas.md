---
title: SQL (PaaS) Database vs. SQL Server in de cloud op VM’s (IaaS) | Microsoft Docs
description: 'Informatie over welke SQL Server-cloudoptie geschikt is voor uw toepassing: Azure SQL (PaaS) Database of SQL Server in de cloud op Azure Virtual Machines.'
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: SQL Server-cloud, SQL Server in de cloud, PaaS-database, SQL-cloudserver, DBaaS
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 90427402d348fdbcec9f573848ed7cc0c5b03e2d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254652"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Kies een SQL Server-cloudoptie: Azure SQL (PaaS) Database of SQL Server op Azure Virtual Machines (IaaS)

In Azure, hebt u uw SQL Server-workloads die worden uitgevoerd in een gehoste infrastructuur (IaaS) of die wordt uitgevoerd als een gehoste service ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): een SQL database-engine, op basis van de Enterprise-editie van SQL Server, die is geoptimaliseerd voor het ontwikkelen van moderne toepassingen. Azure SQL Database biedt verschillende implementatieopties:

  - U kunt een individuele database te implementeren een [logische server](sql-database-logical-servers.md).
  - U kunt implementeren in een [elastische pool](sql-database-elastic-pool.md) op een [logische server](sql-database-logical-servers.md) delen van resources en kosten te verlagen.
  - U kunt implementeren op een [Azure SQL Database beheerde instanties](sql-database-managed-instance.md).

   In de volgende afbeelding worden deze implementatieopties weergegeven:

     ![implementatieopties](./media/sql-database-technical-overview/deployment-options.png)

     > [!NOTE]
     > In alle drie versies voegt Azure SQL Database extra functies die niet beschikbaar in SQL Server, zoals ingebouwde intelligentie en het beheer zijn. Een logische server met één en gepoolde databases biedt de meeste functies van SQL Server-database-scoped van. Met Azure SQL Database Managed Instance biedt Azure SQL Database gedeelde bronnen voor databases en extra functies exemplaar binnen het bereik. Azure SQL Database Managed Instance biedt ondersteuning voor migratie van de database met minimale of geen wijzigingen van de database.

- [SQL Server op Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server geïnstalleerd en wordt gehost in de cloud op Windows Server of Linux-machines (VM's) die worden uitgevoerd op Azure, ook wel bekend als een infrastructure-as-a-service (IaaS). SQL Server op virtuele machines van Azure is een goede optie voor het migreren van on-premises SQL Server-databases en toepassingen zonder een wijziging van de database. Alle recente versies en edities van SQL Server zijn beschikbaar voor installatie in een IaaS-machine. Het belangrijkste verschil tussen de SQL-Database is dat SQL Server-VM's volledige controle over de database-engine kunnen. U kunt kiezen wanneer onderhoud/patching wordt gestart, het herstelmodel in simple of bulksgewijs geregistreerde om in te schakelen sneller laden minder aan te melden, onderbreken of starten van engine wanneer dat nodig is, wijzigen en u de SQL Server database engine volledig kunt aanpassen. Met deze extra controle wordt geleverd met toegevoegde verantwoordelijkheid voor het beheren van de virtuele machines.

Informatie over hoe elke optie voor implementatie in het Microsoft-gegevensplatform past en hulp bij het die overeenkomt met de juiste optie voor uw zakelijke vereisten. Of u nu kostenbesparingen of minimaal beheer het belangrijkst vindt, met dit artikel kunt u beslissen welke benadering de bedrijfsvereisten levert die u het belangrijkst vindt.

## <a name="microsofts-sql-data-platform"></a>SQL-gegevensplatform van Microsoft

Een van de eerste belangrijke punten in een discussie over Azure ten opzichte van on-premises SQL Server-databases is dat u alles kunt gebruiken. Het gegevensplatform van Microsoft maakt gebruik van SQL Server-technologie en maakt deze beschikbaar op fysieke on-premises machines, privécloudomgevingen, externe gehoste privécloudomgevingen en openbare cloud. Met SQL Server op virtuele Azure-machines kunt u voldoen aan de unieke en diverse bedrijfsbehoeften met een combinatie van on-premises en in de cloud gehoste implementaties terwijl dezelfde set serverproducten, ontwikkelingsprogramma's en kennis wordt gebruikt in deze omgevingen.

   ![Opties SQL Server cloud: SQL server op IaaS of SaaS-SQL Database in de cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Zoals we zien in het diagram, wordt elke aanbieding gekenmerkt door het niveau van beheer dat u hebt over de infrastructuur (op de X-as) en de mate van kostenefficiëntie die wordt bereikt door consolidatie en automatisering op databaseniveau (op de Y-as).

Bij het ontwerpen van een toepassing zijn vier eenvoudige opties beschikbaar voor het hosten van het SQL Server-onderdeel van de toepassing:

- SQL Server op niet-gevirtualiseerde fysieke computers
- SQL Server in on-premises gevirtualiseerde machines (privécloud)
- SQL Server in virtuele Azure-machines (openbare cloud van Microsoft)
- Azure SQL-database (openbare cloud van Microsoft)

In de volgende gedeelten vindt u meer informatie over SQL Server in de openbare cloud van Microsoft: Azure SQL Database en SQL Server op Azure Virtual Machines. Bovendien verkent u algemene zakelijke motivators om te bepalen welke optie het meest geschikt is voor uw toepassing.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>De Azure SQL Database en SQL Server op Azure Virtual Machines

- **Azure SQL Database**

Een relationele database-as-a-service (DBaaS) die wordt gehost in de Azure-cloud die in de branchecategorie van valt *Platform-as-a-Service (PaaS)*. [SQL-database](sql-database-technical-overview.md) is gebouwd op gestandaardiseerde hardware en software die eigendom is van, en wordt gehost en beheerd door Microsoft. Met SQL Database, kunt u ingebouwde functies en functionaliteiten die uitgebreide configuratie in SQL Server vereist. Als u SQL Database gebruikt, betaalt u per gebruik met opties voor het omhoog schalen voor meer vermogen zonder onderbrekingen. Azure SQL Database is een ideale omgeving voor het ontwikkelen van nieuwe toepassingen in de cloud. En met [Azure SQL Database Managed Instance](sql-database-managed-instance.md), kunt u uw eigen licentie meenemen. Bovendien is deze optie biedt alle voordelen van de PaaS van Azure SQL Database maar worden de mogelijkheden die eerder alleen beschikbaar in SQL-VM's waren toegevoegd. Dit omvat een systeemeigen virtuele netwerk (VNet) en in de buurt van 100% compatibiliteit met on-premises SQL Server. [Beheerd exemplaar](sql-database-managed-instance.md) is ideaal voor on-premises database-migraties naar Azure met minimale wijzigingen nodig zijn.

- **SQL Server op Azure Virtual Machines (VM's)**

Valt in de branchecategorie *Infrastructure-as-a-Service (IaaS)* en kunt u voor het uitvoeren van SQL Server in een virtuele machine in de cloud. [Virtuele machines met SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) ook uitvoeren op gestandaardiseerde hardware die is die eigendom zijn, die wordt gehost en beheerd door Microsoft. Wanneer u SQL Server op een virtuele machine, kunt u alleen betalen-als u overal voor een SQL Server-licentie die al zijn opgenomen in een SQL Server-installatiekopie of een bestaande licentie eenvoudig te gebruiken. U kunt ook stoppen of hervatten van de virtuele machine, indien nodig.

Over het algemeen zijn deze twee SQL-opties geoptimaliseerd voor verschillende doeleinden:

- **Azure SQL Database**

Geoptimaliseerd voor de totale beheerkosten voor beperken tot het minimum dat voor de inrichting en beheer van een groot aantal databases. Dit vermindert beheerkosten omdat u geen virtuele machines, besturingssysteem of database-software hoeft te beheren. U hoeft upgrades, hoge beschikbaarheid of [back-ups](sql-database-automated-backups.md) niet te beheren. Over het algemeen kan Azure SQL Database het aantal databases dat wordt beheerd door één IT of ontwikkelingsresource aanzienlijk verhogen. [Elastische pools](sql-database-elastic-pool.md) bieden ook ondersteuning voor architecturen voor SaaS-toepassing met meerdere tenants met functies zoals isolatie van tenants en de mogelijkheid om te schalen om kosten te verlagen door het delen van resources met databases. [Azure SQL Database Managed Instance](sql-database-managed-instance.md) biedt ondersteuning voor functies inschakelen van eenvoudige migratie van bestaande toepassingen, evenals het delen van resources met databases binnen het bereik van-exemplaar.

- **Met SQL Server op Azure Virtual machines**

Geoptimaliseerd voor het migreren van bestaande toepassingen naar Azure of bestaande on-premises toepassingen naar de cloud in hybride implementaties uit te breiden. Bovendien kunt u SQL Server op een virtuele machine gebruiken om traditionele SQL Server-toepassingen te ontwikkelen en te testen. Met SQL Server op Azure Virtual Machines hebt u de volledige beheerdersrechten van een speciaal exemplaar van SQL Server en een cloud-gebaseerde VM. Het is een ideale keuze als een organisatie al IT-bronnen beschikbaar heeft voor het onderhouden van de virtuele machines. Met deze mogelijkheden kunt u een aangepast systeem bouwen voor de specifieke prestaties en beschikbaarheidsvereisten van uw toepassing.

De volgende tabel geeft een overzicht van de belangrijkste kenmerken van de SQL Database en SQL Server op Azure Virtual Machines:

| | Azure SQL Database<br>Logische servers, elastische pools en individuele databases | Azure SQL Database<br>Beheerd exemplaar |Azure Virtual Machine<br>SQL Server |
| --- | --- | --- |---|
| **Ideaal voor:** |Nieuwe cloudtoepassingen die u wilt gebruiken van de nieuwste stabiele SQL Server-functies en tijdsbeperkingen in ontwikkeling en marketing. | Nieuwe toepassingen of bestaande on-premises toepassingen die u wilt gebruiken voor de nieuwste stabiele SQL Server-functies en die worden gemigreerd naar de cloud met minimale wijzigingen worden aangebracht.  | Bestaande toepassingen die snelle migratie naar de cloud met minimale wijzigingen worden aangebracht of geen wijzigingen vereist. Snelle ontwikkelings- en testscenario's als u geen on-premises SQL Server-hardware wilt kopen die niet bestemd is voor productie. |
|  | Teams die ingebouwde hoge beschikbaarheid, herstel na noodgevallen en upgrademogelijkheden van de database nodig hebben. | Hetzelfde als de SQL-Database. | Teams die kunnen configureren, fijn afstemmen, aanpassen en beheren van hoge beschikbaarheid, herstel na noodgevallen en patchen voor SQL Server. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. | |
|  | Teams die het onderliggende besturingssysteem en configuratie-instellingen niet willen beheren. | Hetzelfde als de SQL-Database. | U hebt een aangepaste omgeving met volledige beheerdersrechten nodig. | |
|  | Databases van maximaal 100 TB. | Hetzelfde als de SQL-Database. | SQL Server-exemplaren met maximaal 64 TB aan opslag. Het exemplaar kan zoveel databases ondersteunen als nodig is. |
| **Compatibiliteit** | Biedt ondersteuning voor de meeste on-premises op databaseniveau mogelijkheden. | Ondersteunt bijna alle on-premises mogelijkheden op exemplaarniveau en op databaseniveau. | Biedt ondersteuning voor alle on-premises mogelijkheden. |
| **Bronnen:** | U wilt geen IT-resources voor configuratie en beheer van de onderliggende infrastructuur implementeren, maar u wilt zich richten op het niveau van de toepassing. | Hetzelfde als de SQL-Database. | U hebt een aantal IT-resources voor configuratie en beheer. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. |
| **Totale eigendomskosten:** | Elimineert hardwarekosten en verlaagt administratieve kosten. | Hetzelfde als de SQL-Database. | Elimineert hardwarekosten. |
| **Bedrijfscontinuïteit:** |Naast [ingebouwde infrastructuurmogelijkheden voor fouttolerantie](sql-database-high-availability.md), Azure SQL Database biedt functies, zoals [geautomatiseerde back-ups](sql-database-automated-backups.md), [Point-In-Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore), [geo-herstel](sql-database-recovery-using-backups.md#geo-restore), en [failover-groepen en actieve geo-replicatie](sql-database-geo-replication-overview.md) om bedrijfscontinuïteit te verhogen. Zie voor meer informatie [SQL Database business continuity overview](sql-database-business-continuity.md). | Hetzelfde als de SQL-Database, plus de gebruiker geïnitieerde, kopie-alleen back-ups zijn beschikbaar. | Met SQL Server op Azure Virtual Machines kunt u een oplossing met hoge beschikbaarheid en herstel na een noodgeval instellen voor de specifieke behoeften van uw database. Daarmee creëert u een systeem dat is geoptimaliseerd voor uw toepassing. U kunt zelf tests en failovers uitvoeren wanneer dit nodig is. Zie voor meer informatie [High Availability and Disaster Recovery for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybride cloud:** |Uw on-premises toepassing heeft toegang tot gegevens in Azure SQL Database. | [Implementatie van systeemeigen virtuele netwerk](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration) en verbinding met uw on-premises-omgeving met behulp van Azure Express Route- of VPN-Gateway. | Met SQL Server op Azure Virtual Machines krijgt u de toepassingen die deels in de cloud draaien en deels on-premises. Zo kunt u uw on-premises netwerk en Active Directory-domein uitbreiden naar de cloud met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Bovendien kunt u on-premises gegevensbestanden opslaan in Azure Storage met behulp van [SQL Server-gegevensbestanden in Azure](https://msdn.microsoft.com/library/dn385720.aspx). Zie voor meer informatie [Inleiding voor SQL Server 2014 hybride cloud](https://msdn.microsoft.com/library/dn606154.aspx). |
|  | Ondersteunt [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee om gegevens te repliceren. | Replicatie wordt niet ondersteund voor Azure SQL Database Managed Instance. | Biedt volledige ondersteuning voor [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [Always On Availability Groups](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services en upfunctie om gegevens te repliceren. Ook worden traditionele SQL Server-back-ups volledig ondersteund. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke redenen om Azure SQL Database of SQL Server op Azure Virtual Machines te kiezen

### <a name="cost"></a>Kosten

Of u nu een startup bent die geld nodig heeft of een team in een goedlopend bedrijf met een krap budget, beperkte middelen zijn vaak de belangrijkste reden bij het bepalen hoe u uw databases wilt hosten. In dit deel kunt u lezen over de grondbeginselen van facturering en licenties in Azure met betrekking tot deze twee opties voor relationele databases: SQL Database en SQL Server op Azure Virtual Machines. Ook leest u over hoe u de totale kosten voor de toepassing berekent.

#### <a name="billing-and-licensing-basics"></a>Grondbeginselen facturering en licenties

Op dit moment **SQL-Database** wordt verkocht als een service en is beschikbaar in verschillende Servicelagen met andere prijzen voor resources, die allemaal worden per uur gefactureerd tegen een vast tarief op basis van de servicelaag en compute-grootte die u kiest. Met SQL Database Managed Instance kunt u ook uw eigen licentie brengen. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/). U kunt dynamisch aanpassen van de service-lagen en compute-grootten zodat deze overeenkomt met de uiteenlopende doorvoerbehoeften van uw toepassing. Voor de meest recente informatie over de huidige ondersteunde servicecategorieën, Zie [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). U kunt ook maken [elastische pools](sql-database-elastic-pool.md) pieken voor het delen van resources met database-exemplaren te verlagen en geschikt voor gebruik.

Met **SQL Database** wordt de databasesoftware automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor uw beheerkosten worden verlaagd. Bovendien kunt u met de [ingebouwde back-up](sql-database-automated-backups.md)mogelijkheden aanzienlijk op kosten besparen, vooral wanneer u een groot aantal databases hebt.

Met **SQL Server op Azure Virtual Machines** kunt u alle door het platform geleverde SQL Server-installatiekopieën (met een licentie) gebruiken of uw SQL Server-licentie meenemen. Alle ondersteunde SQL Server-versies (2008R2, 2012, 2014, 2016) en edities (Developer, Express, Web, Standard, Enterprise) zijn beschikbaar. Bovendien zijn er Bring Your Own License-versies (BYOL) van de installatiekopieën beschikbaar. Wanneer u de door Azure geleverde installatiekopieën gebruikt, zijn de operationele kosten afhankelijk van de VM-grootte en de versie van SQL Server die u kiest. U betaalt het licentiekosten per minuut van SQL Server en de Windows- of Linux-Server, samen met de Azure Storage-kosten voor de VM-schijven, ongeacht de VM-grootte of editie van SQL Server. Met de optie factureren per minuut kunt u SQL Server blijven gebruiken zonder aanvullende SQL Server-licenties te kopen. Als u uw eigen SQL Server-licentie naar Azure meenemen, in rekening gebracht voor de server en alleen de kosten voor de opslag. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

#### <a name="calculating-the-total-application-cost"></a>De totale kosten voor de toepassing berekenen

Wanneer u met een cloudplatform begint, worden de kosten van het uitvoeren van uw toepassing omvat de kosten voor ontwikkeling van nieuwe en beheerkosten, plus de kosten voor de openbare cloud platform-service.

**Als u Azure SQL Database gebruikt:**

- Geminimaliseerde beheerkosten
- Beperkte verlagen voor gemigreerde toepassingen
- Servicekosten voor SQL-Database
- Geen kosten voor het aanschaffen van hardware

**Als u SQL Server op VM’s van Azure gebruikt:**

- Hogere beheerkosten voor het
- Beperkt tot er geen ontwikkelingskosten voor gemigreerde toepassingen
- Servicekosten voor virtuele Machine
- SQL Server-licentie kosten
- Geen kosten voor het aanschaffen van hardware

Zie de volgende bronnen voor meer informatie over prijzen:

- [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
- [Prijzen virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure prijscalculator](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Beheer

Veel bedrijven besluiten over te stappen naar een cloudservice zowel vanwege offloading van beheercomplexiteit als de kosten. Met IaaS en PaaS, Microsoft worden beheerd door de onderliggende infrastructuur en automatisch worden gerepliceerd van alle gegevens voor herstel na noodgevallen, configureert en de databasesoftware upgrades, beheert taakverdeling en voert transparante failover uit als er een Serverfout binnen een datacenter.

- Met **Azure SQL Database**, u kunt doorgaan met het beheren van uw database, maar u niet meer nodig hebt voor het beheren van de database-engine, het server-besturingssysteem of de hardware.  Voorbeelden van items die u kunt blijven beheren zijn onder meer databases en aanmeldingen, index en query afstemmen, en controle en beveiliging.  Bovendien vereist configureren van hoge beschikbaarheid naar een ander datacenter minimale configuratie en beheer.
- Met **SQL Server op Azure Virtual Machines** hebt u volledige controle over het besturingssysteem en de configuratie van het SQL Server-exemplaar. Met een VM kunt u zelf bepalen wanneer een upgrade van het besturingssysteem en de databasesoftware nodig is en wanneer u aanvullende software zoals antivirusprogramma's moet installeren. Sommige geautomatiseerde functies worden geleverd om patchen, het maken van een back-up en hoge beschikbaarheid aanzienlijk te vereenvoudigen. Bovendien kunt u de grootte van de VM, het aantal schijven en hun opslagconfiguraties beheren. Met Azure kunt de grootte van een VM indien nodig wijzigen. Zie voor meer informatie [Virtual Machine en Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)

Voor veel IT-afdelingen is het voldoen aan uptimeverplichtingen van een Service Level Agreement (SLA) een topprioriteit. In dit gedeelte kijken we welke SLA van toepassing is op de databasehostingopties.

Voor **SQL-Database**, biedt Microsoft een beschikbaarheids-SLA van 99,99%. Zie [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/) voor de meest recente informatie.

Voor **SQL Server die wordt uitgevoerd op Azure Virtual Machines**, biedt Microsoft een beschikbaarheids-SLA van 99,95% die alleen van toepassing is op de virtuele machine. Deze SLA heeft geen betrekking op de processen (zoals SQL Server) die worden uitgevoerd op de VM en vereist dat u ten minste twee exemplaren van de virtuele machine in een beschikbaarheidsset host. Voor de meest recente informatie, zie de [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Voor database hoge beschikbaarheid (HA) in virtuele machines, moet u configureren een van de opties voor ondersteunde hoge beschikbaarheid in SQL Server, zoals [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Het gebruik van een ondersteunde optie voor hoge beschikbaarheid biedt geen extra SLA, maar u kunt wel een databasebeschikbaarheid van >99,99% behalen.

### <a name="market"></a>Verplaatsen naar Azure

**Logische SQL Database-servers, elastische pools en individuele databases** is de juiste oplossing voor de cloud ontworpen toepassingen wanneer de productiviteit van ontwikkelaars en snelle time-to-market voor nieuwe oplossingen van essentieel belang zijn. Met programmatische DBA-functies is het ideaal voor cloud-architecten en -ontwikkelaars aangezien het de noodzaak voor het beheren van het onderliggende besturingssysteem en de database vermindert.

**SQL Database Managed Instance** vereenvoudigt de migratie van bestaande toepassingen naar Azure SQL Database, zodat u kunt de gemigreerde database-toepassingen op de markt kunnen in Azure.

**Met SQL Server op Azure Virtual machines** is ideaal als uw bestaande of nieuwe toepassingen grote databases is vereist of toegang tot alle functies in SQL Server of Windows/Linux, en u wilt voorkomen dat de tijd en kosten van het ophalen van nieuwe on-premises hardware. Het is ook geschikt wanneer u wilt voor het migreren van bestaande on-premises toepassingen en databases naar Azure als-is - in gevallen waarin Azure SQL Database Managed Instance niet geschikt is. Omdat u niet wijzigen van de presentatie-, toepassings- en gegevenslagen wilt, bespaart u tijd en budget op uw bestaande oplossing opnieuw te ontwerpen. In plaats daarvan kunt u zich richten op het migreren van uw oplossingen naar Azure en het uitvoeren van prestatieoptimalisatie die mogelijk wordt vereist door het Azure-platform. Zie voor meer informatie [Best practices prestaties for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [Uw eerste Azure SQL Database](sql-database-get-started-portal.md) om aan de slag te gaan met SQL Database.
- Zie [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Zie [Provision a SQL Server virtual machine in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) om aan de slag te kunnen met SQL Server op Azure VM’s.
