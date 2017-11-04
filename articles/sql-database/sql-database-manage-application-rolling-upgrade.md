---
title: Rolling toepassingsupgrades - Azure SQL Database | Microsoft Docs
description: Informatie over het gebruiken van Azure SQL Database geo-replicatie ter ondersteuning van online upgrades van uw cloudtoepassing.
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 58f42859-1e37-463c-a3d8-a3ca2e867148
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 07/16/2016
ms.author: sashan
ms.openlocfilehash: deb91d55e5b796f7b1b53a99866156fe492e0a24
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Het beheren van rollende upgrades van cloud-toepassingen met behulp van SQL-Database actieve geo-replicatie
> [!NOTE]
> [Actieve geo-replicatie](sql-database-geo-replication-overview.md) is nu beschikbaar voor alle databases in alle lagen.
> 

Informatie over het gebruik [geo-replicatie](sql-database-geo-replication-overview.md) in SQL-Database om in te schakelen rollende upgrades van uw cloudtoepassing. Omdat de upgrade is een bewerking verstoren, moet het deel van uw zakelijke continuïteit planning en ontwerp. In dit artikel we kijken naar twee verschillende methoden van het upgradeproces te organiseren en de voordelen en -en nadelen van elke optie bespreken. Voor de doeleinden van dit artikel gebruiken we een eenvoudige toepassing die uit een website die is verbonden met een individuele database als de gegevenslaag bestaat. Ons doel is om bij te werken van versie 1 van de toepassing naar versie 2 zonder aanzienlijke invloed op de gebruikerservaring. 

Bij het evalueren van de upgrade-opties moet u rekening houden met de volgende factoren:

* De impact op de beschikbaarheid van de toepassing tijdens upgrades. Hoe lang de functie van de toepassing mogelijk beperkt of gedegradeerd.
* De mogelijkheid in het geval van een mislukte upgrade terugdraaien.
* De kwetsbaarheid van de toepassing als een niet-verwante onherstelbare fout tijdens de upgrade optreedt.
* Totaal aantal dollar kosten.  Dit omvat aanvullende redundantie en incrementele kosten van de tijdelijke die worden gebruikt door het upgradeproces. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Bijwerken van toepassingen die afhankelijk van de databaseback-ups voor herstel na noodgevallen zijn
Als uw toepassing afhankelijk van automatische databaseback-ups is en geo-herstel voor herstel na noodgevallen gebruikt, is het meestal geïmplementeerd naar één Azure-regio. In dit geval omvat het upgradeproces het maken van een back-implementatie van alle onderdelen die zijn betrokken bij de upgrade. Als u wilt de overlast minimaliseren wordt u gebruikmaken van Azure Traffic Manager (WATM) met het failover-profiel.  Het volgende diagram illustreert de operationele omgeving vóór het upgradeproces. Het eindpunt <i>contoso 1.azurewebsites.net</i> vertegenwoordigt een productiesite van de toepassing die moet worden bijgewerkt. Om aan te bieden de mogelijkheid terugdraaien de upgrade, u moet een fase sleuf maken met een volledig gesynchroniseerde kopie van de toepassing. De volgende stappen zijn vereist voor het voorbereiden van de toepassing voor de upgrade:

1. Een fase sleuf maken voor de upgrade. Om te doen die een secundaire database (1) maken en implementeren van een identieke website in dezelfde Azure-regio. Bewaken van de secundaire om te zien als het seeding proces is voltooid.
2. Een failover-profiel maken in WATM met <i>contoso 1.azurewebsites.net</i> als online-eindpunt en <i>contoso 2.azurewebsites.net</i> als offline. 

> [!NOTE]
> Noteer de voorbereidende stappen heeft geen invloed op de toepassing in de productiesite en deze kan worden gebruikt in de modus volledige toegang.
>  

