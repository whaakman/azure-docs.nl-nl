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
ms.date: 01/29/2019
ms.openlocfilehash: 1aa3960e3a974703cfecec2bd28fc41f74f7df96
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238404"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Rolling upgrades van cloud-toepassingen met behulp van SQL-Database van actieve geo-replicatie beheren

Meer informatie over het gebruik van [actieve geo-replicatie](sql-database-auto-failover-group.md) in SQL-Database voor het inschakelen van rolling upgrades van uw cloudtoepassing. Omdat een verstorende bewerking is voor upgrade, moeten deze deel uitmaken van uw zakelijke continuïteit planning en ontwerp. In dit artikel we kijken naar twee verschillende methoden van het upgradeproces en bespreken de voor- en -en nadelen van elke optie. Voor de doeleinden van dit artikel gebruiken we een toepassing die uit een website die is verbonden met een enkele database als de gegevenslaag bestaat. Ons doel is om bij te werken van versie 1 van de toepassing op versie 2 zonder aanzienlijke invloed op de ervaring voor de eindgebruiker.

Bij het evalueren van de upgrade-opties, moet u de volgende factoren overwegen:

* Gevolgen voor de beschikbaarheid van toepassingen tijdens upgrades. Hoe lang de functie van de toepassing is mogelijk beperkt of gedegradeerd.
* De mogelijkheid om terugdraaien als de upgrade mislukt.
* Beveiligingsproblemen van de toepassing als een niet-gerelateerde onherstelbare fout tijdens de upgrade optreedt.
* Totaal aantal dollar kosten.  Dit omvat aanvullende redundantie en incrementele kosten voor de tijdelijke onderdelen die worden gebruikt door het upgradeproces.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Upgrade van toepassingen die afhankelijk van de databaseback-ups voor herstel na noodgevallen zijn

Als uw toepassing is afhankelijk van automatische databaseback-ups en geografisch herstel voor herstel na noodgevallen gebruikt, wordt deze geïmplementeerd naar één Azure-regio. Als u wilt de onderbreking van de eindgebruiker minimaliseren, maakt u een faseringsomgeving in die regio met alle toepassingsonderdelen die betrokken zijn bij de upgrade. Het volgende diagram illustreert de operationele omgeving voordat u het upgradeproces. Het eindpunt `contoso.azurewebsites.net` vertegenwoordigt een productiesite van de web-App. Om in te schakelen de mogelijkheid om de upgrade terugdraaien, moet u een staging-site maken met behulp van een volledig gesynchroniseerde kopie van de database. De volgende stappen maakt u een faseringsomgeving voor de upgrade:

