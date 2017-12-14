---
title: Ontwerp noodhersteloplossingen - Azure SQL Database | Microsoft Docs
description: Informatie over het ontwerpen van uw cloudoplossing voor herstel na noodgevallen voor het kiezen van het juiste failover-patroon.
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2db99057-0c79-4fb0-a7f1-d1c057ec787f
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.workload: Inactive
ms.openlocfilehash: 9d12fb8a7dbd3bb763e42fd0981d7ef18b57248b
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Disaster recovery strategieën voor toepassingen met elastische pools SQL-Database
In de afgelopen jaren hebt u geleerd dat cloudservices niet betrouwbare zijn en onherstelbare incidenten optreden. SQL-Database biedt verschillende mogelijkheden op te geven voor de bedrijfscontinuïteit van uw toepassing, wanneer deze incidenten optreden. [Elastische pools](sql-database-elastic-pool.md) en individuele databases ondersteunen dezelfde soort mogelijkheden voor herstel na noodgevallen. In dit artikel beschrijft enkele DR strategieën voor elastische pools die gebruikmaken van deze functies van SQL Database business continuity.

In dit artikel maakt gebruik van patroon van de volgende canonieke ISV SaaS-toepassing:

<i>Een moderne cloud-gebaseerde webtoepassing voorziet in een SQL-database voor elke gebruiker. De ISV veel klanten gebruikt, en daarom veel databases, tenant-databases genoemd. Omdat de tenant-databases doorgaans onvoorspelbare activiteit patronen hebben, gebruikt de ISV een elastische pool de database te maken kosten zeer voorspelbare gedurende langere perioden. De elastische groep vereenvoudigt u ook het prestatiebeheer wanneer de gebruikersactiviteit bereikt. Naast de tenant-databases gebruikt de toepassing ook verschillende databases beheren gebruikersprofielen, beveiliging, verzamelen gebruikspatronen enzovoort. Beschikbaarheid van de afzonderlijke tenants heeft geen gevolgen voor de beschikbaarheid van de toepassing als geheel. Echter, de beschikbaarheid en prestaties van databases management is essentieel voor een functie van de toepassing en als de databases management offline zijn de gehele toepassing offline is.</i>  

Dit artikel wordt beschreven DR strategieën die betrekking hebben op een aantal scenario's uit kosten gevoelige starten van toepassingen met strenge beschikbaarheidsvereisten.

## <a name="scenario-1-cost-sensitive-startup"></a>Scenario 1. Kosten gevoelige opstarten
<i>Ik ben een bedrijf opstarten en zeer gevoelige ben kosten.  Ik wil eenvoudiger implementatie en beheer van de toepassing en kan ik een SLA beperkt hebben voor individuele klanten. Maar ik wil zorgen voor de toepassing als geheel nooit offline is.</i>

Om te voldoen aan de vereiste eenvoud, alle databases van de tenant implementeren in een elastische pool in de Azure-regio van uw keuze en beheer databases als individuele databases geogerepliceerde implementeren. Voor het herstel na noodgevallen van tenants, gebruikt u geo-herstel, wordt geleverd zonder extra kosten. Om te controleren of de beschikbaarheid van de management-databases, geo-replicatie ze naar een andere regio met behulp van een auto-failover groep (in-preview) (stap 1). De lopende kosten van de configuratie van het herstel na noodgevallen in dit scenario is gelijk aan de totale kosten van de secundaire databases. Deze configuratie wordt weergegeven in het volgende diagram.

