---
title: Query statistieken verzameling op een Azure Database voor PostgreSQL - Server één optimaliseren
description: Dit artikel wordt beschreven hoe u query statistieken verzameling op een Azure Database voor PostgreSQL - Server één kunt optimaliseren
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 7425ee7916fd71625f336a7af35f6481d1ed2474
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068964"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql---single-server"></a>Query verzamelen van queryuitvoeringsstatistieken op een Azure Database voor PostgreSQL - Server één optimaliseren
Dit artikel wordt beschreven hoe u kunt verzamelen van queryuitvoeringsstatistieken query op een Azure Database for PostgreSQL-server te optimaliseren.

## <a name="use-pgstatsstatements"></a>Gebruik pg_stats_statements
**Pg_stat_statements** is een PostgreSQL-uitbreiding die standaard ingeschakeld in Azure Database voor PostgreSQL. De extensie biedt een manier voor het bijhouden van statistieken voor alle SQL-instructies uitvoeren op een server worden uitgevoerd. Deze module hooks in elke uitvoering van de query en wordt geleverd met een essentieel prestaties. Inschakelen van **pg_stat_statements** krachten query tekst schrijfbewerkingen naar de bestanden op schijf.

Als u de unieke query's met de tekst van lange query hebt of u niet controleren **pg_stat_statements**, uitschakelen **pg_stat_statements** voor de beste prestaties. Wijzig de instelling in om dit te doen, `pg_stat_statements.track = NONE`.

Sommige klantwerkbelastingen hebben gezien tot een prestatieverbetering van 50 procent wanneer **pg_stat_statements** is uitgeschakeld. U neemt bij het uitschakelen van pg_stat_statements een afweging is de modeldatabase groter voor het oplossen van prestatieproblemen.

Om in te stellen `pg_stat_statements.track = NONE`:

- In de Azure-portal, gaat u naar de [PostgreSQL resourcebeheer pagina en selecteert u de blade van de server-parameters](howto-configure-server-parameters-using-portal.md).

  ![PostgreSQL server parameter-blade](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Gebruik de [Azure CLI](howto-configure-server-parameters-using-cli.md) az postgres server configuration is ingesteld op `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="use-the-query-store"></a>De Query Store gebruiken 
De [Query Store](concepts-query-store.md) functie in Azure Database voor PostgreSQL een doeltreffende methode biedt voor het volgen van querystatistieken. Het is raadzaam deze functie als alternatief voor het gebruik van *pg_stats_statements*. 

## <a name="next-steps"></a>Volgende stappen
U kunt `pg_stat_statements.track = NONE` in de [Azure-portal](howto-configure-server-parameters-using-portal.md) of met behulp van de [Azure CLI](howto-configure-server-parameters-using-cli.md).

Zie voor meer informatie: 
- [Query Store-gebruiksscenario's](concepts-query-store-scenarios.md) 
- [Aanbevolen procedures voor query Store](concepts-query-store-best-practices.md) 
