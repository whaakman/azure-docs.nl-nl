---
title: Resources op Azure SQL Database | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u uw database schalen door het toevoegen of verwijderen van toegewezen resources.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 06/25/2019
ms.openlocfilehash: d8949f63dfa9b409cc14fe9c3bbed70f23a73c86
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357137"
---
# <a name="dynamically-scale-database-resources-with-minimal-downtime"></a>Dynamisch schalen database-resources met minimale downtime

Azure SQL Database kunt u meer resources dynamisch toevoegen aan uw database met minimale [downtime](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/), maar er is een switch gedurende waar-verbinding is verbroken met de database gedurende een korte tijd, die kan worden met behulp van de logica voor nieuwe pogingen worden verminderd.

## <a name="overview"></a>Overzicht

Wanneer de vraag naar uw Apps toeneemt van een handjevol apparaten en klanten naar enkele miljoenen, Azure SQL Database kan worden geschaald op elk gewenst moment met minimale downtime. Schaalbaarheid is een van de belangrijkste kenmerken van PaaS waarmee u meer resources dynamisch toevoegen aan uw service wanneer dat nodig is. Azure SQL Database kunt u eenvoudig resources (CPU-kracht, geheugen, i/o-doorvoer en opslag) toegewezen aan uw databases te wijzigen.

U kunt problemen met prestaties vanwege toegenomen gebruik van uw toepassing kan niet worden hersteld met behulp van indexeren of herschrijven querymethode beperken. Meer resources toe te voegen, kunt u snel reageren wanneer de database komt binnen via de huidige resourcelimieten en meer vermogen moet om de binnenkomende werkbelasting te verwerken. Azure SQL Database kunt u omlaag schalen de resources wanneer ze niet nodig zijn de kosten verlagen.

U hoeft niet te hoeven maken over het aanschaffen van hardware en het wijzigen van de onderliggende infrastructuur. Vergroten/verkleinen database kan eenvoudig worden uitgevoerd via Azure portal met behulp van een schuifregelaar.

![Databaseprestaties schalen](media/sql-database-scalability/scale-performance.svg)

Azure SQL Database biedt de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

- De [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) biedt een combinatie van rekenkracht, geheugen en i/o-resources in drie Servicelagen voor lichte tot zware workloads van databases: Basic, Standard en Premium. Prestatieniveaus binnen elke laag bieden een andere combinatie van deze resources, waaraan u extra opslagbronnen kunt toevoegen.
- De [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) kunt u het aantal vCores, het bedrag of geheugen, en de hoeveelheid en de snelheid van de opslag kiezen. Deze aankoopmodel biedt drie Servicelagen: Algemeen gebruik, bedrijfskritiek en grootschalige.

U kunt uw eerste app ontwikkelen op een kleine, één database tegen lage kosten per maand in de servicelaag Basic, Standard- of algemeen gebruik en wijzig vervolgens de servicelaag handmatig of via een programma op elk gewenst moment in de servicelaag Premium en bedrijfskritiek om te voldoen aan de ne EDS van uw oplossing. U kunt het prestatieniveau aanpassen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.

> [!NOTE]
> Dynamische schaalbaarheid is iets anders dan automatisch schalen. Voor automatisch schalen is als een service automatisch op basis van criteria schaalt, terwijl dynamische schaalbaarheid kunt u handmatig schalen met een minimale downtime.

Eén Azure SQL Database ondersteunt handmatige dynamische schaalbaarheid, maar niet automatisch schalen. Voor een meer *automatische* ervaring zou u elastische pools kunnen gebruiken. Hiermee kunnen databases resources in een pool delen op basis van afzonderlijke databasebehoeften.
Er zijn echter scripts die kunnen worden geautomatiseerd schaalbaarheid voor één Azure SQL Database. Zie [PowerShell gebruiken voor het controleren en schalen van één Azure SQL-database](scripts/sql-database-monitor-and-scale-database-powershell.md) voor een voorbeeld.

U kunt wijzigen [DTU-Servicelagen](sql-database-service-tiers-dtu.md) of [vCore kenmerken](sql-database-vcore-resource-limits-single-databases.md) op elk gewenst moment met minimale downtime voor uw toepassing (doorgaans minder dan vier seconden). Voor veel bedrijven en apps is het kunnen maken van databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. Voor dit scenario gebruikt u een elastische pool met een bepaald aantal edtu's die worden gedeeld tussen meerdere databases in de groep.

![Inleiding tot SQL Database: Individuele database dtu's per laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Alle drie versies van Azure SQL Database bieden een mogelijkheid voor het dynamisch schalen van uw databases:

- Met een [individuele database](sql-database-single-database-scale.md), gebruikt u een [DTU](sql-database-dtu-resource-limits-single-databases.md) of [vCore](sql-database-vcore-resource-limits-single-databases.md) modellen voor het definiëren van de maximale hoeveelheid resources die worden toegewezen aan elke database.
- Een [Managed Instance](sql-database-managed-instance.md) maakt gebruik van [vCores](sql-database-managed-instance.md#vcore-based-purchasing-model) modus en kunt u maximaal CPU-kernen en het maximumaantal opslag toegewezen aan uw exemplaar definiëren. Alle databases in de instantie delen de resources die zijn toegewezen aan het exemplaar.
- [Elastische pools](sql-database-elastic-pool-scale.md) kunt u voor het definiëren van de maximale limiet is per groep databases in de groep.

> [!NOTE]
> U kunt verwachten dat het einde van een korte verbinding wanneer de schaal omhoog/omlaag schalen proces is voltooid. Als u hebt geïmplementeerd [Pogingslogica voor tijdelijke fouten standaard](sql-database-connectivity-issues.md#retry-logic-for-transient-errors), dan zult u niet de failover.

## <a name="alternative-scale-methods"></a>Schaal van alternatieve methoden

Schalen van resources is de eenvoudigste en de meest efficiënte manier om prestaties te verbeteren van uw database zonder code van de database of de toepassing te wijzigen. In sommige gevallen, zelfs de hoogste Servicelagen, compute-grootten en optimalisaties mogelijk niet verwerken uw werkbelasting op geslaagd en goedkope manier. In dat geval hebt u deze extra opties voor het schalen van uw database:

- [Read scale-out](sql-database-read-scale-out.md) is een functie die beschikbaar zijn in waar u ontvangt een alleen-lezen replica van uw gegevens waar u de veeleisende alleen-lezen query's, zoals rapporten kunt uitvoeren. Alleen-lezen replica wordt uw workload alleen-lezen worden verwerkt zonder gebruik van bronnen op uw primaire database.
- [Database sharding](sql-database-elastic-scale-introduction.md) is een set technieken waarmee u uw gegevens splitsen in meerdere databases en ze onafhankelijk worden geschaald.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het verbeteren van de prestaties van de database door het veranderen van databasecode [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over het laten ingebouwde database intelligence optimaliseren van uw database [automatisch afstemmen](sql-database-automatic-tuning.md).
- Zie voor informatie over Read Scale-out in de Azure SQL Database-service, hoe u [alleen-lezen replica's gebruiken om te laden van de alleen-lezen querywerkbelastingen saldo](sql-database-read-scale-out.md).
- Zie voor meer informatie over een Database sharding [uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md).
