---
title: Noodhersteloplossingen - Azure SQL-Database ontwerpen | Microsoft Docs
description: Informatie over het ontwerpen van uw cloudoplossing voor herstel na noodgevallen door het kiezen van het juiste failover-patroon.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 08/27/2018
ms.openlocfilehash: 9ed01103a83073d1bbb22f476a9115af816571aa
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166551"
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Strategieën voor noodherstel voor toepassingen die gebruikmaken van SQL Database elastische pools
In de afgelopen jaren hebben we geleerd dat cloudservices niet betrouwbare zijn en catastrofale incidenten optreden. SQL Database biedt verschillende mogelijkheden te geven voor de bedrijfscontinuïteit van uw toepassing wanneer deze incidenten optreden. [Elastische pools](sql-database-elastic-pool.md) en individuele databases ondersteunen dezelfde soort disaster recovery (DR) mogelijkheden. In dit artikel beschrijft verschillende strategieën voor herstel na Noodgevallen voor elastische pools die gebruikmaken van deze functies voor bedrijfscontinuïteit SQL-Database.

In dit artikel wordt het volgende patroon van canonical ISV-SaaS-toepassing:

<i>Een SQL-database inricht een moderne cloud-gebaseerde webtoepassing voor elke eindgebruiker. De ISV heeft veel klanten en daarom maakt gebruik van veel databases, bekend als de tenant-databases. Omdat de tenant-databases meestal onvoorspelbare activiteit hebben, de ISV maakt gebruik van een pool voor elastische database te maken de kosten zeer voorspelbare gedurende langere tijd. Beheer van de prestaties van vereenvoudigt de elastische pool ook wanneer de gebruikersactiviteit pieken. Naast de tenant-databases gebruikt de toepassing ook meerdere databases beheren gebruikersprofielen, beveiliging, verzamelen van gebruikspatronen enzovoort. Beschikbaarheid van de afzonderlijke tenants heeft geen invloed op de beschikbaarheid van de toepassing als geheel. Echter, de beschikbaarheid en prestaties van beheer van databases is van essentieel belang voor de functie van de toepassing en als de databases management offline zijn de gehele toepassing offline is.</i>  

Dit artikel worden besproken die betrekking hebben op een scala aan scenario's van kosten gevoelige opstarttoepassingen te met strikte beschikbaarheidsvereisten strategieën voor herstel na Noodgevallen.

> [!NOTE]
> Als u van Premium en bedrijfskritiek databases en elastische pools gebruikmaakt, kunt u ze flexibele bij regionale uitval door deze te converteren naar de configuratie van de zone-redundante implementatie. Zie [Zone-redundante databases](sql-database-high-availability.md).

## <a name="scenario-1-cost-sensitive-startup"></a>Scenario 1. Kosten van gevoelige opstarten
<i>Kan ik een eigen Start-up ben en ben zeer gevoelige kosten.  Ik wil Vereenvoudig de implementatie en beheer van de toepassing en ik kan een beperkte SLA voor individuele klanten. Maar ik wil zorgen voor de toepassing als geheel nooit offline is.</i>

Om te voldoen aan de vereiste eenvoud, alle tenantdatabases implementeren in een elastische pool in Azure-regio van uw keuze en beheer van databases als individuele databases geo-replicatie implementeren. Voor het herstel na noodgeval voor tenants, gebruikt u geo-restore, dat wordt geleverd zonder extra kosten. Om te garanderen de beschikbaarheid van de management-databases, geo-replicatie ze naar een andere regio met behulp van een automatische failover groep (stap 1). De lopende kosten van de configuratie van het herstel na noodgevallen in dit scenario is gelijk aan de totale kosten van de secundaire databases. Deze configuratie wordt weergegeven in het volgende diagram.

