---
title: Dump en herstellen in Azure-Database voor PostgreSQL | Microsoft Docs
description: Beschrijft hoe een PostgreSQL-database in een bestand en de PostgreSQL-database terugzetten vanaf een archiefbestand voor PostgreSQL door pg_dump in Azure-Database gemaakt.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 28727117dbd37f9c595b488639a632b4c7404496
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migreer uw PostgreSQL-database met behulp van de dump en terugzetten
U kunt [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) een PostgreSQL-database in een dumpbestand uitpakken en [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) de PostgreSQL-database herstellen uit een archiefbestand gemaakt door pg_dump.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een [Azure-Database voor PostgreSQL server](quickstart-create-server-database-portal.md) met firewall-regels waarmee toegang en database onder deze.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) en [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) opdrachtregelprogramma's geïnstalleerd

Volg deze stappen voor dump en uw PostgreSQL-database terugzetten:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Maken van een bestand met behulp van pg_dump met de gegevens worden geladen
Als u wilt een back-up van een bestaande PostgreSQL-database on-premises of in een virtuele machine, voer de volgende opdracht:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Bijvoorbeeld, als er een lokale server en een database met de naam **testdb** erin
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Herstel de gegevens op de doel-Azure-Database voor PostrgeSQL met pg_restore
Nadat u de doeldatabase hebt gemaakt, kunt u de opdracht pg_restore en -d,--dbname-parameter voor het herstellen van de gegevens in de doeldatabase van het dumpbestand.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
In dit voorbeeld kunt u de gegevens terugzetten vanaf het dumpbestand **testdb.dump** in de database **mypgsqldb** op doelserver **mypgserver 20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van exporteren en importeren [Migreer uw PostgreSQL-database met exporteren en importeren](howto-migrate-using-export-and-import.md)