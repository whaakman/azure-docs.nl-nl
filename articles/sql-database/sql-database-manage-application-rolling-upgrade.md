---
title: Rolling upgrades van toepassingen - Azure SQL Database | Microsoft Docs
description: Informatie over het gebruik van Azure SQL Database geo-replicatie voor de ondersteuning van online upgrades van uw cloudtoepassing.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: sashan
ms.openlocfilehash: 37960995c89c2b30d90ac45dcd8cc44d80088398
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818613"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Rolling upgrades van cloud-toepassingen met behulp van SQL-Database van actieve geo-replicatie beheren
> [!NOTE]
> [Actieve geo-replicatie](sql-database-geo-replication-overview.md) is nu beschikbaar voor alle databases in alle lagen.
> 

Meer informatie over het gebruik van [geo-replicatie](sql-database-geo-replication-overview.md) in SQL-Database voor het inschakelen van rolling upgrades van uw cloudtoepassing. Omdat een verstorende bewerking is voor upgrade, moeten deze deel uitmaken van uw zakelijke continuïteit planning en ontwerp. In dit artikel we kijken naar twee verschillende methoden van het upgradeproces en bespreken de voor- en -en nadelen van elke optie. Voor de doeleinden van dit artikel gebruiken we een eenvoudige toepassing die uit een website die is verbonden met een enkele database als de gegevenslaag bestaat. Ons doel is om bij te werken van versie 1 van de toepassing op versie 2 zonder aanzienlijke invloed op de ervaring voor de eindgebruiker. 

Bij het evalueren van de upgrade-opties moet u de volgende factoren overwegen:

* Gevolgen voor de beschikbaarheid van toepassingen tijdens upgrades. Hoe lang de functie van de toepassing is mogelijk beperkt of gedegradeerd.
* De mogelijkheid om het forestfunctionaliteitsniveau terug te zetten in het geval van een upgrade mislukt.
* Beveiligingsproblemen van de toepassing als een niet-gerelateerde onherstelbare fout tijdens de upgrade optreedt.
* Totaal aantal dollar kosten.  Dit omvat aanvullende redundantie en incrementele kosten voor de tijdelijke onderdelen die worden gebruikt door het upgradeproces. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Upgrade van toepassingen die afhankelijk van de databaseback-ups voor herstel na noodgevallen zijn
Als uw toepassing is afhankelijk van automatische databaseback-ups en geografisch herstel voor herstel na noodgevallen gebruikt, is het meestal naar één Azure-regio geïmplementeerd. In dit geval omvat het upgradeproces het maken van een back-implementatie van alle onderdelen van de toepassing die betrokken zijn bij de upgrade. Als u wilt de onderbreking van de eindgebruiker minimaliseren u gebruik van Azure Traffic Manager (ATM) door het failover-profiel.  Het volgende diagram illustreert de operationele omgeving voordat u het upgradeproces. Het eindpunt <i>contoso-1.azurewebsites.net</i> vertegenwoordigt een productiesite van de toepassing die moet worden bijgewerkt. Als u wilt inschakelen, de mogelijkheid om terug te draaien de upgrade, u moet een fase sleuf maken met behulp van een volledig gesynchroniseerde kopie van de toepassing. De volgende stappen zijn vereist om voor te bereiden van de toepassing voor de upgrade:

1. Een fase sleuf maken voor de upgrade. Om te doen die een secundaire database (1) maken en implementeren van een identieke-website in dezelfde Azure-regio. Controleer de secundaire server om te zien als het seeden proces is voltooid.
2. Een failover-profiel maken in ATM met <i>contoso-1.azurewebsites.net</i> als online-eindpunt en <i>contoso-2.azurewebsites.net</i> als offline. 

> [!NOTE]
> Houd er rekening mee de voorbereidende stappen heeft geen gevolgen voor de toepassing in de productiesite en deze kan worden gebruikt in de modus voor volledige toegang.
>  

