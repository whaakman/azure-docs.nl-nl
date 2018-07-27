---
title: Wereldwijd beschikbare services met behulp van Azure SQL-Database ontwerpen | Microsoft Docs
description: Leer meer over toepassingsontwerp voor maximaal beschikbare services met behulp van Azure SQL Database.
keywords: herstel na noodgevallen, oplossingen voor herstel na noodgevallen, back-up van app-gegevens, geo-replicatie, in de cloud business continuity plannen
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 88a6e1a66390b2b317e1e30a71455ad693e6d7df
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39264651"
---
# <a name="designing-globally-available-services-using-azure-sql-database"></a>Algemeen beschikbare services met behulp van Azure SQL-Database ontwerpen

Tijdens het bouwen en implementeren van cloudservices met Azure SQL Database, gebruikt u [failover-groepen en actieve geo-replicatie](sql-database-geo-replication-overview.md) voor herstelmogelijkheden bij regionale storingen en onherstelbare fouten. De functie kunt u wereldwijd gedistribueerde toepassingen die zijn geoptimaliseerd voor lokale toegang tot de gegevens maken. Dit artikel worden algemene toepassingspatronen, met inbegrip van de voor- en -en nadelen van elke optie. 

> [!NOTE]
> Als u van Premium en bedrijfskritiek databases en elastische pools gebruikmaakt, kunt u ze flexibele bij regionale uitval door deze te converteren naar de configuratie van de zone-redundante implementatie. Zie [Zone-redundante databases](sql-database-high-availability.md).  

## <a name="scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime"></a>Scenario 1: Met behulp van twee Azure-regio's voor bedrijfscontinuïteit met minimale downtime
In dit scenario hebben de toepassingen de volgende kenmerken: 
*   Toepassing is actief in een Azure-regio
*   Alle Databasesessies vereisen lezen en schrijven (RW) aan gegevens
*   Weblaag en de gegevenslaag moeten worden geplaatst om te verminderen latentie en verkeer kosten 
*   Fundamentele, downtime is een hogere bedrijfsrisico voor deze toepassingen dan verlies van gegevens

In dit geval is de topologie van de implementatie van toepassing geoptimaliseerd voor het verwerken van regionale rampen als alle onderdelen van de toepassing samen failover plaatsvindt moeten. In het onderstaande diagram ziet u deze topologie. Voor geografische redundantie worden van de toepassing resources geïmplementeerd in de regio A en B. De resources in de regio B zijn echter niet gebruikt, totdat een regio uitvalt. Een failovergroep is geconfigureerd tussen de twee regio's voor het beheren van verbinding met de database, replicatie en failover. De webservice in beide regio's is geconfigureerd voor toegang tot de database via de lezen / schrijven-listener  **&lt;failover-groepsnaam&gt;. database.windows.net** (1). Traffic manager is ingesteld voor het gebruik [prioriteit routeringsmethode](../traffic-manager/traffic-manager-configure-priority-routing-method.md) (2).  

> [!NOTE]
> [Azure traffic manager](../traffic-manager/traffic-manager-overview.md) ter illustratie wordt alleen in dit artikel wordt gebruikt. U kunt een load balancing-oplossing die ondersteuning biedt voor de routeringsmethode prioriteit.    
>

Deze configuratie voordat een storing in het volgende diagram wordt weergegeven:

![Scenario 1. De configuratie voordat de storing.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-a.png)

Na een storing in de primaire regio detecteert de service SQL Database dat de primaire database niet toegankelijk is en failover naar de secundaire regio op basis van de parameters van het automatische failover-beleid (1 activeert). Afhankelijk van de SLA van uw toepassing, kunt u een respijtperiode die de tijd tussen de detectie van de serviceonderbreking en de failover zelf bepaalt configureren. Het is mogelijk dat de eindpunt-failover van traffic manager wordt gestart voordat de failover-groep met de failover van de database wordt geactiveerd. In dat geval de web-App niet onmiddellijk opnieuw verbinding maken met de database. Maar de inlogmodus worden automatisch uitgevoerd zodra de databasefailover is voltooid. Als de mislukte regio teruggezet en weer online is, wordt de oude primaire automatisch opnieuw verbinding als een nieuwe secundaire. Het volgende diagram ziet u de configuratie na een failover.
 
