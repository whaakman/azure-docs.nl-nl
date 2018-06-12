---
title: Replicatie-gegevens in om te repliceren van gegevens in Azure-Database voor MySQL configureren.
description: In dit artikel wordt beschreven hoe gegevens in de replicatie instellen voor Azure-Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: f9517cb552130e340310abc4affdad8bdadc26fe
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265748"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Het configureren van Azure-Database voor replicatie van MySQL-gegevens in

In dit artikel leert u hoe u gegevens in de replicatie instellen in de Azure-Database voor de MySQL-service configureren van primaire en replicaservers.

In dit artikel wordt ervan uitgegaan dat er ten minste enige ervaring met MySQL-servers en databases.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Een MySQL-server moet worden gebruikt als een replica maken

1. Maak een nieuwe Azure-Database voor de MySQL-server

   Maak een nieuwe MySQL-server (ex. 'replica.mysql.database.azure.com'). Raadpleeg [maken van een Azure-Database voor de MySQL-server met behulp van de Azure-portal](quickstart-create-mysql-server-database-using-azure-portal.md) voor het maken van de server. Deze server is de replicaserver '' in de gegevens in replicatie.

   > [!IMPORTANT]
   > Deze server moet worden gemaakt in de Prijscategorieën voor algemene doeleinden of geoptimaliseerd voor geheugen.
   > 

2. Dezelfde gebruikersaccounts en de bijbehorende rechten maken

   Gebruikersaccounts worden niet gerepliceerd van de primaire server naar de replicaserver. Als u van plan zodat gebruikers beschikken over toegang tot de replica-server bent, moet u handmatig maken van alle accounts en de bijbehorende machtigingen op de nieuwe Azure-Database voor de MySQL-server.

## <a name="configure-the-primary-server"></a>De primaire server configureren

1. Binaire logboekregistratie inschakelen

   Controleer als binaire logboekregistratie is ingeschakeld op de primaire met de volgende opdracht: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Als de variabele [ `log_bin` ](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) wordt geretourneerd met de waarde 'ON', binaire logboekregistratie is ingeschakeld op uw server. 

   Als `log_bin` is geretourneerd met de waarde 'OFF', schakelt u binaire logboekregistratie door het bestand my.cnf dus bewerkt die `log_bin=ON` en start de server voor de wijziging door te voeren.

