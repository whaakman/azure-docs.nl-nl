---
title: Het dumpen en terugzetten in de Azure Database for PostgreSQL
description: Beschrijft hoe u een PostgreSQL-database voor het uitpakken naar een dumpbestand en terugzetten vanuit een bestand dat is gemaakt door pg_dump in Azure Database voor PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213647"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migreren van de PostgreSQL-database met behulp van dumpen en terugzetten
U kunt [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) om op te halen van een PostgreSQL-database naar een dumpbestand en [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) de PostgreSQL-database herstellen vanuit een archiefbestand die zijn gemaakt door pg_dump.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md) aan de firewallregels voor het toestaan van toegang en database daaronder.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) en [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) opdrachtregelprogramma's geïnstalleerd

Volg deze stappen voor het dumpen en terugzetten van de PostgreSQL-database:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Maken van een bestand met behulp van pg_dump met de gegevens worden geladen
Back-ups maken van een bestaande PostgreSQL-database on-premises of in een virtuele machine, moet u de volgende opdracht uitvoeren:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Bijvoorbeeld, als u hebt een database met de naam en een lokale server **testdb** erin
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Kopieer de back-upbestanden naar een Azure-blob/store en het herstel uitvoeren van daaruit, moet je kunt veel sneller dan het uitvoeren van het herstel via Internet.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Herstel de gegevens in de doel-Azure Database voor PostrgeSQL pg_restore gebruiken
Als u de doel-database hebt gemaakt, kunt u de opdracht pg_restore en de d-,--dbname-parameter voor het terugzetten van de gegevens in de doeldatabase uit het dumpbestand.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Met inbegrip van de oorzaken van de--niet-owner parameter op alle objecten die zijn gemaakt tijdens het terugzetten van eigendom zijn van de gebruiker die is opgegeven met--username. Zie voor meer informatie, de officiële documentatie van PostgreSQL op [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> Als uw PostgreSQL-server SSL-verbindingen vereist (op die standaard in Azure Database voor PostgreSQL-servers), stelt u een omgevingsvariabele `PGSSLMODE=require` zodat het hulpprogramma pg_restore maakt verbinding met SSL. Zonder SSL, kan de fout lezen  `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Voer de opdracht in de Windows-opdrachtregel, `SET PGSSLMODE=require` voordat u de opdracht pg_restore uitvoert. Voer de opdracht in Linux of Bash `export PGSSLMODE=require` voordat u de opdracht pg_restore uitvoert.
>

In dit voorbeeld kunt u de gegevens terugzetten vanaf het dumpbestand **testdb.dump** in de database **mypgsqldb** op doelserver **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van exporteren en importeren, [migreren van de PostgreSQL-database met behulp van exporteren en importeren](howto-migrate-using-export-and-import.md).
- Voor meer informatie over het migreren van databases met Azure Database voor PostgreSQL, Zie de [handleiding voor databasemigratie](http://aka.ms/datamigration).
