---
title: Migreren van een database met behulp van importeren en exporteren in Azure-Database voor PostgreSQL
description: Beschrijft hoe een PostgreSQL-database in een scriptbestand en importeer de gegevens in de doeldatabase van dat bestand.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 5250b10c2d9645e25c5b6f6f0f34daa50608e51b
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736640"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migreer uw PostgreSQL-database met exporteren en importeren
U kunt [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) een PostgreSQL-database in een scriptbestand uitpakken en [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) de gegevens in de doeldatabase van dat bestand importeert.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een [Azure-Database voor PostgreSQL server](quickstart-create-server-database-portal.md) met firewall-regels waarmee toegang en database onder deze.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) opdrachtregelprogramma geïnstalleerd
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) opdrachtregelprogramma geïnstalleerd

Volg deze stappen om te exporteren en importeren van uw PostgreSQL-database.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Maken van een scriptbestand met pg_dump met de gegevens worden geladen
Exporteren van uw bestaande PostgreSQL-database on-premises of in een virtuele machine naar een sql-scriptbestand, de volgende opdracht in uw bestaande omgeving worden uitgevoerd:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Bijvoorbeeld, als er een lokale server en een database met de naam **testdb** erin:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>De gegevens op de doel-Azure-Database importeren voor PostgreSQL
U kunt de psql vanaf de opdrachtregel en de parameter--dbname (-d) de gegevens wilt importeren in de Azure-Database voor PostgreSQL-server en gegevens laden van het sql-bestand.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
In dit voorbeeld wordt psql hulpprogramma en een scriptbestand met de naam **testdb.sql** uit de vorige stap om gegevens te importeren in de database **mypgsqldb** op de doelserver  **mydemoserver.postgres.database.Azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van de dump en terugzetten [Migreer uw PostgreSQL-database met behulp van de dump en terugzetten](howto-migrate-using-dump-and-restore.md).
- Voor meer informatie over het migreren van databases met Azure-Database voor PostgreSQL, Zie de [Database Migratiehandleiding](http://aka.ms/datamigration). 
