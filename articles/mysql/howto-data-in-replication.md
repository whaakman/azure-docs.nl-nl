---
title: Als u wilt repliceren van gegevens in Azure Database voor MySQL is de replicatie van gegevens in configureren.
description: In dit artikel wordt beschreven hoe u gegevens in de replicatie instellen voor Azure Database voor MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: 18ef70d64523bc4001fa7d9a35a7f803b8050613
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539616"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Azure Database voor MySQL-gegevens in replicatie configureren

In dit artikel leert u hoe u kunt instellen in de Azure Database for MySQL-service is de replicatie van gegevens in door de hoofd- en replica-servers te configureren. Gegevens in replicatie kunt u gegevens synchroniseren met een master MySQL-server die on-premises worden uitgevoerd in virtuele machines of database-services die door andere cloudproviders worden gehost in een replica in de Azure Database for MySQL-service. 

In dit artikel wordt ervan uitgegaan dat u ten minste enige ervaring met MySQL-servers en databases hebt.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Een MySQL-server moet worden gebruikt als replica maken

1. Maak een nieuwe Azure-Database voor MySQL-server

   Maak een nieuwe MySQL-server (ex.) "replica.mysql.database.azure.com"). Raadpleeg [maken van een Azure Database for MySQL-server met behulp van de Azure-portal](quickstart-create-mysql-server-database-using-azure-portal.md) voor het maken van de server. Deze server is de 'replica'-server in de gegevens in replicatie.

   > [!IMPORTANT]
   > De Azure Database for MySQL-server moet worden gemaakt in de Prijscategorieën voor algemeen gebruik of geoptimaliseerd voor geheugen.
   > 

2. Dezelfde gebruikersaccounts en de bijbehorende machtigingen maken

   Gebruikersaccounts worden niet gerepliceerd van de hoofd-server naar de replicaserver. Als u van plan gebruikers voorzien van toegang tot de replica-server bent, moet u handmatig maken van alle accounts en de bijbehorende machtigingen op de zojuist gemaakte Azure Database voor MySQL-server.

## <a name="configure-the-master-server"></a>De hoofd-server configureren
De volgende stappen uit voorbereiden en configureren van de MySQL-server die gehost on-premises, in een virtuele machine of de databaseservice die wordt gehost door andere cloudproviders voor replicatie van gegevens in. Deze server is de 'master' in de gegevens in replicatie. 

1. Binaire logboekregistratie inschakelen

   Controleer als binaire logboekregistratie is ingeschakeld op het model door de volgende opdracht uit: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Als de variabele [ `log_bin` ](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) wordt geretourneerd met de waarde 'aan', binaire logboekregistratie is ingeschakeld op uw server. 

   Als `log_bin` is geretourneerd met de waarde 'OFF', inschakelen in binaire registratie door het bestand my.cnf zo bewerken dat `log_bin=ON` en start opnieuw op de server voor de wijziging door te voeren.

2. Instellingen van de hoofd-server

   Gegevens in replicatie moet de parameter `lower_case_table_names` consistent tussen de hoofd- en replica-servers. Deze parameter is 1 standaard in Azure Database for MySQL. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Maak een nieuwe replicatierol en machtigingen instellen

   Maak een gebruikersaccount op de hoofd-server die is geconfigureerd met bevoegdheden voor replicatie. Dit kan worden gedaan via SQL-opdrachten of een hulpprogramma zoals MySQL Workbench. Overweeg of u van plan gerepliceerd met SSL bent, zoals dit worden opgegeven moet bij het maken van de gebruiker. Raadpleeg de MySQL-documentatie om te begrijpen hoe u [gebruikersaccounts toevoegen](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) op de hoofd-server. 

   De nieuwe replicatierol hebt gemaakt is in de onderstaande opdrachten toegang kunnen krijgen tot het model van elke machine, niet alleen de computer die als host fungeert voor het model zelf. Dit wordt gedaan door op te geven "syncuser@'%' ' in de opdracht create-gebruiker. Zie de MySQL-documentatie voor meer informatie over [op te geven accountnamen](https://dev.mysql.com/doc/refman/5.7/en/account-names.html).

   **SQL-opdracht**

   *Replicatie met SSL*

   Om SSL te vereisen voor alle verbindingen van de gebruiker, gebruik de volgende opdracht om een gebruiker te maken: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *Replicatie zonder SSL*

   Als SSL niet vereist voor alle verbindingen is, gebruikt u de volgende opdracht uit om een gebruiker te maken:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL Workbench**

   Voor het maken van de replicatierol van de in MySQL Workbench, opent u de **gebruikers en bevoegdheden** vanuit het deelvenster de **Management** deelvenster. Klik vervolgens op **-Account toevoegen**. 
 
   ![Gebruikers en machtigingen](./media/howto-data-in-replication/users_privileges.png)

   Typ de gebruikersnaam in de **aanmeldingsnaam** veld. 

   ![Gebruiker synchroniseren](./media/howto-data-in-replication/syncuser.png)
 
   Klik op de **beheerdersrollen** deelvenster en selecteer vervolgens **Replicatie secundair apparaat** uit de lijst met **globale bevoegdheden**. Klik vervolgens op **toepassen** om de replicatierol te maken.

   ![Replicatie Secundair apparaat](./media/howto-data-in-replication/replicationslave.png)


