---
title: Uitschalen met Azure SQL Database | Microsoft Docs
description: Software als een Service (SaaS)-ontwikkelaars eenvoudig elastische, schaalbare databases kunt maken in de cloud met behulp van deze hulpprogramma 's
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: d15a2e3f-5adf-41f0-95fa-4b945448e184
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 5bb6d17ffd047ae91476c52750f293414d1dfdd5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="scaling-out-with-azure-sql-database"></a>Uitbreiden met Azure SQL Database
Kunt u eenvoudig uitschalen Azure SQL-databases via de **elastische Database** hulpprogramma's. Deze hulpprogramma's en onderdelen kunnen u de databaseresources vrijwel onbeperkte van **Azure SQL Database** om oplossingen voor transactionele werkbelastingen en met name de Software als een Service (SaaS)-toepassingen te maken. Elastische databasefuncties bestaan uit het volgende:

* [Clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md): de clientbibliotheek is een functie waarmee u maken en onderhouden van shard-databases.  Zie [aan de slag met hulpprogramma's voor elastische Database](sql-database-elastic-scale-get-started.md).
* [Elastische Database gesplitste merge tool](sql-database-elastic-scale-overview-split-and-merge.md): verplaatst gegevens tussen de shard-databases. Dit is handig voor het verplaatsen van gegevens uit een multitenant-database naar een single-tenant-database (of andersom). Zie [elastische database gesplitste Merge tool zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Elastische Database taken](sql-database-elastic-jobs-overview.md) (preview): taken gebruiken voor het beheren van grote aantallen Azure SQL-databases. Gemakkelijk administratieve bewerkingen, zoals wijzigingen in het schema, Referentiebeheer, verwijzing Gegevensupdates, prestatiegegevens verzamelen of verzamelen van telemetriegegevens in de tenant (klant) met behulp van taken uitvoeren.
* [Elastische databasequery](sql-database-elastic-query-overview.md) (preview): Hiermee kunt u een Transact-SQL-query die meerdere databases omvat uit te voeren. Hierdoor kan de verbinding met de hulpprogramma's voor rapportage zoals Excel, Power BI, Tableau, enz.
* [Elastische transacties](sql-database-elastic-transactions-overview.md): deze functie kunt u transacties op meerdere verschillende databases in Azure SQL Database uitvoeren. Elastische databasetransacties zijn beschikbaar voor .NET-toepassingen die gebruikmaken van ADO .NET en integreren met de bekende programmering ervaring met de [System.Transaction klassen](https://msdn.microsoft.com/library/system.transactions.aspx).

De onderstaande afbeelding toont een architectuur met de **elastische Database functies** ten opzichte van een verzameling van databases.

In deze afbeelding vertegenwoordigen de kleuren van de database schema's. Databases met dezelfde kleur delen hetzelfde schema.

1. Een set **Azure SQL-databases** worden gehost op Azure met behulp van sharding-architectuur.
2. De **clientbibliotheek voor elastische Database** wordt gebruikt voor het beheren van een set shard.
3. Een subset van de databases worden die pakketten in een **elastische pool**. (Zie [wat is er een groep?](sql-database-elastic-pool.md)).
4. Een **elastische Database-taak** gepland of ad-hoc T-SQL-scripts uitvoeren op alle databases.
5. De **gesplitste merge tool** wordt gebruikt voor het verplaatsen van gegevens van één shard naar een andere.
6. De **elastische Database query** kunt u een query die alle databases in de shard-set omvat schrijven.
7. **Elastische transacties** kunt u transacties op meerdere verschillende databases uitvoeren. 

![Hulpprogramma's voor elastische databases][1]

## <a name="why-use-the-tools"></a>Waarom de hulpprogramma's gebruiken?
Achieving elasticiteit en schaal voor cloud-toepassingen eenvoudig VM's en de blob-opslag simpelweg toevoegen of afgetrokken eenheden of vergroot power. Maar het moeilijk voor stateful gegevensverwerking in relationele databases is gebleven. Uitdagingen ontstaan in deze scenario's:

* Groeien en capaciteit voor de relationele database deel uitmaakt van uw workload te verkleinen.
* Het beheren van hotspots die kunnen ontstaan die invloed hebben op een specifieke subset van gegevens -, zoals een bijzonder bezet end-klant (tenant).

Traditioneel zijn scenario's zoals deze door te investeren in grotere schaal databaseservers ter ondersteuning van de toepassing gericht. Deze optie wordt echter beperkt in de cloud waarbij alle bijbehorende verwerkingen op vooraf gedefinieerde basishardware gebeurt. Distributie van gegevens en verwerking tussen meerdere databases identiek gestructureerd bevat (een patroon voor scale-out 'sharding' genoemd) in plaats daarvan een alternatief voor traditionele omhoog schalen benaderingen zowel op het gebied kosten en de elasticiteit.

## <a name="horizontal-and-vertical-scaling"></a>Horizontale en verticale schalen
De afbeelding hieronder ziet de horizontale en verticale afmetingen van de schaal, die de manieren waarop die de elastische databases kunnen worden geschaald.

![Horizontale en verticale Scaleout][2]

Horizontaal schalen verwijst naar het toevoegen of verwijderen van databases om aan te passen capaciteit of de algehele prestaties. Dit is een afkorting 'schalen' uit. Sharding, waarin de gegevens in een verzameling van identieke gestructureerde databases zijn gepartitioneerd is een veelgebruikte manier voor het implementeren van horizontaal schalen.  

Verticale schaling verwijst naar het vergroten of verkleinen van het prestatieniveau van een individuele database — dit staat ook bekend als "omhoog schalen."

De meeste databasetoepassingen voor cloud-scale wordt een combinatie van deze twee strategieën gebruiken. Bijvoorbeeld, kunt een servicetoepassing Software als een horizontaal schalen voor het inrichten van nieuwe end-klanten en verticaal schalen dat elke eindgebruiker database kan vergroten of verkleinen van resources naar behoefte door de werkbelasting.

* Horizontaal schalen wordt beheerd met behulp van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md).
* Azure PowerShell-cmdlets te wijzigen van de servicelaag met verticale schalen moet worden uitgevoerd of door het plaatsen van databases in een elastische pool.

## <a name="sharding"></a>Sharding
*Sharding* is een techniek voor de distributie van grote hoeveelheden identiek gestructureerde gegevens over een aantal onafhankelijke databases. Het is vooral populaire met cloud-ontwikkelaars maken van Software als een Service (SAAS)-aanbiedingen voor end-klanten of bedrijven. Deze end-klanten worden vaak aangeduid als 'tenants'. Sharding mogelijk zijn vereist voor een aantal redenen zijn:  

* De totale hoeveelheid gegevens is te groot om te passen binnen de beperkingen van één database
* De transactiedoorvoer van de algemene werklast overschrijdt de mogelijkheden van een individuele database
* Tenants mogelijk fysieke isolatie van elkaar, zodat de afzonderlijke databases nodig zijn voor elke tenant
* Mogelijk verschillende secties van een database moet zich bevinden in verschillende geografieën voor naleving, prestaties of geopolitieke redenen.

In andere scenario's, zoals de opname van gegevens van gedistribueerde apparaten kan sharding worden gebruikt voor het vervullen van een set databases die tijdelijk zijn geordend. Een aparte database kan bijvoorbeeld worden toegewezen aan elke dag of week. In dat geval kan de sharding-sleutel een geheel getal dat de datum (aanwezig is in alle rijen van de shard-tabel) en query's bij het ophalen van informatie voor een bepaald datumbereik van de toepassing naar de subset van de databases die betrekking hebben op het desbetreffende bereik moeten worden gerouteerd.

Sharding werkt het beste als elke transactie in een toepassing beperkt tot een enkele waarde van een sharding-sleutel worden kan. Dit zorgt ervoor dat alle transacties lokaal op een specifieke database worden.

## <a name="multi-tenant-and-single-tenant"></a>Multitenant- en één tenant
Sommige toepassingen gebruiken de eenvoudigste manier voor het maken van een aparte database voor elke tenant. Dit is de **patroon voor één tenant sharding** die zorgt voor isolatie, de mogelijkheid back-up/herstel en resource schalen aan de granulatie van de tenant. Met één tenant sharding, elke database is gekoppeld aan een specifieke tenant-id-waarde (of waarde van de klant), maar die sleutel moet niet altijd aanwezig zijn in de gegevens zelf. Het is de verantwoordelijkheid van de toepassing voor het routeren van elke aanvraag naar de juiste database - en de clientbibliotheek kunt dit vereenvoudigen.

![Één tenant ten opzichte van meerdere tenants][4]

Andere scenario's voor meerdere tenants samen pack in de databases in plaats van ze te isoleren in afzonderlijke databases. Dit is een typische **multitenant sharding patroon** - en deze mag worden aangedreven door het feit dat een toepassing grote aantallen zeer kleine tenants beheert. In een multitenant-sharding zijn de rijen in de databasetabellen alle ontworpen voor het transport van een sleutel die identificeert de tenant-ID of sharding-sleutel. Opnieuw de toepassingslaag is verantwoordelijk voor het doorsturen van aanvraag voor een tenant met de juiste database en dit kan worden ondersteund door de clientbibliotheek voor elastische database. Bovendien beveiliging kan worden gebruikt voor filteren welke rijen toegang elke tenant - voor meer informatie tot hebben Zie [multitenant-toepassingen met elastische database-hulpprogramma's en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md). Opnieuw distribueren van gegevens tussen databases zijn mogelijk vereist met het patroon multitenant sharding en dit proces wordt vergemakkelijkt door het hulpprogramma voor elastische database gesplitste samenvoegen. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Gegevens vanaf meerdere verplaatsen naar één tenancymodus databases
Wanneer u een SaaS-toepassing maakt, is het typische aanbieden potentiële klanten een evaluatieversie van de software. In dit geval is het voordelig in een multitenant-database gebruiken voor de gegevens. Wanneer een kandidaat een klant wordt, is een één-tenant-database echter beter omdat deze betere prestaties biedt. Als de klant had gemaakt van gegevens tijdens de proefperiode, gebruikt u de [gesplitste merge tool](sql-database-elastic-scale-overview-split-and-merge.md) voor het verplaatsen van de gegevens van meerdere tenants naar de nieuwe database voor één tenant.

## <a name="next-steps"></a>Volgende stappen
Zie voor een voorbeeld-app die u laat zien van de clientbibliotheek [aan de slag met hulpprogramma's van elastische Datababase](sql-database-elastic-scale-get-started.md).

Zie het converteren van bestaande databases voor het gebruik van de hulpprogramma's [migreren van bestaande databases voor scale-out](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie de details van de elastische groep [prijs- en Prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool.md), of maak een nieuwe pool met [elastische pools](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