> [!NOTE]
> Alle transacties na de failover zijn verloren gaan tijdens het opnieuw verbinden. Nadat de failover is voltooid, kan de toepassing in de regio B opnieuw verbinding maken met en opnieuw starten van de gebruiker verwerken. Zowel de web-App als de primaire database zijn nu in de regio B en blijven CO-locaties. n>

![Scenario 1. Configuratie na een failover](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-b.png)

Als een storing in de regio B gebeurt, het replicatieproces tussen de primaire en secundaire database wordt onderbroken, maar de koppeling tussen de twee blijft intact (1). Verkeer die worden beheerd detecteert dat de verbinding met de regio B verbroken is en wordt de eindpunt-web-app 2 gemarkeerd als gedegradeerd (2). Prestaties van de toepassing wordt in dit geval niet beïnvloed, maar de database beschikbaar wordt gesteld en daarom hoger risico van gegevensverlies in geval regio een mislukt achter elkaar.

> [!NOTE]
> Voor herstel na noodgevallen raden wij de configuratie met de implementatie van de toepassing is beperkt in twee regio's. Dit is omdat de meeste van de Azure-geografieën alleen twee regio's hebben. Deze configuratie biedt geen bescherming voor uw toepassing van een gelijktijdige catastrofale uitval van beide regio's. U kunt uw databases in een derde regio met herstellen in het onwaarschijnlijke geval van een dergelijke storing, [geo-restore-bewerking](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

 Zodra de onderbreking is verholpen, wordt de secundaire database automatisch opnieuw gesynchroniseerd met de primaire. Tijdens de synchronisatie, kan de prestaties van de primaire worden beïnvloed. De specifieke impact afhankelijk is van de hoeveelheid gegevens van de nieuwe primaire verkregen nadat de failover. Het volgende diagram illustreert een storing in de secundaire regio:

![Scenario 1. Configuratie na een storing in de secundaire regio.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-c.png)

De sleutel **voordelen** van dit ontwerppatroon zijn:

* De dezelfde web-App is geïmplementeerd op beide regio's zonder configuratie regiospecifieke en vereist geen aanvullende logica voor het beheren van failover. 
* De prestaties van toepassingen worden niet beïnvloed door failover als de web-App en de database zijn altijd dezelfde locatie bevindt.

De belangrijkste **negatieve gevolgen voor de** is dat de toepassingsresources in de regio B deel van de tijd volledig worden benut.

## <a name="scenario-2-azure-regions-for-business-continuity-with-maximum-data-preservation"></a>Scenario 2: Azure-regio's voor bedrijfscontinuïteit met behoud van gegevens
Deze optie is het meest geschikt voor toepassingen met de volgende kenmerken:

* Verlies van gegevens is hoog bedrijfsrisico. De databasefailover kan alleen worden gebruikt als een laatste toevlucht als de storing wordt veroorzaakt door een onherstelbare fout.
* De toepassing ondersteunt alleen-lezen en lees-/ modi van bewerkingen en u kunt gebruiken in 'alleen-lezen modus' voor een bepaalde periode.

In dit patroon, de toepassing schakelt over naar modus alleen-lezen wanneer de verbindingen lezen / schrijven beginnen met het ophalen van de time-outfouten optreden. De Web-App wordt geïmplementeerd voor beide regio's en voeg een verbinding met het lezen / schrijven-listener-eindpunt en verschillende verbinding met het kenmerk alleen-lezen-listener-eindpunt (1). Het Traffic manager-profiel moet worden gebruikt [prioriteit routering](../traffic-manager/traffic-manager-configure-priority-routing-method.md). [Eindpunt controleren](../traffic-manager/traffic-manager-monitoring.md) moet worden ingeschakeld voor het toepassingseindpunt van de in elke regio (2).

