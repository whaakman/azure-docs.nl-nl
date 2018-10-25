---
title: Migreren van uw MariaDB-database met behulp van dumpen en terugzetten in Azure Database voor MariaDB
description: In dit artikel wordt uitgelegd dat twee algemene manieren om te back-up en herstellen van databases in uw Azure Database voor MariaDB, met behulp van hulpprogramma's zoals mysqldump, MySQL Workbench en PHPMyAdmin.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 5ac80ef36241b58812832c66511c0a09d49b365f
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986981"
---
# <a name="migrate-your-mariadb-database-to-azure-database-for-mariadb-using-dump-and-restore"></a>Uw MariaDB-database migreren naar Azure Database voor MariaDB met behulp van dumpen en terugzetten
Dit artikel wordt uitgelegd twee algemene manieren om te back-up en herstellen van databases in uw Azure Database voor MariaDB
- Dumpen en terugzetten vanaf de opdrachtregel (met behulp van mysqldump) 
- Dump maken en herstellen met behulp van PHPMyAdmin

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, moet u beschikken over:
- [Azure Database voor MariaDB-server maken-Azure portal maken](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [mysqldump](https://mariadb.com/kb/en/library/mysqldump/) opdrachtregelprogramma moet zijn geïnstalleerd op een virtuele machine.
- MySQL Workbench [MySQL Workbench downloaden](https://dev.mysql.com/downloads/workbench/), Toad, Navicat of andere hulpprogramma van derden MySQL dumpen en terugzetten van opdrachten.

## <a name="use-common-tools"></a>Gebruik algemene hulpprogramma 's
Gebruik algemene hulpprogramma's en hulpprogramma's, zoals MySQL Workbench, mysqldump, Toad of Navicat op afstand verbinding maken en herstellen van gegevens in Azure Database voor MariaDB. Gebruik deze hulpprogramma's op de clientcomputer met een internetverbinding verbinding maakt met de Azure Database voor MariaDB. Een versleuteld met SSL-verbinding gebruiken voor aanbevolen procedures voor beveiliging, Zie ook [configureren van SSL-verbindingen in Azure Database voor MariaDB](concepts-ssl-connection-security.md). U hoeft niet de om dumpbestanden te verplaatsen naar een locatie speciale cloud bij het migreren naar Azure Database voor MariaDB. 

## <a name="common-uses-for-dump-and-restore"></a>Veelvoorkomende toepassingen voor dumpen en terugzetten
U kunt MySQL-hulpprogramma's zoals mysqldump en mysqlpump met dump en load-databases in een Azure Database voor MariaDB-server in verschillende algemene scenario's. 

<!--In other scenarios, you may use the [Import and Export](howto-migrate-import-export.md) approach instead.-->

- Gebruik database dumpen wanneer u de volledige database migreert. Deze aanbeveling heeft bij het verplaatsen van een grote hoeveelheid gegevens of wanneer u wilt beperken van de serviceonderbreking voor live sites of toepassingen. 
-  Zorg ervoor dat alle tabellen in de database de InnoDB-opslag-engine gebruiken bij het laden van gegevens in Azure Database voor MariaDB. Azure Database voor MariaDB ondersteunt alleen InnoDB-opslag-engine en daarom biedt geen ondersteuning voor alternatieve opslag-engines. Als uw tabellen zijn geconfigureerd met andere opslag-engines, kunt u deze naar de InnoDB-engine-indeling voor de migratie naar Azure Database voor MariaDB converteren.
   Bijvoorbeeld, hebt u een WordPress- of Web-App met behulp van de MyISAM-tabellen, converteert u eerst deze tabellen door te migreren naar InnoDB-indeling voor het herstellen met Azure Database voor MariaDB. Gebruik de component `ENGINE=InnoDB` om in te stellen de engine gebruikt bij het maken van een nieuwe tabel, klikt u vervolgens de gegevens overdragen naar de compatibele tabel vóór het herstellen. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Om te voorkomen dat eventuele compatibiliteitsproblemen, zorg ervoor dat dezelfde versie van MariaDB op de bron- en -systemen wordt gebruikt tijdens het dumpen van databases. Bijvoorbeeld, als uw bestaande MariaDB-server versie 10.2 is, moet klikt u vervolgens u migreren naar Azure Database voor MariaDB geconfigureerd voor het uitvoeren van versie 10.2. De `mysql_upgrade` opdracht werkt niet in een Azure Database voor MariaDB-server en wordt niet ondersteund. Als u upgraden naar versies van MariaDB wilt, eerst dump of de lagere versie-database exporteren naar een hogere versie van MariaDB in uw eigen omgeving. Voer `mysql_upgrade`, voordat u probeert te migreren naar een Azure-Database voor MariaDB.

## <a name="performance-considerations"></a>Prestatieoverwegingen
Voor optimale prestaties, bekijk van deze overwegingen bij het dumpen van grote databases:
-   Gebruik de `exclude-triggers` optie in mysqldump tijdens het dumpen van databases. Triggers uitsluiten van bestanden om te voorkomen dat de trigger-opdrachten uitvoeren tijdens het herstellen van de gegevens. 
-   Gebruik de `single-transaction` optie om in te stellen van de transactie-isolatiemodus HERHAALBARE lezen en een begin TRANSACTION SQL-instructie verzendt naar de server voordat het dumpen van gegevens. Dumpen van een groot aantal tabellen binnen een transactie van zorgt ervoor dat sommige extra opslagruimte om te worden verbruikt tijdens het terugzetten. De `single-transaction` optie en de `lock-tables` optie zijn sluiten elkaar wederzijds uit omdat vergrendeling tabellen zorgt ervoor dat alle in afwachting van transacties voor impliciet worden doorgevoerd. Als u wilt dump grote tabellen, combineren de `single-transaction` optie met de `quick` optie. 
-   Gebruik de `extended-insert` syntaxis met meerdere rijen met lijsten met verschillende waarden. Dit resulteert in een kleiner bestand en voegt versnelt wanneer het bestand opnieuw is geladen.
-  Gebruik de `order-by-primary` optie in mysqldump tijdens het dumpen van databases, zodat de gegevens in de volgorde van de primaire sleutel in een script vastgelegd.
-   Gebruik de `disable-keys` optie in mysqldump tijdens het dumpen van gegevens, voor het uitschakelen van beperkingen voor referentiële sleutels vóór belasting. Refererende sleutel controles uit te schakelen biedt prestaties. Inschakelen van de beperkingen en controleer of de gegevens na het laden om referentiële integriteit te waarborgen.
-   Gepartitioneerde tabellen, indien van toepassing gebruiken.
-   Gegevens parallel laden. Voorkomen dat te veel parallelle uitvoering die zou u een resourcelimiet bereikt en bewaken van resources met behulp van de metrische gegevens beschikbaar zijn in de Azure-portal. 
-   Gebruik de `defer-table-indexes` optie in mysqlpump tijdens het dumpen van databases, zodat het maken van een index gebeurt nadat tabellen gegevens zijn geladen.
-   Kopieer de back-upbestanden naar een Azure-blob/store en het herstel uitvoeren van daaruit, moet je kunt veel sneller dan het uitvoeren van het herstel via Internet.

## <a name="create-a-backup-file"></a>Een back-upbestand maken
Als u wilt back-up van een bestaande database voor MariaDB op de lokale on-premises server of in een virtuele machine, voer de volgende opdracht uit met behulp van mysqldump: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

De parameters voor zijn:
- [uname] De gebruikersnaam voor de database 
- [wachtwoord] Het wachtwoord voor uw database (Let er is geen ruimte tussen -p en het wachtwoord) 
- [dbname] De naam van uw database 
- [backupfile.sql] de bestandsnaam van de back-up van uw database 
- [--opt] De optie mysqldump 

Bijvoorbeeld, als u wilt back-up van een database met de naam 'testdb' op uw server MariaDB met de gebruikersnaam 'testuser' en er is geen wachtwoord voor een bestand testdb_backup.sql, gebruik de volgende opdracht. De opdracht back-ups van de `testdb` database naar een bestand met de naam `testdb_backup.sql`, waarin alle van de SQL-instructies die nodig zijn voor de database opnieuw maken. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Selecteer specifieke tabellen in uw database om back-up maken, de tabel-namen gescheiden door spaties. Bijvoorbeeld, als u wilt back-up alleen Tabel1 en table2 tabellen uit de 'testdb', gaat u als volgt in dit voorbeeld: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Als u wilt back-up van meer dan één database in één keer, gebruik de--database schakelen en de database-namen gescheiden door spaties. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Als u wilt back-up van alle databases in de server in één keer, moet u de--optie alle databases.
```bash
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-server"></a>Een database maken op de doelserver
Een lege database maken op de doel-Azure Database voor MariaDB-server waar u om de gegevens te migreren. Gebruik een hulpprogramma zoals MySQL Workbench, Toad of Navicat om de database te maken. De database kan dezelfde naam hebben als de database die is opgenomen in de gedumpte gegevens of u een database met een andere naam maken kunt.

Als u wilt verbinding maken, zoekt u de verbindingsgegevens in de **overzicht** van uw Azure Database voor MariaDB.

![Informatie over de verbinding in Azure portal](./media/howto-migrate-dump-restore/1_server-overview-name-login.png)

Voeg de verbindingsinformatie in uw MySQL Workbench toe.

![MySQL Workbench-verbindingsreeks](./media/howto-migrate-dump-restore/2_setup-new-connection.png)

## <a name="restore-your-mariadb-database"></a>Herstellen van uw database voor MariaDB
Als u de doel-database hebt gemaakt, kunt u de mysql-opdracht of MySQL Workbench kunt gebruiken voor het terugzetten van de gegevens in de zojuist gemaakte database uit het dumpbestand specifieke.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
In dit voorbeeld de gegevens in de zojuist gemaakte database op de doel-Azure Database voor MariaDB-server te herstellen.
```bash
$ mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exporteren met behulp van PHPMyAdmin
Als u wilt exporteren, kunt u de algemene hulpprogramma phpMyAdmin, die u mogelijk al hebt geïnstalleerd lokaal in uw omgeving. Uw MariaDB-database met behulp van PHPMyAdmin exporteren:
1. Open phpMyAdmin.
2. Selecteer uw database. Klik op de naam van de database in de lijst aan de linkerkant. 
3. Klik op de **exporteren** koppeling. Een nieuwe pagina wordt weergegeven om de dump van de database weer te geven.
4. In het gebied exporteren, klikt u op de **Alles selecteren** koppeling naar de tabellen in uw database kiezen. 
5. Klik in het gebied van de opties voor SQL op de gewenste opties. 
6. Klik op de **opslaan als bestand** optie en de bijbehorende compressie optie en klik vervolgens op de **gaat** knop. Een dialoogvenster wordt weergegeven waarin u het bestand lokaal op te slaan.

## <a name="import-using-phpmyadmin"></a>Importeren met behulp van PHPMyAdmin
Importeren van uw database is vergelijkbaar met het exporteren. De volgende acties uitvoeren:
1. Open phpMyAdmin. 
2. Klik in de installatiepagina phpMyAdmin **toevoegen** om toe te voegen van uw Azure Database voor MariaDB-server. Geef de verbindingsgegevens en aanmeldingsgegevens.
3. Maak een database met de juiste naam en selecteert u deze aan de linkerkant van het scherm. Voor het herschrijven van de bestaande database, klikt u op de naam van de database, schakel alle selectievakjes naast de tabelnamen van de en selecteer **neerzetten** verwijderen van de bestaande tabellen. 
4. Klik op de **SQL** koppeling om de pagina waar u kunt typen in SQL-opdrachten, of upload uw SQL-bestand weer te geven. 
5. Gebruik de **Bladeren** om het databasebestand niet vinden. 
6. Klik op de **gaat** knop exporteren van de back-up, voert u de SQL-opdrachten en opnieuw maken van de database.

## <a name="next-steps"></a>Volgende stappen
- [Verbinding maken met toepassingen met Azure Database voor MariaDB](./howto-connection-string.md).
 
<!--
- For more information about migrating databases to Azure Database for MariaDB, see the [Database Migration Guide](https://aka.ms/datamigration).
-->