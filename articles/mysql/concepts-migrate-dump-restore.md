---
title: Migreer uw MySQL-database met behulp van de dump en MySQL in Azure-Database herstellen | Microsoft Docs
description: Dit artikel wordt uitgelegd twee algemene manieren om te back-up en herstellen van databases in uw Azure-Database voor MySQL, met de hulpprogramma's zoals mysqldump, MySQL Workbench en PHPMyAdmin.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: e962fd65244ceebfc7544dc5a1d1956dad811fea
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Uw MySQL-database migreren naar Azure-Database voor MySQL met behulp van de dump en terugzetten
Dit artikel wordt uitgelegd twee algemene manieren om te back-up en MySQL-databases in uw Azure-Database herstellen
- Dump en terugzetten vanaf de opdrachtregel (via mysqldump) 
- Dump en herstellen met behulp van PHPMyAdmin 

## <a name="before-you-begin"></a>Voordat u begint
U moet stap in deze handleiding instructies, wilt hebben:
- [Azure-Database maken voor de server van de MySQL - Azure-portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) opdrachtregelprogramma geïnstalleerd op een computer.
- MySQL-Workbench [MySQL Workbench downloaden](https://dev.mysql.com/downloads/workbench/), Toad, Navicat of andere hulpprogramma van derden MySQL doen dump en opdrachten herstellen.

## <a name="use-common-tools"></a>Algemene hulpprogramma's gebruiken
Algemene hulpprogramma's en hulpprogramma's gebruiken zoals MySQL Workbench, mysqldump, Toad of Navicat op afstand verbinding maken en herstellen van gegevens voor MySQL in Azure-Database. Gebruik deze hulpprogramma's op de clientcomputer met een internetverbinding verbinding maken met de Azure-Database voor MySQL. Een SSL versleutelde verbinding gebruiken voor best practices voor beveiliging, Zie ook [configureren van SSL-verbindingen in Azure-Database voor MySQL](concepts-ssl-connection-security.md). U hoeft niet de om dumpbestanden te verplaatsen naar een willekeurige locatie speciale cloud wanneer u migreert naar Azure-Database voor MySQL. 

## <a name="common-uses-for-dump-and-restore"></a>Veelvoorkomende toepassingen voor dump en herstel
U kunt de MySQL-hulpprogramma's zoals mysqldump en mysqlpump met dump en load-databases in een Azure MySQL-Database in verschillende algemene scenario's. In andere scenario's, mag u de [importeren en exporteren](concepts-migrate-import-export.md) in plaats daarvan benaderen.

- Gebruik database dumpen wanneer u de gehele database migreert. Deze aanbeveling bevat bij het verplaatsen van een grote hoeveelheid gegevens MySQL, of wanneer u wilt minimaliseren onderbreking van de service voor live sites of toepassingen. 
-  Zorg ervoor dat alle tabellen in de database de InnoDB opslag-engine gebruiken bij het laden van gegevens in Azure-Database voor MySQL. Azure MySQL-Database ondersteunt alleen InnoDB opslag-engine en daarom biedt geen ondersteuning voor alternatieve opslag-engines. Als uw tabellen zijn geconfigureerd met andere motoren opslag, kunt u deze in de indeling van de engine InnoDB vóór de migratie naar Azure Database voor MySQL converteren.
   Bijvoorbeeld, als u een WordPress- of Web-App met behulp van de tabellen MyISAM hebt, converteert u eerst deze tabellen door te migreren naar InnoDB indeling, voordat u met Azure-Database terugzetten voor MySQL. Gebruik de component `ENGINE=InnoDB` om in te stellen de engine die wordt gebruikt bij het maken van een nieuwe tabel, brengt u de gegevens in de tabel compatibel vóór het herstellen. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Om te voorkomen dat eventuele compatibiliteitsproblemen, zorg ervoor dat dezelfde versie van MySQL op de bron- en doelserver systemen wordt gebruikt wanneer het dumpen van databases. Bijvoorbeeld als de bestaande MySQL-server versie 5.7, moet vervolgens u migreren naar Azure Database voor MySQL geconfigureerd versie 5.7 wordt uitgevoerd. De `mysql_upgrade` opdracht werkt niet in een Azure-Database voor de MySQL-server en wordt niet ondersteund. Als u upgraden naar versies van MySQL wilt, eerst dump of de lagere versie database exporteren naar een hogere versie van MySQL in uw eigen omgeving. Voer `mysql_upgrade`, voordat u de migratie naar een Azure-Database voor MySQL.

## <a name="performance-considerations"></a>Prestatieoverwegingen
Om prestaties te optimaliseren, nemen kennisgeving van deze overwegingen bij het dumpen van grote databases:
-   Gebruik de `exclude-triggers` optie in mysqldump wanneer het dumpen van databases. Triggers uitsluiten van dumpbestanden om te voorkomen dat de trigger-opdrachten starten tijdens het herstellen van gegevens. 
-   Gebruik de `single-transaction` optie in te stellen van de transactie isolatiemodus op REPEATABLE READ- en een starten transactie SQL-instructie voordat het dumpen van gegevens naar de server verzendt. Dumpen van een groot aantal tabellen binnen een transactie van zorgt ervoor dat een aantal extra opslagruimte te worden gebruikt tijdens het terugzetten. De `single-transaction` optie en de `lock-tables` optie sluiten elkaar wederzijds uit omdat vergrendeling tabellen ervoor zorgt dat in behandeling zijnde transacties worden impliciet doorgegeven. Als u wilt dump grote tabellen, combineren de `single-transaction` optie met de `quick` optie. 
-   Gebruik de `extended-insert` meerdere rij syntaxis met lijsten met verschillende waarden. Dit resulteert in een kleiner bestand en voegt versneld wanneer het bestand opnieuw wordt geladen.
-  Gebruik de `order-by-primary` optie in mysqldump wanneer het dumpen van databases, zodat de gegevens in volgorde van de primaire sleutel in een script vastgelegd.
-   Gebruik de `disable-keys` optie in mysqldump wanneer het dumpen van gegevens, voor het uitschakelen van de referentiële-sleutelbeperkingen vóór belasting. Refererende sleutel controles uit te schakelen, biedt betere prestaties. Schakel de beperkingen en controleer de gegevens na de belasting om ervoor te zorgen referentiële integriteit.
-   Gepartitioneerde tabellen, indien van toepassing gebruiken.
-   Gegevens parallel worden geladen. Vermijd te veel parallelle uitvoering die zou leiden dat u een resource-limiet bereikt en bewaken van resources met behulp van de metrische gegevens beschikbaar zijn in de Azure-portal. 
-   Gebruik de `defer-table-indexes` optie in mysqlpump wanneer het dumpen van databases, zodat het maken van een index gebeurt nadat tabellen gegevens zijn geladen.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Een back-upbestand te maken vanaf de opdrachtregel met behulp van mysqldump
Als u wilt back-up van een bestaande MySQL-database op de lokale on-premises server of in een virtuele machine, voer de volgende opdracht: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

De parameters voor zijn:
- [uname] Uw database-gebruikersnaam 
- [pass] Het wachtwoord voor uw database (Let er is geen ruimte tussen -p en het wachtwoord) 
- [dbname] De naam van uw database 
- [backupfile.sql] de bestandsnaam op van de back-up van uw database 
- [--opt] De optie mysqldump 

Bijvoorbeeld, als u wilt back-up van een database met naam 'testdb' op uw MySQL-server met de gebruikersnaam 'testgebruiker' en er is geen wachtwoord voor een bestand testdb_backup.sql, gebruik de volgende opdracht. De opdracht back-ups van de `testdb` database naar een bestand met de naam `testdb_backup.sql`, waarin alle SQL-instructies die nodig zijn voor de database opnieuw maken. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Selecteer specifieke tabellen in uw database back-up, de tabel-namen gescheiden door spaties. Bijvoorbeeld als u wilt back-up alleen table1 en table2 tabellen van de 'testdb', voert u in dit voorbeeld: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

Back-up wilt meerdere databases in één keer, gebruiken de--database switch en de database-namen gescheiden door spaties. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Als u wilt back-up van alle databases in de server in één keer, moet u de--optie all-databases.
```bash
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>Een database maken op de doel-Azure-Database voor de MySQL-server
Een lege database maken op de doel-Azure-Database voor de MySQL-server waarop u wilt migreren van de gegevens. Gebruik een hulpprogramma zoals MySQL Workbench, Toad of Navicat om de database te maken. De database kan dezelfde naam hebben als de database die is opgenomen in de gedumpte gegevens of u een database met een andere naam maken kunt.

Om verbinding te maken, door de verbindingsgegevens op de pagina eigenschappen in uw Azure-Database voor de MySQL te vinden.
![De verbindingsgegevens niet vinden in de Azure-portal](./media/concepts-migrate-dump-restore/1_server-properties-name-login.png)

Voeg de verbindingsinformatie in uw MySQL-Workbench.
![MySQL-Workbench verbindingsreeks](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Uw MySQL-database met een opdrachtregel of MySQL Workbench herstellen
Nadat u de doeldatabase hebt gemaakt, kunt u de mysql-opdracht of het MySQL Workbench kunt gebruiken voor het terugzetten van de gegevens in de nieuwe database gemaakt van het dumpbestand voor specifieke.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
In dit voorbeeld wordt de gegevens in de zojuist gemaakte database op de doel-Azure-Database voor de MySQL-server te herstellen.
```bash
$ mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exporteren met behulp van PHPMyAdmin
Als u wilt exporteren, kunt u de algemene hulpprogramma phpMyAdmin die u mogelijk al hebt geïnstalleerd lokaal in uw omgeving. Uw MySQL-database met behulp van PHPMyAdmin exporteren:
- Open phpMyAdmin.
- Selecteer uw database. Klik op de naam van de database in de lijst aan de linkerkant. 
- Klik op de **exporteren** koppeling. Een nieuwe pagina wordt weergegeven om weer te geven van de dump database.
- In het gebied exporteren, klikt u op de **Alles selecteren** koppeling naar de tabellen in uw database kiezen. 
- Klik op de gewenste opties in het gebied van de opties SQL. 
- Klik op de **opslaan als bestand** optie en de bijbehorende compressie optie en klik vervolgens op de **gaat** knop. Een dialoogvenster waarin u het bestand lokaal op te slaan, worden weergegeven.

## <a name="import-using-phpmyadmin"></a>Importeren met PHPMyAdmin
Importeren van uw database is vergelijkbaar met het exporteren. De volgende acties uitvoeren:
- Open phpMyAdmin. 
- Klik in de installatiepagina phpMyAdmin **toevoegen** uw Azure-Database voor de MySQL-server toevoegen. Geef de verbindingsgegevens en aanmeldingsgegevens.
- Maak een geschikte naam database en selecteer het aan de linkerkant van het scherm. Als u wilt de bestaande database opnieuw kunt schrijven, klikt u op de naam van de database, schakel de selectievakjes naast de tabelnamen en selecteer **Drop** verwijderen van de bestaande tabellen. 
- Klik op de **SQL** koppeling naar de pagina waar u kunt typen in SQL-opdrachten, of uw SQL-bestand uploadt weergeven. 
- Gebruik de **Bladeren** om te zoeken van het databasebestand. 
- Klik op de **gaat** knop exporteren van de back-up, de SQL-opdrachten uit te voeren en de database opnieuw maken.

## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met toepassingen met Azure-Database voor MySQL](./howto-connection-string.md)
