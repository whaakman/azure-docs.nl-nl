---
title: Replicatie van gegevens in op Azure Database voor MariaDB configureren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gegevens in de replicatie instellen in Azure Database voor MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 21e8a88cc6f03b4d54a6c5299b0b6be36cc32d6d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444792"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>Replicatie van gegevens in op Azure Database voor MariaDB configureren

In dit artikel wordt beschreven hoe u gegevens in replicatie instellen in Azure Database voor MariaDB door de hoofd- en replica-servers te configureren. In dit artikel wordt ervan uitgegaan dat u enige ervaring met MariaDB-servers en databases hebt.

Voor het maken van een replica in de Azure Database voor MariaDB-service, synchroniseert gegevens in replicatie de gegevens van een master MariaDB server on-premises, in virtuele machines (VM's), of in de database van cloudservices.

> [!NOTE]
> Als de hoofd-server versie 10.2 of hoger, raden wij aan dat u gegevens in replicatie instellen met behulp van [globale transactie-ID](https://mariadb.com/kb/en/library/gtid/).


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Een MariaDB-server moet worden gebruikt als een replica maken

1. Maak een nieuwe Azure-Database voor MariaDB-server (bijvoorbeeld replica.mariadb.database.azure.com). De server is de replica-server in de gegevens in replicatie.

    Zie voor meer informatie over het maken van de server [een Azure Database voor MariaDB-server maken met behulp van de Azure-portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > U moet de Azure Database voor MariaDB-server maken in de Prijscategorieën voor algemeen gebruik of geoptimaliseerd voor geheugen.

2. Maak identieke gebruikersaccounts en de bijbehorende bevoegdheden.
    
    Gebruikersaccounts worden niet gerepliceerd van de hoofd-server naar de replica-server. Voor toegang tot de replica-server voor gebruikers, moet u handmatig alle accounts en de bijbehorende machtigingen op de zojuist gemaakte Azure-Database voor MariaDB-server maken.

## <a name="configure-the-master-server"></a>De hoofd-server configureren

De volgende stappen voorbereiden en configureren van de MariaDB-server die gehost on-premises, in een virtuele machine of in een cloud-database-service voor replicatie van gegevens in. De MariaDB-server is het model in de gegevens in replicatie.

1. Binaire logboekregistratie inschakelen.
    
    Als u wilt zien als binaire logboekregistratie is ingeschakeld op de hoofddoelserver, voer de volgende opdracht:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Als de variabele [ `log_bin` ](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) retourneert de waarde `ON`, binaire logboekregistratie is ingeschakeld op uw server.

   Als `log_bin` retourneert de waarde `OFF`, bewerk de **my.cnf** bestand zodat `log_bin=ON` Hiermee schakelt u binaire registratie. De server om de wijziging van kracht opnieuw opstarten.

2. Hoofd-server-instellingen configureren.

    Gegevens in replicatie moet de parameter `lower_case_table_names` consistent tussen de hoofd- en replica-servers. De `lower_case_table_names` parameter is ingesteld op `1` standaard in Azure Database voor MariaDB.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Maak een nieuwe replicatierol en machtigingen instellen.

   Maak een gebruikersaccount op de hoofd-server die geconfigureerd met bevoegdheden voor replicatie. U kunt een account maken met behulp van SQL-opdrachten of MySQL Workbench. Als u van plan bent om te repliceren met SSL, moet u dit opgeven wanneer u het gebruikersaccount maakt.
   
   Zie voor informatie over het toevoegen van gebruikersaccounts op de hoofd-server, de [MariaDB documentatie](https://mariadb.com/kb/en/library/create-user/).

   Met behulp van de volgende opdrachten, de nieuwe replicatierol toegang tot het model van elke machine, niet alleen de computer die als host fungeert voor het model zelf. Geef voor deze toegang **syncuser\@'%'** in de opdracht om een gebruiker te maken.
   
   Zie voor meer informatie over MariaDB-documentatie, [op te geven accountnamen](https://mariadb.com/kb/en/library/create-user/#account-names).

   **SQL-opdracht**

   - Replicatie met SSL

       Om SSL te vereisen voor alle verbindingen van de gebruiker, voer de volgende opdracht om een gebruiker te maken:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - Replicatie zonder SSL

       Als SSL is niet vereist voor alle verbindingen, voert u de volgende opdracht uit om een gebruiker te maken:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   Maken van de replicatie-functie in MySQL Workbench, in de **Management** venster **gebruikers en bevoegdheden**. Selecteer vervolgens **-Account toevoegen**.
 
   ![Gebruikers en machtigingen](./media/howto-data-in-replication/users_privileges.png)

   Voer een gebruikersnaam in de **aanmeldingsnaam** veld.

   ![Gebruiker synchroniseren](./media/howto-data-in-replication/syncuser.png)
 
   Selecteer de **beheerdersrollen** deelvenster en klik vervolgens in de lijst met **globale bevoegdheden**, selecteer **Slave-replicatie**. Selecteer **toepassen** om de replicatierol te maken.

   ![Replicatie Secundair apparaat](./media/howto-data-in-replication/replicationslave.png)


4. Stel de hoofd-server naar de modus alleen-lezen.

   Voordat u een database dumpen, moet de server worden geplaatst in de modus alleen-lezen. Het model kan in de modus alleen-lezen, schrijven transacties niet verwerken. Om te voorkomen impact op bedrijf, het kenmerk alleen-lezen-venster te plannen tijdens een tijdstip buiten de piekuren.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Verkrijg de huidige binaire logboekbestandsnaam en offset.

   Voer de opdracht uit om te bepalen van de huidige binaire logboekbestandsnaam en offset, [ `show master status` ](https://mariadb.com/kb/en/library/show-master-status/).
    
   ```sql
   show master status;
   ```
   De resultaten moeten vergelijkbaar met de volgende tabel:
   
   ![Resultaten van master-Status](./media/howto-data-in-replication/masterstatus.png)

   Noteer de naam van het binaire bestand, omdat deze in latere stappen moet worden gebruikt.
   
6. De positie GTID ophalen (optioneel, nodig zijn voor de replicatie met GTID).

   De functie uitvoeren [ `BINLOG_GTID_POS` ](https://mariadb.com/kb/en/library/binlog_gtid_pos/) om op te halen van de positie GTID voor de bijbehorende binlog-bestandsnaam en offset.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Dump maken en terugzetten van de hoofd-server

1. Alle databases van de hoofdserver dump.

   Mysqldump gebruiken voor het dumpen van alle databases van de hoofd-server. Het is niet nodig voor het dumpen van de MySQL-bibliotheek en bibliotheek testen.

    Zie voor meer informatie, [dumpen en terugzetten](howto-migrate-dump-restore.md).

2. Stel de hoofd-server naar de modus lezen/schrijven.

   Nadat de database heeft zijn gedumpt, wijzigt u de master MariaDB server terug naar de modus lezen/schrijven.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Het dumpbestand herstellen naar de nieuwe server.

   Het dumpbestand terugzetten naar de server hebt gemaakt in de Azure Database voor MariaDB-service. Zie [dumpen en terugzetten](howto-migrate-dump-restore.md) voor informatie over het herstellen van een bestand naar een MariaDB-server.

   Als het bestand groot is, uploadt u deze aan een virtuele machine in Azure binnen dezelfde regio als de replicaserver. Deze herstellen met de Azure Database voor MariaDB-server van de virtuele machine.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>De hoofd- en replica-servers voor het starten van replicatie van gegevens in een koppeling

1. Stel de hoofd-server.

   Alle gegevens in replicatie-functies worden uitgevoerd door opgeslagen procedures. U vindt alle procedures op [gegevens in door replicatie opgeslagen Procedures](reference-data-in-stored-procedures.md). Opgeslagen procedures kunnen worden uitgevoerd in de MySQL-shell of MySQL Workbench.

   Om te koppelen van twee servers en replicatie starten, moet u zich aanmelden bij de doelserver van de replica in de Azure database voor MariaDB-service. Vervolgens stelt het externe exemplaar als de hoofd-server met behulp van de `mysql.az_replication_change_master` of `mysql.az_replication_change_master_with_gtid` opgeslagen procedure uit op de Azure database voor MariaDB-server.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   of
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: hostnaam van de hoofd-server
   - master_user: de gebruikersnaam voor de hoofd-server
   - master_password: wachtwoord voor de hoofd-server
   - master_log_file: binaire logboekbestandsnaam die worden uitgevoerd `show master status`
   - master_log_pos: binaire logboekpositie die worden uitgevoerd `show master status`
   - master_gtid_pos: GTID positie die worden uitgevoerd `select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: Context van de CA-certificaat. Als u SSL niet gebruikt, in een lege string.* doorgeven
    
    
    \* We raden aan te geven in de parameter master_ssl_ca als een variabele. Zie voor meer informatie de volgende voorbeelden.

   **Voorbeelden**

   - Replicatie met SSL

       Maak de variabele `@cert` door het uitvoeren van de volgende opdrachten:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       Replicatie met SSL is ingesteld tussen een hoofd-server die wordt gehost in het domein BedrijfA.com zou en een replica-server die wordt gehost in Azure Database voor MariaDB. Deze opgeslagen procedure wordt uitgevoerd op de replica.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - Replicatie zonder SSL

       Replicatie zonder SSL is ingesteld tussen een hoofd-server die wordt gehost in het domein BedrijfA.com zou en een replica-server die wordt gehost in Azure Database voor MariaDB. Deze opgeslagen procedure wordt uitgevoerd op de replica.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Replicatie starten.

   Roep de `mysql.az_replication_start` opgeslagen procedure replicatie moet worden gestart.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Controleer de replicatiestatus.

   Roep de [ `show slave status` ](https://mariadb.com/kb/en/library/show-slave-status/) opdracht op de replica-server om de replicatiestatus van de weer te geven.
    
   ```sql
   show slave status;
   ```

   Als `Slave_IO_Running` en `Slave_SQL_Running` zijn in de status `yes`, en de waarde van `Seconds_Behind_Master` is `0`, replicatie werkt. `Seconds_Behind_Master` Geeft aan hoe laat de replica is. Als de waarde niet `0`, en vervolgens de replica is updates worden verwerkt.

4. Bijwerken van de bijbehorende servervariabelen om gegevens in replicatie veiliger (alleen vereist voor replicatie zonder GTID).
    
    Vanwege een beperking van de systeemeigen replicatie in MariaDB, moet u instellen [ `sync_master_info` ](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) en [ `sync_relay_log_info` ](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) variabelen op replicatie zonder het GTID-scenario.

    De slave-server controleren `sync_master_info` en `sync_relay_log_info` variabelen om ervoor te zorgen dat de gegevens in replicatie is stabiel en stelt u de variabelen `1`.
    
## <a name="other-stored-procedures"></a>Andere opgeslagen procedures

### <a name="stop-replication"></a>Replicatie stoppen

Als u wilt stoppen met replicatie tussen de hoofd- en replica-server, gebruikt u de volgende opgeslagen procedure:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Verwijder de replicatierelatie

Als u wilt verwijderen van de relatie tussen de hoofd- en replica-server, gebruikt u de volgende opgeslagen procedure:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Overslaan van de Replicatiefout

Voor het overslaan van een fout bij de replicatie en replicatie is toegestaan, gebruikt u de volgende opgeslagen procedure:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Data Replication](concepts-data-in-replication.md) voor Azure Database voor MariaDB.