![Afbeelding 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Als er een storing optreedt in de primaire regio, worden de stappen om uw toepassing online aangegeven door het volgende diagram.

* De failovergroep initieert automatische failover van de database management naar de DR-regio. De toepassing automatisch wordt hersteld naar de nieuwe primaire en alle nieuwe accounts en tenant-databases zijn gemaakt in de DR-regio. De bestaande klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* De elastische pool maken met dezelfde configuratie als de oorspronkelijke groep (2).
* Gebruik geo-herstel kopieën van de tenant om databases te maken (3). U kunt overwegen de afzonderlijke herstelbewerkingen wordt geactiveerd door de eindgebruiker verbindingen of sommige andere toepassingsspecifieke prioriteit-schema worden gebruikt.


Op dit moment uw toepassing in de DR-regio weer online is, maar sommige klanten vertraging ondervindt bij het openen van hun gegevens.

![Afbeelding 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Als de storing tijdelijke is, is het mogelijk dat de primaire regio is hersteld door Azure voordat alle herstelbewerkingen voor de database voltooid in de DR-regio zijn. In dit geval indelen verplaatsen van de toepassing naar de primaire regio. Het proces duurt de stappen in het volgende diagram wordt geïllustreerd.

* Aanvragen voor alle openstaande geo-herstel annuleren.   
* Failover van de management-databases naar de primaire regio (5). Na het herstel van de regio komen de oude primaire automatisch secundaire replica's. Schakel nu ze over rollen opnieuw. 
* Wijzig de verbindingsreeks van de toepassing om te verwijzen naar de primaire regio. Nu worden alle nieuwe accounts en tenant-databases gemaakt in de primaire regio. Sommige bestaande klanten hun eigen gegevens zien tijdelijk niet beschikbaar.   
* Stel alle databases in de DR-pool voor alleen-lezen om te controleren of dat ze kunnen niet worden gewijzigd in de DR-regio (6). 
* Voor elke database in de DR-groep die is gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende databases in de primaire groep (7). 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (8). 
* Verwijderen van de DR-groep (9)

Uw toepassing is op dit moment online in de primaire regio met alle tenantdatabases beschikbaar zijn in de primaire groep.

![Afbeelding 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

De sleutel **profiteren** van deze strategie is lage constant doorlopende kosten voor de laag gegevensredundantie. Back-ups worden automatisch gemaakt door de service SQL Database met geen toepassing herschrijven en zonder extra kosten.  De kosten worden alleen wanneer de elastische databases worden hersteld. De **balans** is dat het volledig herstel van alle tenantdatabases aanzienlijke tijd. De hoeveelheid tijd is afhankelijk van het totale aantal herstelbewerkingen die u in de DR-regio starten en de totale grootte van de tenant-databases. Zelfs als u bepaalde tenants herstelbewerkingen boven andere prioriteit, wordt u concurrerende met alle andere herstelbewerkingen die geïnitieerd zijn in dezelfde regio bevinden als de service arbitrates servicequotum geldt en om te beperken de gevolgen zijn voor de bestaande klanten databases. Het herstel van de tenant-databases kan niet verder worden gestart totdat de nieuwe elastische groep in de DR-regio is gemaakt.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenario 2. Volwassen toepassing met gelaagde service
<i>Ik ben een goed ontwikkelde SaaS-toepassing met gelaagde service-aanbiedingen en verschillende Sla's voor de proefversie klanten en voor het betalen van klanten. Ik heb voor de proefversie klanten om de kosten zo veel mogelijk te beperken. Proefversie klanten downtime kunnen duren, maar ik wil de kans verkleinen. Voor de betalende klanten is geen uitvaltijd een risico vlucht. Dus ik om ervoor te zorgen dat betalende zijn klanten altijd toegang hebben tot hun gegevens.</i> 

Ter ondersteuning van dit scenario, de proeftenants van betaalde tenants worden gescheiden door ze in afzonderlijke elastische pools te plaatsen. De proefversie klanten hebben lagere edtu's of vCores per tenant en een lagere SLA met een langere hersteltijd. De betalende klanten zich in een groep met hogere edtu's of vCores per tenant en een hogere SLA. Om te garanderen de laagste hersteltijd, zijn de betalende klanten tenantdatabases geo-replicatie. Deze configuratie wordt weergegeven in het volgende diagram. 

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Zoals in het eerste scenario is de management-databases zijn heel actief, zodat u een database met geo-replicatie voor het (1 gebruiken). Dit zorgt ervoor dat de voorspelbare prestaties voor nieuwe abonnementen, profielupdates en andere beheerbewerkingen. De regio waarin de voorverkiezingen uit te brengen van de management-databases zich bevinden, is de primaire regio en de regio waarin de secundaire replica's van de management-databases zich bevinden, is de DR-regio.

De tenant-databases de betalende klanten hebben actieve databases in de 'betaalde' groep ingericht in de primaire regio. Richt een secundaire groep met dezelfde naam in de DR-regio. Elke tenant is geo-replicatie naar de secundaire groep (2). Hierdoor kunnen snel herstel van de tenantdatabases met behulp van failover. 

Als er een storing optreedt in de primaire regio, worden de herstelstappen te brengen van uw toepassing online weergegeven in het volgende diagram:

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Onmiddellijk een failover de beheer-databases naar de DR-regio (3).
* Wijzig de verbindingsreeks van de toepassing om te verwijzen naar de DR-regio. Nu worden alle nieuwe accounts en tenant-databases gemaakt in de DR-regio. De bestaande proefversie klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* Failover van de betaalde tenant-databases aan de groep in de DR-regio naar onmiddellijk herstel hun beschikbaarheid (4). Nadat de failover een snelle metagegevens niveau wijzigen is, kunt u overwegen een optimalisatie waar de afzonderlijke failovers op aanvraag worden geactiveerd door de eindgebruiker-verbindingen. 
* Als uw secundaire pool-eDTU grootte of vCore-waarde lager dan de primaire is omdat de secundaire databases de capaciteit voor het verwerken van de logboeken wijzigen terwijl ze secundaire replica's zijn alleen vereist, onmiddellijk de groepscapaciteit nu voor de volledige werkbelasting verhogen van alle tenants (5). 
* De nieuwe elastische pool maken met dezelfde naam en de dezelfde configuratie in de DR-regio voor de proefversie klanten databases (6). 
* Nadat de proefversie klanten groep is gemaakt, gebruikt u geo-herstel om terug te zetten van de afzonderlijke proefversie van uw tenant-databases naar de nieuwe pool (7). Houd rekening met de afzonderlijke herstelbewerkingen wordt geactiveerd door de eindgebruiker verbindingen of enige andere toepassingsspecifieke prioriteit-schema worden gebruikt.

Uw toepassing is op dit moment weer online in de DR-regio. Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging ondervinden bij het openen van hun gegevens.

Wanneer de primaire regio is hersteld door Azure *nadat* u de toepassing in de DR-regio die u kunt doorgaan met het uitvoeren van de toepassing in die regio hebt teruggezet of u kunt een failback uitvoeren naar de primaire regio. Als de primaire regio is hersteld *voordat* het failoverproces is voltooid, kunt u failback meteen. De failback worden de stappen in het volgende diagram wordt geïllustreerd: 

![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Aanvragen voor alle openstaande geo-herstel annuleren.   
* Failover van de management-databases (8). Na het herstel van de regio worden de oude primaire automatisch de secundaire server. Nu wordt de primaire opnieuw.  
* Failover van de betaalde tenant-databases (9). Op dezelfde manier, na het herstel van de regio, de oude primaire automatisch worden de secundaire replica's. Nu de primaire opnieuw worden. 
* Stel de herstelde proefversie databases die zijn gewijzigd in de DR-regio naar alleen-lezen (10).
* Wijzig of verwijder de bijbehorende database in de proefversie klanten primaire groep (11) voor elke database in de proefversie klanten DR-toepassingen die zijn gewijzigd sinds het herstel. 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (12). 
* Verwijderen van de DR-groep (13) 

> [!NOTE]
> De failoverbewerking is asynchroon. Als u wilt minimaliseren, de hersteltijd is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren. 
> 
> 

De sleutel **profiteren** van deze strategie is dat u de hoogste SLA voor de betalende klanten. Daarnaast garandeert u dat de nieuwe proefversies gedeblokkeerd zijn zodra de proefversie DR-groep is gemaakt. De **balans** is dat deze instelling zorgt voor de totale kosten van de tenant-databases met de kosten van de secundaire DR-groep voor klanten worden betaald. Bovendien als de secundaire groep een andere grootte heeft, de betalende klanten ondervinden lagere prestaties na een failover totdat de upgrade van toepassingen in de DR-regio is voltooid. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenario 3. Geografisch gedistribueerde toepassing met gelaagde service
<i>Ik heb een goed ontwikkelde SaaS-toepassing met de gelaagde service biedt. Ik wil bieden een zeer agressief SLA mijn betaalde klanten en het risico van impact minimaliseren wanneer er storingen optreden omdat zelfs korte onderbreking leiden ontevredenheid van de klant tot kan. Het is essentieel dat de betalende klanten altijd toegang hun gegevens tot. De experimenten zijn gratis en er een is geen Sla tijdens de proefperiode. </i> 

Gebruik ter ondersteuning van dit scenario, drie afzonderlijke elastische pools. Twee gelijke grootte van toepassingen met hoge edtu's of vCores per database in twee verschillende regio's bevatten de betaalde klanten tenant-databases inrichten. De derde groep met de proefversie tenants kan minder edtu's of vCores per database en worden ingericht in een van de twee regio's.

Om te garanderen de laagste hersteltijd tijdens een onderbreking, worden de betalende klanten tenantdatabases geo-replicatie met 50% van de primaire databases in elk van de twee regio's. Elke regio heeft op dezelfde manier, 50% van de secundaire databases. Op deze manier als een regio offline is, is slechts 50% van de betaalde klanten databases worden beïnvloed en er een failover. De andere databases blijven intact. Deze configuratie wordt weergegeven in het volgende diagram:

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Als in de vorige scenario's, de databases beheer van vrij actief zijn zodat ze configureren als individuele databases geo-replicatie (1). Hierdoor wordt de voorspelbare prestaties van de nieuwe klant abonnementen, profielupdates en andere beheerbewerkingen. Regio A is de primaire regio voor het beheer van databases en de regio B wordt gebruikt voor herstel van de management-databases.

De betalende klanten tenant-databases zijn ook geo-replicatie, maar met primaire en secundaire replica's verdeeld over verschillende regio's A en B (2) de regio. Op deze manier de primaire tenant-databases beïnvloed door de storing kunnen fungeren als failover voor de andere regio en beschikbaar is. De andere helft van de tenant-databases zijn niet helemaal te worden beïnvloed. 

Het volgende diagram illustreert de herstelstappen als er een storing optreedt in de regio A.

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Onmiddellijk een failover de beheer-databases naar regio B (3).
* De verbindingsreeks van de toepassing om te verwijzen naar de beheer-databases in de regio B. wijzigen de databases beheren om te controleren of de nieuwe accounts en tenant-databases worden gemaakt in de regio B en de bestaande tenant-databases zijn er gevonden ook wijzigen. De bestaande proefversie klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* Failover van de betaalde tenant-databases in de groep 2 in regio B onmiddellijk herstellen hun beschikbaarheid (4). Nadat de failover een snelle metagegevens niveau wijzigen is, kunt u overwegen een optimalisatie waar de afzonderlijke failovers op aanvraag worden geactiveerd door de eindgebruiker-verbindingen. 
* Sinds nu groep 2 bevat alleen primaire databases, de totale werkbelasting in de groep van toepassingen toeneemt en onmiddellijk kunt verhogen de eDTU-grootte (5) of het aantal vCores. 
* De nieuwe elastische pool maken met dezelfde naam en de dezelfde configuratie in de regio B voor de proefversie klanten databases (6). 
* Nadat de groep is gemaakt. Gebruik geo-herstel de afzonderlijke proefversie van uw tenant-database herstellen naar de pool (7). U kunt overwegen de afzonderlijke herstelbewerkingen wordt geactiveerd door de eindgebruiker verbindingen of sommige andere toepassingsspecifieke prioriteit-schema worden gebruikt.

> [!NOTE]
> De failoverbewerking is asynchroon. Om te beperken de hersteltijd, is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren. 
> 

Op dit moment is uw toepassing in de regio B. weer online Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging ondervinden bij het openen van hun gegevens.

Wanneer een regio is hersteld moet u beslissen of u gebruiken gebied B voor proefversie klanten of failback wilt naar gebruikmaken van de proefversie klanten in de regio A. Één criterium wordt mogelijk de % van de proefversie van uw tenant-databases die zijn gewijzigd nadat het herstel. Ongeacht deze beslissing moet u opnieuw de betaalde tenants tussen twee pools te verdelen. het volgende diagram illustreert het proces als de proefversie van uw tenant-databases een failback naar regio A.  

![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Aanvragen voor alle openstaande geo-herstel naar proefversie DR-groep annuleren.   
* Failover van de database management (8). Na het herstel van de regio, is de oude primaire automatisch de secundaire geworden. Nu wordt de primaire opnieuw.  
* Selecteer welke betaalde tenant databases een failback naar groep 1 en initiëren failover naar de secundaire replica's (9 uitvoeren). Na het herstel van de regio werden alle databases in 1 groep automatisch secundaire replica's. Nu weer 50% van deze voorverkiezingen uit te brengen. 
* Verklein de groep 2 van de oorspronkelijke eDTU (10) of het aantal vCores.
* Verzameling alle hersteld proefversie databases in de regio B naar alleen-lezen (11).
* Wijzig of verwijder de bijbehorende database in de proefversie primaire groep (12) voor elke database in de proefversie DR-groep die is gewijzigd sinds het herstel. 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (13). 
* Verwijderen van de DR-groep (14) 

De sleutel **voordelen** van deze strategie zijn:

* Ondersteunt de agressief SLA voor de betalende klanten omdat het ervoor zorgt dat een storing kan niet van invloed zijn op meer dan 50% van de tenant-databases. 
* Dit zorgt ervoor dat de nieuwe proefversies gedeblokkeerd zijn zodra de audittrail DR-groep is gemaakt tijdens het herstel. 
* Hierdoor efficiënter gebruik van de groepscapaciteit als 50% van de secundaire databases in de groep 1 en 2 van toepassingen worden gegarandeerd minder dan de primaire databases actief zijn.

De belangrijkste **wisselwerking** zijn:

* De CRUD-bewerkingen voor het beheer van databases hebben een lagere latentie voor de eindgebruikers die is verbonden met een dan de regio voor de eindgebruikers die is verbonden met de regio B als ze worden uitgevoerd op de primaire database management.
* Complexere ontwerp van de database management is vereist. Elke record van de tenant heeft bijvoorbeeld een locatie-code die moet worden gewijzigd tijdens de failover en failback.  
* De betalende klanten ondervinden lagere prestaties dan normaal totdat de upgrade van toepassingen in de regio B is voltooid. 

## <a name="summary"></a>Samenvatting
In dit artikel richt zich op de strategieën voor noodherstel voor de databaselaag die worden gebruikt door een ISV-SaaS-toepassing met meerdere tenants. De strategie die u kiest is gebaseerd op de behoeften van de toepassing, zoals het bedrijfsmodel, de SLA die u wilt aanbieden aan uw klanten en budgetstatus beperking enzovoort. Elke beschreven strategie geeft een overzicht van de voordelen en de verhouding, zodat u een gefundeerde beslissing nemen kunt. Uw specifieke toepassing waarschijnlijk bevat ook andere Azure-onderdelen. Zodat u de richtlijnen van hun zakelijke continuïteit controleren en het herstellen van de databaselaag met hen. Raadpleeg voor meer informatie over het beheren van herstel van database-toepassingen in Azure, [cloudoplossingen ontwerpen voor herstel na noodgevallen](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
* Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor archivering, [kopiëren van de database](sql-database-copy.md).

