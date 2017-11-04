---
title: "SQL (PaaS) Database vs. SQL Server in de cloud op VM’s (IaaS) | Microsoft Docs"
description: 'Informatie over welke SQL Server-cloudoptie geschikt is voor uw toepassing: Azure SQL (PaaS) Database of SQL Server in de cloud op Azure Virtual Machines.'
services: sql-database, virtual-machines
keywords: SQL Server-cloud, SQL Server in de cloud, PaaS-database, SQL-cloudserver, DBaaS
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cjgronlund
ms.assetid: 7467f422-b77d-4b60-9cb5-0f1ec17ec565
ms.service: sql-database
ms.custom: DBs & servers
ms.workload: Active
ms.tgt_pltfrm: vm-windows-sql-server
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: carlrab
ms.openlocfilehash: 436166fcb0fa9103c6b702b63d93a0b222d536d0
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Kies een SQL Server-cloudoptie: Azure SQL (PaaS) Database of SQL Server op Azure Virtual Machines (IaaS)
Azure heeft twee opties om SQL Server-workloads te hosten in Microsoft Azure:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): een systeemeigen SQL Database in de cloud, ook wel bekend als een platform als een servicedatabase (PaaS) of als een database als een service (DBaaS) die is geoptimaliseerd voor ontwikkeling van apps voor software als een service (SaaS). Biedt compatibiliteit met de meeste functies van SQL Server. Zie [Wat is PaaS](https://azure.microsoft.com/overview/what-is-paas/) voor meer informatie over PaaS.
* [SQL Server op Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server die is geïnstalleerd en wordt gehost in de cloud op Windows Server Virtual Machines (VM's) die worden uitgevoerd op Azure, ook wel bekend als een infrastructuur als een service (IaaS).
  SQL Server op Azure Virtual Machines is geoptimaliseerd om bestaande SQL Server-toepassingen te migreren. Alle versies en edities van SQL Server zijn beschikbaar. Biedt 100% compatibiliteit met SQL Server. Hierdoor kunt u zoveel databases als nodig hosten en meerdere databasetransacties uitvoeren. Biedt volledig beheer voor SQL Server en Windows.

Ontdek hoe elke optie in het Microsoft-gegevensplatform past en krijg hulp bij het vinden van de juiste optie voor uw zakelijke vereisten. Of u nu kostenbesparingen of minimaal beheer het belangrijkst vindt, met dit artikel kunt u beslissen welke benadering de bedrijfsvereisten levert die u het belangrijkst vindt.

## <a name="microsofts-data-platform"></a>Gegevensplatform van Microsoft
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
**Azure SQL Database** is een relationele database-as-a-service (DBaaS) die wordt gehost in de Azure-cloud en die in de branchecategorie van *Software-as-a-Service (SaaS)* en *Platform-as-a-Service (PaaS)* valt. [SQL-database](sql-database-technical-overview.md) is gebouwd op gestandaardiseerde hardware en software die eigendom is van, en wordt gehost en beheerd door Microsoft. Met SQL Database kunt u rechtstreeks op de service ontwikkelen met behulp van de ingebouwde functies en functionaliteiten. Als u SQL Database gebruikt, betaalt u per gebruik met opties voor het omhoog schalen voor meer vermogen zonder onderbrekingen.

**SQL Server op Azure Virtual Machines** valt in de branchecategorie *infrastructuur als een service (IaaS)* en stelt u in staat SQL Server uit te voeren binnen een virtuele machine in de cloud. Net als bij SQL Database is deze gebouwd op gestandaardiseerde hardware die eigendom is van en die wordt gehost en beheerd door Microsoft. Wanneer u SQL Server op een virtuele machine gebruikt, kunt u eenvoudig een bestaande licentie gebruiken, of kunt u een SQL Server-licentie gebruiken die al is opgenomen in een installatiekopie van SQL Server en alleen betalen voor wanneer u deze gebruikt. U kunt de virtuele machine ook gemakkelijk naar behoefte omhoog/omlaag schalen en onderbreken/hervatten.

Over het algemeen zijn deze twee SQL-opties geoptimaliseerd voor verschillende doeleinden:

* **Azure SQL Database** is geoptimaliseerd voor het minimaliseren van de totale kosten om veel databases in te richten en te beheren. Dit vermindert beheerkosten omdat u geen virtuele machines, besturingssysteem of database-software hoeft te beheren. U hoeft upgrades, hoge beschikbaarheid of [back-ups](sql-database-automated-backups.md) niet te beheren. Over het algemeen kan Azure SQL Database het aantal databases dat wordt beheerd door één IT of ontwikkelingsresource aanzienlijk verhogen.
* **SQL Server die wordt uitgevoerd op Azure Virtual Machines** is geoptimaliseerd om bestaande toepassingen te migreren naar Azure of bestaande on-premises-toepassingen uit te breiden naar de cloud in hybride implementaties. Bovendien kunt u SQL Server op een virtuele machine gebruiken om traditionele SQL Server-toepassingen te ontwikkelen en te testen. Met SQL Server op Azure Virtual Machines hebt u de volledige beheerdersrechten van een speciaal exemplaar van SQL Server en een cloud-gebaseerde VM. Het is een ideale keuze als een organisatie al IT-bronnen beschikbaar heeft voor het onderhouden van de virtuele machines. Met deze mogelijkheden kunt u een aangepast systeem bouwen voor de specifieke prestaties en beschikbaarheidsvereisten van uw toepassing.

De volgende tabel geeft een overzicht van de belangrijkste kenmerken van de SQL Database en SQL Server op Azure Virtual Machines:

| **Ideaal voor:** | **Azure SQL Database** | **SQL Server op een virtuele Azure-machine** |
| --- | --- | --- |
|  |Nieuwe in de cloud ontworpen toepassingen met tijdsbeperkingen op het gebied van ontwikkeling en marketing. |Bestaande toepassingen waar snelle migratie naar de cloud met minimale wijzigingen voor nodig is. Snelle ontwikkelings- en testscenario's als u geen on-premises SQL Server-hardware wilt kopen die niet bestemd is voor productie. |
|  | Teams die ingebouwde hoge beschikbaarheid, herstel na noodgevallen en upgrademogelijkheden van de database nodig hebben. |Teams die hoge beschikbaarheid, herstel na noodgevallen en patchen van SQL Server kunnen configureren en beheren. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. | |
|  | Teams die het onderliggende besturingssysteem en configuratie-instellingen niet willen beheren. |U moet een aangepaste omgeving met volledige beheerdersrechten. | |
|  | Databases van maximaal 4 TB of meer databases die kunnen worden [horizontaal of verticaal gepartitioneerd](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) met behulp van een scale-out-patroon. |SQL Server-exemplaren met maximaal 64 TB aan opslag. Het exemplaar kan zoveel databases ondersteunen als nodig is. | |
|  | | |
| **Bronnen:** |U wilt geen IT-resources voor configuratie en beheer van de onderliggende infrastructuur implementeren, maar u wilt zich richten op de toepassingslaag. |U hebt een aantal IT-resources voor configuratie en beheer. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. |
| **Totale eigendomskosten:** |Elimineert hardwarekosten en verlaagt administratieve kosten. |Elimineert hardwarekosten. |
| **Bedrijfscontinuïteit:** |Naast de mogelijkheden voor infrastructuur ingebouwde fouttolerantie, biedt Azure SQL Database-functies, zoals [geautomatiseerde back-ups](sql-database-automated-backups.md), [punt In tijd terugzetten](sql-database-recovery-using-backups.md#point-in-time-restore), [geo-restore](sql-database-recovery-using-backups.md#geo-restore), en [actieve geo-replicatie](sql-database-geo-replication-overview.md) zakelijke continuïteit te verhogen. Zie voor meer informatie [SQL Database business continuity overview](sql-database-business-continuity.md). |Met SQL Server op Azure Virtual Machines kunt u een oplossing met hoge beschikbaarheid en herstel na een noodgeval instellen voor de specifieke behoeften van uw database. Daarmee creëert u een systeem dat is geoptimaliseerd voor uw toepassing. U kunt zelf tests en failovers uitvoeren wanneer dit nodig is. Zie voor meer informatie [High Availability and Disaster Recovery for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybride cloud:** |Uw on-premises toepassing heeft toegang tot gegevens in Azure SQL Database. |Met SQL Server op Azure Virtual Machines krijgt u de toepassingen die deels in de cloud draaien en deels on-premises. Zo kunt u uw on-premises netwerk en Active Directory-domein uitbreiden naar de cloud met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Bovendien kunt u on-premises gegevensbestanden opslaan in Azure Storage met behulp van [SQL Server-gegevensbestanden in Azure](http://msdn.microsoft.com/library/dn385720.aspx). Zie voor meer informatie [Inleiding voor SQL Server 2014 hybride cloud](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Ondersteunt [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee om gegevens te repliceren. |Biedt volledige ondersteuning voor [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn Availability Groups](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), Integration Services en back-upfunctie voor logboekbestanden om gegevens te repliceren. Ook worden traditionele SQL Server-back-ups volledig ondersteund. | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke redenen om Azure SQL Database of SQL Server op Azure Virtual Machines te kiezen
### <a name="cost"></a>Kosten
Of u nu een startup bent die geld nodig heeft of een team in een goedlopend bedrijf met een krap budget, beperkte middelen zijn vaak de belangrijkste reden bij het bepalen hoe u uw databases wilt hosten. In dit deel kunt u lezen over de grondbeginselen van facturering en licenties in Azure met betrekking tot deze twee opties voor relationele databases: SQL Database en SQL Server op Azure Virtual Machines. Ook leest u over hoe u de totale kosten voor de toepassing berekent.

#### <a name="billing-and-licensing-basics"></a>Grondbeginselen facturering en licenties
**SQL Database** wordt aan klanten verkocht als een service en niet met een licentie.  [SQL Server op Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) wordt verkocht met een licentie waarvoor u per minuut betaalt. Als u een bestaande licentie hebt, kunt u deze ook gebruiken.  

Op dit moment is **SQL Database** beschikbaar in verschillende servicelagen, die allemaal per uur worden gefactureerd tegen een vast tarief op basis van de servicelaag en het prestatieniveau dat u kiest. Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/). De servicecategorieën Basic, Standard, Premium en Premium RS zijn ontworpen om voorspelbare prestaties te leveren met meerdere prestatieniveaus, zodat kan worden voldaan aan de piekvereisten van uw toepassing. U kunt wisselen tussen servicecategorieën en prestatieniveaus om te voldoen aan de uiteenlopende doorvoerbehoeften van uw toepassing. Als er veel transacties plaatsvinden in uw database en er veel gelijktijdige gebruikers ondersteund moeten worden, raden wij de Premium servicecategorie aan. Voor de meest recente informatie over de huidige ondersteunde servicecategorieën, zie [Azure SQL Database servicecategorieën](sql-database-service-tiers.md). U kunt ook [elastische pools](sql-database-elastic-pool.md) maken om resources voor prestaties te delen met database-exemplaren.

Met **SQL Database** wordt de databasesoftware automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor uw beheerkosten worden verlaagd. Bovendien kunt u met de [ingebouwde back-up](sql-database-automated-backups.md)mogelijkheden aanzienlijk op kosten besparen, vooral wanneer u een groot aantal databases hebt.

Met **SQL Server op Azure Virtual Machines** kunt u alle door het platform geleverde SQL Server-installatiekopieën (met een licentie) gebruiken of uw SQL Server-licentie meenemen. Alle ondersteunde SQL Server-versies (2008R2, 2012, 2014, 2016) en edities (Developer, Express, Web, Standard, Enterprise) zijn beschikbaar. Bovendien zijn er Bring Your Own License-versies (BYOL) van de installatiekopieën beschikbaar. Wanneer u de door Azure geleverde installatiekopieën gebruikt, zijn de operationele kosten afhankelijk van de VM-grootte en de versie van SQL Server die u kiest. U betaalt licentiekosten van SQL Server en Windows Server per minuut, samen met de kosten van Azure Storage voor de VM-schijven, ongeacht de VM-grootte of versie van SQL Server. Met de optie factureren per minuut kunt u SQL Server blijven gebruiken zonder aanvullende SQL Server-licenties te kopen. Als u uw eigen SQL Server-licentie meeneemt naar Azure, worden alleen Windows Server en de kosten voor opslag in rekening gebracht. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>De totale kosten voor de toepassing berekenen
Als u een cloudplatform gaat gebruiken, bestaan de kosten van het uitvoeren van uw toepassing uit de ontwikkelings- en beheerkosten en de servicekosten voor het openbare cloudplatform.

Dit is de gedetailleerde kostenberekening voor uw toepassing die wordt uitgevoerd in SQL Database en SQL Server op Azure VM’s:

**Als u Azure SQL Database gebruikt:**

*Totale kosten van de toepassing = geminimaliseerde beheerkosten + softwareontwikkelingskosten + servicekosten voor SQL Database*

**Als u SQL Server op VM’s van Azure gebruikt:**

*Totale kosten van de toepassing = uiterst geminimaliseerde softwareontwikkelingskosten + beheerkosten + SQL Server- en Windows Server-licentiekosten + Azure Storage-kosten*

Zie de volgende bronnen voor meer informatie over prijzen:

* [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
* [Prijzen virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Azure prijscalculator](https://azure.microsoft.com/pricing/calculator/)

> [!NOTE]
> Er is een kleine subset van functies op SQL Server die niet van toepassing zijn op of niet beschikbaar zijn in SQL Database. Zie [SQL Database Features](sql-database-features.md) (Functies van SDL Database) en [SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Informatie over SQL Database Transact-SQL) voor meer informatie. Als u een bestaande SQL Server-oplossing naar de cloud verplaatst, raadpleegt u het artikel [Migrating a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md). Wanneer u een bestaande on-premises SQL Server-toepassing naar SQL Database migreert, dan kunt u de toepassing bijwerken om te profiteren van de mogelijkheden die cloudservices bieden. U kunt bijvoorbeeld [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) of [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) te gebruiken voor het hosten van uw toepassingslaag om meer kosten te besparen.
> 
> 

### <a name="administration"></a>Beheer
Veel bedrijven besluiten over te stappen naar een cloudservice zowel vanwege offloading van beheercomplexiteit als de kosten. Met **SQL Database** beheert Microsoft de onderliggende hardware. Microsoft repliceert automatisch alle gegevens voor hoge beschikbaarheid, configureert de databasesoftware en werkt deze bij, beheert taakverdeling en voert transparante failover uit als er een serverfout optreedt. U kunt uw database blijven beheren, maar u hoeft de database-engine, het server-besturingssysteem of de hardware niet langer te beheren.  Voorbeelden van items die u kunt blijven beheren zijn onder meer databases en aanmeldingen, index en query afstemmen, en controle en beveiliging. 

Met **SQL Server op Azure Virtual Machines** hebt u volledige controle over het besturingssysteem en de configuratie van het SQL Server-exemplaar. Met een VM kunt u zelf bepalen wanneer een upgrade van het besturingssysteem en de databasesoftware nodig is en wanneer u aanvullende software zoals antivirusprogramma's moet installeren. Sommige geautomatiseerde functies worden geleverd om patchen, het maken van een back-up en hoge beschikbaarheid aanzienlijk te vereenvoudigen. Bovendien kunt u de grootte van de VM, het aantal schijven en hun opslagconfiguraties beheren. Met Azure kunt de grootte van een VM indien nodig wijzigen. Zie voor meer informatie [Virtual Machine en Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)
Voor veel IT-afdelingen is het voldoen aan uptimeverplichtingen van een Service Level Agreement (SLA) een topprioriteit. In dit gedeelte kijken we welke SLA van toepassing is op de databasehostingopties.

Voor de **SQL Database**-servicelagen Basic, Standard, Premium en Premium RS biedt Microsoft een beschikbaarheids-SLA van 99,99%. Zie [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/) voor de meest recente informatie. Zie [Servicecategorieën](sql-database-service-tiers.md) voor de meest recente informatie over SQL Database servicelagen en de ondersteunde plannen voor bedrijfscontinuïteit.

Voor **SQL Server die wordt uitgevoerd op Azure Virtual Machines**, biedt Microsoft een beschikbaarheids-SLA van 99,95% die alleen van toepassing is op de virtuele machine. Deze SLA heeft geen betrekking op de processen (zoals SQL Server) die worden uitgevoerd op de VM en vereist dat u ten minste twee exemplaren van de virtuele machine in een beschikbaarheidsset host. Voor de meest recente informatie, zie de [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Voor hoge beschikbaarheid (HA) van de database binnen virtuele machines, moet u een van de ondersteunde opties voor hoge beschikbaarheid in SQL Server configureren, zoals [AlwaysOn-beschikbaarheidsgroepen](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Het gebruik van een ondersteunde optie voor hoge beschikbaarheid biedt geen extra SLA, maar u kunt wel een databasebeschikbaarheid van >99,99% behalen.

### <a name="market"></a>Implementatietijd
**SQL Database** is de juiste oplossing voor toepassingen die zijn ontworpen voor de cloud wanneer de productiviteit van ontwikkelaars en snelle implementatietijd essentieel zijn. Met programmatische DBA-functies is het ideaal voor cloud-architecten en -ontwikkelaars aangezien het de noodzaak voor het beheren van het onderliggende besturingssysteem en de database vermindert. U kunt bijvoorbeeld de [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) en [PowerShell-Cmdlets](http://msdn.microsoft.com/library/mt740629.aspx) gebruiken om beheerbewerkingen te automatiseren en beheren voor duizenden databases. Met functies zoals [elastische pools](sql-database-elastic-pool.md) kunt u zich richten op de toepassingslaag en uw oplossing sneller leveren aan de markt.

**SQL Server die wordt uitgevoerd op Azure Virtual Machines** is ideaal als uw bestaande of nieuwe toepassingen grote databases, onderling verbonden databases of toegang tot alle functies in SQL Server of Windows vereisen. Het is ook geschikt wanneer u bestaande on-premises toepassingen en databases wilt migreren naar Azure. Omdat u de presentatie-, toepassings- en gegevenslagen niet hoeft te wijzigen, bespaart u tijd en geld op het opnieuw vormgeven van uw architectuur. In plaats daarvan kunt u zich richten op het migreren van uw oplossingen naar Azure en het uitvoeren van prestatieoptimalisatie die mogelijk wordt vereist door het Azure-platform. Zie voor meer informatie [Best practices prestaties for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Samenvatting
In dit artikel werden SQL Database en SQL Server op Azure Virtual Machines (VM's) verkend en algemene zakelijke motivators besproken die invloed kunnen hebben op uw beslissing. Hier volgt een samenvatting met enkele suggesties:

Kies voor een **Azure SQL Database** als:

* U ontwikkelt nieuwe cloudgebaseerde toepassingen om te profiteren van de kostenbesparingen en optimalisatie van de prestaties die cloudservices bieden. Deze aanpak biedt de voordelen van een volledig beheerde cloudservice, helpt de implementatietijd te verlagen en kan zorgen voor kostenoptimalisatie op de lange termijn.
* U wilt dat Microsoft algemene managementbewerkingen uitvoert op uw databases en vereist betere beschikbaarheids-SLA's voor databases.

Kies voor **SQL Server op Azure VM's** als:

* U hebt bestaande on-premises toepassingen die u wilt migreren of uitbreiden naar de cloud, of als u wilt maken van bedrijfstoepassingen groter is dan 4 TB. Met deze aanpak profiteert u van 100% SQL-compatibiliteit, een grote databasecapaciteit, volledig beheer van SQL Server en Windows en beveiligt u tunneling naar on-premises. Deze aanpak minimaliseert de kosten voor ontwikkeling en wijzigingen van bestaande toepassingen.
* U hebt bestaande IT-resources en kunt eigenaar zijn van patchen, back-ups en hoge beschikbaarheid van databases. Houd er rekening mee dat sommige geautomatiseerde functies deze bewerkingen aanzienlijk kunnen vereenvoudigen. 

## <a name="next-steps"></a>Volgende stappen
* Zie [Uw eerste Azure SQL Database](sql-database-get-started-portal.md) om aan de slag te gaan met SQL Database.
* Zie [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Zie [Provision a SQL Server virtual machine in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) om aan de slag te kunnen met SQL Server op Azure VM’s.

