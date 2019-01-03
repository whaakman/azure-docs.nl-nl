---
title: Het dumpen en terugzetten in de Azure Database for PostgreSQL
description: Beschrijft hoe u een PostgreSQL-database voor het uitpakken naar een dumpbestand en terugzetten vanuit een bestand dat is gemaakt door pg_dump in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 41a5f2eab78d68bdb1f51b423955cfefa5a541b8
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53538582"
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


## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Herstel de gegevens in de doel-Azure Database voor PostrgeSQL pg_restore gebruiken
Nadat u de doel-database hebt gemaakt, kunt u de opdracht pg_restore en de d-,--dbname-parameter voor het terugzetten van de gegevens in de doeldatabase uit het dumpbestand.
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

## <a name="optimizing-the-migration-process"></a>Het migratieproces optimaliseren

Een manier om uw bestaande PostgreSQL-database migreren naar Azure Database for PostgreSQL-service is het back-up van de database op de bron- en deze herstellen in Azure. Om te beperken van de tijd die nodig is om de migratie te voltooien, kunt u overwegen de volgende parameters met de back-up en herstel van opdrachten.

> [!NOTE]
> Zie de artikelen voor informatie over de gedetailleerde syntaxis, [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) en [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).
>

### <a name="for-the-backup"></a>Voor de back-up
- Een back-up met de switch Fc - zodat u de terugzetbewerking parallel uitvoeren kunt snelheid verhogen. Bijvoorbeeld:

    ```
    pg_dump -h MySourceServerName -U MySourceUserName -Fc -d MySourceDatabaseName > Z:\Data\Backups\MyDatabaseBackup.dump
    ```

### <a name="for-the-restore"></a>Voor het herstellen
- Het is raadzaam dat u de back-upbestand naar een Azure-VM in dezelfde regio als de Azure Database for PostgreSQL-server u migreert verplaatsen naar en voer de pg_restore van die VM te verminderen van de netwerklatentie. We raden u ook aan dat de virtuele machine wordt gemaakt met [versnelde netwerken](../virtual-network/create-vm-accelerated-networking-powershell.md) ingeschakeld.
- Het standaard moet worden gedaan, maar het dumpbestand om te controleren dat de instructies van de index maken na het invoegen van de gegevens zijn geopend. Als dit niet het geval is, verplaatst u de instructies van de index maken nadat de gegevens worden ingevoegd.
- Herstellen met de schakelopties -Fc- en -j *#* naar parallel het terugzetten. *#* is het aantal kernen op de doelserver. U kunt ook proberen met *#* ingesteld op twee keer het aantal kernen van de doelserver om te zien van de impact. Bijvoorbeeld:

    ```
    pg_restore -h MyTargetServer.postgres.database.azure.com -U MyAzurePostgreSQLUserName -Fc -j 4 -d MyTargetDatabase Z:\Data\Backups\MyDatabaseBackup.dump
    ```

- U kunt ook het dumpbestand bewerken door de opdracht toe te voegen *synchronous_commit instellen = uit;* aan het begin en de opdracht *synchronous_commit instellen = on;* aan het einde. Niet te schakelen op aan het einde, voordat de apps de gegevens wijzigen, kan dit leiden tot volgende verlies van gegevens.

Houd er rekening mee te testen en valideren van deze opdrachten in een testomgeving voordat u ze in productie gebruiken.

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van exporteren en importeren, [migreren van de PostgreSQL-database met behulp van exporteren en importeren](howto-migrate-using-export-and-import.md).
- Voor meer informatie over het migreren van databases met Azure Database voor PostgreSQL, Zie de [handleiding voor databasemigratie](https://aka.ms/datamigration).
