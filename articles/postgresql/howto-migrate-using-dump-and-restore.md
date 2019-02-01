---
title: Het dumpen en terugzetten in de Azure Database for PostgreSQL
description: Beschrijft hoe u een PostgreSQL-database voor het uitpakken naar een dumpbestand en terugzetten vanuit een bestand dat is gemaakt door pg_dump in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2018
ms.openlocfilehash: 366a38951363d52df3d52d3a670943dc41211c8a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493997"
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

- Op de doel-Azure Database for PostgreSQL-server, Stel desgewenst het volgende voordat u de terugzetbewerking:
    - Uitschakelen query bijhouden van prestaties, omdat deze statistische gegevens niet nodig zijn tijdens de migratie. U kunt dit doen door in te stellen pg_stat_statements.track pg_qs.query_capture_mode en pgms_wait_sampling.query_capture_mode op NONE.

    - Een hoge Computing en geheugen sku, zoals 32 vCore geoptimaliseerd voor geheugen, gebruiken om de migratie te versnellen. U kunt gemakkelijk schalen omlaag naar de gewenste sku nadat het herstel voltooid is. Hoe hoger de sku, de meer paralellism u kunt bereiken met het verhogen van de bijbehorende `-j` parameter in de opdracht pg_restore. 

    - Meer IOPS op de doelserver kan de herstel-prestaties verbeteren. U kunt meer IOPS inrichten door de opslaggrootte van de server. Deze instelling is niet ongedaan worden gemaakt, maar houd rekening met of een hogere IOPS voordeel uw huidige workload in de toekomst hebben veel.

Houd er rekening mee te testen en valideren van deze opdrachten in een testomgeving voordat u ze in productie gebruiken.

## <a name="next-steps"></a>Volgende stappen
- Zie voor het migreren van een PostgreSQL-database met behulp van exporteren en importeren, [migreren van de PostgreSQL-database met behulp van exporteren en importeren](howto-migrate-using-export-and-import.md).
- Voor meer informatie over het migreren van databases met Azure Database voor PostgreSQL, Zie de [handleiding voor databasemigratie](https://aka.ms/datamigration).
