---
title: Kiezen tussen SQL-keuzes in azure | Microsoft Docs
description: Meer informatie over het kiezen van de implementatie opties in Azure SQL Database en tussen Azure SQL Database en SQL Server op virtuele machines van Azure
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
keywords: SQL Server-cloud, SQL Server in de cloud, PaaS-database, SQL-cloudserver, DBaaS
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/11/2019
ms.openlocfilehash: 344d201489a409824bb52f928ba5a87bd968500a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567099"
---
# <a name="choose-the-right-sql-server-option-in-azure"></a>Kies de optie juiste SQL Server in azure

In azure kunt u uw SQL Server-workloads uitvoeren in een gehoste infra structuur (IaaS) of worden uitgevoerd als een gehoste service ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)). Binnen PaaS hebt u meerdere implementatie opties en service lagen binnen elke implementatie optie. De belang rijke vraag die u moet stellen bij het kiezen tussen PaaS of IaaS is dat u uw Data Base wilt beheren, patches wilt Toep assen, back-ups wilt maken of u deze bewerkingen wilt delegeren naar Azure?
Afhankelijk van het antwoord hebt u de volgende opties:

- [Azure SQL Database](sql-database-technical-overview.md): Een volledig beheerde SQL database-engine, gebaseerd op de nieuwste stabiele Enter prise-editie van SQL Server. Dit is een relationele data base-as-a-Service (DBaaS) die wordt gehost in de Azure-Cloud en die valt onder de branche categorie van *platform-as-a-Service (PaaS)* . SQL database heeft meerdere implementatie opties, die allemaal zijn gebaseerd op gestandaardiseerde hardware en software die eigendom is van, worden gehost en onderhouden door micro soft. Met SQL Database kunt u ingebouwde functies en functionaliteit gebruiken waarvoor een uitgebreide configuratie nodig is wanneer deze wordt gebruikt in SQL Server (on-premises of op een virtuele machine van Azure). Als u SQL Database gebruikt, betaalt u per gebruik met opties voor het omhoog schalen voor meer vermogen zonder onderbrekingen. SQL Database heeft extra functies die niet beschikbaar zijn in SQL Server, zoals ingebouwde hoge Beschik baarheid, intelligentie en beheer. Azure SQL Database biedt de volgende implementatie opties:
  
  - Als [één data base](sql-database-single-database.md) met een eigen set resources die worden beheerd via een SQL database-server. Een enkele data base is vergelijkbaar met een [Inge sloten data base](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) in SQL Server. Deze optie is geoptimaliseerd voor de ontwikkeling van moderne toepassingen van nieuwe Cloud toepassingen.
  - Een [elastische pool](sql-database-elastic-pool.md), een verzameling data bases met een gedeelde set resources die worden beheerd via een SQL database-server. U kunt afzonderlijke data bases naar en uit een elastische pool verplaatsen. Deze optie is geoptimaliseerd voor de ontwikkeling van moderne toepassingen van nieuwe Cloud toepassingen met behulp van de SaaS-toepassing met meerdere tenants.
  - [Beheerde instantie](sql-database-managed-instance.md), een verzameling systeem-en gebruikers databases met een gedeelde set resources. Een beheerd exemplaar is vergelijkbaar met een exemplaar van de [Microsoft SQL Server data base engine] waarin gedeelde bronnen worden aangeboden voor data bases en aanvullende functies met een exemplaar bereik. Een beheerd exemplaar ondersteunt database migratie van on-premises met een minimale wijziging van de data base. Deze optie biedt alle PaaS-voor delen van Azure SQL Database, maar voegt mogelijkheden toe die eerder alleen beschikbaar waren in SQL-Vm's. Dit omvat een systeem eigen virtueel netwerk (VNet) en een bijna 100% compatibel met on-premises SQL Server.
