---
title: Kritieke-bedrijfslaag - Azure SQL Database-service | Microsoft Docs
description: Meer informatie over de Azure SQL Database laag bedrijfskritiek
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: aecbc9415fc7fe93b5df355c97a1a51a74f01e97
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882736"
---
# <a name="business-critical-tier---azure-sql-database"></a>Kritieke-bedrijfslaag - Azure SQL Database

> [!NOTE]
> Kritieke bedrijfslaag wordt Premium in DTU-aankoopmodel genoemd. Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-service-tiers.md).

Azure SQL Database is gebaseerd op SQL Server Database Engine-architectuur die wordt aangepast aan de cloudomgeving om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid. Er zijn drie architectuur modellen die worden gebruikt in Azure SQL Database:
- Algemeen doel/Standard 
- Kritieke zakelijke/Premium
- Hyperscale

Model van de laag Premium/bedrijfskritiek service is gebaseerd op een cluster van database-engine verwerkt. Deze architectuur is gebaseerd op een feit dat er altijd een quorum met knooppunten voor beschikbare database-engine en heeft minimale prestatie-impact op de werkbelasting, zelfs tijdens onderhoudswerkzaamheden.

Azure upgrades en patches onderliggende besturingssysteem, stuurprogramma's en SQL Server Database Engine transparant met de minimale uitvaltijd voor eindgebruikers. 

Premium-beschikbaarheid is ingeschakeld in de Servicelagen Premium en bedrijfskritiek van Azure SQL Database en het is bedoeld voor intensieve werkbelastingen die invloed op de prestaties vanwege de lopende onderhoudsbewerkingen kunnen tolereren.

Azure SQL-database in de premium-model, kan worden geïntegreerd reken- en op één knooppunt. Hoge beschikbaarheid in deze architectuur wordt bereikt door middel van replicatie van berekening (SQL Server Database Engine-proces) en opslag (lokaal gekoppelde SSD) geïmplementeerd in een cluster van vier knooppunten met behulp van technologie die vergelijkbaar is met SQL Server [altijd op Beschikbaarheidsgroepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Cluster met knooppunten voor database-engine](media/sql-database-managed-instance/business-critical-service-tier.png)

Zowel de SQL-database engine-proces en de onderliggende mdf/ldf-bestanden worden geplaatst op hetzelfde knooppunt met lage latentie aan uw workload biedt lokaal gekoppelde SSD-opslag. Hoge beschikbaarheid is geïmplementeerd met behulp van technologie die vergelijkbaar is met SQL Server [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Elke database is een cluster van de databaseknooppunten met één primaire database, die toegankelijk is voor de workload van een klant en een drie secundaire processen met kopieën van gegevens. Het primaire knooppunt pushes voortdurend de wijzigingen op secundaire knooppunten om ervoor te zorgen dat de gegevens beschikbaar op secundaire replica's, zijn als het primaire knooppunt om een bepaalde reden vastloopt. Failover wordt verwerkt door de SQL Server Database Engine – één secundaire replica wordt het primaire knooppunt en een nieuwe secundaire replica is gemaakt om ervoor te zorgen voldoende knooppunten in het cluster. De werkbelasting wordt automatisch omgeleid naar de nieuwe primaire knooppunt.

Bovendien bedrijfskritiek cluster beschikt over een ingebouwde [Read Scale-Out](sql-database-read-scale-out.md) mogelijkheid waarmee u kunt gratis van kosten in rekening gebracht ingebouwde alleen-lezen-knooppunt kan worden gebruikt voor het kenmerk alleen-lezen query's uitvoeren (bijvoorbeeld rapporten) die niet van invloed op prestaties van uw primaire workload.

## <a name="when-to-choose-this-service-tier"></a>Wanneer deze servicelaag kiezen?

Zakelijke kritieke-laag is ontworpen voor de toepassingen waarvoor lage latentie antwoorden van de onderliggende SSD-opslag (1-2 ms in gemiddelde), snel herstel als de onderliggende infrastructuur is mislukt of hoeft te ontlasten van rapporten, analyses, en alleen-lezen Als u query's naar de gratis kosten in rekening gebracht leesbare secundaire replica van de primaire database.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [algemeen](sql-database-service-tier-general-purpose.md) en [grootschalige](sql-database-service-tier-hyperscale.md) lagen.
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md).
- Zie voor meer opties voor hoge beschikbaarheid en herstel na noodgevallen [bedrijfscontinuïteit](sql-database-business-continuity.md).