![Configuratie van SQL Database Ga-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Zodra de voorbereidende stappen zijn voltooid. de toepassing is gereed voor de daadwerkelijke upgrade. Het volgende diagram illustreert de stappen voor het upgradeproces. 

1. Stel de primaire database in de productiesite naar alleen-lezen-modus (3). Dit wordt gegarandeerd dat de productie-exemplaar van de toepassing (V1) blijven alleen-lezen tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1- en V2-database-exemplaren.  
2. Verbreek de verbinding met de secundaire database met behulp van de geplande beëindiging-modus (4). Er wordt een volledig gesynchroniseerde onafhankelijke kopie van de primaire database gemaakt. Deze database wordt bijgewerkt.
3. Schakel de primaire database naar de modus lezen-schrijven en het upgrade-script uitvoeren in de fase sleuf (5).     

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Als de upgrade is voltooid bent u nu klaar om over te schakelen van de eindgebruikers op de voorbereide kopie van de toepassing. Er worden nu de productiesite van de toepassing.  Dit omvat een aantal meer stappen zoals geïllustreerd in het volgende diagram.

1. Het online eindpunt in het profiel WATM switch <i>contoso 2.azurewebsites.net</i>, die verwijzen naar de V2-versie van de website (6). Nu wordt de productiesite met de V2-toepassing en het verkeer door eindgebruikers wordt doorverwezen.  
2. Als u niet langer de toepassingsonderdelen V1 zodat u veilig kunt ze (7) verwijderen.   

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het upgrade-script de sleuf fase moet worden overwogen aangetast. Als u wilt terugdraaien van de toepassing naar de status van de vóór de upgrade moet u gewoon de toepassing in de productiesite naar volledige toegang herstellen. De vereiste stappen worden op het volgende diagram weergegeven.    

1. Het database-exemplaar ingesteld op alleen-lezen-modus (8). Hiermee herstelt u de volledige V1 functioneel in de productiesite.
2. Analyse van de hoofdmap de oorzaak en de waarmee is geknoeid onderdelen verwijderen in de fase sleuf (9). 

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien is geen wijzigingen in WATM profiel vereist omdat deze al naar verwijst <i>contoso 1.azurewebsites.net</i> als het actieve eindpunt.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

De sleutel **voordeel** van deze optie is een toepassing in één regio met behulp van een aantal eenvoudige stappen te upgraden. Het aantal dollar van de upgrade is relatief laag is. De belangrijkste **afweging** is dat als een onherstelbare fout tijdens de upgrade optreedt het herstel van de status van de vóór de upgrade eruit ziet nieuwe implementatie van de toepassing in een andere regio en het terugzetten van de database vanaf een back-up met geo-herstel. Dit proces zal leiden tot aanzienlijke downtime.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Bijwerken van toepassingen die afhankelijk van de database geo-replicatie voor herstel na noodgevallen zijn
Als uw toepassing gebruikmaakt van geo-replicatie voor bedrijfscontinuïteit, wordt geïmplementeerd naar ten minste twee verschillende regio's met een actieve implementatie in de primaire regio en een stand-by-implementatie in de regio van de back-up. Naast de eerder genoemde factoren, moet het upgradeproces waarborgen dat:

* De toepassing blijft te allen tijde tijdens het upgradeproces onherstelbare fouten beveiligd
* De geografisch redundante componenten van de toepassing zijn in combinatie met de actieve onderdelen bijgewerkt

Als u wilt deze doelstellingen te behalen wordt u gebruikmaken van Azure Traffic Manager (WATM) met het failover-profiel met een actieve en drie back-eindpunten.  Het volgende diagram illustreert de operationele omgeving vóór het upgradeproces. De websites <i>contoso 1.azurewebsites.net</i> en <i>contoso dr.azurewebsites.net</i> vertegenwoordigen een productiesite van de toepassing met volledige geografische redundantie. Om aan te bieden de mogelijkheid terugdraaien de upgrade, u moet een fase sleuf maken met een volledig gesynchroniseerde kopie van de toepassing. Omdat u nodig hebt om ervoor te zorgen dat de toepassing snel herstellen kunt als een onherstelbare fout tijdens het upgradeproces optreedt, moet de sleuf fase geografisch redundante ook. De volgende stappen zijn vereist voor het voorbereiden van de toepassing voor de upgrade:

1. Een fase sleuf maken voor de upgrade. Om te doen die een secundaire database (1) maken en implementeren van een identieke kopie van de website in dezelfde Azure-regio. Bewaken van de secundaire om te zien als het seeding proces is voltooid.
2. Een geografisch redundante secundaire database maken in de sleuf fase door geo-replicatie de secundaire database naar de back-regio (dit heet "teruggekoppeld geo-replicatie"). Bewaken van de back-up secundaire om te zien als het seeding proces is voltooid (3).
3. Maken van een stand-by-kopie van de website in de back-regio en koppel deze aan de geografisch redundante secundaire (4).  
4. Toevoegen van extra eindpunten <i>contoso 2.azurewebsites.net</i> en <i>contoso 3.azurewebsites.net</i> het failover-profiel in WATM als offline eindpunten (5). 

> [!NOTE]
> Noteer de voorbereidende stappen heeft geen invloed op de toepassing in de productiesite en deze kan worden gebruikt in de modus volledige toegang.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Zodra de voorbereidende stappen zijn voltooid, is de fase sleuf gereed is voor de upgrade. Het volgende diagram illustreert de Upgradestappen.

1. Stel de primaire database in de productiesite naar alleen-lezen-modus (6). Dit wordt gegarandeerd dat de productie-exemplaar van de toepassing (V1) blijven alleen-lezen tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1- en V2-database-exemplaren.  
2. Verbreek de verbinding met de secundaire database in dezelfde regio als de geplande beëindiging-modus (7) gebruiken. Wordt er een volledig gesynchroniseerde onafhankelijke kopie van de primaire database, die een primaire na beëindiging automatisch worden gemaakt. Deze database wordt bijgewerkt.
3. De primaire database inschakelen in de sleuf fase in de modus lezen-schrijven en uitvoeren van het upgrade-script (8).    

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Als de upgrade is voltooid u nu klaar eindgebruikers overschakelen naar de V2-versie van de toepassing bent. Het volgende diagram illustreert de vereiste stappen.

1. Overschakelen van het actieve eindpunt in het profiel WATM <i>contoso 2.azurewebsites.net</i>, welke nu verwijst naar de V2-versie van de website (9). Wordt nu een productiesite met de toepassing V2 en eindgebruikers verkeer wordt omgeleid voor het. 
2. Als u niet langer de V1-toepassing zodat u veilig kunt verwijderen (10 en 11).  

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het upgrade-script de sleuf fase moet worden overwogen aangetast. Als u wilt de toepassing naar de status van de vóór de upgrade terugdraaien herstellen u gewoon met behulp van de toepassing in de productiesite met volledige toegang. De vereiste stappen worden op het volgende diagram weergegeven.    

1. Stel de kopie van de primaire database in de productiesite naar de modus lezen-schrijven (12). Hiermee herstelt u de volledige V1 functioneel in de productiesite.
2. Analyse van de hoofdmap de oorzaak en de waarmee is geknoeid onderdelen verwijderen in de sleuf fase (13 en 14). 

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien is geen wijzigingen in WATM profiel vereist omdat deze al naar verwijst <i>contoso 1.azurewebsites.net</i> als het actieve eindpunt.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen voor een cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

De sleutel **voordeel** van deze optie is dat u de geografisch redundante kopie parallel en de toepassing upgraden kunt uw zakelijke continuïteit in gevaar tijdens de upgrade. De belangrijkste **afweging** is dat het dubbele redundantie van elk toepassingsonderdeel vereist, en daarom hogere Dollarkosten maakt. Hierbij wordt ook een gecompliceerdere werkstroom. 

## <a name="summary"></a>Samenvatting
De twee upgrade methoden beschreven in het artikel verschillen in complexiteit en de kosten dollar maar ze beide ligt de nadruk op het tijdstip waarop de gebruiker beperkt tot alleen-lezen bewerkingen worden voor het minimaliseren. Deze tijd is rechtstreeks door de duur van het upgrade-script gedefinieerd. Niet is afhankelijk van de grootte van de database, de servicelaag die u hebt gekozen, de configuratie van de website en andere factoren die u kunt eenvoudig niet controleren. Dit komt doordat de voorbereidende stappen worden ontkoppeld van de Upgradestappen en kunnen worden uitgevoerd zonder enige impact op de productietoepassing. De efficiëntie van het upgrade-script is het van groot belang dat bepaalt van de gebruikerservaring tijdens upgrades. De beste manier kunt u deze verbeteren dus uw inspanningen gericht op het upgrade-script zo efficiënt mogelijk te maken.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van zakelijke continuïteit en scenario's [Business continuity overview](sql-database-business-continuity.md).
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor meer informatie over het gebruik van automatische back-ups voor herstel, [een database herstellen via automatische back-ups](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-geo-replication-overview.md).