![Configuratie van SQL Database-Go-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

De toepassing is gereed voor de daadwerkelijke upgrade, zodra de voorbereidende stappen zijn voltooid. Het volgende diagram illustreert de stappen die betrokken zijn bij het upgradeproces. 

1. De primaire database instellen in de productiesite naar de modus alleen-lezen (3). Dit zorgt ervoor dat de productie-instantie van de toepassing (V1) alleen-lezen tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1 en V2-database-exemplaren, blijven.  
2. De secundaire database met behulp van de geplande beëindiging-modus (4) de verbinding verbreken. Een volledig gesynchroniseerde onafhankelijke kopie van de primaire database wordt gemaakt. Deze database wordt bijgewerkt.
3. Schakel de primaire database naar de modus lezen / schrijven en uitvoeren van het upgrade-script in de fase sleuf (5).     

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Als de upgrade is voltooid bent u nu klaar om over te schakelen van de eindgebruikers naar de voorbereide kopie van de toepassing. De productiesite van de toepassing nu verandert.  Dit omvat het enkele extra stappen zoals wordt geïllustreerd in het volgende diagram.

1. Het eindpunt van de online in de ATM-profiel dat u wilt overschakelen <i>contoso-2.azurewebsites.net</i>, die verwijzen naar de V2-versie van de website (6). Nu wordt de productiesite met het V2-toepassing en het verkeer van eindgebruikers door wordt omgeleid naar deze.  
2. Als u de onderdelen van de V1-toepassing niet meer nodig, zodat u veilig kunt verwijderen (7).   

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het script bijwerken de sleuf fase moet worden beschouwd als aangetast. Als u wilt terugdraaien van de toepassing naar de status van vóór de upgrade moet u gewoon de toepassing in de productiesite naar volledige toegang herstellen. De stappen voor het worden op het volgende diagram weergegeven.    

1. Stel het database-exemplaar naar de modus lezen / schrijven (8). Hiermee herstelt u de volledige V1 functioneel in de productiesite.
2. De analyses van hoofdoorzaken uitvoeren en verwijderen van de aangetaste onderdelen in de fase sleuf (9). 

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien vereist geen wijzigingen in de ATM-profiel als deze al naar verwijst <i>contoso-1.azurewebsites.net</i> als het actief eindpunt.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

De sleutel **voordeel** van deze optie is dat u een toepassing in één regio met behulp van een set eenvoudige stappen kunt upgraden. De Dollarkosten van de upgrade is relatief laag. De belangrijkste **negatieve gevolgen voor de** is dat als er een onherstelbare fout optreedt tijdens de upgrade het herstel van de status van de vóór de upgrade zal hebben betrekking op nieuwe implementatie van de toepassing in een andere regio en het herstellen van de database vanuit back-up met geo-herstel. Dit proces zal leiden tot significante downtime.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Upgrade van toepassingen die afhankelijk van de database geo-replicatie voor herstel na noodgevallen zijn
Als uw toepassing maakt gebruik van geo-replicatie voor bedrijfscontinuïteit, wordt deze geïmplementeerd op ten minste twee verschillende regio's met een actieve implementatie in de primaire regio en een stand-by-implementatie in de regio van de back-up. Naast de factoren die eerder vermeld, moet het upgradeproces garanderen dat:

* De toepassing blijft te allen tijde tijdens het upgradeproces onherstelbare fouten beveiligd
* De geografisch redundante onderdelen van de toepassing geüpdatet zijn in combinatie met de actieve onderdelen

Als u wilt deze doelen kan bereiken u gebruik van Azure Traffic Manager (ATM) met het profiel voor een failover met één actieve en drie back-eindpunten.  Het volgende diagram illustreert de operationele omgeving voordat u het upgradeproces. De websites <i>contoso-1.azurewebsites.net</i> en <i>contoso-dr.azurewebsites.net</i> vertegenwoordigen een productiesite van de toepassing met volledige geografische redundantie. Als u wilt inschakelen, de mogelijkheid om terug te draaien de upgrade, u moet een fase sleuf maken met behulp van een volledig gesynchroniseerde kopie van de toepassing. Omdat u nodig hebt om ervoor te zorgen dat de toepassing snel herstellen kunt als een onherstelbare fout tijdens het upgradeproces optreedt, moet de sleuf fase kan ook geografisch redundant. De volgende stappen zijn vereist om voor te bereiden van de toepassing voor de upgrade:

1. Een fase sleuf maken voor de upgrade. Om te doen die een secundaire database (1) maken en implementeren van een identieke kopie van de website in dezelfde Azure-regio. Controleer de secundaire server om te zien als het seeden proces is voltooid.
2. Een geografisch redundante secundaire database maken in de fase sleuf door geo-replicatie de secundaire database naar de back-regio (dit is met de naam 'geo-replicatie in een keten'). De back-ups controleren secundaire om te zien of het seeden proces voltooid (3).
3. Een stand-by-kopie van de website in de regio van de back-up maken en koppelen aan de geografisch redundante secundaire (4).  
4. Voeg de extra eindpunten <i>contoso-2.azurewebsites.net</i> en <i>contoso-3.azurewebsites.net</i> het failover-profiel in ATM-als offline eindpunten (5). 

> [!NOTE]
> Houd er rekening mee de voorbereidende stappen heeft geen gevolgen voor de toepassing in de productiesite en deze kan worden gebruikt in de modus voor volledige toegang.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Zodra de voorbereidende stappen zijn voltooid, is de fase sleuf gereed is voor de upgrade. Het volgende diagram illustreert de stappen voor de upgrade.

1. De primaire database instellen in de productiesite naar de modus alleen-lezen (6). Dit zorgt ervoor dat de productie-instantie van de toepassing (V1) alleen-lezen tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1 en V2-database-exemplaren, blijven.  
2. Verbreek de secundaire database in dezelfde regio met behulp van de geplande beëindiging-modus (7). Een volledig gesynchroniseerde onafhankelijke kopie van de primaire database, automatisch een primaire na beëindiging wordt wordt gemaakt. Deze database wordt bijgewerkt.
3. Inschakelen van de primaire database in de sleuf fase naar de modus lezen / schrijven en uitvoeren van het script bijwerken (8).    

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Als de upgrade is voltooid u nu klaar om over te schakelen van de eindgebruikers naar de V2-versie van de toepassing bent. Het volgende diagram illustreert de stappen die betrokken zijn.

1. Het actieve eindpunt in de ATM-profiel dat u wilt overschakelen <i>contoso-2.azurewebsites.net</i>, die nu verwijst naar de V2-versie van de website (9). Wordt nu een productiesite met het V2-toepassing en verkeer van eindgebruikers door wordt omgeleid naar deze. 
2. Als u de V1-toepassing niet meer nodig, zodat u veilig kunt verwijderen (10 en 11).  

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het script bijwerken de sleuf fase moet worden beschouwd als aangetast. Als u wilt de toepassing naar de status van vóór de upgrade terugdraaien terugkeren u gewoon naar het gebruik van de toepassing in de productiesite met volledige toegang. De stappen voor het worden op het volgende diagram weergegeven.    

1. Stel het primaire database-exemplaar in de productiesite naar de modus lezen / schrijven (12). Hiermee herstelt u de volledige V1 functioneel in de productiesite.
2. De analyses van hoofdoorzaken uitvoeren en verwijderen van de aangetaste onderdelen in de sleuf fase (13 en 14). 

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien vereist geen wijzigingen in de ATM-profiel als deze al naar verwijst <i>contoso-1.azurewebsites.net</i> als het actief eindpunt.
> 
> 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

De sleutel **voordeel** van deze optie is dat u zowel de toepassing en de geografisch redundante kopie parallel upgraden kunt zonder verlies van de bedrijfscontinuïteit tijdens de upgrade. De belangrijkste **negatieve gevolgen voor de** is dat het dubbele redundantie van elk toepassingsonderdeel vereist, en daarom worden hogere Dollarkosten in rekening gebracht. Hierbij wordt ook een meer complexe workflow. 

## <a name="summary"></a>Samenvatting
De twee upgrademethode die worden beschreven in het artikel verschillen in complexiteit en de dollar kosten, maar ze beide zich richten op het minimaliseren van de tijd wanneer de eindgebruiker beperkt tot alleen-lezen bewerkingen is. Deze tijd wordt rechtstreeks gedefinieerd door de duur van het upgrade-script. Het is niet afhankelijk van de grootte van de database, de servicelaag die u hebt gekozen, de configuratie van de website en andere factoren die u eenvoudig kunt niet bepalen. Dit is omdat de voorbereidende stappen zijn ontkoppeld van de Upgradestappen en kunnen worden uitgevoerd zonder gevolgen voor de productietoepassing. De efficiëntie van het upgrade-script is de belangrijkste factor waarmee wordt bepaald ervaring voor de eindgebruiker tijdens upgrades. Zo is de beste manier kunt u deze verbeteren door ons te richten van uw inspanningen op het maken van het upgrade-script zo efficiënt mogelijk.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
* Voor meer informatie over Azure SQL Database geautomatiseerde back-ups, Zie [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
* Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor herstel, [een database herstellen vanuit de automatische back-ups](sql-database-recovery-using-backups.md).
* Zie voor meer informatie over opties voor sneller herstel, [actieve geo-replicatie](sql-database-geo-replication-overview.md).


