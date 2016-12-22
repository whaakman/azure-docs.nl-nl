---
title: Wat is SQL Database? Inleiding tot SQL Database | Microsoft Docs
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
ms.date: 11/08/2016
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 37534ff6366bd519cec50bc033b2bcd8f8bda5c7


---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Wat is SQL Database? Inleiding tot SQL Database
SQL Database is een relationele database-service in de cloud op basis van de toonaangevende Microsoft SQL Server-engine met bedrijfskritieke mogelijkheden. SQL Database biedt voorspelbare prestaties, schaalbaarheid zonder uitvaltijd, bedrijfscontinuïteit en gegevensbeveiliging, allemaal praktisch zonder beheer. U kunt zich richten op het sneller ontwikkelen en op de markt brengen van apps, in plaats van het beheer van virtuele machines en infrastructuur. Omdat SQL Database is gebaseerd op de [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx)-engine, worden bestaande SQL Server-hulpprogramma's, -bibliotheken en -API's ondersteund voor een soepele verplaatsing en uitbreiding naar de cloud.

Dit artikel bevat een inleiding tot de belangrijkste concepten en functies van SQL Database met betrekking tot prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar meer gedetailleerde informatie. Als u zover bent, kunt u in een paar minuten [uw eerste SQL-database maken](sql-database-get-started.md) of [een elastische pool maken](sql-database-elastic-pool-create-portal.md). Zie voor meer informatie deze video van 30 minuten.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON326/player]
> 
> 

## <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en schaal aanpassen zonder uitvaltijd
SQL-databases zijn beschikbaar in verschillende *servicelagen*: Basic, Standard en Premium. Elke servicelaag biedt [verschillende niveaus qua prestaties en mogelijkheden](sql-database-service-tiers.md) voor lichte tot zware workloads van databases. U kunt uw eerste app ontwikkelen op een kleine database voor slechts enkele euro's per maand. Vervolgens kunt u handmatig of programmatisch [de servicelaag wijzigen](sql-database-scale-up.md) als uw app wereldwijd een succes wordt, zonder uitvaltijd voor uw app of uw klanten.

Voor veel bedrijven en apps is het kunnen maken van zelfstandige databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren.

[Elastische groepen](sql-database-elastic-pool.md) in SQL Database vormen de oplossing voor dit probleem. Het concept is eenvoudig. U wijst prestaties aan een pool toe en betaalt voor de collectieve prestaties van de pool in plaats van voor de prestaties van zelfstandige databases. U hoeft de prestaties van de database niet omhoog of omlaag te schalen. De databases in de pool, de *elastische databases*, worden automatisch omhoog of omlaag geschaald om aan de vraag te voldoen. Elastische databases tellen mee voor het verbruik tot het maximum voor de pool is bereikt. Zo blijven uw kosten voorspelbaar, ook al is uw databasegebruik dat niet. Bovendien kunt u [databases aan de groep toevoegen of eruit verwijderen](sql-database-elastic-pool-manage-portal.md). Zo kan uw app kan worden opgeschaald van een handjevol databases naar duizenden databases, allemaal binnen het budget dat u zelf bepaalt. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

Welke oplossing u ook kiest, individuele of elastische databases, u kunt altijd nog switchen. U kunt zelfstandige databases combineren met elastische pools, en de servicelagen van zelfstandige databases en pools snel en eenvoudig aanpassen aan de situatie. Bovendien kunt u, dankzij de kracht en het bereik van Azure, andere Azure-services combineren en integreren met SQL Database om te voldoen aan de behoeften voor uw unieke app-ontwerp, kosten te besparen en resources efficiënt te beheren. Daarnaast kunt u nieuwe zakelijke verkoopkansen creëren.

Maar hoe kunt u de relatieve prestaties van databases en databasegroepen vergelijken? Hoe weet u wanneer u moet stoppen met omhoog of omlaag schalen? Het geheim zit in de ingebouwde prestatiecontrole- en meldingsprogramma's en de prestatiebeoordelingen op basis van Database Transaction Units (DTU's) voor zelfstandige databases, en elastische DTU's (eDTU's) voor Elastic Databases en databasepools. Hiermee kunt u snel de gevolgen van omhoog of omlaag schalen beoordelen op basis van uw huidige prestaties of de prestaties van een project. Zie [SQL Database-opties en prestaties: wat is er beschikbaar in elke servicelaag](sql-database-service-tiers.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Continuïteit van uw app en uw bedrijf
De toonaangevende serviceovereenkomst [(SLA)](http://azure.microsoft.com/support/legal/sla/) van Azure met 99,99% beschikbaarheid dankzij een wereldwijd netwerk van door Microsoft beheerde datacenters, zorgt u ervoor dat uw app continu (24 uur per dag, 7 dagen per week) in de lucht blijft. Elke SQL-database is voorzien van ingebouwde beveiliging, fouttolerantie en gegevensbeveiliging die u anders zelf zou moeten kopen of ontwerpen, ontwikkelen en beheren. Toch kan het zijn dat u, afhankelijk van de eisen van uw bedrijf, aanvullende beveiligingslagen nodig hebt, zodat uw app en uw bedrijfsvoering snel kunnen worden hersteld na een noodgeval, fout of andere storing. Elke servicelaag van SQL Database bevat een uitgebreide set functies en opties voor bedrijfscontinuïteit, waarmee u ervoor kunt zorgen dat u aan de slag kunt gaan en blijven. Met behulp van herstelpunten kunt u een database terugzetten naar een eerdere toestand, tot 35 dagen geleden. Als er een storing optreedt in het datacenter dat uw databases host, kunt u bovendien databases herstellen met geografisch redundante back-upkopieën, of een failover naar databasereplica's in een andere regio uitvoeren. U kunt ook replica's gebruiken voor upgrades of verplaatsingen naar andere regio's.

![Geo-replicatie in SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

Zie [Bedrijfscontinuïteit](sql-database-business-continuity.md) voor meer informatie over de verschillende functies voor bedrijfscontinuïteit die beschikbaar zijn per servicelaag.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
SQL Server heeft traditiegetrouw een uitstekende gegevensbeveiliging. Deze traditie wordt voortgezet in SQL Database, met functies voor toegangsbeperking, gegevensbeveiliging en activiteitsbewaking. Zie [Beveiliging van uw SQL-database](sql-database-security.md) voor een kort overzicht van beveiligingsopties in SQL Database. Zie het [Beveiligingscentrum voor SQL Server Database-engine en SQL Database](https://msdn.microsoft.com/library/bb510589) voor een uitgebreid overzicht van beveiligingsfuncties. Bezoek ook eens het [Vertrouwenscentrum van Azure](https://azure.microsoft.com/support/trust-center/security/) voor informatie over de beveiliging van het Azure-platform.

## <a name="next-steps"></a>Volgende stappen
Nu u de inleiding tot SQL Database hebt gelezen en weet wat SQL Database is, bent u klaar voor de volgende stappen:

* Zie de [Pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/) voor berekeningen en vergelijkingen van de kosten voor zelfstandige databases en elastische pools.
* Lees meer over [elastische groepen](sql-database-elastic-pool.md).
* Ga aan de slag met het [maken van uw eerste database](sql-database-get-started.md).
* [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
* Bouw uw eerste app in C#, Java, Node.js, PHP, Python of Ruby: [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)



<!--HONumber=Dec16_HO2-->


