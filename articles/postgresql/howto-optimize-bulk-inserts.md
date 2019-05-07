---
title: Bulksgewijs invoegen in een Azure Database voor PostgreSQL - Server één optimaliseren
description: Dit artikel wordt beschreven hoe u bulk insert-bewerkingen op een Azure Database voor PostgreSQL - Server één kunt optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: c1ae29f7c498a79af09aaaf6d7aeae29561aa500
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067032"
---
# <a name="optimize-bulk-inserts-and-use-transient-data-on-an-azure-database-for-postgresql---single-server"></a>Bulksgewijs invoegen te optimaliseren en tijdelijke gegevens op een Azure Database voor PostgreSQL - Server één gebruiken 
Dit artikel wordt beschreven hoe u bulk insert-bewerkingen optimaliseren en gebruik van tijdelijke gegevens op een Azure Database voor PostgreSQL-server.

## <a name="use-unlogged-tables"></a>Niet-geregistreerde tabellen gebruiken
Als u de werkbelasting van bewerkingen die betrekking hebben op tijdelijke gegevens of die grote gegevenssets bulksgewijs invoegen hebt, overweeg het gebruik van niet-geregistreerde tabellen.

Niet-geregistreerde tabellen is een PostgreSQL-functie die effectief kan worden gebruikt om te optimaliseren bulksgewijs invoegen. Write-Ahead logboekregistratie (WAL) maakt gebruik van PostgreSQL. Het biedt atomisch en duurzaamheid, standaard. Atomisch, consistent, isolatie en duurzaamheid vormen de ACID-eigenschappen. 

Invoegen in een niet-geregistreerde tabel betekent dat PostgreSQL voegt zonder schrijven in de transactie aanmelden, welke zelf een i/o-bewerking is. Deze tabellen zijn als gevolg hiervan aanzienlijk sneller dan gewone tabellen.

Gebruik de volgende opties om een niet-geregistreerde tabel te maken:
- Een nieuwe niet-geregistreerde tabel maken met behulp van de syntaxis van de `CREATE UNLOGGED TABLE <tableName>`.
- Convert een bestaande tabel naar een niet-geregistreerde tabel vastgelegd met behulp van de syntaxis van de `ALTER TABLE <tableName> SET UNLOGGED`.  

Als u wilt het proces terugdraaien, gebruikt u de syntaxis `ALTER TABLE <tableName> SET LOGGED`.

## <a name="unlogged-table-tradeoff"></a>Negatieve gevolgen voor de niet-geregistreerde tabel
Niet-geregistreerde tabellen zijn niet crash-safe. Een niet-geregistreerde tabel automatisch afgekapt na een crash of onderhevig aan het afsluiten niet schoon. De inhoud van een niet-geregistreerde tabel worden niet ook gerepliceerd naar de stand-by-servers. Geen indexen gemaakt in een niet-geregistreerde tabel worden ook automatisch niet-geregistreerde. Na de insert-bewerking is voltooid, converteert u de tabel zodat de insert duurzame is aangemeld.

Sommige klantwerkbelastingen ondervonden ongeveer een prestatieverbetering van 15 tot 20 procent wanneer niet-geregistreerde tabellen zijn gebruikt.

## <a name="next-steps"></a>Volgende stappen
Controleer uw werkbelasting van het gebruik van tijdelijke gegevens en grote bulksgewijs invoegen. Zie de volgende PostgreSQL-documentatie:
 
- [Tabel SQL-opdrachten maken](https://www.postgresql.org/docs/current/static/sql-createtable.html)
