---
title: Wat is de service Azure SQL Database? | Microsoft Docs
description: 'Get an introduction to SQL Database: technical details and capabilities of Microsoft''s relational database management system (RDBMS) in the cloud.'
keywords: inleiding in sql,intro in sql,wat is sql-database
services: sql-database
documentationcenter: 
author: shontnew
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a139b97e20685a5ced513c1fd180a74e91c497a7
ms.lasthandoff: 04/15/2017

---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Wat is SQL Database? Inleiding tot SQL Database
SQL Database is een relationele database-service in de Microsoft Cloud op basis van de toonaangevende Microsoft SQL Server-engine en is in staat bedrijfskritieke workloads af te handelen. SQL Database biedt voorspelbare prestaties op meerdere serviceniveaus, dynamische schaalbaarheid zonder uitvaltijd, bedrijfscontinuïteit en gegevensbeveiliging, allemaal praktisch zonder beheer. Dankzij deze mogelijkheden kunt u zich richten op het sneller ontwikkelen en op de markt brengen van apps, in plaats van kostbare tijd en middelen in te zetten voor het beheer van virtuele machines en infrastructuur. Omdat SQL Database is gebaseerd op de [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx)-engine, worden bestaande SQL Server-hulpprogramma's, -bibliotheken en -API's ondersteund. Als gevolg hiervan kunt u eenvoudig nieuwe oplossingen ontwikkelen, uw bestaande SQL Server-oplossingen verplaatsen en uw bestaande SQL Server-oplossingen uitbreiden naar de Microsoft Cloud zonder nieuwe vaardigheden te hoeven aanleren.

Dit artikel bevat een inleiding tot de belangrijkste concepten en functies van SQL Database met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar meer gedetailleerde informatie. Zie deze Quick Starts om snel aan de slag te gaan:
 - [Een SQL-database maken in Azure Portal](sql-database-get-started-portal.md)  
 - [Een SQL-database maken met de Azure CLI](sql-database-get-started-cli.md)
 - [Een SQL-database maken met PowerShell](sql-database-get-started-powershell.md)

