---
title: Resources op Azure SQL Database | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u uw database schalen door het toevoegen of verwijderen van toegewezen resources.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/07/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: f55ce511f6ba90c27e149ac90bbd2c8aa0b3c742
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921512"
---
# <a name="scale-database-resources"></a>Resources op database

Azure SQL Database kunt u meer resources dynamisch toevoegen aan uw database met minimale downtime.

## <a name="overview"></a>Overzicht

Wanneer de vraag naar uw Apps toeneemt van een handjevol apparaten en klanten naar enkele miljoenen, Azure SQL Database kan worden geschaald op elk gewenst moment met minimale downtime. Schaalbaarheid is een van de belangrijkste kenmerken van PaaS waarmee u meer resources dynamisch toevoegen aan uw service wanneer dat nodig is. Azure SQL Database kunt u eenvoudig resources (CPU-kracht, geheugen, i/o-doorvoer en opslag) toegewezen aan uw databases te wijzigen.  
U kunt problemen met prestaties vanwege toegenomen gebruik van uw toepassing kan niet worden hersteld met behulp van indexeren of herschrijven querymethode beperken. Meer resources toe te voegen, kunt u snel reageren wanneer de database komt binnen via de huidige resourcelimieten en meer vermogen moet om de binnenkomende werkbelasting te verwerken. Azure SQL Database kunt u omlaag schalen de resources wanneer ze niet nodig zijn de kosten verlagen.
U hoeft niet te hoeven maken over het aanschaffen van hardware en het wijzigen van de onderliggende infrastructuur. Vergroten/verkleinen database kan eenvoudig worden uitgevoerd via Azure portal met behulp van een schuifregelaar.

![Databaseprestaties schalen](media/sql-database-scalability/scale-performance.svg)

Azure SQL Database biedt een [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) of de [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md). 
-   De [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) biedt een combinatie van rekenkracht, geheugen en i/o-resources in drie Servicelagen voor lichte tot zware workloads van databases: Basic, Standard en Premium. Prestatieniveaus binnen elke laag bieden een andere combinatie van deze resources, waaraan u extra opslagbronnen kunt toevoegen.
-   De [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) (preview) kunt u het aantal vCores, het bedrag of geheugen, en de hoeveelheid en de snelheid van de opslag kiezen.
U kunt uw eerste app ontwikkelen op één enkele kleine database voor slechts enkele euro's per maand. Vervolgens kunt u op elk gewenst moment handmatig of programmatisch de servicelaag wijzigen, zodat deze beter past bij de behoeften van uw oplossing. U kunt het prestatieniveau aanpassen zonder uitvaltijd voor uw app of voor uw klanten. Dankzij dynamische schaalbaarheid kan uw database op een transparante manier snel reageren op veranderende resourcevereisten en betaalt u alleen voor de resources die u nodig hebt wanneer u ze nodig.


> [!NOTE]
> Dynamische schaalbaarheid is iets anders dan automatisch schalen. Automatisch schalen vindt plaats wanneer een service automatisch wordt geschaald op basis van criteria, terwijl u met dynamische schaalbaarheid handmatig kunt schalen zonder uitvaltijd.
>


Eén Azure SQL Database ondersteunt handmatige dynamische schaalbaarheid, maar niet automatisch schalen. Voor een meer *automatische* ervaring zou u elastische pools kunnen gebruiken. Hiermee kunnen databases resources in een pool delen op basis van afzonderlijke databasebehoeften.
Er zijn echter scripts die kunnen worden geautomatiseerd schaalbaarheid voor één Azure SQL Database. Zie [PowerShell gebruiken voor het controleren en schalen van één Azure SQL-database](scripts/sql-database-monitor-and-scale-database-powershell.md) voor een voorbeeld.


Alle drie versies van Azure SQL Database bieden een mogelijkheid voor het dynamisch schalen van uw databases:
-   In [één Azure SQL-Database](sql-database-single-database-scale.md), gebruikt u een [DTU](sql-database-dtu-resource-limits-single-databases.md) of [vCore](sql-database-vcore-resource-limits-single-databases.md) modellen voor het definiëren van de maximale hoeveelheid resources die worden toegewezen aan elke database.
-   [Azure SQL Managed Instance](sql-database-managed-instance.md) maakt gebruik van [vCores](/azure/sql-database/sql-database-managed-instance#vcore-based-purchasing-model-preview) modus en kunt u maximaal CPU-kernen en het maximumaantal opslag toegewezen aan uw exemplaar definiëren. Alle databases in de instantie delen de resources die zijn toegewezen aan het exemplaar.
-   [Elastische Pools van Azure SQL](sql-database-elastic-pool-scale.md) kunt u voor het definiëren van de maximale limiet is per groep databases in de groep.

## <a name="alternative-scale-methods"></a>Schaal van alternatieve methoden
Schalen van resources is de eenvoudigste en de meest efficiënte manier om prestaties te verbeteren van uw database zonder code van de database of de toepassing te wijzigen.
In sommige gevallen, zelfs de hoogste prestatielagen en prestatieoptimalisatie van de mogelijk niet verwerken uw werkbelasting op geslaagd en goedkope manier. In dat geval hebt u andere opties voor het schalen van uw database:
-   [Read scale-out](sql-database-read-scale-out.md) is een functie die beschikbaar zijn in waar u ontvangt een alleen-lezen replica van uw gegevens waar u de veeleisende alleen-lezen query's, zoals rapporten kunt uitvoeren. Rood alleen-lezen replica wordt uw workload alleen-lezen worden verwerkt zonder gebruik van bronnen op uw primaire database.
-   [Database sharding](sql-database-elastic-scale-introduction.md) is een set technieken waarmee u uw gegevens splitsen in meerdere databases en ze onafhankelijk worden geschaald.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over het verbeteren van de prestaties van de database door het veranderen van databasecode [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md).
- Zie voor meer informatie over het laten ingebouwde database intelligence optimaliseren van uw database [automatisch afstemmen](sql-database-automatic-tuning.md).
- Zie voor informatie over Read Scale-out in de Azure SQL Database-service, hoe u [alleen-lezen replica's gebruiken om te laden van de alleen-lezen querywerkbelastingen saldo](sql-database-read-scale-out.md).
- Zie voor meer informatie over een Database sharding [uitbreiden met Azure SQL Database](sql-database-elastic-scale-introduction.md).