![Afbeelding 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Als er een storing optreedt in de primaire regio, worden de herstelstappen te brengen van uw toepassing online geïllustreerd door het volgende diagram.

* De groep failover initieert automatische failover van de management-database naar de DR-regio. De toepassing automatisch wordt hersteld naar de nieuwe primaire en alle nieuwe accounts en tenant-databases in de DR-regio worden gemaakt. De bestaande klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* Maak de elastische groep met dezelfde configuratie als de oorspronkelijke groep (2).
* Geo-restore kopieën te maken van de tenant databases (3) gebruiken. U kunt overwegen de activering van de afzonderlijke herstelacties door de eindgebruiker verbindingen of sommige andere toepassingsspecifieke prioriteit schema gebruiken.


Op dit moment uw toepassing in het gebied DR weer online is, maar sommige klanten krijgen met vertraging bij het openen van hun gegevens.

![Afbeelding 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Als de storing tijdelijke is, is het mogelijk dat de primaire regio door Azure wordt hersteld voordat alle herstelbewerkingen voor de database in de regio DR voltooid zijn. In dit geval indelen verplaatsen van de toepassing terug naar de primaire regio. Het proces neemt de stappen in het volgende diagram wordt geïllustreerd.

* Annuleer alle openstaande geo-restore-aanvragen.   
* Failover van de management-databases naar de primaire regio (5). De oude primaire na het herstel van de regio zijn automatisch secundaire replica's geworden. Nu overgeschakeld rollen opnieuw. 
* Wijzig de verbindingsreeks van de toepassing om te verwijzen naar de primaire regio. Nu worden alle nieuwe accounts en tenant-databases gemaakt in de primaire regio. Sommige klanten hun eigen gegevens zien tijdelijk niet beschikbaar.   
* Stel alle databases in de groep DR op alleen-lezen om te controleren of dat ze kunnen niet worden gewijzigd in de regio voor Noodherstel (6). 
* Voor elke database in de DR-groep die is gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende databases in de primaire groep (7). 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (8). 
* DR-groep (9) verwijderen

De toepassing is op dit moment online in de primaire regio met alle tenant databases beschikbaar in de primaire groep.

![Afbeelding 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

De sleutel **profiteren** is van deze strategie lage lopende kosten voor gegevensredundantie laag. Back-ups worden automatisch gemaakt door de service SQL Database met geen herschrijven van de toepassing en zonder extra kosten.  De kosten worden alleen wanneer de elastische databases zijn hersteld. De **nadeel** is dat het volledig herstel van alle databases van de tenant aanzienlijke tijd. De hoeveelheid tijd is afhankelijk van het totaal aantal herstelacties die u in de regio DR starten en de totale grootte van de tenant-databases. Zelfs als u bepaalde tenants herstelacties boven andere prioriteit, concurreren alle andere herstelacties die geïnitieerd zijn in dezelfde regio als de service arbitrates en beperkt om te beperken van de algehele impact op de bestaande klanten databases. Bovendien kan het herstel van de tenant-databases niet starten totdat de nieuwe elastische pool in de regio DR wordt gemaakt.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenario 2. Volwassen toepassing met gelaagde service
<i>Ik ben een goed ontwikkelde SaaS-toepassing met gelaagde service aanbiedingen en andere serviceovereenkomsten voor proefversie klanten en voor het betalen van klanten. Ik heb de zo veel mogelijk kosten voor de proefversie klanten. Proefversie klanten uitvaltijd kunnen duren, maar ik wil verminderen de kans op. Uitvaltijd is voor de betalende klanten vlucht risico. Klanten zijn altijd toegang tot hun gegevens dus ik wil ervoor zorgen dat betaalt.</i> 

Ter ondersteuning van dit scenario, de proeftenants van betaald tenants worden gescheiden door ze in afzonderlijke elastische pools te plaatsen. De evaluatieversie klanten hebben lagere eDTU per tenant en lagere SLA met een langere periode voor herstel. De betalende klant zijn in een pool met hogere eDTU per tenant en een hogere SLA. Om te waarborgen de laagste hersteltijd, zijn de betalende klant tenant databases geo-replicatie. Deze configuratie wordt weergegeven in het volgende diagram. 

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Zoals in het eerste scenario zijn de databases management behoorlijk actief, zodat u één geogerepliceerde database voor (1 gebruiken). Hierdoor worden de voorspelbare prestaties voor het nieuwe klantabonnementen profielupdates en andere beheerbewerkingen. De regio waarin de primaire van de management-databases zich bevinden, is de primaire regio en de regio waarin de secundaire replica's van de management-databases zich bevinden, is de DR-regio.

De betalende klant tenant databases hebt active databases in de 'betaald' groep ingericht in de primaire regio. Een secundaire groep met dezelfde naam in de regio DR inrichten. Elke tenant is geo-replicatie naar de secundaire groep (2). Hierdoor kunnen snel herstel van de tenant-databases met behulp van failover. 

Als er een storing optreedt in de primaire regio, worden de herstelstappen te brengen van uw toepassing online weergegeven in het volgende diagram:

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Onmiddellijk een failover de beheer-databases naar de DR regio (3).
* Wijzig de verbindingsreeks van de toepassing om te verwijzen naar de DR-regio. Nu worden alle nieuwe accounts en tenant-databases gemaakt in de regio Noodherstel. De bestaande proefversie klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* Failover-overschakeling van de betaalde tenant databases in de groep in de regio DR direct herstellen hun beschikbaarheid (4). Nadat de failover een wijziging van de snelle metagegevens-niveau is, houd rekening met een optimalisatie waar de afzonderlijke failovers op aanvraag worden geïnitieerd door de eindgebruiker-verbindingen. 
* Als de grootte van uw secundaire pool-eDTU lager dan de primaire is omdat de secundaire databases worden alleen de capaciteit voor het verwerken van de logboeken wijzigen terwijl ze secundaire replica's zijn vereist, onmiddellijk de groepscapaciteit nu voor de volledige werkbelasting van alle tenants (verhogen 5). 
* De nieuwe elastische pool maken met dezelfde naam en de dezelfde configuratie in de regio Noodherstel voor de proefversie klanten databases (6). 
* Zodra de proefversie klanten groep is gemaakt, gebruikt u geo-herstel de afzonderlijke proeftenant databases herstellen naar de nieuwe toepassingen (7). Houd rekening met de afzonderlijke herstelacties door de eindgebruiker verbindingen activerende of sommige andere toepassingsspecifieke prioriteit schema gebruiken.

De toepassing is op dit moment weer online in DR regio. Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging ondervinden bij het openen van hun gegevens.

Wanneer de primaire regio worden hersteld door Azure *nadat* de toepassing in de DR-regio die u kunt doorgaan met het uitvoeren van de toepassing in deze regio is hersteld of u kunt een failback naar de primaire regio. Als de primaire regio is hersteld *voordat* het failoverproces is voltooid, kunt mislukt back meteen. De failback neemt de stappen die wordt weergegeven in het volgende diagram: 

![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Annuleer alle openstaande geo-restore-aanvragen.   
* Failover van de management-databases (8). Wanneer de regio is hersteld worden de oude primaire automatisch de secundaire. Nu wordt de primaire opnieuw.  
* Failover van de databases betaald tenant (9). Op deze manier na het herstel van de regio, de oude primaire automatisch worden de secundaire replica's. Nu worden de primaire opnieuw. 
* Stel de herstelde proefversie databases die in de regio DR hebt gewijzigd in alleen-lezen (10).
* Voor elke database in de proefversie klanten DR-toepassingen die gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende database in de proefversie klanten primaire groep (11). 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (12). 
* DR-groep (13) verwijderen 

> [!NOTE]
> De failoverbewerking is asynchroon. De hersteltijd is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren minimaliseren. 
> 
> 

De sleutel **profiteren** van deze strategie is dat u de hoogste SLA voor de betalende klanten. Ook wordt hiermee gegarandeerd dat de nieuwe proefversies geblokkeerd zijn als de proef DR-adresgroep is gemaakt. De **nadeel** klanten is dat deze installatie verhoogt de totale kosten van de databases van de tenant door de kosten van de secundaire DR-toepassingen voor betaald. Bovendien, als de secundaire groep een andere grootte heeft, de betalende klanten ondervinden lagere prestaties na een failover totdat de upgrade van toepassingen in de regio DR is voltooid. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenario 3. Geografisch verspreide toepassing met gelaagde service
<i>Ik heb een goed ontwikkelde SaaS-toepassing met gelaagde service biedt. Ik wil een zeer agressief SLA aan mijn betaald klanten te bieden en het risico van impact minimaliseren wanneer er storingen optreden omdat zelfs korte onderbreking van de klant ergernis kan veroorzaken. Het is essentieel dat de betalende klant altijd toegang hun gegevens tot. De proefversies zijn gratis en een SLA niet tijdens de proefperiode wordt aangeboden.</i> 

Gebruik ter ondersteuning van dit scenario, drie afzonderlijke elastische pools. Voorzien in twee groepen van gelijke grootte met hoge edtu's per database in twee verschillende regio's voor de betaalde klanten tenant databases bevatten. De derde-groep met de proeftenants kan lagere edtu's per database en in een van de twee regio's worden ingericht.

Om te waarborgen de laagste hersteltijd tijdens uitval, zijn de betalende klant tenant databases geogerepliceerde met 50% van de primaire databases in elk van de twee regio's. Daarnaast heeft elke regio 50% van de secundaire databases. Op deze manier als een regio offline is, slechts 50% van de betaalde klanten databases worden beïnvloed en failover. De andere databases blijven behouden. Deze configuratie wordt weergegeven in het volgende diagram:

![Afbeelding 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Als in de vorige scenario's en de databases management behoorlijk actief zijn zodat ze configureren als individuele databases geogerepliceerde (1). Hierdoor wordt de voorspelbare prestaties van de klant nieuwe abonnementen, profielupdates en andere beheerbewerkingen. Regio A is de primaire regio voor de beheer-databases en de regio B wordt gebruikt voor herstel van de management-databases.

De betalende klant tenant databases zijn ook geogerepliceerde maar met primaire en secundaire servers verdeeld tussen regio A en B (2) de regio. Op deze manier de tenant primaire databases beïnvloed door de storing kunnen failover naar de andere regio en beschikbaar. De andere helft van de tenant-databases zijn niet van invloed op alle. 

Het volgende diagram illustreert de herstelstappen te nemen als er een storing optreedt in regio A.

![Afbeelding 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Onmiddellijk een failover de beheer-databases naar regio B (3).
* De verbindingstekenreeks van de toepassing verwijst naar de databases management in regio B. wijzigen van de management-databases te controleren of de nieuwe accounts en tenant-databases worden gemaakt in de regio B en de bestaande tenant databases vindt u er ook wijzigen. De bestaande proefversie klanten hun eigen gegevens zien tijdelijk niet beschikbaar.
* Failover-overschakeling van de betaalde tenant databases aan groep 2 in regio B direct herstellen hun beschikbaarheid (4). Nadat de failover een wijziging van de snelle metagegevens-niveau is, kunt u overwegen een optimalisatie waar de afzonderlijke failovers op aanvraag worden geïnitieerd door de eindgebruiker-verbindingen. 
* Omdat nu toepassingen 2 bevat alleen primaire databases, de totale werkbelasting in de groep toeneemt en mogelijk onmiddellijk groter eDTU (5). 
* De nieuwe elastische pool maken met dezelfde naam en dezelfde configuratie in het gebied van B voor de proefversie klanten databases (6). 
* Zodra de groep is gemaakt. Gebruik geo-herstel de afzonderlijke proeftenant-database herstellen in de groep (7). U kunt overwegen de activering van de afzonderlijke herstelacties door de eindgebruiker verbindingen of sommige andere toepassingsspecifieke prioriteit schema gebruiken.

> [!NOTE]
> De failoverbewerking is asynchroon. Om te beperken de hersteltijd, is het belangrijk dat u de tenant-databases failover-opdracht in batches van ten minste 20 databases uitvoeren. 
> 

Op dit moment is uw toepassing weer online in regio B. Alle betalende klanten hebben toegang tot hun gegevens tijdens de proefperiode klanten vertraging ondervinden bij het openen van hun gegevens.

Wanneer een regio kunnen worden hersteld moet u bepalen of u wilt gebruiken gebied B voor proefversie klanten of failback gebruikmaken van de evaluatieversie klanten in regio A. Een criteria zijn de % van de proefversie van uw tenant-databases is gewijzigd sinds het herstel. Ongeacht deze beslissing moet u de betaalde tenants tussen twee groepen opnieuw verdelen. het volgende diagram illustreert het proces als de databases proeftenant failback naar regio A.  

![Afbeelding 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Alle openstaande geo-restore aanvragen aan proefversie DR-groep annuleren.   
* Failover van de management-database (8). Na het herstel van de regio is de oude primaire automatisch de secundaire geworden. Nu wordt de primaire opnieuw.  
* Selecteer welke betaald tenant databases failback naar groep 1 en initiëren failover naar de secundaire replica's (9). Na het herstel van de regio werden alle databases in de groep 1 automatisch secundaire replica's. Nu 50% van deze primaire opnieuw worden. 
* Reduceer de grootte van de groep 2 in de oorspronkelijke eDTU (10).
* Alle hersteld proefversie databases in de regio B naar alleen-lezen (11).
* Voor elke database in de proefversie DR-groep die is gewijzigd sinds het herstel, hernoemen of verwijderen van de bijbehorende database in de proefversie primaire groep (12). 
* Kopieer de bijgewerkte databases uit de DR-groep aan de primaire groep (13). 
* DR-groep (14) verwijderen 

De sleutel **voordelen** van deze strategie zijn:

* Het ondersteunt de agressief SLA voor de betalende klanten omdat Hiermee zorgt u ervoor dat een storing kan geen invloed heeft op meer dan 50% van de tenant-databases. 
* Dit zorgt ervoor dat de nieuwe proefversies geblokkeerd zijn zodra de audittrail DR-groep is gemaakt tijdens het herstel. 
* Kunt u efficiënter gebruikmaken van de groepscapaciteit als 50% van de secundaire databases in groep 1 en 2 van de toepassingen zijn gegarandeerd minder actieve dan de primaire-databases.

De belangrijkste **-en nadelen** zijn:

* De CRUD-bewerkingen op de management-databases hebben lagere latentie voor de eindgebruikers die is verbonden met een dan de regio voor de eindgebruikers verbonden regio B als ze worden uitgevoerd op basis van de primaire database management.
* Complexere ontwerp van de management-database vereist. Elke record tenant heeft bijvoorbeeld een locatiecode die moet worden gewijzigd tijdens failover en failback.  
* De betalende klanten ondervinden lagere prestaties dan normaal totdat de upgrade van toepassingen in de regio B is voltooid. 

## <a name="summary"></a>Samenvatting
Dit artikel is gericht op de strategieën voor het herstel na noodgevallen voor de databaselaag gebruikt door een multitenant SaaS ISV toepassing. De strategie die u kiest is gebaseerd op de behoeften van de toepassing, zoals het bedrijfsmodel, de SLA die u wenst te bieden aan uw klanten, budget beperking enzovoort. Een strategie voor elke beschreven licht de voordelen en een afweging zodat u een gefundeerde beslissing nemen kunt. Uw specifieke toepassing die waarschijnlijk is tevens andere Azure-onderdelen. Daarom vindt u de richtlijnen van hun zakelijke continuïteit en het herstel van de databaselaag met hen te organiseren. Raadpleeg voor meer informatie over het beheren van herstel van databasetoepassingen in Azure, [cloudoplossingen ontwerpen voor herstel na noodgevallen](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity overview](sql-database-business-continuity.md).
* Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een database herstellen vanuit back-ups service geïnitieerde](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-geo-replication-overview.md).
* Zie voor meer informatie over het gebruik van automatische back-ups voor archivering, [kopiëren van de database](sql-database-copy.md).

