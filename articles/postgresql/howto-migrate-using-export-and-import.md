---
title: Migreren van een database met behulp van importeren en exporteren in Azure Database for PostgreSQL
description: Hierin wordt beschreven hoe een PostgreSQL-database voor het uitpakken naar een scriptbestand en importeer de gegevens in de doeldatabase van dat bestand.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 5f6a83e33ea443839059b267dfb8043ae48af039
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987066"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migreren van de PostgreSQL-database met behulp van exporteren en importeren
U kunt [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) om op te halen van een PostgreSQL-database in een scriptbestand en [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) de gegevens importeren in de doeldatabase van dat bestand.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md) aan de firewallregels voor het toestaan van toegang en database daaronder.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) opdrachtregelprogramma moet zijn geïnstalleerd
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) opdrachtregelprogramma moet zijn geïnstalleerd

Volg deze stappen als u wilt exporteren en importeren van de PostgreSQL-database.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Maken van een scriptbestand met behulp van pg_dump met de gegevens worden geladen
Exporteren van uw bestaande PostgreSQL-database on-premises of in een virtuele machine naar een sql-script-bestand, kunt u de volgende opdracht uitvoeren in uw bestaande omgeving:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Bijvoorbeeld, als u hebt een database met de naam en een lokale server **testdb** erin:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Importeer de gegevens op de doel-Azure Database for PostgreSQL
U kunt de psql-opdrachtregel en de parameter--dbname (-d) de gegevens importeren in de Azure Database for PostgreSQL-server en gegevens laden uit het sql-bestand.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
In dit voorbeeld maakt gebruik van hulpprogramma psql en een scriptbestand met de naam **testdb.sql** uit de vorige stap om gegevens te importeren in de database **mypgsqldb** op de doelserver  **mydemoserver.postgres.database.Azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van dumpen en terugzetten, [migreren van de PostgreSQL-database met behulp van dumpen en terugzetten](howto-migrate-using-dump-and-restore.md).
- Voor meer informatie over het migreren van databases met Azure Database voor PostgreSQL, Zie de [handleiding voor databasemigratie](https://aka.ms/datamigration). 
