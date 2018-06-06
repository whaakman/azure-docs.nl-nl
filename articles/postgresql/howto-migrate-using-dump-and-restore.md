---
title: Dump en herstellen in Azure-Database voor PostgreSQL
description: Beschrijft hoe een PostgreSQL-database in een dumpbestand en herstellen van een bestand gemaakt door pg_dump in Azure-Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 586df8d72dc05104bbf589eabcf3bd2245c268c8
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737245"
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

> [!IMPORTANT]
> Kopieer de back-upbestanden naar een Azure-blob/store en het terugzetten van uitvoeren vanaf daar moet veel sneller dan het uitvoeren van het herstel via Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Herstel de gegevens op de doel-Azure-Database voor PostrgeSQL met pg_restore
Nadat u de doeldatabase hebt gemaakt, kunt u de opdracht pg_restore en -d,--dbname-parameter voor het herstellen van de gegevens in de doeldatabase van het dumpbestand.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Inclusief de oorzaken van de--Nee-owner parameter op alle objecten die zijn gemaakt tijdens het terugzetten van eigendom zijn van de gebruiker opgegeven met--gebruikersnaam. Zie de officiële PostgreSQL-documentatie voor meer informatie op [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

In dit voorbeeld kunt u de gegevens terugzetten vanaf het dumpbestand **testdb.dump** in de database **mypgsqldb** op doelserver **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van exporteren en importeren [Migreer uw PostgreSQL-database met exporteren en importeren](howto-migrate-using-export-and-import.md).
- Voor meer informatie over het migreren van databases met Azure-Database voor PostgreSQL, Zie de [Database Migratiehandleiding](http://aka.ms/datamigration).
