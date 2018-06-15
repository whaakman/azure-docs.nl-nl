---
title: Gebruikers maken in Azure-Database voor PostgreSQL-server
description: Dit artikel wordt beschreven hoe u nieuwe gebruikersaccounts voor interactie met een Azure-Database voor PostgreSQL-server kunt maken.
services: postgresql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 87a73929185112190d5dd6698e014db225ebc08e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
ms.locfileid: "29575637"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Gebruikers maken in Azure-Database voor PostgreSQL-server 
Dit artikel wordt beschreven hoe u gebruikers in een Azure-Database voor PostgreSQL-server kunt maken.

## <a name="the-server-admin-account"></a>Het beheerdersaccount voor de server
Wanneer u eerst uw Azure-Database voor PostgreSQL gemaakt, u hebt een server admin-gebruikersnaam en wachtwoord opgegeven. Voor meer informatie kunt u de [Quick Start](quickstart-create-server-database-portal.md) om te zien van de stapsgewijze benadering. Omdat de gebruikersnaam van de server-beheerder een aangepaste naam is, kunt u vinden op de gewenste server admin gebruikersnaam van de Azure-portal.

De Azure-Database voor PostgreSQL-server is gemaakt met de 3 standaardrollen gedefinieerd. U kunt deze rollen zien door de opdracht uit te voeren: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- de gebruiker server admin

De gebruiker server admin is lid van de rol azure_pg_admin. De server admin-account is echter niet deel uit van de functie azure_superuser. Aangezien deze service een beheerde PaaS-service is, is alleen Microsoft onderdeel van de functie supergebruiker. 

De PostgreSQL-engine gebruikt bevoegdheden toegang tot de databaseobjecten, zoals beschreven in de [PostgreSQL productdocumentatie](https://www.postgresql.org/docs/current/static/sql-createrole.html). In de Azure-Database voor PostgreSQL, de gebruiker server admin deze bevoegdheden verleend: aanmelding, NOSUPERUSER, OVERNEMEN, CREATEDB, CREATEROLE, NOREPLICATION

Het gebruikersaccount voor server-beheerder kan worden gebruikt voor het maken van extra gebruikers en die gebruikers aan de rol azure_pg_admin verlenen. De server admin-account kan ook worden gebruikt minder bevoegde gebruikers en rollen die toegang tot afzonderlijke databases en schema's hebben maken.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Aanvullende beheergebruikers maken in Azure-Database voor PostgreSQL
1. De verbinding informatie en beheer de gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt gemakkelijk de servernaam en het aanmelden gegevens van de server vinden **overzicht** pagina of de **eigenschappen** pagina in de Azure-portal. 

2. Gebruik de beheerdersaccount en het wachtwoord verbinding maken met uw database-server. Gebruik uw voorkeur clienthulpprogramma, zoals pgAdmin of psql.
   Als u hoe u verbinding maakt weet, Zie [verbinding maken met de PostgreSQL-Database met behulp van psql in de Cloud-Shell](./quickstart-create-server-database-portal.md#connect-to-the-postgresql-database-by-using-psql-in-cloud-shell)

3. Bewerken en voer de volgende SQL-code. Vervang uw nieuwe gebruikersnaam voor de tijdelijke aanduidingswaarde < new_user > en het wachtwoord van de tijdelijke aanduiding vervangt door uw eigen sterk wachtwoord. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Het maken van databasegebruikers in Azure-Database voor PostgreSQL

1. De verbinding informatie en beheer de gebruikersnaam niet ophalen.
   Voor verbinding met uw databaseserver moet u beschikken over de volledige servernaam en aanmeldingsreferenties van de beheerder. U kunt gemakkelijk de servernaam en het aanmelden gegevens van de server vinden **overzicht** pagina of de **eigenschappen** pagina in de Azure-portal. 

2. Gebruik de beheerdersaccount en het wachtwoord verbinding maken met uw database-server. Gebruik uw voorkeur clienthulpprogramma, zoals pgAdmin of psql.

3. Bewerken en voer de volgende SQL-code. Vervang de tijdelijke aanduidingswaarde `<db_user>` met uw beoogde nieuwe gebruikersnaam en het tijdelijke aanduidingswaarde `<newdb>` met de databasenaam van uw eigen. Vervang het wachtwoord van de tijdelijke aanduiding met uw eigen sterk wachtwoord. 

   Deze sql-syntaxis voor code maakt een nieuwe database met de naam testdb, voorbeeld. Deze maakt een nieuwe gebruiker in de PostgreSQL-service en verleent rechten verbinding met de nieuwe database voor die gebruiker. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE testdb TO <db_user>;
   ```

4. Met behulp van een beheerdersaccount, wellicht moet u extra rechten voor het beveiligen van de objecten in de database. Raadpleeg de [PostgreSQL documentatie](https://www.postgresql.org/docs/current/static/ddl-priv.html) voor meer informatie over databaserollen en bevoegdheden. Bijvoorbeeld: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Aanmelden bij uw server, de aangewezen-database met de nieuwe gebruikersnaam en wachtwoord opgeven. Dit voorbeeld toont de psql vanaf de opdrachtregel. Met deze opdracht wordt u gevraagd om het wachtwoord voor de gebruikersnaam. Vervang uw eigen servernaam, databasenaam en gebruikersnaam.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Volgende stappen
Open de firewall voor de IP-adressen van de nieuwe gebruikers machines waarmee ze verbinding kunnen maken: [maken en beheren van Azure Database voor firewallregels PostgreSQL met behulp van de Azure-portal](howto-manage-firewall-using-portal.md) of [Azure CLI](howto-manage-firewall-using-cli.md).

Zie voor meer informatie over Gebruikersaccountbeheer PostgreSQL-productdocumentatie voor [databaserollen en bevoegdheden](https://www.postgresql.org/docs/current/static/user-manag.html), [GRANT syntaxis](https://www.postgresql.org/docs/current/static/sql-grant.html), en [bevoegdheden](https://www.postgresql.org/docs/current/static/ddl-priv.html).