1. Een secundaire database in dezelfde Azure-regio maken. Controleer de secundaire server om te zien of het seeden proces voltooid (1).
2. Maak een nieuwe implementatiesite voor uw webtoepassing met de naam 'Staging'. Het wordt geregistreerd in DNS met URL `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Houd er rekening mee de productiesite heeft geen gevolgen voor de voorbereidende stappen en deze kan worden gebruikt in de modus voor volledige toegang.
>  

![Configuratie van SQL Database-Go-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Zodra de voorbereidende stappen zijn voltooid, wordt de toepassing is gereed voor de daadwerkelijke upgrade. Het volgende diagram illustreert de stappen die betrokken zijn bij het upgradeproces.

1. De primaire database is ingesteld op de modus alleen-lezen (3). In deze modus wordt gegarandeerd dat de productiesite van de web-App (V1) blijft alleen-lezen tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1 en V2-database-exemplaren.  
2. De secundaire database met behulp van de geplande beëindiging-modus (4) de verbinding verbreken. Een volledig gesynchroniseerde onafhankelijke kopie van de primaire database wordt gemaakt. Deze database wordt bijgewerkt.
3. Schakel de primaire database naar de modus lezen / schrijven en uitvoeren van het script bijwerken (5).

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Als de upgrade is voltooid, bent u nu gereed voor de eindgebruikers overschakelen naar het bijgewerkte exemplaar van de toepassing. Het wordt nu een productiesite geworden.  Het overschakelen, omvat enkele extra stappen zoals wordt geïllustreerd in het volgende diagram.

1. Activeer een wisselbewerking tussen productie en fasering slots van de web-App (6). Het wordt de URL's van de twee sleuven overschakelen. Nu `contoso.azurewebsites.net` verwijst naar een V2-versie van de website en de database (productie-omgeving).  
2. Als u niet meer nodig hebt van de V1-versie, die werd een tijdelijke kopie nadat de wisseling, kunt u de staging envoronment (7).

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het script bijwerken de sleuf fase moet worden beschouwd als aangetast. Aan de toepassing naar de status van vóór de upgrade terugdraaien, moet u de toepassing in de productiesite naar volledige toegang herstellen. De stappen voor het worden op het volgende diagram weergegeven.

1. Stel het database-exemplaar naar de modus lezen / schrijven (8). Dit wordt de volledige V1 functioneel van de productie-exemplaar herstellen.
2. De analyses van hoofdoorzaken uitvoeren en uit bedrijf nemen de faseringsomgeving (9).

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien is niet vereist voor DNS-wijzigingen Als u een wisselbewerking nog niet hebt uitgevoerd.

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

De sleutel **voordeel** van deze optie is dat u een toepassing in één regio met behulp van een set eenvoudige stappen kunt upgraden. De Dollarkosten van de upgrade is relatief laag. De belangrijkste **negatieve gevolgen voor de** is dat als er een onherstelbare fout optreedt tijdens de upgrade van het herstel van de status van de vóór de upgrade wordt betrekking hebben op opnieuw distribueren van de toepassing in een andere regio en het herstellen van de database vanuit back-up met geo-herstel. Dit proces zal leiden tot significante downtime.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Upgrade van toepassingen die afhankelijk van de database geo-replicatie voor herstel na noodgevallen zijn

Als uw toepassing maakt gebruik van actieve geo-replicatie of failover-groepen voor zakelijke continuïteit en wordt deze geïmplementeerd op ten minste twee verschillende regio's met een actieve primaire database in de primaire regio en een secundaire database alleen-lezen in de regio van de back-up. Naast de factoren die eerder vermeld, moet het upgradeproces garanderen dat:

* De toepassing blijft te allen tijde tijdens het upgradeproces onherstelbare fouten beveiligd
* De geografisch redundante onderdelen van de toepassing geüpdatet zijn in combinatie met de actieve onderdelen

Voor het bereiken van deze doelstellingen, naast het gebruik van de implementatiesites voor Web-App, wordt u gebruikmaken van Azure Traffic Manager (ATM) met een profiel voor een failover met één actieve en één back-eindpunten.  Het volgende diagram illustreert de operationele omgeving voordat u het upgradeproces. De websites `contoso-1.azurewebsites.net` en `contoso-dr.azurewebsites.net` vertegenwoordigen een productie-omgeving van de toepassing met volledige geografische redundantie. De productie-omgeving bevat de volgende onderdelen:

1. Productiesite van de webtoepassing `contoso-1.azurewebsites.net` in de primaire regio (1)
2. Primaire database in de primaire regio (2) 
3. Een stand-by-exemplaar van de web-App in de back-regio (3)
4. Secundaire database via geo-replicatie in de back-regio (4)
5. Azure traffic manager performance-profiel met online-eindpunt `contoso-1.azurewebsites.net` en offline-eindpunt `contoso-dr.azurewebsites.net`

Om in te schakelen de mogelijkheid om de upgrade terugdraaien, moet u een faseringsomgeving maken met behulp van een volledig gesynchroniseerde kopie van de toepassing. Omdat u nodig hebt om ervoor te zorgen dat de toepassing snel herstellen kunt als een onherstelbare fout tijdens het upgradeproces optreedt, moet de testomgeving kan ook geografisch redundant. De volgende stappen zijn vereist voor het maken van een faseringsomgeving voor de upgrade:

1. Implementeren van een staging-site van de web-App in de primaire regio (6)
2. Een secundaire database maken in de primaire Azure-regio (7). Configureer de staging-site van de webtoepassing tot stand te brengen. 
3. Een andere geografisch redundante secundaire database in de regio van de back-up maken door te repliceren van de secundaire database in de primaire regio (dit is met de naam "geo-replicatie in een keten") (8).
3. Implementeren van een staging-site van het exemplaar van de web-toepassing in de back-regio (9) en configureer deze als u wilt verbinding maken met de geo-secundaire gemaakt in stap (9).


> [!NOTE]
> Houd er rekening mee de voorbereidende stappen heeft geen gevolgen voor de toepassing in de productiesite en blijft het volledig functioneel in de modus lezen-schrijven.

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Zodra de voorbereidende stappen zijn voltooid, is de staging-omgeving gereed is voor de upgrade. Het volgende diagram illustreert de stappen voor de upgrade.

1. De primaire database instellen in de productiesite naar de modus alleen-lezen (10). In deze modus wordt gegarandeerd dat de productiedatabase (V1) niet worden gewijzigd tijdens de upgrade waardoor wordt voorkomen dat de gegevensdivergentie tussen de V1 en V2-database-exemplaren.  
2. Verbreek de secundaire database in dezelfde regio met behulp van de geplande beëindiging-modus (11). Een onafhankelijke maar volledig gesynchroniseerde kopie van de productiedatabase wordt gemaakt. Deze database wordt bijgewerkt.
3. Voer het script bijwerken op basis van `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` en de primaire faseringsdatabase (12). Wijzigingen in de database zal automatisch worden gerepliceerd naar de secundaire fasering 

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Als de upgrade is voltooid, bent u nu klaar om over te schakelen van de eindgebruikers naar de V2-versie van de toepassing. Het volgende diagram illustreert de stappen die betrokken zijn.

1. Activeer een wisselbewerking tussen productie en fasering slots van de web-App in de primaire regio (13) en in de back-regio (14). Versie 2 van de toepassing nu wordt een productiesite met behulp van een redundante kopie in de back-regio.
2. Kunt u de de faseringsomgeving als u de V1-toepassing (15 en 16) niet meer nodig hebt.  

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Als tijdens de upgrade mislukt is, bijvoorbeeld vanwege een fout in het script bijwerken de faseringsomgeving worden beschouwd als in een inconsistente status. U terugkeren naar het gebruik van v1-versie van de toepassing in productie-omgeving te draaien, maar de toepassing naar de status van vóór de upgrade. De vereiste stappen worden op het volgende diagram weergegeven.

1. Stel het primaire database-exemplaar in de productiesite naar de modus lezen / schrijven (17). Het herstelt u de volledige V1 functioneel in de productiesite.
2. Uitvoeren van de analyses van hoofdoorzaken en herstellen of verwijderen van de faseringsomgeving (18 en 19).

Op dit moment de toepassing volledig functioneel is en de stappen voor de upgrade kunnen worden herhaald.

> [!NOTE]
> Het terugdraaien is niet vereist voor DNS-wijzigingen omdat u niet een wisselbewerking hebt uitgevoerd.

![Configuratie van SQL Database-geo-replicatie. Herstel na noodgevallen in de cloud.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

De sleutel **voordeel** van deze optie is dat u zowel de toepassing en de geografisch redundante kopie parallel upgraden kunt zonder verlies van de bedrijfscontinuïteit tijdens de upgrade. De belangrijkste **negatieve gevolgen voor de** is dat het dubbele redundantie van elk toepassingsonderdeel vereist, en daarom worden hogere Dollarkosten in rekening gebracht. Hierbij wordt ook een meer complexe workflow.

## <a name="summary"></a>Samenvatting

De twee upgrademethode die worden beschreven in het artikel verschillen in complexiteit en de dollar kosten, maar ze beide zich richten op het minimaliseren van de tijd wanneer de eindgebruiker beperkt tot alleen-lezen bewerkingen is. Deze tijd wordt rechtstreeks gedefinieerd door de duur van het upgrade-script. Het is niet afhankelijk van de grootte van de database, de servicelaag die u hebt gekozen, de configuratie van de website en andere factoren die u eenvoudig kunt niet bepalen. De voorbereidende stappen zijn ontkoppeld van de Upgradestappen en niet van invloed op de productietoepassing. De efficiëntie van het upgrade-script is een belangrijke factor waarmee wordt bepaald ervaring voor de eindgebruiker tijdens upgrades. Zo is de beste manier kunt u deze verbeteren door ons te richten van uw inspanningen op het maken van het upgrade-script zo efficiënt mogelijk.  

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht voor zakelijke continuïteit en scenario's, [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md).
* Zie voor meer informatie over Azure SQL Database actieve geo-replicatie, [leesbare secundaire databases met behulp van actieve geo-replicatie maken](sql-database-active-geo-replication.md).
* Zie voor meer informatie over Azure SQL Database-Failover-groepen, [automatische failover-groepen gebruiken voor het inschakelen van transparante en gecoördineerd failover van meerdere databases](sql-database-auto-failover-group.md).
* Zie voor meer informatie over implementatiesites en testomgeving in Azure App Service, [faseringsomgevingen in Azure App Service instellen](../app-service/deploy-staging-slots.md).  
* Zie voor meer Azure traffic manager-profielen, [een Azure Traffic Manager-profiel beheren](../traffic-manager/traffic-manager-manage-profiles.md).  


