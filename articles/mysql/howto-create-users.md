---
title: Gebruikers maken in Azure Database voor MySQL-server
description: Dit artikel wordt beschreven hoe u nieuwe gebruikersaccounts om te communiceren met een Azure Database for MySQL-server kunt maken.
author: jasonwhowell
ms.author: jasonh
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: e8714777b1f9f08de4d02fcb44c25197cdc48899
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53546008"
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>Gebruikers maken in Azure Database voor MySQL-server 
Dit artikel wordt beschreven hoe u gebruikers in een Azure Database for MySQL-server kunt maken.

Wanneer u eerst uw Azure Database for MySQL hebt gemaakt, u hebt een gebruiker van de serverbeheerder en het wachtwoord opgegeven. Voor meer informatie kunt u volgen de [snelstartgids](quickstart-create-mysql-server-database-using-azure-portal.md). U vindt de gebruiker aanmeldnamen van serverbeheerder vanuit Azure portal.

De gebruiker server admin wordt bepaalde bevoegdheden voor de server die worden beschreven: SELECTEREN, INVOEGEN, BIJWERKEN, VERWIJDEREN, MAKEN, VERWIJDEREN, OPNIEUW LADEN, VERWERKEN, VERWIJZINGEN, INDEX, ALTER, DATABASES, GEVEN TIJDELIJKE TABELLEN MAKEN, VERGRENDELEN VAN TABELLEN, WORDEN UITGEVOERD, HET MAKEN VAN SLAVE-REPLICATIE, REPLICATIE-CLIENT WEERGEVEN, WEERGEVEN, MAKEN ROUTINEMATIGE, ROUTINE WIJZIGEN, GEBRUIKER MAKEN , GEBEURTENIS, TRIGGER

Nadat de Azure Database for MySQL-server is gemaakt, kunt u het eerste gebruikersaccount van de server-beheerder kunt gebruiken voor het maken van extra gebruikers en beheerderstoegang verlenen. Het serverbeheerdersaccount kan ook worden gebruikt om minder bevoegde gebruikers die toegang tot afzonderlijke databaseschema hebben te maken.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>Hoe u extra beheergebruikers maken in Azure Database for MySQL
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

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>Databasegebruikers in Azure Database voor MySQL maken

1. De verbinding informatie en beheerder gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig vinden voor de servernaam en aanmeldingsgegevens informatie van de server **overzicht** pagina of het **eigenschappen** pagina in de Azure portal. 

2. De beheerdersaccount en het wachtwoord om verbinding met uw database-server te gebruiken. Gebruik uw favoriete clienthulpprogramma, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u niet hoe u verbinding maakt weet, Zie [gebruik MySQL Workbench verbinding maken en gegevens op te vragen](./connect-workbench.md)

3. Bewerken en voer de volgende SQL-code. Vervang de tijdelijke aanduidingswaarde `db_user` met uw beoogde nieuwe gebruikersnaam en een tijdelijke aanduiding `testdb` met de databasenaam van uw eigen.

   De syntaxis van deze sql-code maakt een nieuwe database met de naam testdb als voorbeeld. Vervolgens maakt een nieuwe gebruiker in de MySQL-service en alle bevoegdheden naar het nieuwe databaseschema verleent (testdb.\*) voor die gebruiker. 

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

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```

## <a name="next-steps"></a>Volgende stappen
Open de firewall voor IP-adressen van de nieuwe gebruikers machines waarmee ze verbinding kunnen maken: [Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van de Azure-portal](howto-manage-firewall-using-portal.md) of [Azure CLI](howto-manage-firewall-using-cli.md).

Zie voor meer informatie met betrekking tot beheer van gebruikersaccounts, MySQL-productdocumentatie voor [Gebruikersaccountbeheer](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html), [verlenen syntaxis](https://dev.mysql.com/doc/refman/5.7/en/grant.html), en [bevoegdheden](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).