Het volgende diagram illustreert deze configuratie voordat een storing:

![Scenario 2. De configuratie voordat de storing.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-a.png)

Wanneer u traffic manager detecteert een verbindingsfout naar een regio, overgeschakeld het gebruikersverkeer automatisch naar de toepassingsexemplaar in de regio B. Met dit patroon is het belangrijk dat u de respijtperiode verlies van gegevens naar een voldoende hoge waarde, bijvoorbeeld 24 uur. Het zorgt ervoor dat gegevens verloren gaan als de onderbreking is verholpen in die tijd wordt voorkomen. Wanneer de Web-App in de regio B is geactiveerd mislukken de lezen / schrijven-bewerkingen. Het moet op dat moment overschakelen naar de modus alleen-lezen (1). In deze modus worden de aanvragen automatisch doorgestuurd naar de secundaire database. Als de onderbreking is veroorzaakt door een onherstelbare fout, niet kan waarschijnlijk het worden verholpen binnen de respijtperiode. Wanneer het verloopt de failover-groep triggers de failover. Na de lezen / schrijven-listener is beschikbaar en de verbindingen met het stoppen (2) mislukt. Het volgende diagram illustreert de twee fasen van het herstelproces.

> [!NOTE]
> Als de storing in de primaire regio binnen de respijtperiode is beperkt, traffic manager detecteert dat het herstel van de verbinding in de primaire regio en gebruikersverkeer schakelt terug naar het exemplaar van de toepassing in de regio A. Dit exemplaar van de toepassing wordt hervat en werkt in de modus lezen / schrijven met behulp van de primaire database in de regio A zoals wordt geïllustreerd door het vorige diagram.
>

![Scenario 2. Disaster recovery fasen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-b.png)

Als een storing in de regio B gebeurt, detecteert de traffic manager het uitvallen van het eindpunt web-app-2 in de regio B en markeert het gedegradeerd (1). De failovergroep verandert in de tussentijd de alleen-lezen-listener voor een regio (2). Deze onderbreking heeft geen invloed op de ervaring van eindgebruikers, maar de primaire database is beschikbaar tijdens de storing. Het volgende diagram wordt een fout in de secundaire regio:

![Scenario 2. Onderbreking van de secundaire regio.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-c.png)

Zodra de onderbreking is verholpen, de secundaire database is direct gesynchroniseerd met de primaire en de alleen-lezen-listener is overgeschakeld naar de secundaire database in de regio B. Tijdens de synchronisatie kan de prestaties van de primaire enigszins worden beïnvloed, afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd.

Dit ontwerppatroon heeft diverse **voordelen**:

* Wordt voorkomen dat er gegevens verloren gaan tijdens de tijdelijke uitval.
* Uitvaltijd is alleen afhankelijk van hoe snel de verbindingsfout, die kan geconfigureerd worden in traffic manager wordt gedetecteerd.

De **negatieve gevolgen voor de** is dat de toepassing kunnen moet werken in de modus alleen-lezen.

## <a name="scenario-3-application-relocation-to-a-different-geography-without-data-loss-and-near-zero-downtime"></a>Scenario 3: Toepassing verplaatsingen naar een andere Geografie bevinden zonder verlies van gegevens en in de buurt van uitvaltijd 
In dit scenario heeft de toepassing de volgende kenmerken: 
* De eindgebruikers toegang tot de toepassing vanaf verschillende fysieke locaties
* De toepassing bevat alleen-lezen-werkbelastingen die niet afhankelijk van volledige synchronisatie met de meest recente updates
* Schrijftoegang tot de gegevens moeten in hetzelfde geografische gebied voor het merendeel van de gebruikers worden ondersteund 
* Lezen latentie is essentieel voor de eindgebruiker 


