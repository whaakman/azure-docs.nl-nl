---
title: Gebruikers maken in Azure-Database voor de MySQL-server | Microsoft Docs
description: Dit artikel wordt beschreven hoe u nieuwe gebruikersaccounts voor interactie met een Azure-Database voor de MySQL-server kunt maken.
services: mysql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: 8adb74e11570ac60ad3b898b737cff4699f2bbf1
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>Gebruikers maken in Azure-Database voor de MySQL-server 
Dit artikel wordt beschreven hoe u gebruikers in een Azure-Database voor de MySQL-server kunt maken.

Wanneer u eerst uw Azure-Database voor MySQL gemaakt, u hebt een server admin-aanmeldingsnaam en wachtwoord opgegeven. Voor meer informatie kunt u de [Quick Start](quickstart-create-mysql-server-database-using-azure-portal.md). U kunt uw server admin aanmeldingsnaam van de Azure-portal kunt vinden.

De gebruiker server admin opgehaald bepaalde bevoegdheden voor uw server, zoals vermeld: selecteren, invoegen, bijwerken, verwijderen, maken, verwijderen, opnieuw laden, proces, verwijzingen, INDEX, ALTER, DATABASES weergeven, tijdelijke tabellen maken, VERGRENDELEN op tabellen, uitvoeren, replicatie SLAVE, replicatie CLIENT, WEERGAVE MAKEN, WEERGEVEN, MAKEN ROUTINEMATIGE, ALTER ROUTINE, TRIGGER-GEBEURTENIS-GEBRUIKER

Zodra de Azure-Database voor de MySQL-server is gemaakt, kunt u de eerste gebruikersaccount voor server-beheerder voor het maken van extra gebruikers en beheerderstoegang verlenen tot ze. De server admin-account kan ook worden gebruikt minder bevoegde gebruikers die toegang tot een individuele database, schema hebben maken.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>Aanvullende beheergebruikers maken in Azure-Database voor MySQL
1. De verbinding informatie en beheer de gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt gemakkelijk de servernaam en het aanmelden gegevens van de server vinden **overzicht** pagina of de **eigenschappen** pagina in de Azure-portal. 

2. Gebruik de beheerdersaccount en het wachtwoord verbinding maken met uw database-server. Gebruik uw voorkeur clienthulpprogramma, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u hoe u verbinding maakt weet, Zie [gebruik MySQL Workbench verbinding maakt en gegevens opvragen](./connect-workbench.md)

3. Bewerken en voer de volgende SQL-code. Uw nieuwe gebruikersnaam voor de aanduidingswaarde vervangt `new_master_user`. Deze syntaxis verleent de vermelde rechten op alle databaseschema (*.*) aan de gebruikersnaam (new_master_user in dit voorbeeld). 

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

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>Het maken van databasegebruikers in Azure-Database voor MySQL

1. De verbinding informatie en beheer de gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt gemakkelijk de servernaam en het aanmelden gegevens van de server vinden **overzicht** pagina of de **eigenschappen** pagina in de Azure-portal. 

2. Gebruik de beheerdersaccount en het wachtwoord verbinding maken met uw database-server. Gebruik uw voorkeur clienthulpprogramma, zoals MySQL Workbench, mysql.exe, HeidiSQL of anderen. 
   Als u hoe u verbinding maakt weet, Zie [gebruik MySQL Workbench verbinding maakt en gegevens opvragen](./connect-workbench.md)

3. Bewerken en voer de volgende SQL-code. Vervang de tijdelijke aanduidingswaarde `db_user` met uw beoogde nieuwe gebruikersnaam en het tijdelijke aanduidingswaarde `testdb` met de databasenaam van uw eigen.

   Deze sql-syntaxis voor code maakt een nieuwe database met de naam testdb voorbeeld. Vervolgens maakt een nieuwe gebruiker in de MySQL-service en deze alle bevoegdheden voor het schema van de nieuwe database verleent (testdb.\*) voor die gebruiker. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Controleer of de verleent in de database.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Aanmelden bij de server, de aangewezen-database met de nieuwe gebruikersnaam en wachtwoord opgeven. Dit voorbeeld toont de mysql-opdrachtregel. Met deze opdracht wordt u gevraagd om het wachtwoord voor de gebruikersnaam. Vervang uw eigen servernaam, databasenaam en gebruikersnaam.

   ```azurecli-interactive
   mysql --host myserver4demo.mysql.database.azure.com --database testdb --user db_user@myserver4demo -p
   ```

## <a name="next-steps"></a>Volgende stappen
Open de firewall voor de IP-adressen van de nieuwe gebruikers machines waarmee ze verbinding kunnen maken: [maken en beheren van Azure-Database voor firewallregels van MySQL met behulp van de Azure-portal](howto-manage-firewall-using-portal.md) of [Azure CLI](howto-manage-firewall-using-cli.md).

Zie voor meer informatie over Gebruikersaccountbeheer MySQL-productdocumentatie voor [Gebruikersaccountbeheer](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html), [GRANT syntaxis](https://dev.mysql.com/doc/refman/5.7/en/grant.html), en [bevoegdheden](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).
