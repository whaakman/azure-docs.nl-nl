---
title: Gebruikers maken in Azure Database voor MariaDB-server
description: Dit artikel wordt beschreven hoe u nieuwe gebruikersaccounts om te communiceren met een Azure Database voor MariaDB-server kunt maken.
author: jasonwhowell
ms.author: jasonh
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 189e122e04d56d28c1e1e94d328569647614a124
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542124"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>Gebruikers maken in Azure Database voor MariaDB 
Dit artikel wordt beschreven hoe u gebruikers kunt maken in Azure Database voor MariaDB.

Wanneer u eerst uw Azure Database voor MariaDB hebt gemaakt, u hebt een gebruiker van de serverbeheerder en het wachtwoord opgegeven. Voor meer informatie kunt u volgen de [snelstartgids](quickstart-create-mariadb-server-database-using-azure-portal.md). U vindt de gebruiker aanmeldnamen van serverbeheerder vanuit Azure portal.

De gebruiker server admin wordt bepaalde bevoegdheden voor de server die worden beschreven: SELECTEREN, INVOEGEN, BIJWERKEN, VERWIJDEREN, MAKEN, VERWIJDEREN, OPNIEUW LADEN, VERWERKEN, VERWIJZINGEN, INDEX, ALTER, DATABASES, GEVEN TIJDELIJKE TABELLEN MAKEN, VERGRENDELEN VAN TABELLEN, WORDEN UITGEVOERD, HET MAKEN VAN SLAVE-REPLICATIE, REPLICATIE-CLIENT WEERGEVEN, WEERGEVEN, MAKEN ROUTINEMATIGE, ROUTINE WIJZIGEN, GEBRUIKER MAKEN , GEBEURTENIS, TRIGGER

Nadat de Azure Database voor MariaDB-server is gemaakt, kunt u het eerste gebruikersaccount van de server-beheerder kunt gebruiken voor het maken van extra gebruikers en beheerderstoegang verlenen. Het serverbeheerdersaccount kan ook worden gebruikt om minder bevoegde gebruikers die toegang tot afzonderlijke databaseschema hebben te maken.

## <a name="create-additional-admin-users"></a>Aanvullende beheergebruikers maken
1. De verbinding informatie en beheerder gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig vinden voor de servernaam en aanmeldingsgegevens informatie van de server **overzicht** pagina of het **eigenschappen** pagina in de Azure portal. 

2. De beheerdersaccount en het wachtwoord om verbinding met uw database-server te gebruiken. Gebruik uw favoriete clienthulpprogramma, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u niet hoe u verbinding maakt weet, Zie [gebruik MySQL Workbench verbinding maken en gegevens op te vragen](./connect-workbench.md)

3. Bewerken en voer de volgende SQL-code. Vervang de gebruikersnaam van uw nieuwe voor de tijdelijke aanduidingswaarde `new_master_user`. Deze syntaxis geeft de vermelde machtigingen op alle schema's van de database (*.*) aan de gebruikersnaam (new_master_user in dit voorbeeld). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Controleer of de verleent 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Databasegebruikers maken

1. De verbinding informatie en beheerder gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig vinden voor de servernaam en aanmeldingsgegevens informatie van de server **overzicht** pagina of het **eigenschappen** pagina in de Azure portal. 

2. De beheerdersaccount en het wachtwoord om verbinding met uw database-server te gebruiken. Gebruik uw favoriete clienthulpprogramma, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u niet hoe u verbinding maakt weet, Zie [gebruik MySQL Workbench verbinding maken en gegevens op te vragen](./connect-workbench.md)

3. Bewerken en voer de volgende SQL-code. Vervang de tijdelijke aanduidingswaarde `db_user` met uw beoogde nieuwe gebruikersnaam en een tijdelijke aanduiding `testdb` met de databasenaam van uw eigen.

   De syntaxis van deze sql-code maakt een nieuwe database met de naam testdb als voorbeeld. Vervolgens maakt een nieuwe gebruiker in de Azure Database voor MariaDB-service en alle bevoegdheden naar het nieuwe databaseschema verleent (testdb.\*) voor die gebruiker. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Controleer of de verleent binnen de database.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Meld u aan bij de server, de aangewezen database, met behulp van de nieuwe gebruikersnaam en het wachtwoord op te geven. Dit voorbeeld toont de mysql-opdrachtregel. Met deze opdracht wordt u gevraagd om het wachtwoord voor de naam van de gebruiker. Vervangen door uw eigen servernaam, databasenaam en gebruikersnaam.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
Zie voor meer informatie over het beheer van gebruikersaccounts, MariaDB-documentatie voor [Gebruikersaccountbeheer](https://mariadb.com/kb/en/library/user-account-management/), [verlenen syntaxis](https://mariadb.com/kb/en/library/grant/), en [bevoegdheden](https://mariadb.com/kb/en/library/grant/#privilege-levels).

## <a name="next-steps"></a>Volgende stappen
Open de firewall voor IP-adressen van de nieuwe gebruikers machines waarmee ze verbinding kunnen maken: [Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van Azure portal](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->