Om te voldoen aan deze vereisten die u nodig hebt om te garanderen dat het apparaat van de gebruiker **altijd** verbinding maakt met de toepassing geïmplementeerd in hetzelfde geografische gebied voor de alleen-lezen bewerkingen, zoals browsegegevens, analytics, enzovoort. Terwijl de OLTP-bewerkingen worden verwerkt in hetzelfde geografische gebied **van de tijd die de meeste**. Bijvoorbeeld, gedurende de dag tijd OLTP-bewerkingen in hetzelfde geografische gebied worden verwerkt, maar buiten kantooruren kunnen ze worden verwerkt in een andere Geografie bevinden. Als de eindgebruiker activiteit meestal tijdens de werkuren gebeurt, kunt u garanderen dat de optimale prestaties voor de meeste van de gebruikers de meeste van de tijd. Het volgende diagram toont deze topologie. 
 
Resources van de toepassing moeten worden geïmplementeerd in elke geografische locatie waar u substantieel gebruik aanvraag hebt. Bijvoorbeeld, als uw toepassing actief in de Verenigde Staten gebruikt wordt, moeten Zuidoost-Azië de toepassing en de Europese Unie worden geïmplementeerd op alle van deze locaties. De primaire database moet worden dynamisch overgeschakeld van één Geografie naar het volgende aan het einde van de werkuren. Deze methode wordt aangeroepen 'volgen de zon'. De OLTP-werkbelasting wordt altijd verbinding met de database via de lezen / schrijven-listener  **&lt;failover-groepsnaam&gt;. database.windows.net** (1). De werkbelasting voor alleen-lezen verbinding maakt met de lokale database rechtstreeks met behulp van het servereindpunt databases  **&lt;-servernaam&gt;. database.windows.net** (2). Traffic manager is geconfigureerd met de [routeringsmethode voor prestaties](../traffic-manager/traffic-manager-configure-performance-routing-method.md). Het zorgt ervoor dat de eindgebruiker apparaat is verbonden met de webservice in de dichtstbijzijnde regio. Traffic manager moet worden ingesteld met het bewaken van het eindpunt is ingeschakeld voor elke web service-eindpunt (3).

> [!NOTE]
> De configuratie van de failover wordt gedefinieerd welke regio wordt gebruikt voor failover. Omdat de nieuwe primaire in een andere Geografie bevinden de failover-leidt tot een langere latentie voor zowel OLTP als alleen-lezen-werkbelastingen is totdat de betrokken regio weer online is.
>

![Scenario 3. Configuratie met primaire in VS-Oost.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-a.png)

Aan het einde van de dag (bijvoorbeeld om 23: 00 uur lokale tijd) moeten de actieve databases worden overgeschakeld naar de volgende regio (Noord-Europa). Deze taak kan volledig worden geautomatiseerd met behulp van [Azure service voor het plannen](../scheduler/scheduler-intro.md).  De taak bestaat uit de volgende stappen uit:
* Primaire server in de failovergroep overschakelen naar Noord-Europa met behulp van beschrijvende failover (1)
* Verwijderen van de failovergroep tussen VS-Oost en Noord-Europa
* Maak een nieuwe failovergroep met dezelfde naam maar tussen Noord-Europa en Oost-Azië (2). 
* De primaire in Noord-Europa en secundaire in Oost-Azië toevoegen aan deze failovergroep (3).


Het volgende diagram illustreert de nieuwe configuratie na de geplande failover:

![Scenario 3. Overstappen op de primaire naar Noord-Europa.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-b.png)