- [SQL Server op Azure virtual machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) valt in de branche categorie *Infrastructure-as-a-Service (IaaS)* en biedt u de mogelijkheid om SQL Server in een volledig beheerde virtuele machine in de Azure-Cloud uit te voeren. [SQL Server virtuele machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) worden ook uitgevoerd op gestandaardiseerde hardware die eigendom is van, wordt gehost en wordt beheerd door micro soft. Wanneer u SQL Server op een virtuele machine gebruikt, kunt u betalen per gebruik voor een SQL Server licentie die al is opgenomen in een SQL Server installatie kopie of kunt u eenvoudig een bestaande licentie gebruiken. U kunt ook de virtuele machine stoppen of hervatten als dat nodig is. SQL Server geïnstalleerd en gehost in de Cloud op Windows Server-of Linux virtual machines (Vm's) die worden uitgevoerd op Azure, ook wel een IaaS (Infrastructure as a Service) genoemd. SQL Server op virtuele machines van Azure is een goede optie voor het migreren van on-premises SQL Server data bases en toepassingen zonder dat er wijzigingen in de Data Base zijn. Alle recente versies en versies van SQL Server zijn beschikbaar voor installatie in een virtuele IaaS-machine. Het belangrijkste verschil van SQL Database is dat SQL Server Vm's volledige controle over de data base-engine toestaan. U kunt kiezen wanneer onderhoud/patching wordt gestart, om het herstel model te wijzigen in eenvoudig of bulksgewijs geregistreerd, zodat het logboek sneller kan worden geladen, zodat de engine zo nodig kan worden gepauzeerd of gestart, en u de SQL Server data base-engine volledig kunt aanpassen. Met dit extra besturings element krijgt u de verantwoordelijkheid om de virtuele machines te beheren.

De belangrijkste verschillen tussen deze opties worden weer gegeven in de volgende tabel:

| SQL Server op VM | Beheerd exemplaar in SQL Database | Eén data base/elastische pool in SQL Database |
| --- | --- | --- |
|U hebt volledige controle over de SQL Server-engine.<br/>Maxi maal 99,95% Beschik baarheid.<br/>Volledige pariteit met de overeenkomende versie van on-premises SQL Server.<br/>Gecorrigeerde versie van de bekende data base-engine.<br/>Eenvoudige migratie van SQL Server on-premises.<br/>Privé-IP-adres in azure VNet.<br/>U hebt de mogelijkheid om toepassingen of services te implementeren op de host waar SQL Server is geplaatst.| Hoge compatibiliteit met SQL Server on-premises.<br/>gegarandeerde Beschik baarheid van 99,99%.<br/>Ingebouwde back-ups, patches, herstel.<br/>Nieuwste stabiele data base-engine versie.<br/>Eenvoudige migratie van SQL Server.<br/>Privé-IP-adres in azure VNet.<br/>Ingebouwde geavanceerde intelligentie en beveiliging.<br/>Online wijziging van resources (CPU/opslag).|De meestgebruikte SQL Server-functies zijn beschikbaar.<br/>gegarandeerde Beschik baarheid van 99,99%.<br/>Ingebouwde back-ups, patches, herstel.<br/>Nieuwste stabiele data base-engine versie.<br/>De mogelijkheid om de benodigde resources (CPU/opslag) toe te wijzen aan afzonderlijke data bases.<br/>Ingebouwde geavanceerde intelligentie en beveiliging.<br/>Online wijziging van resources (CPU/opslag).|
|U moet uw back-ups en patches beheren.<br>U moet uw eigen oplossing voor hoge Beschik baarheid implementeren.<br/>Er is een downtime bij het wijzigen van de resources (CPU/opslag)|Er is nog een aantal SQL Server functies die niet beschikbaar zijn.<br/>Geen gegarandeerd exacte onderhouds tijd (maar bijna transparant).<br/>De compatibiliteit met de SQL Server versie kan alleen worden bereikt door database compatibiliteits niveaus te gebruiken.|Migratie van SQL Server kan moeilijk zijn.<br/>Sommige SQL Server functies zijn niet beschikbaar.<br/>Geen gegarandeerd exacte onderhouds tijd (maar bijna transparant).<br/>De compatibiliteit met de SQL Server versie kan alleen worden bereikt door database compatibiliteits niveaus te gebruiken.<br/>Privé-IP-adres kan niet worden toegewezen (u kunt de toegang beperken met behulp van firewall regels).|

Meer informatie over hoe elke implementatie optie in het micro soft-gegevens platform past en hulp krijgt bij het vinden van de juiste optie voor uw bedrijfs vereisten. Of u nu kostenbesparingen of minimaal beheer het belangrijkst vindt, met dit artikel kunt u beslissen welke benadering de bedrijfsvereisten levert die u het belangrijkst vindt.

## <a name="microsofts-sql-data-platform"></a>Het SQL-gegevens platform van micro soft

Een van de eerste belangrijke punten in een discussie over Azure ten opzichte van on-premises SQL Server-databases is dat u alles kunt gebruiken. Het gegevensplatform van Microsoft maakt gebruik van SQL Server-technologie en maakt deze beschikbaar op fysieke on-premises machines, privécloudomgevingen, externe gehoste privécloudomgevingen en openbare cloud. Met SQL Server op virtuele Azure-machines kunt u voldoen aan de unieke en diverse bedrijfsbehoeften met een combinatie van on-premises en in de cloud gehoste implementaties terwijl dezelfde set serverproducten, ontwikkelingsprogramma's en kennis wordt gebruikt in deze omgevingen.

   ![Opties voor Cloud SQL Server: SQL Server op IaaS of SaaS SQL database in de Cloud.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Zoals we zien in het diagram, wordt elke aanbieding gekenmerkt door het niveau van beheer dat u hebt over de infrastructuur (op de X-as) en de mate van kostenefficiëntie die wordt bereikt door consolidatie en automatisering op databaseniveau (op de Y-as).

Bij het ontwerpen van een toepassing zijn vier eenvoudige opties beschikbaar voor het hosten van het SQL Server-onderdeel van de toepassing:

- SQL Server op niet-gevirtualiseerde fysieke computers
- SQL Server in on-premises gevirtualiseerde machines (privécloud)
- SQL Server in virtuele Azure-machines (openbare cloud van Microsoft)
- Azure SQL-database (openbare cloud van Microsoft)

In de volgende secties vindt u informatie over SQL Server in de open bare cloud van micro soft: Azure SQL Database en SQL Server op virtuele machines in Azure. Bovendien verkent u algemene zakelijke motivators om te bepalen welke optie het meest geschikt is voor uw toepassing.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>De Azure SQL Database en SQL Server op virtuele Azure-machines

Over het algemeen zijn deze twee SQL-opties geoptimaliseerd voor verschillende doeleinden:

- **Azure SQL Database**

Geoptimaliseerd om de totale beheer kosten te verlagen tot het minimum voor het inrichten en beheren van veel data bases. Dit vermindert beheerkosten omdat u geen virtuele machines, besturingssysteem of database-software hoeft te beheren. U hoeft upgrades, hoge beschikbaarheid of [back-ups](sql-database-automated-backups.md) niet te beheren. Over het algemeen kan Azure SQL Database het aantal databases dat wordt beheerd door één IT of ontwikkelingsresource aanzienlijk verhogen. [Elastische Pools](sql-database-elastic-pool.md) ondersteunen ook SaaS multi tenant Application architecturen met functies, waaronder Tenant isolatie, en de mogelijkheid om de kosten te verlagen door resources te delen in verschillende data bases. Een [beheerd exemplaar](sql-database-managed-instance.md) biedt ondersteuning voor functies met een exemplaar bereik, waardoor bestaande toepassingen eenvoudig kunnen worden gemigreerd en resources worden gedeeld met data bases.

- **SQL Server uitgevoerd op virtuele machines van Azure**

Geoptimaliseerd voor het migreren van bestaande toepassingen naar Azure of het uitbreiden van bestaande on-premises toepassingen naar de cloud in hybride implementaties. Bovendien kunt u SQL Server op een virtuele machine gebruiken om traditionele SQL Server-toepassingen te ontwikkelen en te testen. Met SQL Server op Azure Virtual Machines hebt u de volledige beheerdersrechten van een speciaal exemplaar van SQL Server en een cloud-gebaseerde VM. Het is een ideale keuze als een organisatie al IT-bronnen beschikbaar heeft voor het onderhouden van de virtuele machines. Met deze mogelijkheden kunt u een aangepast systeem bouwen voor de specifieke prestaties en beschikbaarheidsvereisten van uw toepassing.

De volgende tabel geeft een overzicht van de belangrijkste kenmerken van de SQL Database en SQL Server op Azure Virtual Machines:

| | SQL Database afzonderlijke data bases en elastische Pools | Beheerde exemplaren SQL Database |Virtuele machines van Azure met SQL Server |
| --- | --- | --- |---|
| **Ideaal voor:** |Nieuwe Cloud toepassingen die de nieuwste stabiele SQL Server functies willen gebruiken en tijd beperkingen hebben in ontwikkeling en marketing. | Nieuwe toepassingen of bestaande on-premises toepassingen die de nieuwste stabiele SQL Server functies willen gebruiken en die worden gemigreerd naar de Cloud met minimale wijzigingen.  | Bestaande toepassingen waarvoor snelle migratie naar de Cloud is vereist met minimale wijzigingen of zonder wijzigingen. Snelle ontwikkelings- en testscenario's als u geen on-premises SQL Server-hardware wilt kopen die niet bestemd is voor productie. |
|  | Teams die ingebouwde hoge beschikbaarheid, herstel na noodgevallen en upgrademogelijkheden van de database nodig hebben. | Hetzelfde als SQL Database afzonderlijke en gepoolde data bases. | Teams die hoge Beschik baarheid, herstel na nood gevallen en patches voor SQL Server kunnen configureren, afstemmen, aanpassen en beheren. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. |
|  | Teams die het onderliggende besturingssysteem en configuratie-instellingen niet willen beheren. | Hetzelfde als SQL Database afzonderlijke en gepoolde data bases. | U hebt een aangepaste omgeving met volledige beheerders rechten nodig. |
|  | Data bases van Maxi maal 100 TB. | Maxi maal 8 TB. | SQL Server-exemplaren met maximaal 64 TB aan opslag. Het exemplaar kan zoveel databases ondersteunen als nodig is. |
| **Tussen** | Ondersteunt de meeste on-premises mogelijkheden op database niveau. | Biedt ondersteuning voor vrijwel alle on-premises op het niveau van de instantie en op database niveau. | Ondersteunt alle on-premises mogelijkheden. |
| **Bronnen:** | U wilt geen IT-resources gebruiken voor configuratie en beheer van de onderliggende infra structuur, maar u wilt zich richten op de toepassingslaag. | Hetzelfde als SQL Database afzonderlijke en gepoolde data bases. | U hebt een aantal IT-resources voor configuratie en beheer. Sommige geleverde geautomatiseerde functies vereenvoudigen dit aanzienlijk. |
| **Totale eigendomskosten:** | Elimineert hardwarekosten en verlaagt administratieve kosten. | Hetzelfde als SQL Database afzonderlijke en gepoolde data bases. | Elimineert hardwarekosten. |
| **Bedrijfscontinuïteit:** |Naast [ingebouwde mogelijkheden voor fout tolerantie infrastructuur](sql-database-high-availability.md)biedt Azure SQL database functies, zoals [geautomatiseerde back-ups](sql-database-automated-backups.md), [herstel naar](sql-database-recovery-using-backups.md#point-in-time-restore)een bepaald tijdstip, [geo-Restore](sql-database-recovery-using-backups.md#geo-restore), [actieve geo-replicatie](sql-database-active-geo-replication.md) [en Groepen voor automatische failover](sql-database-auto-failover-group.md) om de bedrijfs continuïteit te verbeteren. Zie voor meer informatie [SQL Database business continuity overview](sql-database-business-continuity.md). | Hetzelfde als SQL Database één en gegroepeerde Data bases, plus door de gebruiker geïnitieerde, alleen-kopiëren back-ups zijn beschikbaar. | Met SQL Server op Azure Virtual Machines kunt u een oplossing met hoge beschikbaarheid en herstel na een noodgeval instellen voor de specifieke behoeften van uw database. Daarmee creëert u een systeem dat is geoptimaliseerd voor uw toepassing. U kunt zelf tests en failovers uitvoeren wanneer dit nodig is. Zie voor meer informatie [High Availability and Disaster Recovery for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Hybride cloud:** |Uw on-premises toepassing heeft toegang tot gegevens in Azure SQL Database. | [Systeem eigen virtuele netwerk implementatie](sql-database-managed-instance-vnet-configuration.md) en connectiviteit met uw on-premises omgeving met behulp van Azure Express Route of VPN gateway. | Met SQL Server op Azure Virtual Machines krijgt u de toepassingen die deels in de cloud draaien en deels on-premises. Zo kunt u uw on-premises netwerk en Active Directory-domein uitbreiden naar de cloud met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Zie [on-premises gegevens oplossingen uitbreiden naar de Cloud](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud)voor meer informatie over hybride cloud oplossingen. |
|  | Ondersteunt [transactionele replicatie van SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) als abonnee om gegevens te repliceren. | Replicatie wordt ondersteund voor een beheerd exemplaar als een preview-functie. | Biedt volledige ondersteuning voor [SQL Server transactionele replicatie](https://msdn.microsoft.com/library/mt589530.aspx), AlwaysOn- [beschikbaarheids groepen](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), integratie Services en de back-upfunctie voor logboek bestanden om gegevens te repliceren. Ook worden traditionele SQL Server-back-ups volledig ondersteund. |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Zakelijke redenen om Azure SQL Database of SQL Server op virtuele Azure-machines te kiezen

Er zijn verschillende factoren die van invloed kunnen zijn op uw beslissing om PaaS of IaaS te kiezen voor het hosten van uw SQL-data bases:

- [Kosten](#cost) : zowel de Paas-als de IaaS-optie zijn basis prijs die betrekking heeft op de onderliggende infra structuur en licenties. Met de optie IaaS moet u echter extra tijd en resources investeren om uw data base te beheren, terwijl u in PaaS deze beheer functies in de prijs hebt opgenomen. Met de optie IaaS kunt u uw resources afsluiten terwijl u ze niet gebruikt om de kosten te verlagen, terwijl PaaS-versie altijd wordt uitgevoerd, tenzij u uw resources verwijdert en opnieuw maakt wanneer dat nodig is.
- Met [PaaS-opties](#administration) kunt u de hoeveelheid tijd beperken die u moet investeren om de data base te beheren. Het kan echter ook het bereik van aangepaste beheer taken en scripts beperken dat u kunt uitvoeren of uitvoeren. De CLR wordt bijvoorbeeld niet ondersteund met één of gegroepeerde Data Base, maar wordt wel ondersteund voor een beheerd exemplaar. Daarnaast bieden geen implementatie opties in PaaS ondersteuning voor het gebruik van tracerings vlaggen.
- [Service Level Agreement](#service-level-agreement-sla) : zowel IaaS als PaaS bieden een hoge, industrie standaard sla. De PaaS-optie garandeert een SLA van 99,99%, terwijl IaaS 99,95% SLA voor infra structuur garandeert, wat betekent dat u extra mechanismen moet implementeren om de beschik baarheid van uw data bases te garanderen. Als u in het uitzonderlijke geval een oplossing met hoge Beschik baarheid wilt implementeren die overeenkomt met PaaS, moet u mogelijk extra SQL Server in de VM maken en AlwaysOn-beschikbaarheids groepen configureren. Dit kan de kosten van uw data base verdubbelen.
- De [tijd om over te stappen op Azure](#market) -SQL Server in azure VM is het exacte resultaat van uw omgeving, dus migratie van on-premises naar Azure SQL VM is niet anders dan het verplaatsen van de data bases van een lokale server naar een andere. Managed instance biedt ook een zeer eenvoudige migratie. Er zijn echter enkele wijzigingen die u moet Toep assen voordat u migreert naar een beheerd exemplaar.

Deze factoren worden besproken in de volgende secties.

### <a name="cost"></a>Kosten

Of u nu een startup bent die geld nodig heeft of een team in een goedlopend bedrijf met een krap budget, beperkte middelen zijn vaak de belangrijkste reden bij het bepalen hoe u uw databases wilt hosten. In deze sectie vindt u meer informatie over de basis principes van facturering en licenties in azure met betrekking tot deze twee relationele database opties: SQL Database en SQL Server op virtuele machines in Azure. Ook leest u over hoe u de totale kosten voor de toepassing berekent.

#### <a name="billing-and-licensing-basics"></a>Grondbeginselen facturering en licenties

Momenteel wordt **SQL database** als een service verkocht en is deze beschikbaar met verschillende implementatie opties en in verschillende service lagen met verschillende prijzen voor resources, die allemaal per uur worden gefactureerd tegen een vast tarief op basis van de servicelaag en de reken grootte die u kiest . Zie op [DTU gebaseerd inkoop model](sql-database-service-tiers-dtu.md) en op [vCore gebaseerd inkoop model](sql-database-service-tiers-vcore.md)voor de meest recente informatie over de huidige ondersteunde service lagen, reken grootten en opslag bedragen.

- Met SQL Database afzonderlijke Data Base kunt u een servicelaag kiezen die aan uw behoeften voldoet vanuit een breed scala aan prijzen vanaf 5 $/maand voor de Basic-laag.
- U kunt [elastische Pools](sql-database-elastic-pool.md) maken om resources te delen tussen data base-instanties om de kosten te verlagen en gebruiks pieken te bieden.
- Met SQL Database Managed Instance kunt u ook uw eigen licentie meenemen. Zie [License Mobility via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/) of [Azure Hybrid Benefit Calculator](https://azure.microsoft.com/pricing/hybrid-benefit/#sql-database) gebruiken om **Maxi maal 40%** op te slaan voor meer informatie over het nemen van uw eigen licentie.

Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/). U kunt Service lagen en reken grootten dynamisch aanpassen zodat deze overeenkomen met de verschillende doorvoer behoeften van uw toepassing.

Met **SQL Database** wordt de databasesoftware automatisch geconfigureerd, hersteld en bijgewerkt door Microsoft, waardoor uw beheerkosten worden verlaagd. Bovendien kunt u met de [ingebouwde back-up](sql-database-automated-backups.md)mogelijkheden aanzienlijk op kosten besparen, vooral wanneer u een groot aantal databases hebt.

Met **SQL Server op Azure Virtual Machines** kunt u alle door het platform geleverde SQL Server-installatiekopieën (met een licentie) gebruiken of uw SQL Server-licentie meenemen. Alle ondersteunde SQL Server-versies (2008R2, 2012, 2014, 2016) en edities (Developer, Express, Web, Standard, Enterprise) zijn beschikbaar. Bovendien zijn er Bring Your Own License-versies (BYOL) van de installatiekopieën beschikbaar. Wanneer u de door Azure geleverde installatiekopieën gebruikt, zijn de operationele kosten afhankelijk van de VM-grootte en de versie van SQL Server die u kiest. Ongeacht de VM-grootte of SQL Server editie betaalt u per minuut licentie kosten van SQL Server en de Windows-of Linux-server, samen met de Azure Storage kosten voor de VM-schijven. Met de optie factureren per minuut kunt u SQL Server blijven gebruiken zonder aanvullende SQL Server-licenties te kopen. Als u uw eigen SQL Server-licentie naar Azure brengt, worden alleen de server-en opslag kosten in rekening gebracht. Zie voor meer informatie over meenemen van uw eigen licentie [License Mobility through Software Assurance on Azure](https://azure.microsoft.com/pricing/license-mobility/). Bovendien wordt uitgaand internetverkeer in rekening gebracht bij u tegen het reguliere [tarief voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/).

#### <a name="calculating-the-total-application-cost"></a>De totale kosten voor de toepassing berekenen

Wanneer u begint met het gebruik van een Cloud platform, bevatten de kosten van het uitvoeren van uw toepassing de kosten voor nieuwe ontwikkelings-en continue beheer kosten, plus de service kosten van het open bare Cloud platform.

**Als u Azure SQL Database gebruikt:**

- Zeer geminimaliseerde beheer kosten
- Beperkte ontwikkelings kosten voor gemigreerde toepassingen (beheerde instanties)
- Service kosten SQL Database
- Geen hardware-aanschaf kosten

**Als u SQL Server op VM’s van Azure gebruikt:**

- Hogere beheer kosten
- Beperkt tot geen ontwikkelings kosten voor gemigreerde toepassingen
- Service kosten van de virtuele machine
- Geen hardware-aanschaf kosten

Zie de volgende bronnen voor meer informatie over prijzen:

- [SQL Database prijzen](https://azure.microsoft.com/pricing/details/sql-database/)
- [Prijs informatie voor virtuele machines](https://azure.microsoft.com/pricing/details/virtual-machines/) voor [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) en voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure prijscalculator](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Beheer

Veel bedrijven besluiten over te stappen naar een cloudservice zowel vanwege offloading van beheercomplexiteit als de kosten. Met IaaS en PaaS beheert micro soft de onderliggende infra structuur en worden alle gegevens automatisch gerepliceerd om herstel na nood gevallen te bieden, de database software te configureren en bij te voegen, de-taak verdeling te beheren en een transparante failover uit te voeren als er een Server fout binnen een Data Center.

- Met **Azure SQL database**kunt u uw data base blijven beheren, maar u hoeft de data base-engine, het besturings systeem of de hardware niet langer te beheren. Voorbeelden van items die u kunt blijven beheren zijn onder meer databases en aanmeldingen, index en query afstemmen, en controle en beveiliging. Daarnaast is het configureren van hoge Beschik baarheid voor een ander Data Center vereist minimale configuratie en beheer.
- Met **SQL Server op Azure Virtual Machines** hebt u volledige controle over het besturingssysteem en de configuratie van het SQL Server-exemplaar. Met een VM kunt u zelf bepalen wanneer een upgrade van het besturingssysteem en de databasesoftware nodig is en wanneer u aanvullende software zoals antivirusprogramma's moet installeren. Sommige geautomatiseerde functies worden geleverd om patchen, het maken van een back-up en hoge beschikbaarheid aanzienlijk te vereenvoudigen. Bovendien kunt u de grootte van de VM, het aantal schijven en hun opslagconfiguraties beheren. Met Azure kunt de grootte van een VM indien nodig wijzigen. Zie voor meer informatie [Virtual Machine en Cloud Service Sizes for Azure](../virtual-machines/windows/sizes.md).

### <a name="service-level-agreement-sla"></a>Service Level Agreement (SLA)

Voor veel IT-afdelingen is het voldoen aan uptimeverplichtingen van een Service Level Agreement (SLA) een topprioriteit. In dit gedeelte kijken we welke SLA van toepassing is op de databasehostingopties.

Voor **SQL database**biedt micro soft een sla voor Beschik baarheid van 99,99%. Zie [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/sql-database/) voor de meest recente informatie.

Voor **SQL Server die wordt uitgevoerd op Azure Virtual Machines**, biedt Microsoft een beschikbaarheids-SLA van 99,95% die alleen van toepassing is op de virtuele machine. Deze SLA heeft geen betrekking op de processen (zoals SQL Server) die worden uitgevoerd op de VM en vereist dat u ten minste twee exemplaren van de virtuele machine in een beschikbaarheidsset host. Voor de meest recente informatie, zie de [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Voor maximale Beschik baarheid van de data base binnen Vm's, moet u een van de ondersteunde opties voor hoge Beschik baarheid configureren in SQL Server, zoals [Always on-beschikbaarheids groepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Het gebruik van een ondersteunde optie voor hoge beschikbaarheid biedt geen extra SLA, maar u kunt wel een databasebeschikbaarheid van >99,99% behalen.

### <a name="market"></a>Tijd om over te stappen op Azure

**SQL database afzonderlijke data bases of elastische Pools** zijn de juiste oplossing voor toepassingen die in de Cloud zijn ontworpen wanneer de productiviteit van ontwikkel aars en snelle time-to-Market voor nieuwe oplossingen cruciaal zijn. Met programmatische DBA-functies is het ideaal voor cloud-architecten en -ontwikkelaars aangezien het de noodzaak voor het beheren van het onderliggende besturingssysteem en de database vermindert.

**SQL database beheerde instantie** vereenvoudigt de migratie van bestaande toepassingen naar Azure SQL database, zodat u snel gemigreerde database toepassingen naar de markt kunt brengen in Azure.

**SQL Server die worden uitgevoerd op virtuele Azure-machines** is perfect als uw bestaande of nieuwe toepassingen grote data bases of toegang tot alle functies in SQL Server of Windows/Linux nodig hebben, en u wilt voor komen dat er nieuwe on-premises hardware worden aangestuurd. Het is ook een goed idee wanneer u bestaande on-premises toepassingen en data bases wilt migreren naar Azure als-in gevallen waarin Azure SQL Database beheerde instantie niet geschikt is. Omdat u de presentatie-, toepassings-en gegevens lagen niet hoeft te wijzigen, bespaart u tijd en budget bij het opnieuw ontwerpen van uw bestaande oplossing. In plaats daarvan kunt u zich richten op het migreren van uw oplossingen naar Azure en het uitvoeren van prestatieoptimalisatie die mogelijk wordt vereist door het Azure-platform. Zie voor meer informatie [Best practices prestaties for SQL Server on Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [Uw eerste Azure SQL Database](sql-database-single-database-get-started.md) om aan de slag te gaan met SQL Database.
- Zie [Prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Zie [Provision a SQL Server virtual machine in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) om aan de slag te kunnen met SQL Server op Azure VM’s.
- [Bepaal het juiste Azure SQL database/Managed instance SKU voor uw on-premises data base](/sql/dma/dma-sku-recommend-sql-db/).