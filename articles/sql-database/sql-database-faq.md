---
title: Azure SQL Database Veelgestelde vragen | Microsoft Docs
description: Antwoorden op algemene vragen klanten vragen over cloud-databases en Azure SQL Database, van Microsoft relationele databasebeheersysteem (RDBMS) en -database als een service in de cloud.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: 5d77c2dc121d7f291fa755f66d7d9a5d7213bf9f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="sql-database-faq"></a>Veelgestelde vragen over SQL Database

## <a name="what-is-the-current-version-of-sql-database"></a>Wat is de huidige versie van SQL-Database?
De huidige versie van SQL Database is V12. Versie V11 buiten gebruik gesteld.

## <a name="what-is-the-sla-for-sql-database"></a>Wat is de SLA voor SQL-Database?
Wordt gegarandeerd dat ten minste 99,99% van de tijd, hebt u connectiviteit tussen uw Microsoft Azure SQL Database en de Internet-gateway, ongeacht de servicelaag. Zie voor meer informatie [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="whatis-the-new-vcore-based-purchasing-model-preview-for-azure-sql-database"></a>De nieuwe vCore gebaseerde aankoopmodel (preview) voor Azure SQL Database Whatis?

De nieuwe aankoopmodel is in aanvulling op het bestaande model op basis van DTU. Het model op basis van vCore is ontworpen om klanten flexibiliteit, controle, transparantie, en een eenvoudige manier om te vertalen naar on-premises werklastvereisten naar de cloud. Ook kunnen klanten scale rekenkracht en opslag op basis van hun werkbelasting behoeften. Individuele database en met het model vCore elastische pool-opties zijn ook in aanmerking voor van 30 procent besparingen met het [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Zie [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md) voor meer informatie. 

## <a name="what-is-a-vcore"></a>Wat is een vCore? 
Een virtuele core vertegenwoordigt de logische CPU aangeboden met de optie te kiezen tussen generaties van hardware. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz processors en Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Is het verplicht om naar het vCore-model over te stappen?
Nee, de introductie van het model op basis van vCore voor de elastische groep en de implementatieopties voor één Database duidt op onze inzet naar keuze en flexibiliteit. Als klanten blijven gebruiken van het model op basis van DTU wilt, niet hoeven te doen met deze aankondiging en hun ervaringen en facturering blijven ongewijzigd. 

In veel gevallen kunnen toepassingen profiteren van de eenvoud van een vooraf geconfigureerde bundel met resources. Daarom blijven we bieden en deze keuzes op basis van DTU voor onze klanten te ondersteunen. Als u deze gebruikt en het voldoet aan uw zakelijke vereisten, moet u blijven doen.

Beide modellen, zowel die op basis van DTU als vCores, blijven naast elkaar bestaan. We introduceren het vCore-model omdat onze klanten om meer duidelijkheid vroegen wat hun databaseresources betreft. Ook wilden ze de mogelijkheid hebben om computing en opslag individueel te schalen. Het model op basis van vCore kan ook aanvullende besparing voor klanten met het actieve Software Assurance via het voordeel van de hybride Azure voor SQL Server.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model-preview"></a>Hoe moet ik kiezen tussen de vs aankopen model op basis van DTU de vCore gebaseerde aankoopmodel (preview)? 
De Data Transmission Unit (DTU) is gebaseerd op gecombineerde meetgegevens van CPU, geheugen en lees- en schrijfbewerkingen. De DTU -prestatieniveaus staan voor vooraf geconfigureerde bundels met resources die bedoeld zijn voor verschillende prestatieniveaus van toepassingen. Klanten die niet wilt zorgen over de onderliggende resources en de voorkeur aan van een vooraf geconfigureerde bundel tijdens elke maand van een vast bedrag betaalt vinden het model op basis van DTU meer geschikt zijn voor hun behoeften. Voor klanten die meer inzicht in de onderliggende resources moeten of schalen ze onafhankelijk voor optimale prestaties, wordt het model op basis van vCore wel de beste keuze.  Bovendien, als een klant een actieve Software Assurance (SA) voor SQL Server heeft, ze kunnen gebruikmaken van hun bestaande investeringen en opslaan van maximaal 30% met [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  In elk van de aankopen modellen bieden de voordelen van een volledig beheerde service zoals automatische back-ups, software-updates en patches. 

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>Wat is het voordeel van de hybride Azure voor SQL Server? 
De [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) helpt u bij de waarde van uw huidige licentiegegevens investeringen maximaliseren en versnellen hun migratie naar de cloud. Azure hybride profiteren voor SQL Server is een voordeel op basis van Azure, waarmee u uw SQL Server-licenties met Software Assurance gebruiken voor het betalen van een lagere frequentie ("base tarief') op de SQL-Database. Azure hybride-voordeel voor SQL Server is beschikbaar op de openbare evaluatieversie van de vCore gebaseerde aankoopmodel (preview) voor SQL-Database individuele databases en elastische pools. U kunt voordeel toepassen, zelfs als de SKU actief is, maar let op dat het basistype tarief toegepast vanaf het moment dat u deze optie in de Azure portal. Er worden niet met terugwerkende kracht tegoeden verleend.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Zijn er dubbele gebruiksrechten met Azure hybride voordeel voor SQL Server?
U hebt 180 dagen van gebruiksrechten tweeledig van de licentie om ervoor te zorgen migraties naadloos worden uitgevoerd. Na die periode van 180 dagen, de SQL Server-licentie kan alleen worden gebruikt in de cloud in SQL-Database en heeft geen dubbele rechten on-premises gebruiken en in de cloud.


## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Hoe Azure hybride voordeel voor SQL Server afwijken van licentiemobiliteit?
Vandaag de dag bieden we SQL Server-klanten met Software Assurance license mobility voordelen waardoor hernieuwde toewijzing van de licenties worden naar de gedeelde servers van derden. Deze vergoeding kan worden gebruikt op Azure IaaS en AWS EC2.
Azure hybride-voordeel voor SQL Server verschilt van licentiemobiliteit in twee hoofdgebieden:
- Deze biedt economische voordelen voor het verplaatsen van maximaal gevirtualiseerde werkbelastingen naar Azure. SQL EE-klanten kunnen krijgen 4 kernen in Azure in de algemene doel-SKU voor elke core ze on-premises voor maximaal gevirtualiseerde toepassingen eigenaar. Licentiemobiliteit is speciale kostenvoordelen niet toegestaan voor het verplaatsen van gevirtualiseerde werkbelastingen naar de cloud.
- Het biedt voor een PaaS-doel in Azure die maximaal compatibel is met SQL Server on-premises – beheerde exemplaar van SQL-Database.

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Wat zijn de specifieke rechten van het voordeel van de hybride Azure voor SQL Server?
Klanten van de SQL-Database heeft de volgende rechten die zijn gekoppeld aan Azure hybride voordeel voor SQL Server:

|Licentie-Footprint|Wat doet Azure hybride voordeel voor SQL-Server ophalen u?|
|---|---|
|SQL Server Enterprise Edition core klanten met SA|<li>Kan Base tarief op algemeen of Business-kritische SKU betalen</li><br><li>1 core lokale = 4 kernen in algemene doel-SKU</li><br><li>1 core lokale = 1 core in essentiële Business-SKU</li>|
|SQL Server Standard Edition core klanten met SA|<li>Kan Base tarief betalen op algemene doel-SKU alleen</li><br><li>1 core lokale = 1 core in de algemene doel-SKU</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>Kan het model op basis van vCore beheerde exemplaar van SQL Database?
[Beheerde exemplaar](sql-database-managed-instance.md) is alleen beschikbaar voor het model op basis van vCore. Zie voor meer informatie, ook de [pagina met prijzen SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/managed/). 

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>Is de kosten van de berekenings- en afhankelijk van de servicelaag die ik kiezen?
De compute-kosten duidt de totale rekencapaciteit die is ingericht voor de toepassing. In de servicelaag kritieke zakelijke toewijzen we automatisch ten minste 3 Always ON-replica's. De prijs vCore is zodat deze extra toewijzing van rekenresources ongeveer 2.7 x hoger in kritieke zakelijke. Om dezelfde reden zijn de hogere opslag prijs per GB in de essentiële zakelijke laag de hoge i/o en lage latentie van de SSD-opslag. Op hetzelfde moment verschilt de kosten van het back-upopslag niet omdat in beide gevallen we een klasse standard-opslag gebruiken.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Hoe ben ik in rekening gebracht voor opslag - gebaseerd op wat ik tevoren configureren of wat de database wordt gebruikt?
Andere soorten opslag wordt anders gefactureerd. Voor de opslag van gegevens u worden in rekening gebracht voor de ingerichte opslag op basis van de maximale database of groepsgrootte die u selecteert. De kosten wordt niet gewijzigd, tenzij u verminderen of die maximaal verhogen. Back-upopslag hoort bij geautomatiseerde back-ups van uw exemplaar. Door de retentieperiode van uw back-ups te vergroten, zal er meer back-upopslag door uw exemplaar worden verbruikt. Er worden geen extra kosten in rekening gebracht voor back-upopslag voor maximaal 100% van uw totale ingerichte serveropslag. Aanvullende verbruik van back-upopslag wordt in rekening gebracht in GB per maand. Als u bijvoorbeeld een database-opslag van 100 GB hebt, krijgt u 100 GB aan back-upopslag, zonder extra kosten. Maar als de back-up 110 GB is, betaalt u voor de aanvullende 10 GB.

Voor back-up van één database u in rekening worden gebracht naar rato op basis van een voor de opslag die is toegewezen aan de databaseback-ups minus de grootte van de database. Voor back-up van een pool voor elastische u in rekening worden gebracht naar rato op basis van een voor de opslag die is toegewezen aan de databaseback-ups van alle databases in de pool minus de maximale grootte van de elastische groep. Een toename van de grootte van de database of de elastische groep of een toename van de Transactiesnelheid meer opslagruimte vereist en dus verhoogt de back-upopslag factuur.  Als u de maximale grootte verhoogt, wordt deze nieuwe bedrag afgetrokken van de grootte van de back-upopslag gefactureerd.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Hoe kan ik de juiste SKU selecteren bij het converteren van een bestaande database naar de nieuwe Servicelagen? 
Voor bestaande toepassingen in SQL-Database met behulp van het model op basis van DTU, is de servicelaag voor algemene doeleinden worden vergeleken met de prijscategorie Standard. De kritieke zakelijke servicelaag is vergelijkbaar met de Premium-laag. In beide gevallen moet u ten minste 1 vCore toewijzen voor elke 100 DTU die uw toepassing wordt gebruikt in het model op basis van DTU.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-performance-levels-compatible-with-all-existing-service-level-objectives-slos"></a>Bieden de nieuwe vCore gebaseerde Servicelagen de prestaties die compatibel zijn met alle bestaande serviceniveaudoelstellingen (Slo's)
De nieuwe vCore gebaseerde Servicelagen zijn vergelijkbaar prestaties mogelijkheden voor alle elastische pools en 100 dtu's of meer databases.  Er wordt nog meer Slo's na verloop van tijd voor sub 100 DTU werkbelastingen toevoegen.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Zijn er database functionele verschillen tussen de Servicelagen van de bestaande DTU en nieuwe vCore? 
De nieuwe Servicelagen ondersteuning voor een uitbreiding van de functies die beschikbaar zijn met de huidige op basis van het DTU-aanbiedingen. De aanvullende functies omvatten een aantal extra dynamische beheerweergaven (DMV's) en configuratieopties voor aanvullende bronnen. 

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Als mijn database CPU-gebonden en niet veel opslag gebruikt, kan ik verhogen de compute zonder te betalen voor extra opslagruimte?
Ja, kunt u het niveau van compute uw toepassing moet en voorkomen dat de opslag ongewijzigd afzonderlijk selecteren. De opslag kan worden ingesteld als minimaal 32GB. 

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>De nieuwe vCore gebaseerde lagen ondersteunt punt naar een bepaald tijstip (PITR) voor vandaag 35 dagen? 
U kunt het bewaren van back-up configureren voor PITR tussen 7 en 35 dagen. De back-ups opslag brengt afzonderlijk op basis van de werkelijke opslagverbruik als deze de opslaghoeveelheid gelijk aan de maximale grootte overschrijdt. In voorbeeld van standaard is de bewaarperiode PITR ingesteld op 7 dagen. In veel gevallen is de maximale grootte voldoende zijn voor het opslaan van 7 dagen van back-ups.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>Waarom kan u de selectie van het genereren van de hardware voor compute toestaan?
Ons doel is maximale flexibiliteit inschakelen zodat u kunt een configuratie voor prestaties die overeenkomt met de behoeften van de toepassing. De bovenstaande tabel toont de verschillen tussen Gen4 en Gen5. In het bijzonder biedt Gen4 hardware aanzienlijk meer geheugen per vCore. Gen5 hardware, kunt u opschalen compute veel hoger. We willen deze verschillen transparant maken zodat u de verhouding tussen het optimale prijs/prestaties voor uw toepassing bereiken kunt.

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>Moet ik mijn toepassing offline te converteren van een database op basis van DTU naar een servicelaag met vCore? 
De nieuwe servicelagen bieden een eenvoudige onlineconversiemethode die vergelijkbaar is met het bestaande upgradeproces van de Standard- naar de Premium-servicelaag en omgekeerd. Deze conversie kan worden gestart via Portal, ARM, PowerShell, Azure CLI of T-SQL. Zie [individuele databases beheren](sql-database-single-database-resources.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Kan ik een database van een servicelaag met vCore converteren naar een DTU gebaseerde? 
Ja, kunt u eenvoudig uw database converteren naar een ondersteunde prestaties doelstelling met behulp van de Portal of programmatisch met Portal, ARM, PowerShell, Azure CLI of T-SQL. Zie [individuele databases beheren](sql-database-single-database-resources.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>Kan ik een upgrade of downgraden tussen de lagen van de service voor algemene doeleinden en kritieke zakelijke? 
Ja, met enkele beperkingen. De doel-SKU moet voldoen aan de maximale database of de elastische groepsgrootte die u hebt geconfigureerd voor uw bestaande implementatie. Als u [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md), de essentiële Business-SKU is alleen beschikbaar voor klanten met Enterprise-editie licenties. Alleen klanten die gemigreerd van on-premises naar Azure hybride voordeel voor SQL Server met Enterprise-editie licenties voor algemene doeleinden kunnen upgraden naar kritieke zakelijke. Zie voor meer informatie [wat zijn de specifieke rechten van het Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Deze conversie resulteert niet in de uitvaltijd en kan worden gestart met behulp van de Portal, ARM, PowerShell, Azure CLI of T-SQL. Zie [individuele databases beheren](sql-database-single-database-resources.md) en [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Ik gebruik een Premium RS-database die niet beschikbaar zijn in het algemeen - kan ik upgraden naar een nieuwe laag en een soortgelijk prijs/prestaties voordeel behalen?
Omdat het model vCore controle over de hoeveelheid ingerichte berekeningen en opslag toestaat, kunt u efficiënter beheren de resulterende kosten, waardoor het een aantrekkelijk doel voor RS Premium-databases. Bovendien de [Azure hybride gebruik voordeel voor SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) biedt een aanzienlijke korting wanneer het model op basis van vCore wordt gebruikt. 

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Hoe vaak kan ik de bronnen per groep aanpassen?
Zo vaak als u wilt. Zie [elastische pools beheren](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Hoe lang duurt het wijzigen van het serviceniveau voor laag of de prestaties van één database of verplaatsen van een database in een elastische groep?
De servicelaag van een database wijzigen en verplaatsen van en naar een groep moet de database op het platform als een achtergrondbewerking worden gekopieerd. De servicelaag wijzigen kan enkele minuten tot enkele uren duren, afhankelijk van de grootte van de databases. In beide gevallen worden in de databases online en beschikbaar blijven tijdens het verplaatsen. Zie voor meer informatie over het wijzigen van individuele databases [wijzigen van de servicelaag van een database](sql-database-service-tiers-dtu.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wanneer moet ik een individuele database versus elastische databases gebruiken?
In het algemeen elastische pools zijn ontworpen voor een typische [patroon software as a service (SaaS)-toepassing](sql-database-design-patterns-multi-tenancy-saas-applications.md), waarbij één database per klant of tenant. Kopen van afzonderlijke databases en overmatige inrichting om te voorzien in de variabele vraag en de piekvraag voor elke database, is vaak zeer onvoordelig. Beheren van de collectieve prestaties van de groep met toepassingen, en de databases omhoog en omlaag geschaald automatisch. Azure intelligent engine raadt aan om een pool voor databases wanneer een gebruikspatroon ontstane. Zie voor meer informatie [elastische pool richtlijnen](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Hoe wordt het gebruik van SQL Database met behulp van het DTU-aankoopmodel weergegeven op mijn factuur?
Facturen SQL-Database op een voorspelbare per uur tarief op basis van de [aanschaffen van een model](sql-database-service-tiers-dtu.md). Werkelijke gebruik wordt berekend en pro rato per uur uitgevoerd, zodat uw factuur mogelijk breuken van een uur weer. Als een database voor 12 uur in een maand bestaat, ziet uw factuur u bijvoorbeeld informatie over het gebruik van 0,5 dagen. 

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wat gebeurt er als een individuele database minder dan een uur actief is of gebruikmaakt van een hogere servicelaag minder dan een uur?
U wordt gefactureerd voor elk uur een database bestaat met de hoogste servicelaag + prestatieniveau die toegepast tijdens dat uur, ongeacht de informatie over het gebruik of of de database minder dan een uur actief was. Als u een individuele database maken en verwijderen van vijf minuten later duidt uw factuur kosten met zich mee voor één database uur. 

Voorbeelden:

* Als u een Basic-database maken en vervolgens onmiddellijk naar de Standard-S1 upgraden, u in rekening worden gebracht met de Standard-S1-frequentie voor het eerste uur.
* Als u een database vanuit Basic naar Premium om 10:00 uur bijwerkt en de upgrade is voltooid om 1:35 uur op de volgende dag u in rekening worden gebracht met de Premium-snelheid begint bij 1:00 uur 
* Als u een database van Premium naar Basic gedowngraded om 11:00 uur en 2:15 uur is voltooid, wordt de database wordt in rekening gebracht Premium tot en met 3:00 uur, waarna deze wordt in rekening gebracht volgens de tarieven voor het Basic.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Hoe wordt gebruik van de elastische groep van toepassingen met behulp van het DTU-aankoopmodel weergegeven op mijn factuur?
Elastische pool kosten weergeven van op uw factuur als elastische dtu's (edtu's) of vCores plus opslag in de stappen die wordt weergegeven op [de pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/). Er zijn geen kosten per database voor elastische pools. U wordt gefactureerd voor elk uur die een adresgroep bestaat op de hoogste eDTU of vCores, ongeacht de informatie over het gebruik of of de groep voor minder dan een uur actief was. 

Op basis van DTU aankopen model voorbeelden:

* Als u een Standard elastische pool met 200 edtu's om 11:18 uur maken, vijf databases toevoegen aan de groep, u in rekening worden gebracht 200 edtu's voor het hele uur beginnen bij 11: 00 in de rest van de dag.
* Op dag 2, 5:05 uur Database 1 begint verbruikt 50 edtu's en constante via de dag bevat. Databases 2-5 fluctueren tussen 0 en 80 edtu's. Tijdens de dag, kunt u vijf andere databases die verschillende edtu's gedurende de dag verbruiken toevoegen. Dag 2 is een volledige dag tegen 200 eDTU in rekening gebracht. 
* Op dag 3, 5 uur u toevoegen een andere 15 databases. Databasegebruik verhoogt gedurende de dag naar het punt waar u besluit om te verhogen edtu's voor de toepassingen van 200 400 20:05 uur Kosten op het niveau van 200 eDTU waren van kracht tot 8 uur en verhoogt naar 400 edtu's voor de resterende vier uur. 

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Hoe koopt elastische pool in rekening gebracht voor de DTU-gebaseerde model?
Elastische pools worden gefactureerd per de volgende kenmerken:

* Een elastische pool wordt in rekening gebracht bij het maken ervan, zelfs wanneer er geen databases in de groep zijn.
* Een elastische pool wordt in rekening gebracht per uur. Dit is dezelfde softwarelicentiecontrole frequentie als voor prestatieniveaus van individuele databases.
* Als de grootte van een elastische groep is gewijzigd, klikt u vervolgens de groep wordt niet in rekening gebracht volgens de nieuwe hoeveelheid resources totdat het formaat is voltooid. Dit volgt hetzelfde patroon als het wijzigen van het prestatieniveau van individuele databases.
* De prijs van een elastische groep is gebaseerd op de bronnen van de groep. De prijs van een elastische groep is afhankelijk van het aantal en het gebruik van de elastische databases binnen deze.

Zie voor meer informatie [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database/), [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md), en [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Hoe wordt het gebruik op basis van vCore weergegeven in mijn factuur? 
In het model op basis van vCore de service wordt gefactureerd op een voorspelbare, per uur tarief op basis van de servicelaag ingerichte compute in vCores, opslag in GB/maand ingericht en back-upopslag verbruikt. Als de opslag voor back-ups overschrijdt de grootte van de totale database (dat wil zeggen, 100% van de databasegrootte), zijn er extra kosten. vCore uren, geconfigureerde databaseopslag verbruikte i/o en back-upopslag worden duidelijk gespecificeerd in de factuur, waardoor het gemakkelijker voor u de details van de resources die u hebt gebruikt. Back-up opslag van maximaal 100% van de maximale databasegrootte is opgenomen, afgezien van dat u wordt gefactureerd GB/maand in een maand verbruikt.

Bijvoorbeeld:
- Als de SQL-database voor 12 uur in een maand bestaat, toont de factuur gebruik 12 uur van vCore. Als de SQL-database is een extra 100 GB aan opslagruimte voorzien, toont de factuur opslaggebruik in GB/maand-eenheden naar rato per uur en aantal IOs gebruikt in een maand.
- Als de SQL-database voor minder dan een uur actief is, wordt u gefactureerd voor elk uur dat de database bestaat met de hoogste servicelaag geselecteerd, opslag- en i/o die toegepast tijdens dat uur, ongeacht de informatie over het gebruik of of de database is actief voor minder dan ingericht een uur.
- Als u een beheerd exemplaar maakt en dit vijf minuten later verwijdert, wordt u voor één database-uur gefactureerd.
- Als u een Managed Instance maakt in de categorie Algemeen gebruik met 8 vCores en vervolgens meteen een upgrade naar 16 vCores uitvoert, wordt voor het eerste uur het tarief voor 16 vCores in rekening gebracht.

> [!NOTE]
> Voor een beperkte periode tot en met 30 juni-2018 zijn de back-kosten en i/o-kosten zijn gratis.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Hoe gaat het gebruik van actieve geo-replicatie in een elastische pool te zien op mijn factuur?
In tegenstelling tot individuele databases, met behulp van [actieve geo-replicatie](sql-database-geo-replication-overview.md) met elastische databases niet direct facturering gevolgen hebben.  U alleen worden in rekening gebracht voor de resources die zijn ingericht voor elk van de groepen (groep primaire en secundaire groep)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Hoe is de auditingfunctie van invloed op mijn factuur?
Controle is ingebouwd in de service SQL Database zonder extra kosten en is beschikbaar op alle Servicelagen. Echter, voor het opslaan van de controlelogboeken controle functie gebruikt die een Azure Storage-account en de tarieven voor tabellen en wachtrijen in Azure Storage-toepassing op basis van de grootte van het controlelogboek.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Hoe stel ik het wachtwoord voor de serverbeheerder
In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-Servers**, selecteert u de server uit de lijst en klik vervolgens op **wachtwoord opnieuw instellen**.

## <a name="how-do-i-manage-databases-and-logins"></a>Hoe kan ik databases en aanmeldingen beheren?
Zie [databases en aanmeldingen beheren](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Hoe maak ik ervoor dat alleen geautoriseerde IP-adressen zijn toegestaan voor toegang tot een server?
Zie [procedure: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wat is er een vertraging van het verwachte replicatie geo-replicatie een database tussen twee regio's binnen de dezelfde Azure Geografie?
Er zijn momenteel ondersteunt een RPO van vijf seconden en de vertraging van replicatie is kleiner dan dat wanneer de secundaire geo-database wordt gehost in Azure gekoppelde regio aanbevolen en op de dezelfde servicelaag.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wat is er een vertraging van de verwachte replicatie als secundaire geo-database wordt gemaakt in dezelfde regio bevinden als de primaire database?
Op basis van empirische gegevens, is er niet te veel verschil tussen regio intra- en vertraging tussen regio replicatie wanneer het Azure aanbevolen gekoppelde regio wordt gebruikt. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Als er een netwerkfout tussen twee regio's, hoe de Pogingslogica werkt wanneer geo-replicatie is ingesteld?
Als er een verbinding verbreken, wordt opnieuw geprobeerd aan elke 10 seconden om verbindingen opnieuw tot stand te brengen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Wat kan ik doen om ervoor te zorgen dat een belangrijke wijziging in de primaire database wordt gerepliceerd?
De geo-secundaire is een replica async en we niet proberen om het volledige synchroniseren met de primaire. Maar bieden we een methode voor het afdwingen van synchronisatie om te controleren of de replicatie van belangrijke wijzigingen (bijvoorbeeld bijwerken van wachtwoorden). Geforceerde synchronisatie van invloed op prestaties omdat de aanroepende thread worden geblokkeerd totdat alle doorgevoerde transacties worden gerepliceerd. Zie voor meer informatie [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Welke hulpprogramma's beschikbaar zijn voor de vertraging van replicatie tussen de primaire database en de secundaire geo-database controleren?
We tonen de vertraging realtime replicatie tussen de primaire database en de geo-secundaire via een DMV. Zie voor meer informatie [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Een database verplaatsen naar een andere server in hetzelfde abonnement
In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u een database in de lijst en klik vervolgens op **kopie**. Zie [kopiëren van een Azure SQL database](sql-database-copy.md) voor meer informatie.

## <a name="to-move-a-database-between-subscriptions"></a>Een database verplaatsen tussen abonnementen
In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-servers** en selecteer vervolgens de server die als host fungeert voor uw database in de lijst. Klik op **verplaatsen**, en selecteer vervolgens de resources te verplaatsen en het abonnement om naar te verplaatsen.
