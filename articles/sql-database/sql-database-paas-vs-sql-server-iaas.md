---
title: SQL (PaaS) Database vs. SQL Server in de cloud op VM’s (IaaS) | Microsoft Docs
description: 'Informatie over welke SQL Server-cloudoptie geschikt is voor uw toepassing: Azure SQL (PaaS) Database of SQL Server in de cloud op Azure Virtual Machines.'
services: sql-database, virtual-machines
keywords: SQL Server-cloud, SQL Server in de cloud, PaaS-database, SQL-cloudserver, DBaaS
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: a212a595c02a048721f1a8753b437f74f2fc4973
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308951"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Kies een SQL Server-cloudoptie: Azure SQL (PaaS) Database of SQL Server op Azure Virtual Machines (IaaS)

In Azure, hebt u uw SQL Server-workloads uitgevoerd in een gehoste infrastructuur (IaaS) of wordt uitgevoerd als een gehoste service ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): een SQL database-engine, op basis van de Enterprise-editie van SQL Server, die is geoptimaliseerd voor ontwikkeling van moderne toepassingen. Azure SQL Database biedt twee versies van SQL als een gehoste service: logische servers en [Azure SQL Database beheerd-exemplaren (preview)](sql-database-managed-instance.md). Met beide versies voegt Azure SQL Database aanvullende functies die niet beschikbaar in SQL Server, zoals ingebouwde intelligentie en beheer zijn. Met de eerste versie, hebt u een logische server met [databases eenmalige](sql-database-servers-databases.md) en u kunt groeperen servers in een [elastische pool](sql-database-elastic-pool.md) resources delen en de kosten te verlagen. Een logische Azure SQL Database-server met één en gegroepeerde databases biedt de meeste functies van SQL Server database-scoped. Met Azure SQL Database beheerd-exemplaar biedt Azure SQL Database gedeelde bronnen voor databases en extra functies exemplaar binnen het bereik. Azure SQL Database beheerd-instantie ondersteunt de databasemigratie met minimale naar geen wijziging van de database.
* [SQL Server op Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server geïnstalleerd en wordt gehost in de cloud op Windows Server of Linux virtuele machines (VM's) uitgevoerd op Azure, ook wel bekend als een infrastructuur als een service (IaaS). SQL Server op virtuele machines in Azure is een goede optie voor het migreren van on-premises SQL Server-databases en toepassingen zonder wijziging van de database. Alle recente versies en edities van SQL Server zijn beschikbaar voor installatie in een virtuele machine voor IaaS. Het belangrijkste verschil uit SQL-Database is dat SQL Server-VM's volledige controle over de database-engine toestaan. U kunt kiezen wanneer onderhoud/patchen wordt gestart, het herstelmodel in simple of bulksgewijs geregistreerde zodat sneller load minder logboek, onderbreken of starten van engine, indien nodig wijzigen en kunt u de SQL Server database engine volledig aanpassen. Met deze extra controles wordt geleverd met toegevoegde verantwoordelijkheid voor het beheren van de virtuele machines.

Ontdek hoe elke optie in het Microsoft-gegevensplatform past en krijg hulp bij het vinden van de juiste optie voor uw zakelijke vereisten. Of u nu kostenbesparingen of minimaal beheer het belangrijkst vindt, met dit artikel kunt u beslissen welke benadering de bedrijfsvereisten levert die u het belangrijkst vindt.

## <a name="microsofts-sql-data-platform"></a>SQL-gegevensplatform van Microsoft

Een van de eerste belangrijke punten in een discussie over Azure ten opzichte van on-premises SQL Server-databases is dat u alles kunt gebruiken. Het gegevensplatform van Microsoft maakt gebruik van SQL Server-technologie en maakt deze beschikbaar op fysieke on-premises machines, privécloudomgevingen, externe gehoste privécloudomgevingen en openbare cloud. Met SQL Server op virtuele Azure-machines kunt u voldoen aan de unieke en diverse bedrijfsbehoeften met een combinatie van on-premises en in de cloud gehoste implementaties terwijl dezelfde set serverproducten, ontwikkelingsprogramma's en kennis wordt gebruikt in deze omgevingen.

   ![Opties SQL Server cloud: SQL server op IaaS of SaaS-SQL Database in de cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Zoals we zien in het diagram, wordt elke aanbieding gekenmerkt door het niveau van beheer dat u hebt over de infrastructuur (op de X-as) en de mate van kostenefficiëntie die wordt bereikt door consolidatie en automatisering op databaseniveau (op de Y-as).

Bij het ontwerpen van een toepassing zijn vier eenvoudige opties beschikbaar voor het hosten van het SQL Server-onderdeel van de toepassing:

* SQL Server op niet-gevirtualiseerde fysieke computers
* SQL Server in on-premises gevirtualiseerde machines (privécloud)
* SQL Server in virtuele Azure-machines (openbare cloud van Microsoft)
* Azure SQL-database (openbare cloud van Microsoft)

In de volgende gedeelten vindt u meer informatie over SQL Server in de openbare cloud van Microsoft: Azure SQL Database en SQL Server op Azure Virtual Machines. Bovendien verkent u algemene zakelijke motivators om te bepalen welke optie het meest geschikt is voor uw toepassing.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>De Azure SQL Database en SQL Server op Azure Virtual Machines

**Azure SQL Database** is een relationele database-as-a-service (DBaaS) die worden gehost in de Azure-cloud die in de branchecategorie van vallen *Platform-as-a-Service (PaaS)*. [SQL-database](sql-database-technical-overview.md) is gebouwd op gestandaardiseerde hardware en software die eigendom is van, en wordt gehost en beheerd door Microsoft. Met SQL-Database, kunt u de ingebouwde functies en functionaliteiten waarvoor uitgebreide configuration in SQL Server gebruiken. Als u SQL Database gebruikt, betaalt u per gebruik met opties voor het omhoog schalen voor meer vermogen zonder onderbrekingen. Azure SQL Database, met ondersteuning voor beide [databases eenmalige](sql-database-servers-databases.md) en [elastische pools](sql-database-elastic-pool.md) voor het delen van resources is een ideale omgeving voor het ontwikkelen van nieuwe toepassingen in de cloud. En met [Azure SQL Database-beheerd instantie](sql-database-managed-instance.md), kunt u uw eigen licentie brengt. Daarnaast is deze optie biedt alle voordelen van Azure SQL Database PaaS maar mogelijkheden die eerder alleen beschikbaar in SQL-VM's waren toevoegt. Dit omvat een systeemeigen virtueel netwerk (VNet) en in de buurt van 100% compatibel met on-premises SQL-Server. [Beheerde exemplaar](sql-database-managed-instance.md) is ideaal voor de lokale database migraties naar Azure met minimale wijzigingen vereist. 

**SQL Server op Azure Virtual Machines** valt in de branchecategorie *infrastructuur als een service (IaaS)* en stelt u in staat SQL Server uit te voeren binnen een virtuele machine in de cloud. [Virtuele machines van SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) ook uitvoeren op gestandaardiseerde hardware die eigendom is, gehost en beheerd door Microsoft. Wanneer u SQL Server op een virtuele machine, kunt u een betalen-als u overal voor een SQL Server-licentie al opgenomen in een installatiekopie van SQL Server of een bestaande licentie eenvoudig te gebruiken. U kunt ook stoppen of hervatten van de virtuele machine, indien nodig.

Over het algemeen zijn deze twee SQL-opties geoptimaliseerd voor verschillende doeleinden:

* **Azure SQL Database** is geoptimaliseerd voor de totale beheerkosten verminderen tot het minimum voor het inrichten en beheren van veel databases. Dit vermindert beheerkosten omdat u geen virtuele machines, besturingssysteem of database-software hoeft te beheren. U hoeft upgrades, hoge beschikbaarheid of [back-ups](sql-database-automated-backups.md) niet te beheren. Over het algemeen kan Azure SQL Database het aantal databases dat wordt beheerd door één IT of ontwikkelingsresource aanzienlijk verhogen. [Elastische pools](sql-database-elastic-pool.md) bieden ook ondersteuning voor SaaS multitenant toepassingsarchitecturen met functies zoals isolatie van tenants en de mogelijkheid om te schalen om kosten te verlagen door het delen van bronnen tussen databases. [Azure SQL Database beheerd-instantie](sql-database-managed-instance.md) biedt ondersteuning voor functies inschakelen van eenvoudige migratie van bestaande toepassingen, evenals het delen van resources voor de databases exemplaar binnen het bereik.
* **SQL Server die wordt uitgevoerd op Azure Virtual Machines** is geoptimaliseerd om bestaande toepassingen te migreren naar Azure of bestaande on-premises-toepassingen uit te breiden naar de cloud in hybride implementaties. Bovendien kunt u SQL Server op een virtuele machine gebruiken om traditionele SQL Server-toepassingen te ontwikkelen en te testen. Met SQL Server op Azure Virtual Machines hebt u de volledige beheerdersrechten van een speciaal exemplaar van SQL Server en een cloud-gebaseerde VM. Het is een ideale keuze als een organisatie al IT-bronnen beschikbaar heeft voor het onderhouden van de virtuele machines. Met deze mogelijkheden kunt u een aangepast systeem bouwen voor de specifieke prestaties en beschikbaarheidsvereisten van uw toepassing.

De volgende tabel geeft een overzicht van de belangrijkste kenmerken van de SQL Database en SQL Server op Azure Virtual Machines:

| | Azure SQL Database<br>Logische servers elastische pools en individuele databases | Azure SQL Database<br>Beheerd exemplaar |Azure Virtual Machine<br>SQL Server |
| --- | --- | --- |---|
| **Ideaal voor:** |Nieuwe cloud ontworpen toepassingen die u wilt gebruiken voor de meest recente stabiele SQL Server-functies andhave tijdsbeperkingen in ontwikkeling en marketing. | Nieuwe toepassingen of bestaande on-premises toepassingen die u wilt gebruiken voor de meest recente stabiele SQL Server-onderdelen en die worden gemigreerd naar de cloud met minimale wijzigingen worden aangebracht.  | Bestaande toepassingen waarvoor snelle migratie naar de cloud met minimale wijzigingen worden aangebracht of er zijn geen wijzigingen vereist. Snelle ontwikkelings- en testscenario's als u geen on-premises SQL Server-hardware wilt kopen die niet bestemd is voor productie. |
|  | Teams die ingebouwde hoge beschikbaarheid, herstel na noodgevallen en upgrademogelijkheden van de database nodig hebben. | Hetzelfde als de SQL-Database. | Teams die kunnen configureren, te optimaliseren, aanpassen en hoge beschikbaarheid, herstel na noodgevallen en patchen voor SQL Server beheren. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. | |
|  | Teams die het onderliggende besturingssysteem en configuratie-instellingen niet willen beheren. | Hetzelfde als de SQL-Database. | U moet een aangepaste omgeving met volledige beheerdersrechten. | |
|  | Databases van maximaal 4 TB of meer databases die kunnen worden [horizontaal of verticaal gepartitioneerd](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) met behulp van een scale-out-patroon. | Hetzelfde als de SQL-Database. | SQL Server-exemplaren met maximaal 64 TB aan opslag. Het exemplaar kan zoveel databases ondersteunen als nodig is. |
| **Compatibiliteit** | Ondersteunt de meeste on-premises databaseniveau mogelijkheden. | Ondersteunt bijna alle on-premises mogelijkheden instantieniveau en databaseniveau. | Ondersteunt alle on-premises mogelijkheden. |
| **Bronnen:** | U wilt geen IT-resources voor configuratie en beheer van de onderliggende infrastructuur implementeren, maar u wilt zich richten op de toepassingslaag. | Hetzelfde als de SQL-Database. | U hebt een aantal IT-resources voor configuratie en beheer. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. |
| **Totale eigendomskosten:** | Elimineert hardwarekosten en verlaagt administratieve kosten. | Hetzelfde als de SQL-Database. | Elimineert hardwarekosten. |
| **Bedrijfscontinuïteit:** |Naast [mogelijkheden voor infrastructuur ingebouwde fouttolerantie](sql-database-high-availability.md), Azure SQL Database biedt de functies, zoals [geautomatiseerde back-ups](sql-database-automated-backups.md), [punt In tijd terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore), [geo-restore](sql-database-recovery-using-backups.md#geo-restore), en [failover groepen en actieve geo-replicatie](sql-database-geo-replication-overview.md) zakelijke continuïteit te verhogen. Zie voor meer informatie [SQL Database business continuity overview](sql-database-business-continuity.md). | Hetzelfde als de SQL-Database, plus de gebruiker wordt gestart, kopie-alleen back-ups zijn beschikbaar. | Met SQL Server op Azure Virtual Machines kunt u een oplossing met hoge beschikbaarheid en herstel na een noodgeval instellen voor de specifieke behoeften van uw database. Daarmee creëert u een systeem dat is geoptimaliseerd voor uw toepassing. U kunt zelf tests en failovers uitvoeren wanneer dit nodig is. Zie voor meer informatie [High Availability and Disaster Recovery for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybride cloud:** |Uw on-premises toepassing heeft toegang tot gegevens in Azure SQL Database. | [Implementatie van systeemeigen virtueel netwerk](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-vnet-configuration) en de verbinding met uw on-premises omgeving met behulp van Azure Express Route- of VPN-Gateway. | Met SQL Server op Azure Virtual Machines krijgt u de toepassingen die deels in de cloud draaien en deels on-premises. Zo kunt u uw on-premises netwerk en Active Directory-domein uitbreiden naar de cloud met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Bovendien kunt u on-premises gegevensbestanden opslaan in Azure Storage met behulp van [SQL Server-gegevensbestanden in Azure](http://msdn.microsoft.com/library/dn385720.aspx). Zie voor meer informatie [Inleiding voor SQL Server 2014 hybride cloud](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Ondersteunt [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee om gegevens te repliceren. | Replicatie wordt niet ondersteund voor Azure SQL Database-beheerde exemplaar. | Volledige ondersteuning voor [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [altijd op beschikbaarheidsgroepen](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services en logboekverzending om gegevens te repliceren. Ook worden traditionele SQL Server-back-ups volledig ondersteund. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke redenen om Azure SQL Database of SQL Server op Azure Virtual Machines te kiezen
### <a name="cost"></a>Kosten
Of u nu een startup bent die geld nodig heeft of een team in een goedlopend bedrijf met een krap budget, beperkte middelen zijn vaak de belangrijkste reden bij het bepalen hoe u uw databases wilt hosten. In dit deel kunt u lezen over de grondbeginselen van facturering en licenties in Azure met betrekking tot deze twee opties voor relationele databases: SQL Database en SQL Server op Azure Virtual Machines. Ook leest u over hoe u de totale kosten voor de toepassing berekent.

#### <a name="billing-and-licensing-basics"></a>Grondbeginselen facturering en licenties

Op dit moment **SQL-Database** als een service wordt verkocht en is beschikbaar in verschillende Servicelagen met verschillende prijzen voor resources, die allemaal per uur worden gefactureerd tegen een vast tarief op basis van de prijscategorie en prestatieniveau serviceniveau u kiest. U kunt ook uw eigen licentie overbrengen met exemplaar van SQL Database-beheerd. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/). U kunt dynamisch Servicelagen en prestatieniveaus overeenkomen met de uiteenlopende doorvoerbehoeften van uw toepassing aanpassen. Voor de meest recente informatie over de huidige ondersteunde servicecategorieën, Zie [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md). U kunt ook maken [elastische pools](sql-database-elastic-pool.md) bereikt voor het delen van bronnen tussen de exemplaren van de database worden de kosten te verlagen en geschikt voor gebruik.

Met **SQL Database** wordt de databasesoftware automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor uw beheerkosten worden verlaagd. Bovendien kunt u met de [ingebouwde back-up](sql-database-automated-backups.md)mogelijkheden aanzienlijk op kosten besparen, vooral wanneer u een groot aantal databases hebt. 

Met **SQL Server op Azure Virtual Machines** kunt u alle door het platform geleverde SQL Server-installatiekopieën (met een licentie) gebruiken of uw SQL Server-licentie meenemen. Alle ondersteunde SQL Server-versies (2008R2, 2012, 2014, 2016) en edities (Developer, Express, Web, Standard, Enterprise) zijn beschikbaar. Bovendien zijn er Bring Your Own License-versies (BYOL) van de installatiekopieën beschikbaar. Wanneer u de door Azure geleverde installatiekopieën gebruikt, zijn de operationele kosten afhankelijk van de VM-grootte en de versie van SQL Server die u kiest. Ongeacht de VM-grootte of editie van SQL Server betaalt u per minuut licentieverlening kosten van SQL Server en de Windows- of Linux-Server samen met de Azure Storage-kosten voor de VM-schijven. Met de optie factureren per minuut kunt u SQL Server blijven gebruiken zonder aanvullende SQL Server-licenties te kopen. Als u uw eigen SQL Server-licentie naar Azure meenemen, worden in rekening gebracht voor server en alleen kosten voor opslag. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

#### <a name="calculating-the-total-application-cost"></a>De totale kosten voor de toepassing berekenen
Wanneer u met een cloudplatform begint, wordt de kosten voor het uitvoeren van uw toepassing bevat de kosten voor ontwikkeling van nieuwe en beheerkosten, plus de servicekosten voor openbare cloud-platform.

**Als u Azure SQL Database gebruikt:**

- Geminimaliseerde beheerkosten
- Ontwikkelingskosten voor gemigreerde toepassingen beperkt
- Servicekosten voor SQL-Database
- Er zijn geen kosten voor hardware aanschaffen

**Als u SQL Server op VM’s van Azure gebruikt:**

- Hogere beheerkosten
- Beperkt tot geen ontwikkelingskosten voor gemigreerde toepassingen
- Virtuele Machine servicekosten
- SQL Server-licentie kosten
- Er zijn geen kosten voor hardware aanschaffen

Zie de volgende bronnen voor meer informatie over prijzen:

* [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
* [Prijzen virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Azure prijscalculator](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Beheer
Veel bedrijven besluiten over te stappen naar een cloudservice zowel vanwege offloading van beheercomplexiteit als de kosten. Met IaaS en PaaS Microsoft worden beheerd door de onderliggende infrastructuur en automatisch alle gegevens voor herstel na noodgevallen repliceert, configureert en upgrades van de database-software, beheert taakverdeling en voert transparante failover uit als er een Serverfout binnen een datacenter. 

- Met **Azure SQL Database**, kunt u doorgaan met het beheren van uw database, maar u niet langer hoeft te beheren van de database-engine, server-besturingssysteem of hardware.  Voorbeelden van items die u kunt blijven beheren zijn onder meer databases en aanmeldingen, index en query afstemmen, en controle en beveiliging.  Daarnaast vereist configureert hoge beschikbaarheid naar een ander datacenter minimale configuratie en beheer.
- Met **SQL Server op Azure Virtual Machines** hebt u volledige controle over het besturingssysteem en de configuratie van het SQL Server-exemplaar. Met een VM kunt u zelf bepalen wanneer een upgrade van het besturingssysteem en de databasesoftware nodig is en wanneer u aanvullende software zoals antivirusprogramma's moet installeren. Sommige geautomatiseerde functies worden geleverd om patchen, het maken van een back-up en hoge beschikbaarheid aanzienlijk te vereenvoudigen. Bovendien kunt u de grootte van de VM, het aantal schijven en hun opslagconfiguraties beheren. Met Azure kunt de grootte van een VM indien nodig wijzigen. Zie voor meer informatie [Virtual Machine en Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)
Voor veel IT-afdelingen is het voldoen aan uptimeverplichtingen van een Service Level Agreement (SLA) een topprioriteit. In dit gedeelte kijken we welke SLA van toepassing is op de databasehostingopties.

Voor **SQL-Database**, biedt Microsoft een beschikbaarheids-SLA van 99,99%. Zie [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/) voor de meest recente informatie. 

Voor **SQL Server die wordt uitgevoerd op Azure Virtual Machines**, biedt Microsoft een beschikbaarheids-SLA van 99,95% die alleen van toepassing is op de virtuele machine. Deze SLA heeft geen betrekking op de processen (zoals SQL Server) die worden uitgevoerd op de VM en vereist dat u ten minste twee exemplaren van de virtuele machine in een beschikbaarheidsset host. Voor de meest recente informatie, zie de [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Voor database hoge beschikbaarheid (HA) vanuit virtuele machines, configureer een van de opties voor hoge beschikbaarheid ondersteund in SQL Server, zoals [altijd op beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Het gebruik van een ondersteunde optie voor hoge beschikbaarheid biedt geen extra SLA, maar u kunt wel een databasebeschikbaarheid van >99,99% behalen.

### <a name="market"></a>Bij het verplaatsen naar Azure
**Logische SQL Database-servers, elastische pools en individuele databases** is de juiste oplossing voor de cloud ontworpen toepassingen wanneer productiviteit van ontwikkelaars en snelle tijd op de markt voor noew oplossingen essentieel zijn. Met programmatische DBA-functies is het ideaal voor cloud-architecten en -ontwikkelaars aangezien het de noodzaak voor het beheren van het onderliggende besturingssysteem en de database vermindert. 

**Beheerde exemplaar van SQL Database** vereenvoudigt de migratie van bestaande toepassingen met Azure SQL Database, zodat u kunt een gemigreerde database toepassingen op de markt in Azure snel te brengen.

**SQL Server wordt uitgevoerd op Azure Virtual machines** is ideaal als uw bestaande of nieuwe toepassingen vereisen grote databases of toegang tot alle functies in SQL Server of Windows of Linux, en u voorkomen dat aan de tijd en kosten wilt van het ophalen van nieuwe hardware van de on-premises. Het is ook geschikt wanneer u wilt dat voor het migreren van bestaande on-premises toepassingen en databases naar Azure-is - in gevallen waarin Azure SQL Database-beheerd instantie niet geschikt is. Omdat u de presentatie-, toepassings- en gegevenslagen niet hoeft te wijzigen, bespaart u tijd en geld op het opnieuw vormgeven van uw architectuur. In plaats daarvan kunt u zich richten op het migreren van uw oplossingen naar Azure en het uitvoeren van prestatieoptimalisatie die mogelijk wordt vereist door het Azure-platform. Zie voor meer informatie [Best practices prestaties for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Samenvatting
In dit artikel werden SQL Database en SQL Server op Azure Virtual Machines (VM's) verkend en algemene zakelijke motivators besproken die invloed kunnen hebben op uw beslissing. Hier volgt een samenvatting met enkele suggesties:

Kies voor een **Azure SQL Database** als:

* U ontwikkelt nieuwe cloudgebaseerde toepassingen om te profiteren van de kostenbesparingen en optimalisatie van de prestaties die cloudservices bieden. Deze aanpak biedt de voordelen van een volledig beheerde cloudservice, helpt de implementatietijd te verlagen en kan zorgen voor kostenoptimalisatie op de lange termijn.
* U wilt dat Microsoft algemene managementbewerkingen uitvoert op uw databases en vereist betere beschikbaarheids-SLA's voor databases.
* U wilt migreren van een bestaande toepassing als-is naar Azure SQL Database-beheerde exemplaar en te profiteren van extra pariteit met SQL Server en/of geavanceerde beveiliging en netwerken. Beheerde exemplaar is een goede keuze voor nieuwe en bestaande toepassingen.

Kies voor **SQL Server op Azure VM's** als:

* U hebt bestaande on-premises toepassingen die u wilt migreren of uitbreiden naar de cloud, of als u wilt maken van bedrijfstoepassingen groter is dan 4 TB. Deze aanpak geeft u het voordeel van het gebruik van de SQL Server-versie en editie van uw keuze, de capaciteit van de grote database, de volledige controle over SQL Server en Windows of Linux en secure tunneling met on-premises. Deze aanpak minimaliseert de kosten voor ontwikkeling en wijzigingen van bestaande toepassingen.
* U hebt bestaande IT-resources en kunt eigenaar zijn van patchen, back-ups en hoge beschikbaarheid van databases. Houd er rekening mee dat sommige geautomatiseerde functies deze bewerkingen aanzienlijk kunnen vereenvoudigen. 

## <a name="next-steps"></a>Volgende stappen

* Zie [Uw eerste Azure SQL Database](sql-database-get-started-portal.md) om aan de slag te gaan met SQL Database.
* Zie [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Zie [Provision a SQL Server virtual machine in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) om aan de slag te kunnen met SQL Server op Azure VM’s.