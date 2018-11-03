---
title: Gebruikers maken in Azure Database for PostgreSQL-server
description: Dit artikel wordt beschreven hoe u nieuwe gebruikersaccounts om te communiceren met een Azure Database for PostgreSQL-server kunt maken.
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/16/2018
ms.openlocfilehash: 45d1f55e60763724aeb1b1685c5c69696475a424
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958097"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Gebruikers maken in Azure Database for PostgreSQL-server 
Dit artikel wordt beschreven hoe u gebruikers in een Azure Database for PostgreSQL-server kunt maken.

## <a name="the-server-admin-account"></a>Het beheerdersaccount voor de server
Wanneer u eerst uw Azure Database for PostgreSQL hebt gemaakt, u hebt een server admin-gebruikersnaam en wachtwoord opgegeven. Voor meer informatie kunt u volgen de [snelstartgids](quickstart-create-server-database-portal.md) om te zien van de stapsgewijze benadering. Aangezien de gebruikersnaam van de server-beheerder een aangepaste naam is, kunt u de gebruikersnaam van de gekozen server admin vanuit Azure portal kunt vinden.

De Azure Database for PostgreSQL-server wordt gemaakt met het 3 standaardrollen gedefinieerd. U kunt deze rollen zien door met de opdracht: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- de gebruiker met beheerdersrechten van uw server

De gebruiker met beheerdersrechten van uw server is lid van de rol azure_pg_admin. Het serverbeheerdersaccount is echter geen onderdeel van de rol azure_superuser. Aangezien deze service een beheerde PaaS-service is, is alleen Microsoft onderdeel van de rol van supergebruiker. 

De PostgreSQL-engine maakt gebruik van bevoegdheden voor het beheren van toegang tot de database-objecten, zoals beschreven in de [PostgreSQL-productdocumentatie](https://www.postgresql.org/docs/current/static/sql-createrole.html). In Azure Database for PostgreSQL, de server-beheerder deze bevoegdheden verleend: aanmelding, NOSUPERUSER, OVERNEMEN, CREATEDB, CREATEROLE, NOREPLICATION

De gebruiker van het serverbeheerdersaccount kan worden gebruikt voor het maken van extra gebruikers en die gebruikers aan de rol azure_pg_admin verlenen. Het serverbeheerdersaccount kan ook worden gebruikt om minder bevoegde gebruikers en rollen die toegang tot afzonderlijke databases en schema's hebben te maken.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Hoe u extra beheerders in Azure Database for PostgreSQL maken
1. De verbinding informatie en beheerder gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig vinden voor de servernaam en aanmeldingsgegevens informatie van de server **overzicht** pagina of het **eigenschappen** pagina in de Azure portal. 

2. De beheerdersaccount en het wachtwoord om verbinding met uw database-server te gebruiken. Gebruik uw favoriete clienthulpprogramma, zoals pgAdmin of psql.
   Als u niet hoe u verbinding maakt weet, Zie [de Snelstartgids](./quickstart-create-server-database-portal.md)

3. Bewerken en voer de volgende SQL-code. Vervang de gebruikersnaam van uw nieuwe voor de tijdelijke aanduiding < new_user > en vervang het wachtwoord van de tijdelijke aanduiding door uw eigen sterk wachtwoord. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Over het maken van databasegebruikers in Azure Database voor PostgreSQL

1. De verbinding informatie en beheerder gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt eenvoudig vinden voor de servernaam en aanmeldingsgegevens informatie van de server **overzicht** pagina of het **eigenschappen** pagina in de Azure portal. 

2. De beheerdersaccount en het wachtwoord om verbinding met uw database-server te gebruiken. Gebruik uw favoriete clienthulpprogramma, zoals pgAdmin of psql.

3. Bewerken en voer de volgende SQL-code. Vervang de tijdelijke aanduidingswaarde `<db_user>` met uw beoogde nieuwe gebruikersnaam en een tijdelijke aanduiding `<newdb>` met de databasenaam van uw eigen. Vervang het wachtwoord van de tijdelijke aanduiding door uw eigen sterk wachtwoord. 

   De syntaxis van deze sql-code maakt een nieuwe database met de naam testdb, als voorbeeld. Het maakt vervolgens een nieuwe gebruiker in de PostgreSQL-service en verleent rechten verbinding met de nieuwe database voor die gebruiker. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE <newdb> TO <db_user>;
   ```

4. Met een beheerdersaccount, wellicht u extra rechten voor het beveiligen van de objecten in de database. Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/current/static/ddl-priv.html) voor meer informatie over databaserollen en bevoegdheden. Bijvoorbeeld: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Meld u aan bij uw server, de aangewezen database, met behulp van de nieuwe gebruikersnaam en het wachtwoord op te geven. In dit voorbeeld ziet de psql-opdrachtregel. Met deze opdracht wordt u gevraagd om het wachtwoord voor de naam van de gebruiker. Vervangen door uw eigen servernaam, databasenaam en gebruikersnaam.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Volgende stappen
De firewall voor IP-adressen van de nieuwe gebruikers-machines in te schakelen om verbinding te openen: [maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van de Azure-portal](howto-manage-firewall-using-portal.md) of [Azure CLI](howto-manage-firewall-using-cli.md).

Zie voor meer informatie met betrekking tot beheer van gebruikersaccounts, PostgreSQL-productdocumentatie voor [databaserollen en bevoegdheden](https://www.postgresql.org/docs/current/static/user-manag.html), [verlenen syntaxis](https://www.postgresql.org/docs/current/static/sql-grant.html), en [bevoegdheden](https://www.postgresql.org/docs/current/static/ddl-priv.html).