2. Instellingen van de primaire server

   Parameter is vereist voor replicatie van gegevens in `lower_case_table_names` consistent zijn tussen de primaire en replica-servers. Deze parameter is 1 standaard in Azure voor MySQL-Database. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Maken van een nieuwe replicatie-rol en machtigingen instellen

   Maak een gebruikersaccount op de primaire server die is geconfigureerd met bevoegdheden voor replicatie. Dit kan worden gedaan via SQL-opdrachten of een hulpprogramma zoals MySQL-Workbench. Overweeg of u van plan gerepliceerd met SSL bent, zoals dit worden opgegeven moet bij het maken van de gebruiker. Raadpleeg de documentatie MySQL om te begrijpen hoe [gebruikersaccounts toevoegen](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) op uw primaire server. 

   De nieuwe replicatie-functie gemaakt is in de onderstaande opdrachten toegang kunnen krijgen tot de primaire vanaf een willekeurige computer, niet alleen de computer die als host fungeert voor de primaire zelf. Dit wordt gedaan door te geven ' syncuser@'%' ' in de opdracht van de gebruiker maken. Zie de MySQL-documentatie voor meer informatie over [geven accountnamen](https://dev.mysql.com/doc/refman/5.7/en/account-names.html).

   **SQL-opdracht**

   *Replicatie met SSL*

   Om SSL te vereisen voor alle verbindingen van de gebruiker, moet u de volgende opdracht gebruiken om een gebruiker te maken: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *Replicatie zonder SSL*

   Als SSL niet vereist voor alle verbindingen is, gebruikt u de volgende opdracht om een gebruiker te maken:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL-Workbench**

   Voor het maken van de replicatie-rol in de MySQL Workbench, opent u de **gebruikers en rechten** van het deelvenster van de **Management** Configuratiescherm. Klik vervolgens op **Account toevoegen**. 
 
   ![Gebruikers en rechten](./media/howto-data-in-replication/users_privileges.png)

   Typ de gebruikersnaam in de **aanmeldingsnaam** veld. 

   ![Synchronisatie-gebruiker](./media/howto-data-in-replication/syncuser.png)
 
   Klik op de **beheerdersrollen** -scherm en selecteer vervolgens **replicatie Slave** uit de lijst met **globale bevoegdheden**. Klik vervolgens op **toepassen** om de replicatie-rol te maken.

   ![Replicatie Slave](./media/howto-data-in-replication/replicationslave.png)


4. Instellen van de primaire server naar de modus alleen-lezen

   Voordat u begint met dump uit de database, moet de server worden geplaatst in de modus alleen-lezen. In de alleen-lezen-modus is de primaire kan niet worden verwerkt alle schrijftransacties. Evalueer de gevolgen voor uw bedrijf en plannen van het venster alleen-lezen in een tijdstip buiten de piekuren indien nodig.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Binair logboekbestand en offset ophalen

   Voer de [ `show master status` ](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) opdracht om te bepalen van de huidige binaire logboek-bestandsnaam en de offset.
    
   ```sql
   show master status;
   ```
   De resultaten moeten volgende. Zorg ervoor dat Let op de naam van het binaire bestand zoals deze wordt gebruikt in latere stappen.

   ![Resultaten van master-Status](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-primary-server"></a>Dump en herstellen van de primaire server

1. Alle databases van primaire server dump

   U kunt mysqldump dump databases van uw primaire. Raadpleeg voor meer informatie, [Dump & herstellen](concepts-migrate-dump-restore.md). Het is niet nodig dump MySQL-bibliotheek en testen van de bibliotheek.

2. De primaire server instellen om de modus lezen/schrijven

   Zodra de database heeft is gedumpt, wijzigt u de primaire MySQL-server terug naar de modus lezen/schrijven.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Dumpbestand terugzetten naar de nieuwe server

   Het dumpbestand terugzetten naar de server gemaakt in de Azure-Database voor de MySQL-service. Raadpleeg [Dump & herstellen](concepts-migrate-dump-restore.md) voor het herstellen van een bestand naar een MySQL-server. Als het dumpbestand groot is, kunt u deze naar een virtuele machine in Azure binnen dezelfde regio bevinden als de replicaserver uploaden. Herstellen naar de Azure-Database voor de MySQL-server van de virtuele machine.

## <a name="link-primary-and-replica-servers-to-start-data-in-replication"></a>Koppeling primaire en replicaservers gegevens in de replicatie te starten

1. De primaire server instellen

   Alle gegevens in de replicatie-functies worden uitgevoerd door de opgeslagen procedures. U vindt alle procedures op [Data opgeslagen replicatieprocedures](reference-data-in-stored-procedures.md). De opgeslagen procedures kunnen worden uitgevoerd in de MySQL-shell of MySQL-Workbench. 

   Twee servers koppelen en replicatie, meld u aan bij de doelserver van de replica in de Azure DB voor MySQL-service starten en het externe exemplaar ingesteld als de primaire server. Dit wordt gedaan met behulp van de `mysql.az_replication_change_primary` opgeslagen procedure voor de Azure DB voor MySQL-server.

   ```sql
   CALL mysql.az_replication_change_primary('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: hostnaam van de primaire server
   - master_user: gebruikersnaam voor de primaire server
   - master_password: wachtwoord voor de primaire server
   - master_log_file: bestandsnaam binair logboek wordt uitgevoerd `show master status`
   - master_log_pos: binaire logboek positie wordt uitgevoerd `show master status`
   - master_ssl_ca: context van de CA-certificaat. Als u geen gebruikmaakt van SSL doorgeven in een lege tekenreeks.
       - Het verdient aanbeveling om door te geven van deze parameter in als een variabele. Zie de volgende voorbeelden voor meer informatie.

   **Voorbeelden**

   *Replicatie met SSL*

   De variabele `@cert` wordt gemaakt met de volgende MySQL-opdrachten: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   Replicatie met SSL is tussen een primaire server gehost in het domein 'BedrijfA.com zou' en een replicaserver die wordt gehost in Azure-Database voor MySQL ingesteld. Deze opgeslagen procedure wordt uitgevoerd op de replica. 

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *Replicatie zonder SSL*

   Replicatie zonder SSL is tussen een primaire server gehost in het domein 'BedrijfA.com zou' en een replicaserver die wordt gehost in Azure-Database voor MySQL ingesteld. Deze opgeslagen procedure wordt uitgevoerd op de replica.

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

2. Replicatie starten

   Roep de `mysql.az_replication_start` opgeslagen procedure om replicatie te initiëren.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Controleer de replicatiestatus van

   Roep de [ `show slave status` ](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) opdracht op de replicaserver naar de replicatiestatus weergeven.
    
   ```sql
   show slave status;
   ```

   Als de status van `Slave_IO_Running` en `Slave_SQL_Running` zijn 'Ja' en de waarde van `Seconds_Behind_Master` '0', replicatie goed werkt. `Seconds_Behind_Master` Hiermee wordt aangegeven hoe laat de replica. Als de waarde geen is '0', betekent dit dat de replica updates wordt verwerkt. 

## <a name="other-stored-procedures"></a>Andere opgeslagen procedures

### <a name="stop-replication"></a>Stop replicatie

Als u wilt stoppen van replicatie tussen de primaire en replica-server, gebruikt u de volgende opgeslagen procedure:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Verwijder de replicatierelatie

Gebruik de volgende opgeslagen procedure voor het verwijderen van de relatie tussen de primaire en replica-server:

```sql
CALL mysql.az_replication_remove_primary;
```

### <a name="skip-replication-error"></a>Replicatiefout overslaan

Een replicatiefout overslaan en wilt doorgaan, gebruikt u de volgende opgeslagen procedure-replicatie:
    
```sql
CALL mysql.az_replication_skip_counter;
```