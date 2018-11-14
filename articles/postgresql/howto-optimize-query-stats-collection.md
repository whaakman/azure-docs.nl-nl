---
title: Query statistieken verzameling in Azure Database voor PostgreSQL-server query statistieken verzameling optimaliseren
description: Dit artikel wordt beschreven hoe u query statistieken verzameling op een Azure Database for PostgreSQL-server kunt optimaliseren.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 8b8d1a5a16a948953838f51edca091121dc14df1
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629020"
---
# <a name="optimize-query-statistics-collection-in-azure-database-for-postgresql-server"></a>Query verzamelen van queryuitvoeringsstatistieken in Azure Database for PostgreSQL-server te optimaliseren 
Dit artikel wordt beschreven voor het verzamelen van queryuitvoeringsstatistieken query in een Azure Database for PostgreSQL-server te optimaliseren.

## <a name="using-pgstatsstatements"></a>Met behulp van pg_stats_statements
**Pg_stat_statements** is een PostgreSQL-uitbreiding die is standaard ingeschakeld in Azure Database voor PostgreSQL. De extensie biedt een manier voor het bijhouden van Uitvoeringsstatistieken van alle SQL-instructies uitvoeren op een server. Deze module wordt echter hooks in elke uitvoering van de query en wordt geleverd met een essentieel prestaties. Inschakelen van **pg_stat_statements** krachten query tekst schrijfbewerkingen naar de bestanden op schijf.

Voor klanten die hebben unieke query's met de tekst van lange query of zijn niet actief te controleren **pg_stat_statements**, wordt aangeraden uitschakelen **pg_stat_statements** voor de beste prestaties door in te stellen `pg_stat_statements.track = NONE`.

Voor sommige klantwerkbelastingen hebben we gezien tot een prestatieverbetering van 50 procent door uit te schakelen **pg_stat_statements**. De verhouding wordt door het uitschakelen van pg_stat_statements is echter de modeldatabase groter voor het oplossen van prestatieproblemen.

Om in te stellen `pg_stat_statements.track = NONE`:

- In de Azure-portal, gaat u naar de [PostgreSQL resourcebeheer pagina en selecteert u de blade van de server-parameters](howto-configure-server-parameters-using-portal.md).

![PostgreSQL server parameter-blade](.\media\howto-optimize-query-stats-collection\pg_stats_statements_portal.png)

- Met behulp van [Azure CLI](howto-configure-server-parameters-using-cli.md), az postgres server configuratieset `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="using-query-store"></a>Met behulp van de Query Store 
De [Query Store](concepts-query-store.md) functie in Azure Database voor PostgreSQL een meer goed presterende-methode biedt voor het bijhouden van querystatistieken en wordt aanbevolen als alternatief voor het gebruik *pg_stats_statements*. 

## <a name="next-steps"></a>Volgende stappen
U kunt `pg_stat_statements.track = NONE` in de [Azure Portal](howto-configure-server-parameters-using-portal.md) of met behulp van [Azure CLI](howto-configure-server-parameters-using-cli.md).

Zie de [Query Store-gebruiksscenario's](concepts-query-store-scenarios.md) en [best practices voor Query Store](concepts-query-store-best-practices.md) voor meer informatie. 