4. De hoofd-server ingesteld op modus alleen-lezen

   Voordat u begint met het dumpen van de database, moet de server worden geplaatst in de modus alleen-lezen. In de modus alleen-lezen, zich het model niet voor het verwerken van transacties die zijn geschreven. Evalueren van de impact op uw bedrijf en plannen van de alleen-lezen-venster in een tijd buiten de piektijden indien nodig.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Binaire logboekbestandsnaam en offset ophalen

   Voer de [ `show master status` ](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) opdracht uit om te bepalen van de huidige binaire logboekbestandsnaam en offset.
    
   ```sql
   show master status;
   ```
   De resultaten moeten als volgende. Zorg ervoor dat u de naam van het binaire bestand Opmerking zoals deze worden gebruikt in latere stappen.

   ![Resultaten van master-Status](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-master-server"></a>Dump maken en terugzetten van hoofd-server

1. Dump van alle databases van hoofd-server

   U kunt mysqldump dump databases van uw model. Raadpleeg voor meer informatie, [dumpen en terugzetten](concepts-migrate-dump-restore.md). Het is niet nodig dump MySQL-bibliotheek en testen van de bibliotheek.

2. Instellen van de master-server met modus lezen/schrijven

   Zodra de database heeft zijn gedumpt, wijzigt u de master MySQL-server terug naar de modus lezen/schrijven.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Bestand herstellen naar nieuwe server

   Het dumpbestand terugzetten naar de server hebt gemaakt in de Azure Database for MySQL-service. Raadpleeg [dumpen en terugzetten](concepts-migrate-dump-restore.md) voor informatie over het herstellen van een bestand naar een MySQL-server. Als het bestand groot is, kunt u deze aan een virtuele machine in Azure binnen dezelfde regio als de replicaserver uploaden. Deze herstellen met de Azure Database voor MySQL-server van de virtuele machine.

## <a name="link-master-and-replica-servers-to-start-data-in-replication"></a>Koppeling hoofd- en replica-servers gegevens in replicatie moet worden gestart

1. Set-hoofdserver

   Alle gegevens in replicatie-functies worden uitgevoerd door opgeslagen procedures. U vindt alle procedures op [gegevens in door replicatie opgeslagen Procedures](reference-data-in-stored-procedures.md). De opgeslagen procedures kunnen worden uitgevoerd in de MySQL-shell of MySQL Workbench. 

   Om te koppelen van twee servers en replicatie, meld u aan bij de doelserver van de replica in de Azure database voor MySQL-service starten en het externe exemplaar instellen als de hoofd-server. Dit wordt gedaan met behulp van de `mysql.az_replication_change_master` opgeslagen procedure uit op de Azure DB voor MySQL-server.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: hostnaam van de hoofd-server
   - master_user: de gebruikersnaam voor de hoofd-server
   - master_password: wachtwoord voor de hoofd-server
   - master_log_file: binaire logboekbestandsnaam die worden uitgevoerd `show master status`
   - master_log_pos: binaire logboekpositie die worden uitgevoerd `show master status`
   - master_ssl_ca: Context van de CA-certificaat. Als u geen gebruik van SSL, geeft u lege tekenreeks.
       - Het verdient aanbeveling om door te geven van deze parameter in als een variabele. Zie de volgende voorbeelden voor meer informatie.

> [!NOTE]
> Als de hoofd-server wordt gehost in een Azure VM, moet u instellen 'Toegang tot Azure-services toestaan' op 'Aan' om toe te staan de hoofd- en replica-servers om te communiceren met elkaar. Deze instelling kan worden gewijzigd van de **verbindingsbeveiliging** opties. Raadpleeg [firewallregels beheren met behulp van portal](howto-manage-firewall-using-portal.md) voor meer informatie.

   **Voorbeelden**

   *Replicatie met SSL*

   De variabele `@cert` worden gemaakt met de volgende opdrachten voor MySQL: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   Replicatie met SSL is ingesteld tussen een hoofd-server die wordt gehost in het domein 'BedrijfA.com zou' en een replica-server die wordt gehost in Azure Database voor MySQL. Deze opgeslagen procedure wordt uitgevoerd op de replica. 

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *Replicatie zonder SSL*

   Replicatie zonder SSL is ingesteld tussen een hoofd-server die wordt gehost in het domein 'BedrijfA.com zou' en een replica-server die wordt gehost in Azure Database voor MySQL. Deze opgeslagen procedure wordt uitgevoerd op de replica.

   ```sql
   CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

2. Replicatie starten

   Roep de `mysql.az_replication_start` opgeslagen procedure om replicatie te initiëren.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Replicatiestatus controleren

   Roep de [ `show slave status` ](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) opdracht op de replica-server om de replicatiestatus van de weer te geven.
    
   ```sql
   show slave status;
   ```

   Als de status van `Slave_IO_Running` en `Slave_SQL_Running` zijn 'Ja' en de waarde van `Seconds_Behind_Master` is '0', replicatie goed werkt. `Seconds_Behind_Master` Geeft aan hoe laat de replica is. Als de waarde geen is '0', betekent dit dat de replica is worden gebruikt voor het verwerken van updates. 

## <a name="other-stored-procedures"></a>Andere opgeslagen procedures

### <a name="stop-replication"></a>Replicatie stoppen

Als u wilt stoppen met replicatie tussen de hoofd- en replica-server, gebruikt u de volgende opgeslagen procedure:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Verwijder de replicatierelatie

Als u wilt verwijderen van de relatie tussen hoofd- en replica-server, gebruikt u de volgende opgeslagen procedure:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-replication-error"></a>Replicatiefout overslaan

Een replicatiefout overslaan en de replicatie wilt doorgaan, gebruikt u de volgende opgeslagen procedure toestaan:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Data Replication](concepts-data-in-replication.md) voor Azure Database for MySQL. 