Als bijvoorbeeld een storing in Noord-Europa gebeurt, wordt de automatische databaseback-failover wordt gestart door de failovergroep, die effectief resulteert in het verplaatsen van de toepassing naar de volgende regio vooraf een schema (1).  De VS-Oost is in dat geval de enige resterende secundaire regio tot Noord-Europa weer online is. De resterende twee regio's fungeren de klanten in alle drie regio's door het overschakelen van rollen. Met Azure scheduler is dienovereenkomstig worden aangepast. Omdat de resterende regio's extra gebruikersverkeer van Europa krijgen, worden de prestaties van de toepassing is beïnvloed niet alleen door extra latentie, maar ook door een toenemend aantal verbindingen van de eindgebruiker. Zodra de onderbreking is verholpen in Noord-Europa, wordt de secundaire database er direct gesynchroniseerd met de huidige primaire. Het volgende diagram illustreert een storing in Noord-Europa:

![Scenario 3. Storing in Noord-Europa.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-c.png)

> [!NOTE]
> U kunt de tijd wanneer de ervaring van de eindgebruiker in Europa is verminderd door de lang latentie kunt verminderen. U doet dat u moeten proactief een kopie van de toepassing implementeren en de secundaire database (s) maken in een andere lokale regio (West-Europa) als vervanging van het offline toepassingsexemplaar in Noord-Europa. Wanneer de laatste weer online is kunt u bepalen of om door te gaan met behulp van West-Europa of verwijderen van de kopie van de toepassing bevat en gaat u terug naar de met behulp van Noord-Europa,
>

De sleutel **voordelen** van dit ontwerp zijn:
* Workload van de toepassing alleen-lezen toegang heeft tot gegevens in de regio kasten te allen tijde. 
* De workload voor lezen / schrijven-toepassing toegang heeft tot gegevens in de dichtstbijzijnde regio gedurende de periode van de hoogste activiteit in elke geografische locatie
* Omdat de toepassing wordt geïmplementeerd in meerdere regio's, kan dit een verlies van een van de regio's zonder significante downtime overbruggen. 

Maar er zijn een aantal **compromissen**:
* Een regionale onderbreking resulteert in de geografische locatie langere latentie worden beïnvloed. Workloads voor lezen / schrijven en alleen-lezen is geleverd door de toepassing in een andere Geografie bevinden. 
* De alleen-lezen-werkbelastingen moeten verbinding maken met een ander eindpunt in elke regio. 


## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Zakelijke continuïteit planning: Kies een toepassing ontwerpen voor noodherstel voor cloud
Uw strategie voor herstel na noodgevallen specifieke cloud kunt combineren of uitbreiden van deze ontwerppatronen voor het beste voldoet aan de behoeften van uw toepassing.  Zoals eerder vermeld, worden de strategie die u kiest is gebaseerd op de SLA die u wilt aanbieden aan uw klanten en de topologie van de implementatie van toepassing. Om u te helpen uw beslissing, vergelijkt de volgende tabel de opties op basis van beoogd herstelpunt (RPO) en de geschatte hersteltijd (ERT).

| Patroon | RPO | ERT |
|:--- |:--- |:--- |
| Actief-passief-implementatie voor herstel na noodgeval met CO-database-toegang |Lees-/ schrijftoegang sec < 5. |Fout detectietijd + DNS TTL |
| Actief / actief-implementatie voor de taakverdeling van toepassing |Lees-/ schrijftoegang sec < 5. |Fout detectietijd + DNS TTL |
| Actief-passief-implementatie voor behoud van gegevens |Alleen-lezentoegang sec < 5. | Alleen-lezentoegang = 0 |
||Lees-/ schrijftoegang = nul | Lees-/ schrijftoegang = fout detectietijd + respijtperiode verlies van gegevens |
|||

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md)
* Zie voor meer informatie over geo-replicatie en failover-groepen, [actieve geo-replicatie](sql-database-geo-replication-overview.md)  
* Zie voor meer informatie over actieve geo-replicatie met elastische Pools [strategieën voor noodherstel elastische Pool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
