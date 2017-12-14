---
title: Ontwerp van de maximaal beschikbare service met behulp van Azure SQL Database | Microsoft Docs
description: Meer informatie over het ontwerp van de toepassing voor maximaal beschikbare services met Azure SQL Database.
keywords: "herstel na noodgevallen, oplossingen voor herstel na noodgevallen, back-up van app-gegevens, geo-replicatie, cloud zakelijke continuïteit plannen"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: e8a346ac-dd08-41e7-9685-46cebca04582
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 12/13/2017
ms.workload: On Demand
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 3d6ad95c1ca316b2e7c3f722315d2ddec03a3716
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Maximaal beschikbare services met behulp van Azure SQL Database ontwerpen

Bij het maken en implementeren van maximaal beschikbare services in Azure SQL-Database, gebruikt u [failover groepen en actieve geo-replicatie](sql-database-geo-replication-overview.md) om herstelmogelijkheden bij uitval van regionale en kritieke fouten optreden. Ook kunt snel herstel van de secundaire databases. Dit artikel is gericht op algemene patronen van de toepassing en de voordelen en -en nadelen van elke optie. Zie voor meer informatie over actieve geo-replicatie met elastische Pools [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime"></a>Scenario 1: Met behulp van twee Azure-regio's voor bedrijfscontinuïteit met minimale downtime
In dit scenario hebben de toepassingen in de volgende kenmerken: 
*   Toepassing is actief in een Azure-regio
*   Alle Databasesessies vereisen lezen en schrijven (RW) om gegevens te
*   Weblaag en gegevenslaag moeten worden geplaatst om de latentie en verkeer kosten 
*   Fundamenteel, uitvaltijd is een hogere bedrijfsrisico voor deze toepassingen dan verlies van gegevens

In dit geval wordt is de topologie van de implementatie van toepassing geoptimaliseerd voor het verwerken van regionale noodsituaties wanneer alle toepassingsonderdelen failover samen moeten. Het onderstaande diagram toont deze topologie. Voor geografische redundantie worden van de toepassing resources geïmplementeerd naar regio A en B. De resources in regio B gebruikt echter niet tot een regio is mislukt. Een groep failover is tussen de twee regio's voor het beheren van de verbinding met de database, replicatie en failover geconfigureerd. De webservice in beide regio's is geconfigureerd voor toegang tot de database via de listener lezen-schrijven  **&lt;failover groepsnaam&gt;. database.windows.net** (1). Het Traffic manager is ingesteld om te gebruiken [routeringsmethode voor prioriteit](../traffic-manager/traffic-manager-configure-priority-routing-method.md) (2).  

> [!NOTE]
> [Azure traffic manager](../traffic-manager/traffic-manager-overview.md) alleen ter illustratie in dit artikel wordt gebruikt. U kunt een oplossing voor taakverdeling die ondersteuning biedt voor de routeringsmethode prioriteit.    
>

Deze configuratie voordat een storing in het volgende diagram wordt weergegeven:

![Scenario 1. De configuratie van voordat de storing.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-a.png)

Na een storing in de primaire regio detecteert de service SQL Database dat de primaire database niet toegankelijk is en failover naar de secundaire regio op basis van de parameters van het beleid automatische failover (1 activeert). U kunt een respijtperiode die de tijd tussen de onderbreking te detecteren en de failover zelf bepaalt configureren, afhankelijk van de SLA voor uw toepassing. Het is mogelijk dat het traffic manager initieert de failover eindpunt voordat de failover-groep de failover van de database activeert. In dat geval de webtoepassing niet onmiddellijk opnieuw verbinding maken met de database. Maar de inlogmodus worden automatisch uitgevoerd zodra de failover van de database is voltooid. Wanneer de mislukte regio teruggezet en weer online is, wordt de oude primaire automatisch opnieuw verbinding als een nieuwe secundaire database. Het onderstaande diagram ziet u de configuratie na een failover.
 
> [!NOTE]
> Alle transacties na de failover zijn verloren gegaan tijdens het opnieuw verbinden. Nadat de failover is voltooid, kan de toepassing in de regio B herstellen en opnieuw starten gebruiker aanvragen. Zowel de webtoepassing en de primaire database zijn nu in regio B en blijven CO-locaties. 
n>

![Scenario 1. Configuratie na een failover](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-b.png)

Als een storing in regio B gebeurt, het replicatieproces tussen de primaire en de secundaire database wordt onderbroken, maar de koppeling tussen de twee blijft intact (1). Verkeer beheerd detecteert dat de verbinding met de regio B verbroken is en de eindpunt-web-app 2 als gedegradeerd (2 markeert). Prestaties van de toepassing niet in dit geval wordt beïnvloed, maar de database wordt blootgesteld en daarom hoger risico van gegevensverlies in geval een regio mislukt achter elkaar.

> [!NOTE]
> Voor herstel na noodgevallen, wordt aangeraden de configuratie met de implementatie van de toepassing is beperkt tot twee regio's. Dit is omdat de meeste van de Azure-regio's slechts twee regio's hebben. Deze configuratie biedt geen bescherming van uw toepassing van een gelijktijdige catastrofale uitval van beide regio's. U kunt uw databases in een derde regio met herstellen in een onwaarschijnlijke geval van een dergelijke storing [geo-restore-bewerking](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

 Zodra de onderbreking is verholpen, wordt de secundaire database automatisch synchroniseert opnieuw met de primaire. Tijdens de synchronisatie, kan de prestaties van de primaire worden beïnvloed. De specifieke impact hangt af van de hoeveelheid gegevens van de nieuwe primaire zijn toegepast sinds de failover. Het volgende diagram illustreert een storing in de secundaire regio:

![Scenario 1. Configuratie na een storing in de secundaire regio.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario1-c.png)

De sleutel **voordelen** van dit ontwerppatroon zijn:

* Dezelfde webtoepassing is geïmplementeerd voor beide regio's zonder configuratie regiospecifieke en hoeft niet meer logica voor het beheren van failover. 
* De prestaties van toepassingen wordt niet beïnvloed door failover als de webtoepassing en de database zijn altijd CO-locaties.

De belangrijkste **afweging** is dat de toepassingsresources in de regio B meestal volledig worden benut.

## <a name="scenario-2-azure-regions-for-business-continuity-with-maximum-data-preservation"></a>Scenario 2: Azure-regio's voor bedrijfscontinuïteit met behoud van gegevens
Deze optie is het meest geschikt voor toepassingen met de volgende kenmerken:

* Verlies van gegevens is grote zakelijke risico. De failover van de database kan alleen worden gebruikt als een laatste toevlucht als de storing wordt veroorzaakt door een onherstelbare fout.
* De toepassing ondersteunt alleen-lezen en lees-/ modi van bewerkingen en u kunt gebruiken in 'alleen-lezen-modus' voor een bepaalde periode.

In dit patroon wordt de toepassing naar de modus alleen-lezen wanneer de alleen-lezen verbindingen start time-outfouten ophalen. De webtoepassing wordt geïmplementeerd op beide regio's en voeg een verbinding met het eindpunt lezen / schrijven-listener en andere verbinding met het kenmerk alleen-lezen-listener-eindpunt (1). Traffic manager-profiel moet worden gebruikt [prioriteit routering](../traffic-manager/traffic-manager-configure-priority-routing-method.md). [Eindpunt bewaking](../traffic-manager/traffic-manager-monitoring.md) moet worden ingeschakeld voor het toepassingseindpunt in elke regio (2).

Het volgende diagram ziet u deze configuratie voordat een storing:

![Scenario 2. De configuratie van voordat de storing.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-a.png)

Als de manager verkeer een verbindingsfout optreedt bij een regio detecteert, overgeschakeld het gebruikersverkeer automatisch naar het toepassingsexemplaar in regio B. Met dit patroon is het belangrijk dat u de respijtperiode met verlies van gegevens naar een voldoende hoge waarde, bijvoorbeeld 24 uur. Hiermee zorgt u ervoor dat gegevens verloren gaan als de onderbreking is verholpen in die tijd is voorkomen. Wanneer de webtoepassing in regio B is geactiveerd mislukken lees-/ schrijfbewerkingen. Deze moet op dat moment overschakelen naar de modus alleen-lezen (1). In deze modus worden de aanvragen automatisch doorgestuurd naar de secundaire database. Worden als de onderbreking is veroorzaakt door een onherstelbare fout, waarschijnlijk het kan niet beperkt binnen de respijtperiode. Het verloopt wanneer de failover de failover-groep-triggers. Na de listener lezen-schrijven beschikbaar en de verbindingen met het stoppen mislukt (2). Het volgende diagram illustreert de twee fasen van het herstelproces.

> [!NOTE]
> Als de onderbreking in de primaire regio binnen de respijtperiode is verholpen, traffic manager detecteert dat het herstel van de verbinding in de primaire regio en gebruikersverkeer schakelt terug naar het toepassingsexemplaar in regio A. Dat toepassingsexemplaar wordt hervat en werkt in de modus lezen / schrijven met behulp van de primaire database in regio A zoals wordt geïllustreerd door het vorige diagram.
>

![Scenario 2. Fasen van het herstel na noodgevallen.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-b.png)

Als een storing in regio B gebeurt, detecteert de traffic manager het uitvallen van het eindpunt web-app-2 in de regio B en markeert deze gedegradeerd (1). De failover-groep verandert in de tussentijd de alleen-lezen-listener voor een regio (2). De onderbreking van deze heeft geen gevolgen voor de eindgebruiker, maar de primaire database tijdens de onderbreking is toegankelijk. Het volgende diagram wordt een fout in de secundaire regio:

![Scenario 2. Onderbreking van de secundaire regio.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario2-c.png)

Zodra de onderbreking is verholpen, de secundaire database direct is gesynchroniseerd met de primaire en de alleen-lezen-listener is overgeschakeld naar de secundaire database in regio B. Tijdens de synchronisatie worden de prestaties van de primaire kan enigszins beïnvloed afhankelijk van de hoeveelheid gegevens die moeten worden gesynchroniseerd.

Dit ontwerppatroon heeft diverse **voordelen**:

* Dit voorkomt het verlies van gegevens tijdens de tijdelijke storingen.
* Uitvaltijd is afhankelijk van alleen hoe snel het traffic manager detecteert de verbindingsfout is configureerbaar.

De **afweging** is dat de toepassing kunnen moet werken in de modus alleen-lezen.

## <a name="scenario-3-application-relocation-to-a-different-geography-without-data-loss-and-near-zero-downtime"></a>Scenario 3: Toepassing verplaatsingen naar een andere Geografie bevinden zonder verlies van gegevens en in de buurt van uitvaltijd 
In dit scenario heeft de toepassing de volgende kenmerken: 
* De eindgebruikers toegang tot de toepassing vanaf verschillende locaties
* De toepassing bevat alleen-lezen werkbelastingen die niet afhankelijk van volledige synchronisatie met de meest recente updates
* Schrijftoegang tot de gegevens moeten in het hetzelfde Geografie voor het merendeel van de gebruikers worden ondersteund 
* Lees latentie is essentieel voor de eindgebruiker 


Om te voldoen aan deze vereisten hoeft te garanderen dat de gebruikersapparaat **altijd** verbinding maakt met de toepassing geïmplementeerd in het hetzelfde Geografie voor de alleen-lezen bewerkingen, zoals surfen gegevens analytics enzovoort. Terwijl de OLTP-bewerkingen worden verwerkt in de dezelfde Geografie **meestal**. Bijvoorbeeld gedurende de dag OLTP-bewerkingen in het hetzelfde Geografie wordt verwerkt, maar buiten kantooruren kunnen ze worden verwerkt in een andere Geografie bevinden. Als de activiteit van de eindgebruiker wordt voornamelijk tijdens de werkuren gebeurt, u kunt garanderen dat de optimale prestaties voor de meeste van de gebruikers die de meeste van de tijd. Het volgende diagram toont deze topologie. 
 
De toepassing resources moeten worden geïmplementeerd in elke Geografie waar u aanzienlijke gebruiksvereisten hebt. Bijvoorbeeld, als uw toepassing actief in de Verenigde Staten gebruikt wordt, moeten Zuidoost-Azië de toepassing en de Europese Unie worden geïmplementeerd op alle deze locaties. De primaire database moet worden dynamisch overgeschakeld van één Geografie naar de volgende aan het einde van de werkuren. Deze methode is aangeroepen 'volgen de sun'. De OLTP-werkbelasting wordt altijd verbinding met de database via de listener lezen-schrijven  **&lt;failover groepsnaam&gt;. database.windows.net** (1). De werkbelasting van de alleen-lezen verbinding maakt met de lokale database rechtstreeks met het eindpunt van de server databases  **&lt;servernaam&gt;. database.windows.net** (2). Het Traffic manager is geconfigureerd met de [routeringsmethode voor prestaties](../traffic-manager/traffic-manager-configure-performance-routing-method.md). Hiermee zorgt u ervoor dat de eindgebruiker apparaat is verbonden met de webservice in de dichtstbijzijnde regio. Het Traffic manager moet worden ingesteld met het bewaken van het eindpunt is ingeschakeld voor elke web service-eindpunt (3).

> [!NOTE]
> De configuratie van de failover-groep definieert welke regio wordt gebruikt voor failover. Omdat de nieuwe primaire bevindt zich in een andere Geografie bevinden de failover-leidt tot een langere latentie voor OLTP- en alleen-lezen werkbelastingen totdat de betrokken regio weer online is.
>

![Scenario 3. Configuratie met primaire in VS-Oost.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-a.png)

Aan het einde van de dag (bijvoorbeeld om 23: 00 uur lokale tijd) moeten de actieve databases worden overgeschakeld naar de volgende regio (Noord-Europa). Deze taak volledig kan worden geautomatiseerd met behulp van [Azure service planning](../scheduler/scheduler-intro.md).  De taak omvat de volgende stappen:
* Overschakelen van de primaire server in de groep failover naar Noord-Europa met beschrijvende failover (1)
* Verwijder de groep failover tussen VS-Oost en Noord-Europa
* Maak een nieuwe failover-groep met dezelfde naam maar tussen Noord-Europa en Oost-Azië, (2). 
* De primaire in Noord-Europa en secundaire in Oost-Azië toevoegen aan deze groep failover (3).


Het volgende diagram illustreert de nieuwe configuratie na de geplande failover:

![Scenario 3. Overgang van de primaire naar Noord-Europa.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-b.png)

Als bijvoorbeeld een storing in Noord-Europa gebeurt, wordt de database met automatische failover wordt geïnitieerd door de groep failover, wat effectief resulteert in het verplaatsen van de toepassing naar de volgende regio voortijdig (1).  De VS-Oost is in dat geval de enige resterende secundaire regio totdat Noord-Europa weer online is. De overige twee gebieden fungeren de klanten in alle drie locaties door het overschakelen van rollen. Azure scheduler moet worden aangepast. Omdat de overige gebieden u extra gebruikersverkeer vanuit Europa, worden de prestaties van de toepassing is beïnvloed door extra latentie maar door een toenemend aantal verbindingen van de eindgebruiker. Zodra de onderbreking is verholpen in Noord-Europa, wordt de secundaire database er onmiddellijk gesynchroniseerd met de huidige primaire. Het volgende diagram illustreert een storing in Noord-Europa:

![Scenario 3. Stroomstoring in Noord-Europa.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/scenario3-c.png)

> [!NOTE]
> U kunt de tijd wanneer de eindgebruiker in Europa is gedegradeerd door de latentie lang verminderen. Hiertoe die u moet proactief een kopie van de toepassing implementeren en de secundaire databases in een andere lokale regio (West-Europa) maken ter vervanging van het offline toepassingsexemplaar in Noord-Europa. Als de laatste weer online kunt u besluiten of u wilt blijven gebruiken, West-Europa of te verwijderen van de kopie van de toepassing er en Ga terug naar het gebruik van Noord-Europa,
>

De sleutel **voordelen** van dit ontwerp zijn:
* De alleen-lezen toepassing werklast heeft toegang tot gegevens in de regio kasten te allen tijde. 
* De lezen-schrijven toepassing werklast toegang heeft tot gegevens in de dichtstbijzijnde regio gedurende de periode van de hoogste activiteit in elke Geografie
* Omdat de toepassing wordt geïmplementeerd voor meerdere regio's, kan deze blijven bewaard als een verlies van een van de regio's zonder aanzienlijke uitvaltijd. 

Maar er een aantal zijn **voor-en nadelen**:
* Een regionale uitval resulteert in het Geografie om te worden beïnvloed door het langere latentie. Werkbelastingen voor alleen-lezen en alleen-lezen is afgehandeld door de toepassing in een andere Geografie bevinden. 
* De alleen-lezen-werkbelastingen moeten verbinding maken met een ander eindpunt in elke regio. 


## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Zakelijke continuïteit planning: Kies het ontwerp van een toepassing voor noodherstel voor cloud
Uw strategie voor noodherstel specifieke cloud kunt combineren of breid deze ontwerppatronen voor het beste voldoet aan de behoeften van uw toepassing.  Zoals eerder vermeld, wordt de strategie die u kiest is gebaseerd op de SLA die u wilt aanbieden aan uw klanten en de topologie van de implementatie van toepassing. Om u te helpen uw beslissing, vergelijkt de volgende tabel de opties op basis van beoogd herstelpunt (RPO) en de geschatte hersteltijd (invoegen).

| Patroon | RPO | INVOEGEN |
|:--- |:--- |:--- |
| Actief / passief-implementatie voor herstel na noodgevallen met toegang tot de database met CO-locaties |Lees-/ schrijftoegang sec < 5. |Fout detectietijd + DNS TTL |
| Implementatie van de actieve toepassing load balancing |Lees-/ schrijftoegang sec < 5. |Fout detectietijd + DNS TTL |
| Actief / passief-implementatie voor het behoud van gegevens |Alleen-lezentoegang sec < 5. | Alleen-lezentoegang = 0 |
||Lees-/ schrijftoegang = nul | Lees-/ schrijftoegang = fout detectietijd + respijtperiode met verlies van gegevens |
|||

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
* Zie voor meer informatie over groepen geo-replicatie en failover [actieve geo-replicatie](sql-database-geo-replication-overview.md)  
* Zie voor meer informatie over actieve geo-replicatie met elastische Pools [elastische Pool disaster recovery strategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