Zie de volgende artikelen voor een reeks Azure CLI- en PowerShell-voorbeelden:
 - [Azure CLI-voorbeelden voor Azure SQL Database](sql-database-cli-samples.md)
 - [Azure PowerShell-voorbeelden voor Azure SQL Database](sql-database-powershell-samples.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en schaal aanpassen zonder uitvaltijd
De SQL Database-service biedt drie servicelagen: Basic, Standard, Premium en Premium RS. Elke servicelaag biedt [verschillende niveaus qua prestaties en mogelijkheden](sql-database-service-tiers.md) voor lichte tot zware workloads van databases. U kunt uw eerste app ontwikkelen op een kleine database voor slechts enkele euro's per maand. Vervolgens kunt u op elk gewenst moment handmatig of programmatisch [de servicelaag wijzigen](sql-database-service-tiers.md), zodat deze beter past bij de behoeften van uw oplossing. U kunt dit doen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.

## <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastische pools voor optimaal resourcegebruik
Voor veel bedrijven en apps is het kunnen maken van enkele databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. [Elastische pools](sql-database-elastic-pool.md) zijn ontworpen om dit probleem te verhelpen. Het concept is eenvoudig. U wijst prestatieresources toe aan een pool in plaats van aan een individuele database en betaalt voor de collectieve prestatieresources van de pool in plaats van voor de prestaties van een individuele database. Met elastische pools hoeft u zich niet bezig te houden met het verhogen en verlagen van de databaseprestaties als de vraag naar resources fluctueert. De gepoolde databases maken naar behoefte gebruik van de prestatieresources van de elastische pool. Gepoolde databases tellen mee voor het verbruik tot het maximum voor de pool is bereikt. Zo blijven uw kosten voorspelbaar, ook al is uw gebruik van de individuele database dat niet. Bovendien kunt u [databases aan de groep toevoegen of eruit verwijderen](sql-database-elastic-pool-manage-portal.md). Zo kan uw app kan worden opgeschaald van een handjevol databases naar duizenden databases, allemaal binnen het budget dat u zelf bepaalt. Ten slotte kunt u ook de minimale en maximale beschikbare resources die voor databases beschikbaar zijn in de pool beheren om ervoor te zorgen dat er geen database in de pool is die alle poolresources gebruikt en dat elke gepoolde database een gegarandeerd minimum aan resources heeft. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

## <a name="blend-single-databases-with-pooled-databases"></a>Individuele databases combineren met gepoolde databases
Waarvoor u ook kiest - individuele databases of elastische pools - u zit er niet aan vast. U kunt individuele databases combineren met elastische pools en de servicelagen van individuele databases en elastische pools snel en eenvoudig aanpassen aan de situatie. Bovendien kunt u, dankzij de kracht en het bereik van Azure, andere Azure-services combineren en integreren met SQL Database om te voldoen aan de behoeften voor uw unieke app-ontwerp, kosten te besparen en resources efficiënt te beheren. Daarnaast kunt u nieuwe zakelijke verkoopkansen creëren.

## <a name="monitoring-and-alerting"></a>Bewaking en waarschuwingen
Maar hoe kunt u de relatieve prestaties van individuele databases en elastische pools vergelijken? Hoe weet u wanneer u moet stoppen met omhoog of omlaag schalen? U gebruikt de [ingebouwde tools voor prestatiebewaking](sql-database-performance.md) en [waarschuwingen](sql-database-insights-alerts-portal.md), gecombineerd met de prestatiebeoordelingen op basis van [Database Transaction Units (DTU's) voor individuele databases en DTU's (eDTU's) voor elastische pools](sql-database-what-is-a-dtu.md). Met behulp van deze tools kunt u snel beoordelen wat de impact is van het aanpassen van de schaal op basis van uw huidige prestatiebehoeften of de prestatiebehoeften van uw project. Zie [SQL Database-opties en prestaties: wat is er beschikbaar in elke servicelaag](sql-database-service-tiers.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
De toonaangevende serviceovereenkomst [(SLA)](http://azure.microsoft.com/support/legal/sla/) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt u ervoor dat uw app continu (24 uur per dag, 7 dagen per week) in de lucht blijft. Elke SQL-database is voorzien van ingebouwde beveiliging, fouttolerantie en [gegevensbeveiliging](sql-database-automated-backups.md) die u anders zelf zou moeten kopen of ontwerpen, ontwikkelen en beheren. Elke servicelaag van SQL Database bevat een uitgebreide set functies en opties voor bedrijfscontinuïteit, waarmee u ervoor kunt zorgen dat u aan de slag kunt gaan en blijven. Met behulp van [herstelpunten](sql-database-recovery-using-backups.md) kunt u een database terugzetten naar een eerdere toestand, tot 35 dagen geleden. U kunt [back-upretentie voor een lange termijn](sql-database-long-term-retention.md) configureren voor het opslaan van back-ups in een beveiligde kluis gedurende maximaal tien jaar. Als er een storing optreedt in het datacenter dat uw databases host, kunt u bovendien databases terugzetten met [geografisch redundante back-upkopieën](sql-database-recovery-using-backups.md). Indien nodig kunt u ook [geografisch redundante leesbare replica's](sql-database-geo-replication-overview.md) configureren in een of meer regio's voor snelle failover in het geval van een storing in een datacenter. U kunt deze replica's ook gebruiken voor snellere leesprestaties in verschillende geografische regio's of voor [toepassingsupgrades zonder uitvaltijd](sql-database-manage-application-rolling-upgrade.md). 

![Geo-replicatie in SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

Zie [Bedrijfscontinuïteit](sql-database-business-continuity.md) voor meer informatie over de verschillende functies voor bedrijfscontinuïteit die beschikbaar zijn per servicelaag.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
SQL Server heeft traditiegetrouw een uitstekende gegevensbeveiliging. Deze traditie wordt voortgezet in SQL Database, met functies voor toegangsbeperking, gegevensbeveiliging en activiteitsbewaking. Zie [Beveiliging van uw SQL-database](sql-database-security-overview.md) voor een kort overzicht van beveiligingsopties in SQL Database. Zie het [Beveiligingscentrum voor SQL Server Database-engine en SQL Database](https://msdn.microsoft.com/library/bb510589) voor een uitgebreid overzicht van beveiligingsfuncties. Bezoek ook eens het [Vertrouwenscentrum van Azure](https://azure.microsoft.com/support/trust-center/security/) voor informatie over de beveiliging van het Azure-platform.

## <a name="next-steps"></a>Volgende stappen
Nu u de inleiding tot SQL Database hebt gelezen en weet wat SQL Database is, bent u klaar voor de volgende stappen:

* Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/) voor hulpprogramma's voor het berekenen en vergelijken van de kosten voor individuele databases en elastische pools.
* Lees meer over [elastische groepen](sql-database-elastic-pool.md).
* Ga aan de slag met het [maken van uw eerste database](sql-database-get-started-portal.md).
* Bouw uw eerste app in C#, Java, Node.js, PHP, Python of Ruby: [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)

