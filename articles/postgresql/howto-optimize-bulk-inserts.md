---
title: Bulksgewijs invoegen in Azure Database for PostgreSQL-server te optimaliseren
description: Dit artikel wordt beschreven hoe u bulk insert-bewerkingen op Azure Database for PostgreSQL-server kunt optimaliseren.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 9d2bfcddc649e4fff68bdba49df0945e88067036
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545226"
---
# <a name="optimizing-bulk-inserts-and-use-of-transient-data-on-azure-database-for-postgresql-server"></a>Bulksgewijs invoegen en het gebruik van tijdelijke gegevens in Azure Database for PostgreSQL-server te optimaliseren 
Dit artikel wordt beschreven hoe u bulk insert-bewerkingen en het gebruik van tijdelijke gegevens op een Azure Database for PostgreSQL-server kunt optimaliseren.

## <a name="using-unlogged-tables"></a>Niet-geregistreerde tabellen gebruiken
Voor klanten die de werkbelasting van bewerkingen die betrekking hebben op tijdelijke gegevens of die grote gegevenssets bulksgewijs invoegen hebben, kunt u met behulp van niet-geregistreerde tabellen.

Niet-geregistreerde tabellen is een PostgreSQL-functie die effectief kan worden gebruikt om te optimaliseren bulksgewijs invoegen. PostgreSQL maakt gebruik van Write-Ahead logboekregistratie (WAL), die standaard atomisch en duurzaamheid van de ACID-eigenschappen. In een niet-geregistreerde tabel invoegen zou, zou gemiddelde PostgreSQL doen voegt zonder schrijven in het transactielogboek, is welke zelf een i/o-bewerking, waardoor deze tabellen aanzienlijk sneller dan gewone tabellen.

Hieronder vindt u de opties voor het maken van een niet-geregistreerde tabel:
- Maak een nieuwe niet-geregistreerde tabel met de syntaxis: `CREATE UNLOGGED TABLE <tableName>`
- Convert een bestaande tabel geregistreerd in een niet-geregistreerde-tabel met de syntaxis: `ALTER <tableName> SET UNLOGGED`.  Dit kan worden teruggedraaid met behulp van de syntaxis: `ALTER <tableName> SET LOGGED`

## <a name="unlogged-table-tradeoff"></a>Negatieve gevolgen voor de niet-geregistreerde tabel
Niet-geregistreerde tabellen maken geen crash-safe. Een niet-geregistreerde tabel automatisch afgekapt na een crash of onderhevig aan het afsluiten niet schoon. De inhoud van een niet-geregistreerde tabel worden ook niet gerepliceerd naar stand-by-servers. Geen indexen gemaakt in een niet-geregistreerde tabel worden ook automatisch niet-geregistreerde.  Nadat de insert-bewerking is voltooid, kunt u de tabel moeten vastgelegd, zodat de insert duurzame is mogelijk converteren.

Maar voor sommige klantwerkbelastingen is er ongeveer een prestatieverbetering van 15-20 procent bij het gebruik van niet-geregistreerde tabellen.

## <a name="next-steps"></a>Volgende stappen
Controleer uw werkbelasting van het gebruik van tijdelijke gegevens en grote bulksgewijs invoegen.  

Raadpleeg de volgende documentatie van de PostgreSQL - [tabel SQL-opdrachten maken](https://www.postgresql.org/docs/current/static/sql-createtable.html)