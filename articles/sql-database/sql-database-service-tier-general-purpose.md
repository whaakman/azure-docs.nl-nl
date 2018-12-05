---
title: Categorie Algemeen gebruik - Azure SQL Database-service | Microsoft Docs
description: Meer informatie over de Azure SQL Database algemeen gebruik-laag
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 8b708818584be6cdb84530ce008295ccee080d94
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882723"
---
# <a name="general-purpose-tier---azure-sql-database"></a>Categorie Algemeen gebruik - Azure SQL Database

> [!NOTE]
> Algemeen doel laag heet standaard binnen het DTU-model te kopen. Zie voor een vergelijking van de vCore gebaseerde aankoopmodel met het op DTU gebaseerde aankoopmodel [Azure SQL Database-modellen en -bronnen aanschaffen](sql-database-service-tiers.md).

Azure SQL Database is gebaseerd op SQL Server Database Engine-architectuur voor de cloudomgeving is aangepast om ervoor te zorgen, zelfs in het geval van infrastructuuruitval voor 99,99% beschikbaarheid. Er zijn drie architectuur modellen die worden gebruikt in Azure SQL Database:
- Algemeen doel 
- Bedrijfskritiek
- Hyperscale

Algemeen gebruik-model is gebaseerd op een scheiding van berekening en opslag. Deze architectuur, is afhankelijk van hoge beschikbaarheid en betrouwbaarheid van Azure Premium Storage die transparant worden gerepliceerd databasebestanden en zonder verlies van gegevens garandeert als de onderliggende infrastructuur fout gebeurt.

De volgende afbeelding ziet vier knooppunten in de standard-architectuur met de gescheiden reken- en storage-lagen.

![Scheiding van berekening en opslag](media/sql-database-managed-instance/general-purpose-service-tier.png)

Er zijn twee lagen in het algemeen gebruik-model:

- Een stateless compute-laag met de `sqlserver.exe` verwerken en bevat alleen tijdelijke en in de cache opgeslagen gegevens (bijvoorbeeld: plancache, buffergroep, kolom-archief van toepassingen). Deze stateless SQL Server-knooppunt wordt beheerd door Azure Service Fabric dat proces initialiseert, bepaalt de status van het knooppunt en failover naar een andere locatie uitvoert, indien nodig.
- Een stateful gegevenslaag met databasebestanden (.mdf/.ldf) die zijn opgeslagen in Azure Premium Storage. Azure Storage zorgt ervoor dat er worden geen gegevens verloren gaan van een record die in een databasebestand wordt geplaatst. Azure Storage heeft ingebouwde beschikbaarheid/redundantie die ervoor zorgt dat elke record in een logboekbestand of de pagina in bestand behouden blijft, zelfs als SQL Server-proces vastloopt.

Wanneer de upgrade van database-engine of -besturingssysteem wordt uitgevoerd, een deel van de onderliggende infrastructuur is mislukt of als een belangrijk probleem wordt gedetecteerd in SQL Server-proces, Azure Service Fabric stateless SQL Server-proces wordt verplaatst naar een andere staatloze compute-knooppunt. Er is een set van ongebruikte knooppunten die wacht op nieuwe compute-service wordt uitgevoerd als een failover van het primaire knooppunt gebeurt om het aantal failover-tijd. Gegevens in Azure Storage-laag wordt niet beïnvloed en gegevens/logboekbestanden zijn gekoppeld aan de nieuwe geïnitialiseerde SQL Server-proces. Dit proces garandeert 99,99% beschikbaarheid, maar dat er bepaalde prestatie-invloeden van zware werkbelasting die wordt uitgevoerd vanwege enige tijd en de nieuwe SQL Server-knooppunt van het feit begint met cold cache.

## <a name="when-to-choose-this-service-tier"></a>Wanneer deze servicelaag kiezen?

Categorie voor algemeen gebruik-service is een standaard service-laag in Azure SQL Database die is ontworpen voor het merendeel van de algemene werkbelastingen. Als u een volledig beheerde database-engine met 99,99% SLA met een opslaglatentie tussen 5 en 10 ms die overeenkomt met de Azure SQL IaaS in de meeste gevallen moet, is de categorie Algemeen gebruik de optie voor u.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [bedrijfskritiek](sql-database-service-tier-business-critical.md) en [grootschalige](sql-database-service-tier-hyperscale.md) lagen.
- Meer informatie over [Service Fabric](../service-fabric/service-fabric-overview.md).
- Zie voor meer opties voor hoge beschikbaarheid en herstel na noodgevallen [bedrijfscontinuïteit](sql-database-business-continuity.md).
