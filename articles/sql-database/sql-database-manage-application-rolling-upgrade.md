---
title: Rolling upgrades van toepassingen - Azure SQL Database | Microsoft Docs
description: Informatie over het gebruik van Azure SQL Database geo-replicatie voor de ondersteuning van online upgrades van uw cloudtoepassing.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 02/13/2019
ms.openlocfilehash: 47fd6c1e2bb342bc1a31fb16a45a5ebc749dca69
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621444"
---
# <a name="manage-rolling-upgrades-of-cloud-applications-by-using-sql-database-active-geo-replication"></a>Rolling upgrades van cloud-Apps beheren met behulp van SQL-Database van actieve geo-replicatie

Meer informatie over het gebruik van [actieve geo-replicatie](sql-database-auto-failover-group.md) in Azure SQL Database om het inschakelen van rolling upgrades van uw cloudtoepassing. Omdat upgrades verstorende bewerkingen zijn, moeten ze deel uitmaken van uw zakelijke continuïteit planning en ontwerp. In dit artikel hebben we kijken naar twee verschillende methoden van het upgradeproces en bespreken de voor- en nadelen van elke optie. Voor de doeleinden van dit artikel verwijzen we naar een toepassing die bestaat uit een website die verbonden met een individuele database als de gegevenslaag. Ons doel is om bij te werken van versie 1 (V1) van de toepassing op versie 2 (V2) zonder aanzienlijke invloed op de gebruikerservaring.

Houd rekening met deze factoren bij het evalueren van de upgrade-opties:

* Gevolgen voor de beschikbaarheid van toepassingen tijdens upgrades, zoals hoelang toepassingsfuncties kunnen worden beperkt of gedegradeerd.
* De mogelijkheid om terugdraaien als de upgrade mislukt.
* Beveiligingsproblemen van de toepassing als een niet-gerelateerde, onherstelbare fout tijdens de upgrade optreedt.
* Totaal aantal dollar kosten. Dit omvat aanvullende databaseredundantie en incrementele kosten voor de tijdelijke onderdelen die worden gebruikt door het upgradeproces.

## <a name="upgrade-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Upgrade uitvoeren van toepassingen die afhankelijk van de databaseback-ups voor herstel na noodgevallen zijn

Als uw toepassing is afhankelijk van automatische databaseback-ups en geografisch herstel voor herstel na noodgevallen gebruikt, wordt deze geïmplementeerd naar één Azure-regio. Om te beperken onderbreking van de gebruiker, maakt u een faseringsomgeving in die regio met alle toepassingsonderdelen die betrokken zijn bij de upgrade. Het eerste diagram illustreert de operationele omgeving voordat u het upgradeproces. Het eindpunt `contoso.azurewebsites.net` vertegenwoordigt een productie-omgeving van de web-app. Als u de upgrade terugdraaien, moet u een faseringsomgeving maken met behulp van een volledig gesynchroniseerde kopie van de database. Volg deze stappen voor het maken van een faseringsomgeving voor de upgrade:

