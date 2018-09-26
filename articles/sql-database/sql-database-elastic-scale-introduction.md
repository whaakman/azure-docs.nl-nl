---
title: Uitschalen met Azure SQL Database | Microsoft Docs
description: Software als een Service (SaaS)-ontwikkelaars eenvoudig elastische schaalbare databases kunt maken in de cloud met behulp van deze hulpprogramma 's
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: ab787e8f659d2f57f23bb87397608736e7f6848a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47164910"
---
# <a name="scaling-out-with-azure-sql-database"></a>Uitbreiden met Azure SQL Database
U kunt gemakkelijk schalen van Azure SQL-databases met behulp van de **Elastic Database** hulpprogramma's. Deze hulpprogramma's en functies, kunt u de bronnen van **Azure SQL Database** om oplossingen voor transactionele werkbelastingen en met name Software als een Service (SaaS)-toepassingen te maken. Functies voor elastische Database worden samengesteld uit de:

* [Clientbibliotheek voor elastic Database](sql-database-elastic-database-client-library.md): de clientbibliotheek is een functie waarmee u maken en onderhouden van shard-databases.  Zie [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md).
* [Elastische Database-hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md): verplaatst gegevens tussen de shard-databases. Dit hulpprogramma is nuttig voor het verplaatsen van gegevens uit een multitenant-database naar een één tenant-database (of omgekeerd). Zie [elastisch splitsen en samenvoegen hulpprogramma databasezelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Taken voor elastic Database](sql-database-elastic-jobs-overview.md) (preview):-taken gebruiken voor het beheren van grote aantallen Azure SQL-databases. Gemakkelijk beheerwerkzaamheden uitvoeren zoals wijzigingen in het schema, beheren van referenties, bijwerken van verwijzingsgegevens, verzamelen van prestatiegegevens of verzamelen van telemetriegegevens tenants (klanten) met behulp van taken.
* [Query voor elastic Database](sql-database-elastic-query-overview.md) (preview): Hiermee kunt u een Transact-SQL-query die meerdere databases omvat uit te voeren. Hiermee kunt verbinding met de hulpprogramma's voor rapportage, zoals Excel, Power BI, Tableau, enzovoort.
* [Elastische transacties](sql-database-elastic-transactions-overview.md): deze functie kunt u uitvoeren van transacties met betrekking meerdere databases in Azure SQL Database tot. Elastische transacties zijn beschikbaar voor .NET-toepassingen met behulp van ADO .NET en integreren met de vertrouwde programmering ervaring met de [System.Transaction klassen](https://msdn.microsoft.com/library/system.transactions.aspx).

De volgende afbeelding toont een architectuur met de **functies voor elastische Database** ten opzichte van een verzameling van databases.

In deze afbeelding vertegenwoordigen de kleuren van de database schema's. Databases met dezelfde kleur delen hetzelfde schema.

1. Een set **Azure SQL-databases** wordt gehost op Azure met behulp van sharding-architectuur.
2. De **Elastic Database-clientbibliotheek** wordt gebruikt voor het beheren van een shard-verzameling.
3. Een subset van de databases wordt geplaatst in een **elastische pool**. (Zie [wat is een pool?](sql-database-elastic-pool.md)).
4. Een **elastische taak** geplande of ad hoc T-SQL-scripts op alle databases wordt uitgevoerd.
5. De **hulpprogramma voor splitsen en samenvoegen** wordt gebruikt voor het verplaatsen van gegevens uit één shard naar een andere.
6. De **elastische databasequery** kunt u een query schrijven waarmee alle databases in de shard-set omvat.
7. **Elastische transacties** kunt u uitvoeren van transacties met betrekking verschillende databases tot. 

![Hulpprogramma's voor elastische databases][1]

## <a name="why-use-the-tools"></a>Waarom de hulpprogramma's gebruiken?
Bereiken van flexibiliteit en schaalbaarheid voor cloud-Apps is eenvoudig voor VM's en blob-opslag - gewoon toevoegen of eenheden aftrekken of power verhogen. Maar er is een uitdaging voor stateful gegevensverwerking in relationele databases gebleven. Uitdagingen naar voren gekomen in deze scenario's:

* Vergroten en verkleinen capaciteit voor de relationele database die deel uitmaken van uw workload.
* Het beheren van hotspots die kunnen ontstaan die betrekking hebben op een specifieke subset van gegevens -, zoals een bezet eindklant (tenant).

Traditioneel zijn scenario's zoals deze opgelost door te investeren in grotere database-servers voor de ondersteuning van de toepassing. Deze optie wordt echter beperkt in de cloud waar alle verwerking op vooraf gedefinieerde basishardware plaatsvindt. Distribueren van gegevens en -verwerking voor verschillende databases van dezelfde gestructureerde biedt (een scale-out-patroon 'sharding' genoemd) in plaats daarvan een alternatief voor traditionele omhoog benaderingen, zowel wat betreft de kosten en flexibiliteit.

## <a name="horizontal-and-vertical-scaling"></a>Horizontaal en verticaal schalen
De volgende afbeelding toont de horizontale en verticale afmetingen van de schaal, dit zijn de manieren waarop die de elastische databases kunnen worden geschaald.

![Horizontale en verticale scale-out][2]

Horizontaal schalen verwijst naar het toevoegen of verwijderen van databases om te kunnen passen capaciteit of algehele prestaties, ook wel genoemd 'schaal' uit. Sharding, waarin de gegevens zijn gepartitioneerd over een verzameling van identieke gestructureerde databases, is een veelgebruikte manier voor het implementeren van horizontaal schalen.  

Verticaal schalen verwijst naar vergroten of verkleinen het berekenen van een individuele database, ook wel bekend als 'omhoog."

De meeste toepassingen op cloudschaal database een combinatie van deze twee strategieën gebruiken. Bijvoorbeeld, kunt een Software als een servicetoepassing horizontaal schalen voor het inrichten van nieuwe end-klanten en verticaal schalen waarmee elke eindgebruiker database vergroten of verkleinen van resources, indien nodig door de werkbelasting.

* Horizontaal schalen wordt beheerd met behulp van de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md).
* Verticaal schalen is uitgevoerd met behulp van Azure PowerShell-cmdlets te wijzigen van de servicelaag of door databases in een elastische pool.

## <a name="sharding"></a>Sharding
*Sharding* is een techniek voor de distributie van grote hoeveelheden identiek gestructureerde gegevens over een aantal onafhankelijke databases. Het is met name populaire met cloudontwikkelaars die het maken van Software als een Service (SAAS)-aanbiedingen voor eindgebruikers of bedrijven. Deze end-klanten worden vaak aangeduid als 'huurder'. Sharding is mogelijk vereist voor een aantal oorzaken hebben:  

* De totale hoeveelheid gegevens is te groot om te passen binnen de beperkingen van een individuele database
* De transactiedoorvoer van de totale werklast groter is dan de mogelijkheden van een individuele database
* Tenants mogelijk fysiek geïsoleerd van elkaar, zodat u afzonderlijke databases nodig zijn voor elke tenant
* Verschillende secties van een database moet mogelijk bevinden zich in verschillende fysieke locaties voor naleving, prestaties of geopolitieke redenen.

In andere scenario's, zoals de opname van gegevens van gedistribueerde apparaten kan sharding worden gebruikt om in te vullen van een set databases die tijdelijk zijn ingedeeld. Een aparte database kan bijvoorbeeld worden toegewezen aan elke dag of week. In dat geval kan de sharding-sleutel een geheel getal dat aangeeft van de datum (aanwezig zijn in alle rijen van de shard tabellen) en query's bij het ophalen van gegevens voor een bepaalde periode moeten worden gerouteerd van de toepassing naar de subset van databases die betrekking hebben op het desbetreffende bereik.

Sharding werkt het beste als elke transactie in een toepassing beperkt tot één waarde van een sharding-sleutel worden kan. Dit zorgt ervoor dat alle transacties lokaal op een specifieke database worden.

## <a name="multi-tenant-and-single-tenant"></a>Meerdere tenants en één tenant
Sommige toepassingen gebruiken de eenvoudigste manier voor het maken van een aparte database voor elke tenant. Deze aanpak is het **één tenant sharding-patroon** die zorgt voor isolatie, back-up/herstel mogelijkheid en resource schalen zonder dat u de granulatie van de tenant. Met sharding van één tenant, elke database is gekoppeld aan een specifieke tenant-id-waarde (of waarde van de klant), maar deze sleutel moet niet altijd aanwezig zijn in de gegevens zelf. Het is de verantwoordelijkheid van de toepassing voor het routeren van elke aanvraag naar de juiste database - en deze kan worden vereenvoudigd door de clientbibliotheek.

![Één tenant ten opzichte van meerdere tenants][4]

Andere scenario's voor meerdere tenants samen pack in de databases in plaats van ze te isoleren in afzonderlijke databases. Dit patroon is een typische **multitenant sharding-patroon** - en deze kan worden aangestuurd door het feit dat een toepassing een groot aantal kleine tenants beheert. In de sharding van meerdere tenants zijn de rijen in de databasetabellen ontworpen voor het uitvoeren van een sleutel voor het identificeren van de tenant-ID of de sharding-sleutel. Nogmaals, de toepassingslaag is verantwoordelijk voor de routering van een tenant-aanvraag naar de juiste database en dit kan worden ondersteund door de clientbibliotheek van elastische database. Bovendien beveiliging op rijniveau kan worden gebruikt om te filteren welke rijen toegang elke tenant - voor meer informatie tot hebben, Zie [multitenant-toepassingen met elastische Databasehulpprogramma's en beveiliging op rijniveau](sql-database-elastic-tools-multi-tenant-row-level-security.md). Opnieuw distribueren van gegevens tussen databases kan nodig zijn met het patroon sharding van meerdere tenants, en wordt mogelijk gemaakt door de elastic database-hulpprogramma voor splitsen en samenvoegen. Zie [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) voor meer informatie over ontwerppatronen voor SaaS-toepassingen met elastische groepen.

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verplaatsen van gegevens uit meerdere databases voor één-tenants
Bij het maken van een SaaS-toepassing, is het typische potentiële klanten bieden een proefversie van de software. In dit geval is het rendabele gebruik van een multitenant-database voor de gegevens. Wanneer een kandidaat een klant wordt, is een database met één tenant echter beter omdat het levert betere prestaties. Als de klant gegevens tijdens de proefperiode is afgelopen gemaakt heeft, gebruikt u de [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) de gegevens van meerdere tenants verplaatsen naar de nieuwe database met één tenant.

## <a name="next-steps"></a>Volgende stappen
Zie voor een voorbeeld-app die laat zien van de clientbibliotheek [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md).

Als u wilt converteren van bestaande databases voor het gebruik van de hulpprogramma's, Zie [bestaande databases migreren voor uitschalen](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie de details van de elastische pool [prijs- en Prestatieoverwegingen voor een elastische pool](sql-database-elastic-pool.md), of maak een nieuwe groep met [elastische pools](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