1. Een secundaire database in dezelfde Azure-regio maken. Controleer de secundaire server om te zien of het seeden proces voltooid (1).
2. Maak een nieuwe omgeving voor uw web-app en roep deze aan 'Fasering'. Het wordt geregistreerd in Azure DNS met de URL `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Deze stappen voor gegevensvoorbereiding heeft geen gevolgen voor de productieomgeving, die kan worden gebruikt in de modus voor volledige toegang.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Wanneer de voorbereidende stappen voltooid zijn, wordt de toepassing is gereed voor de daadwerkelijke upgrade. Het volgende diagram illustreert de stappen die betrokken zijn bij het upgradeproces:

1. De primaire database is ingesteld op de modus alleen-lezen (3). In deze modus wordt gegarandeerd dat de productie-omgeving van de web-app (V1) blijft alleen-lezen tijdens de upgrade, waardoor wordt voorkomen dat gegevens verschillen tussen de V1 en V2-database-exemplaren.
2. Verbinding met het verbreken van de secundaire database met behulp van de geplande beëindiging-modus (4). Deze actie wordt een kopie synchroniseren is voltooid, onafhankelijk van de primaire database gemaakt. Deze database wordt bijgewerkt.
3. Schakel de secundaire database naar de modus lezen / schrijven en uitvoeren van het script bijwerken (5).

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Als de upgrade voltooid is, kunt u nu klaar om te schakelen tussen gebruikers naar het bijgewerkte exemplaar van de toepassing die een productie-omgeving wordt. Schakelen tussen bestaat uit enkele andere stappen, zoals wordt geïllustreerd in het volgende diagram:

1. Activeer een wisselbewerking tussen productie- en testomgevingen van de web-app (6). Met deze bewerking wordt de URL's van de twee omgevingen. Nu `contoso.azurewebsites.net` verwijst naar de V2-versie van de website en de database (productie-omgeving). 
2. Als u niet meer nodig hebt van de V1-versie, die werd een tijdelijke kopie nadat de wisseling, kunt u de faseringsomgeving (7).

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Als tijdens de upgrade is mislukt (bijvoorbeeld vanwege een fout in het script bijwerken), kunt u de testomgeving om te worden aangetast. De toepassing naar de vóór de upgrade status terug om terug te draaien de toepassing in de productieomgeving voor volledige toegang. Het volgende diagram toont de stappen opnieuw:

1. Stel het database-exemplaar naar de modus lezen / schrijven (8). Deze actie worden hersteld door de volledige V1-functionaliteit van de productie-exemplaar.
2. Uitvoeren van de hoofdoorzaak wordt onderzocht en uit bedrijf nemen de faseringsomgeving (9).

Op dit moment de toepassing volledig functioneel is en u kunt de upgrade stappen herhalen.

> [!NOTE]
> Het terugdraaien vereist geen wijzigingen in DNS omdat u een wisselbewerking nog niet hebt uitgevoerd.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Het belangrijkste voordeel van deze optie is dat u een toepassing in één regio bijwerken kunt door een reeks eenvoudige stappen te volgen. De Dollarkosten van de upgrade is relatief laag. 

Het belangrijkste verschil is dat, als er een onherstelbare fout optreedt tijdens de upgrade, het herstel van de status van de vóór de upgrade omvat het opnieuw distribueren van de toepassing in een andere regio en de database herstellen vanuit back-up met behulp van geo-herstel. Dit proces leidt er significante downtime.

## <a name="upgrade-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Upgrade uitvoeren van toepassingen die afhankelijk van de database geo-replicatie voor herstel na noodgevallen zijn

Als uw toepassing gebruikmaakt van actieve geo-replicatie of automatische failover-groepen voor bedrijfscontinuïteit, wordt deze geïmplementeerd op ten minste twee verschillende regio's. Er is een actieve, primaire database in een primaire regio en een alleen-lezen, secundaire database in een back-regio. Samen met de factoren die aan het begin van dit artikel worden vermeld, moet het upgradeproces ook die garanderen:

* De toepassing blijft te allen tijde tijdens het upgradeproces onherstelbare fouten beveiligd.
* De geografisch redundante onderdelen van de toepassing geüpdatet zijn in combinatie met de actieve onderdelen.

Voor het bereiken van deze doelstellingen, naast het gebruik van de Web-Apps-omgevingen, kunt u profiteren van Azure Traffic Manager met behulp van een failover-profiel met één actief eindpunt en een back-eindpunt. Het volgende diagram illustreert de operationele omgeving voordat u het upgradeproces. De websites `contoso-1.azurewebsites.net` en `contoso-dr.azurewebsites.net` vertegenwoordigen een productie-omgeving van de toepassing met volledige geografische redundantie. De productie-omgeving bevat de volgende onderdelen:

* De productie-omgeving van de web-app `contoso-1.azurewebsites.net` in de primaire regio (1)
* De primaire database in de primaire regio (2)
* Een stand-by-exemplaar van de web-app in de back-regio (3)
* De geo-replicatie secundaire database in de back-regio (4)
* Een prestatieprofiel van Traffic Manager met een online-eindpunt met de naam `contoso-1.azurewebsites.net` en een offline-eindpunt met de naam `contoso-dr.azurewebsites.net`

Als u wilt maken het mogelijk is de upgrade terugdraaien, moet u een faseringsomgeving maken met behulp van een volledig gesynchroniseerde kopie van de toepassing. Omdat u nodig hebt om ervoor te zorgen dat de toepassing snel herstellen kunt als een onherstelbare fout tijdens het upgradeproces optreedt, moet de faseringsomgeving ook geografisch redundante zijn. De volgende stappen zijn vereist voor het maken van een faseringsomgeving voor de upgrade:

1. Implementeer een faseringsomgeving van de web-app in de primaire regio (6).
2. Een secundaire database maken in de primaire Azure-regio (7). Configureer de faseringsomgeving van de web-app tot stand te brengen. 
3. Een andere geografisch redundante, secundaire database in de back-regio maken door de secundaire database in de primaire regio te repliceren. (Deze methode wordt aangeroepen *geo-replicatie in een keten*.) (8).
4. Implementeren van een faseringsomgeving van de web-app-exemplaar in de back-regio (9) en configureer deze als u wilt verbinding maken met de geografisch redundante secundaire database gemaakt op (8).

> [!NOTE]
> Deze stappen voor gegevensvoorbereiding heeft geen gevolgen voor de toepassing in de productieomgeving. Deze blijven volledig functioneel in de modus lezen-schrijven.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Wanneer de voorbereidende stappen voltooid zijn, is de staging-omgeving gereed is voor de upgrade. Het volgende diagram illustreert deze stappen voor de upgrade:

1. De primaire database instellen in de productie-omgeving naar de modus alleen-lezen (10). In deze modus wordt gegarandeerd dat de productiedatabase (V1) niet worden gewijzigd tijdens de upgrade, waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1 en V2-database-exemplaren.

```sql
-- Set the production database to read-only mode
ALTER DATABASE <Prod_DB>
SET (ALLOW_CONNECTIONS = NO)
```

2. Geo-replicatie beëindigen door de secundaire (11). Deze actie maakt een onafhankelijke maar volledig gesynchroniseerde kopie van de productiedatabase. Deze database wordt bijgewerkt. Het volgende voorbeeld maakt gebruik van Transact-SQL, maar [PowerShell](/powershell/module/az.sql/remove-azsqldatabasesecondary?view=azps-1.5.0) is ook beschikbaar. 

```sql
-- Disconnect the secondary, terminating geo-replication
ALTER DATABASE <Prod_DB>
REMOVE SECONDARY ON SERVER <Partner-Server>
```

3. Voer het script bijwerken op basis van `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net`, en de primaire faseringsdatabase (12). Wijzigingen in de database zal automatisch worden gerepliceerd naar de secundaire fasering.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Als de upgrade voltooid is, kunt u nu klaar om te schakelen tussen gebruikers naar de V2-versie van de toepassing. Het volgende diagram illustreert de stappen voor het:

1. Activeer een wisselbewerking tussen productie- en testomgevingen van de web-app in de primaire regio (13) en in de back-regio (14). Versie 2 van de toepassing nu wordt een productie-omgeving, met behulp van een redundante kopie in de back-regio.
2. Als u de V1-toepassing (15 en 16) niet meer nodig hebt, kunt u de faseringsomgeving.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Als tijdens de upgrade is mislukt (bijvoorbeeld vanwege een fout in het script bijwerken), kunt u overwegen de faseringsomgeving zich in een inconsistente toestand terechtkomen. Als u wilt de toepassing naar de status van vóór de upgrade terugdraaien, teruggaan naar het gebruik van v1-versie van de toepassing in de productieomgeving. De vereiste stappen worden weergegeven in het volgende diagram:

1. Stel het primaire database-exemplaar in de productie-omgeving naar de modus lezen / schrijven (17). Deze actie worden hersteld door volledige V1-functionaliteit in de productieomgeving.
2. Uitvoeren van de hoofdoorzaak wordt onderzocht en herstellen of verwijderen van de faseringsomgeving (18 en 19).

Op dit moment de toepassing volledig functioneel is en u kunt de upgrade stappen herhalen.

> [!NOTE]
> Het terugdraaien vereist geen dat DNS gewijzigd omdat u een swap-bewerking niet uitvoeren.

![Configuratie van de SQL-Database geo-replicatie voor noodherstel van de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Het belangrijkste voordeel van deze optie is dat u zowel de toepassing en de geografisch redundante kopie parallel upgraden kunt zonder verlies van de bedrijfscontinuïteit tijdens de upgrade.

Het belangrijkste verschil is dat het dubbele redundantie van elk toepassingsonderdeel vereist, en daarom worden hogere Dollarkosten in rekening gebracht. Hierbij wordt ook een meer complexe workflow.

## <a name="summary"></a>Samenvatting

De twee upgrademethode die worden beschreven in het artikel verschillen in complexiteit en Dollarkosten, maar ze beide richten zich op het minimaliseren van hoe lang de gebruiker is beperkt tot alleen-lezen bewerkingen. Deze tijd wordt rechtstreeks gedefinieerd door de duur van het upgrade-script. Deze niet afhankelijk is van de grootte van de database, de servicelaag die u hebt gekozen, de websiteconfiguratie van de of andere factoren die u eenvoudig kunt niet bepalen. Alle voorbereidende stappen zijn ontkoppeld van de Upgradestappen en niet van invloed zijn op de productietoepassing. De efficiëntie van het upgrade-script is een belangrijke factor waarmee wordt bepaald van de gebruikerservaring tijdens upgrades. Dus, is de beste manier om deze ervaring te verbeteren om u te concentreren op het maken van het upgrade-script zo efficiënt mogelijk.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
* Zie voor meer informatie over Azure SQL Database van actieve geo-replicatie, [leesbare secundaire databases met behulp van actieve geo-replicatie maken](sql-database-active-geo-replication.md).
* Zie voor meer informatie over automatische failovergroepen Azure SQL Database, [automatische failover-groepen gebruiken voor het inschakelen van transparante en gecoördineerd failover van meerdere databases](sql-database-auto-failover-group.md).
* Zie voor meer informatie over het faseringsomgevingen in Azure App Service, [faseringsomgevingen in Azure App Service instellen](../app-service/deploy-staging-slots.md).
* Zie voor meer informatie over Azure Traffic Manager-profielen, [een Azure Traffic Manager-profiel beheren](../traffic-manager/traffic-manager-manage-profiles.md).
