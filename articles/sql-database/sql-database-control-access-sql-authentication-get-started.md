---
title: 'SQL-authenticatie: firewalls, verificatie en toegang in Azure SQL Database | Microsoft-documenten'
description: In deze aan de slag-zelfstudie leert u hoe u SQL Server Management Studio en Transact-SQL kunt gebruiken om te werken met firewallregels op server- en databaseniveau, SQL-verificatie, aanmeldingen, gebruikers en rollen voor toegang tot en beheer van Azure SQL Database-servers en Azure SQL-databases.
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: c70b3b23fa95af6614c34bd951943f0559409220
ms.openlocfilehash: cf43790c329ef156ae17579d2281c861533ec201


---
# <a name="sql-server-authentication-access-and-database-level-firewall-rules"></a>SQL Server-verificatie, toegang en firewallregels op databaseniveau

In deze zelfstudie leert u hoe u SQL Server Management Studio kunt gebruiken om te werken met SQL Server-verificatie, aanmeldingen, gebruikers en databaserollen voor toegang tot en machtigingen voor Azure SQL Database-servers en -databases. Wanneer u deze zelfstudie hebt voltooid, kunt u:

- Aanmeldingen en gebruikers maken op basis van SQL Server-verificatie
- Gebruikers toevoegen aan rollen en machtigingen toewijzen aan rollen
- T-SQL gebruiken om firewallregels op databaseniveau en serverniveau te maken 
- Als gebruiker via SSMS verbinding maken met een specifieke database
- Gebruikersmachtigingen in de hoofddatabase en in gebruikersdatabases bekijken

**Geschatte tijd**: deze zelfstudie duurt circa 45 minuten. (We gaan er hierbij vanuit dat u al aan de vereisten voldoet.)

> [!NOTE]
> Deze zelfstudie biedt informatie over deze onderwerpen: [SQL Database-toegang en -controle](sql-database-control-access.md), [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md), [Principals](https://msdn.microsoft.com/library/ms181127.aspx), [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) en [SQL Database-firewallregels](sql-database-firewall-configure.md). Zie [Aan de slag met Azure AD-verificatie](sql-database-control-access-aad-authentication-get-started.md) voor een zelfstudie over Azure Active Directory-verificatie.
>  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-account**. U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](https://azure.microsoft.com/free/) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure-machtigingen voor maken**. U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* **SQL Server Management Studio**. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) voor het downloaden en installeren van de meest recente versie van SQL Server Management Studio (SSMS). Gebruik altijd de nieuwste versie van SSMS bij het verbinden met Azure SQL Database; er worden namelijk voortdurend nieuwe mogelijkheden geïntroduceerd.

* **U hebt de basiszelfstudie voltooid**. U hebt de zelfstudie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio](sql-database-get-started.md) of de equivalente [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie voltooid. Als dit niet het geval is, voltooit u eerst deze vereiste zelfstudie of voert u het PowerShell-script aan het einde van de [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie uit voordat u doorgaat.



## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Aanmelden bij Azure Portal met uw Azure-account
De stappen in deze procedure bevatten informatie over het verbinden met Azure Portal middels uw Azure-account](https://account.windowsazure.com/Home/Index).

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-logical-server-security-information-in-the-azure-portal"></a>Beveiligingsinformatie over een logische server bekijken in Azure Portal

Via de stappen in deze procedure komt u te weten hoe u in Azure Portal informatie bekijkt over de beveiligingsconfiguratie van uw logische server.

1. Open de blade **SQL Server** voor de server en bekijk de informatie op de pagina **Overzicht**.

   ![Serverbeheerdersaccount in Azure Portal](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Noteer de naam van de serverbeheerder van uw logische server. 

3. Als u het wachtwoord niet meer weet, klikt u op **Wachtwoord opnieuw instellen** om een nieuw wachtwoord in te stellen.

4. Als u de verbindingsinformatie van de server nodig hebt, klikt u op **Eigenschappen**.

## <a name="view-server-admin-permissions-using-ssms"></a>De machtigingen van de serverbeheerder bekijken via SSMS

Via de stappen in deze procedure komt u te weten hoe u informatie bekijkt over het serverbeheerdersaccount en de bijbehorende machtigingen in de hoofddatabase en in gebruikersdatabases.

1. Open SQL Server Management Studio en maak verbinding met uw server als de serverbeheerder. Gebruik daarbij SQL Server-verificatie en het serverbeheerdersaccount.

   ![verbinding maken met server](./media/sql-database-get-started/connect-to-server.png)

2. Klik op **Verbinden**.

   ![verbonden met server](./media/sql-database-get-started/connected-to-server.png)

3. In Objectverkenner vouwt u **Beveiliging** uit en vouwt u daarna **Aanmeldingen** uit om de bestaande accounts te bekijken die er bestaan voor uw server. Het enige account dat bestaat bij een nieuwe server is het account van de serverbeheerder.

   ![Aanmeldgegevens van serverbeheerder](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

4. In Objectverkenner vouwt u opeenvolgend **Databases**, **Systeemdatabases**, **Hoofddatabase** en **Beveiliging** uit. Vouw vervolgens **Gebruikers** uit om het gebruikersaccount te bekijken dat in deze database is gemaakt voor de serverbeheerder.

   ![Gebruikersaccount voor de serverbeheerder in de hoofddatabase](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor informatie over de andere gebruikersaccounts die worden weergegeven in het knooppunt Gebruikers.
   >

5. Klik in Objectverkenner met de rechtermuisknop op **hoofd** en klik vervolgens op **Nieuwe query** om een queryvenster te openen dat is verbonden met de hoofddatabase.
6. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert. 

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de hoofddatabase](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

7. Voer in het queryvenster de volgende query uit om informatie te verkrijgen over de machtigingen van de sqladmin-gebruiker in de **hoofd**database. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals p
   JOIN sys.database_permissions prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'sqladmin';
   ```

   ![machtigingen voor serverbeheer in de hoofddatabase](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_master_database.png)

   >[!NOTE]
   > De serverbeheerder is gemachtigd om verbinding te maken met de hoofddatabase, om aanmeldingen en gebruikers te maken, om gegevens uit de tabel .sql_logins te selecteren en om gebruikers toe te voegen aan de databaserollen dbmanager en dbcreator. Dit zijn extra machtigingen, naast de machtigingen die zijn toegewezen aan de openbare rol waarvan alle gebruikers machtigingen overnemen (zoals machtigingen voor het selecteren van gegevens uit bepaalde tabellen). Zie [machtigingen](https://msdn.microsoft.com/library/ms191291.aspx) voor meer informatie.
   >

8. In Objectverkenner vouwt u opeenvolgend **blankdb**, **Beveiliging** en **Gebruikers** uit om het gebruikersaccount te bekijken dat in deze database (en andere gebruikersdatabases) is gemaakt voor de serverbeheerder.

   ![gebruikersaccounts in blankdb](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

9. Klik in Objectverkenner met de rechtermuisknop op **blankdb** en klik vervolgens op **Nieuwe query**.

10. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert.

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de blankdb-database](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

11. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van de dbo-gebruiker. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'dbo';
   ```

   ![machtigingen voor serverbeheer in de blankdb-database](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_blankdb_database.png)

   > [!NOTE]
   > De dbo-gebruikers is lid van de openbare rol en is ook lid van de vaste databaserol db_owner. Zie [Rollen op databaseniveau](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie.
   >

## <a name="create-a-new-user-with-select-permissions"></a>Een nieuwe gebruiker maken met SELECT-machtigingen

Via de stappen in deze procedure leert u hoe u een gebruiker op databaseniveau maakt, hoe u de standaardmachtigingen van een nieuwe gebruiker test (via de openbare rol), hoe u een gebruiker **SELECT**-machtigingen geeft en hoe u deze gewijzigde machtigingen bekijkt.

> [!NOTE]
> Uw gebruikers op databaseniveau worden ook wel [ingesloten gebruikers](https://msdn.microsoft.com/library/ff929188.aspx) genoemd en vergroten de draagbaarheid van uw database. Zie [De Azure SQL Database-beveiliging configureren n beheren voor geografisch herstel en failovers naar een secundaire server](sql-database-geo-replication-security-config.md) voor informatie over de voordelen van draagbaarheid.
>

1. Klik in Objectverkenner met de rechtermuisknop op **sqldbtutorialdb** en klik vervolgens op **Nieuwe query**.
2. Voer de volgende instructie uit in dit queryvenster om een gebruiker met de naam **gebruiker1** te maken in de database sqldbtutorialdb.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![nieuwe gebruiker gebruiker1 sqldbtutorialdb](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van gebruiker1.

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'user1';
   ```

   ![nieuwe gebruikersmachtigingen in een gebruikersdatabase](./media/sql-database-control-access-sql-authentication-get-started/new_user_permissions_in_user_database.png)

   > [!NOTE]
   > Nieuwe gebruikers in een database beschikken alleen over de machtigingen die zijn overgenomen van de openbare rol.
   >

4. Voer de volgende query's uit met de instructie **EXECUTE AS USER** om als **gebruiker1** een query uit te voeren voor de tabel SalesLT.ProductCategory in de database sqldbtutorialdb, met alleen de machtigingen die zijn overgenomen van de openbare rol.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![geen bevoegdheid SELECT](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

   > [!NOTE]
   > De openbare rol biedt standaard geen **SELECT**-machtigingen voor gebruikersobjecten.
   >

5. Voer de volgende instructie uit om **gebruiker1** de **SELECT**-machtigingen uit de tabel SalesLT.ProductCategory te verlenen.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![bevoegdheid SELECT verlenen](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. Voer de volgende query's uit om als **gebruiker1** een query uit te voeren voor de tabel SalesLT.ProductCategory in de database sqldbtutorialdb.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![bevoegdheid SELECT](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-using-t-sql"></a>Een firewallregel op databaseniveau maken met behulp van T-SQL

Via de stappen in deze procedure leert u hoe u een firewallregel op databaseniveau maakt met de in het systeem opgeslagen procedure [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx). Met een firewallregel op databaseniveau kunnen serverbeheerders gebruikers via de Azure SQL Database-firewall toegang verlenen tot specifieke databases.

> [!NOTE]
> [Firewallregels op databaseniveau](sql-database-firewall-configure.md) vergroten de draagbaarheid van uw database. Zie [De Azure SQL Database-beveiliging configureren n beheren voor geografisch herstel en failovers naar een secundaire server](sql-database-geo-replication-security-config.md) voor informatie over de voordelen van draagbaarheid.
>

> [!IMPORTANT]
> Voor het testen van een firewallregel op databaseniveau maakt u verbinding vanaf een andere computer (of verwijdert u de firewallregel op serverniveau in Azure Portal).
>

1. Open SQL Server Management Studio op een computer waarvoor geen firewallregel op serverniveau is ingesteld.

2. Voer in het venster **Verbinding maken met server** de servernaam en verificatiegegevens in om verbinding te maken met het **gebruiker1**-account met behulp van SQL Server-verificatie. 
    
   ![Verbinding maken als gebruiker1 zonder firewallregel1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. Klik op **Opties** om de database op te geven waarmee u verbinding wilt maken. Typ vervolgens **sqldbtutorialdb** in de vervolgkeuzelijst **Verbinding maken met database** op het tabblad **Eigenschappen van verbinding**.
   
   ![Verbinding maken als gebruiker1 zonder firewallregel2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. Klik op **Verbinden**. 

   Er wordt een dialoogvenster weergegeven waarin wordt gemeld dat voor de computer waarmee u verbinding wilt maken met SQL Database, geen firewallregel voor toegang tot de database is ingesteld. 

   ![Verbinding maken als gebruiker1 zonder firewallregel4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)


5. Kopieer het client-IP-adres uit dit dialoogvenster voor gebruik in stap 8.
6. Klik op **OK** om het foutdialoogvenster te sluiten. Sluit het dialoogvenster **Verbinding maken met server** echter niet.
7. Schakel terug naar een computer waarvoor u al een firewallregel op serverniveau hebt gemaakt. 
8. Maak in SSMS verbinding met de database sqldbtutorialdb als serverbeheerder en voer de volgende instructie uit om een firewall op databaseniveau te maken. Gebruik hiervoor het IP-adres (of het adresbereik) uit stap 5.  

   ```
   EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![firewallregel toevoegen](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

9. Schakel weer naar de andere computer en klik op **Verbinding maken** in het dialoogvenster **Verbinding maken met server** om als gebruiker1 verbinding te maken met sqltutorialdb. 

   > [!NOTE]
   > Wanneer u de firewallregel op databaseniveau hebt gemaakt, kan het tot vijf minuten duren voordat deze actief wordt.
   >

10. Wanneer u verbinding hebt gemaakt, vouwt u in Objectverkenner **Databases** uit. **gebruiker1** kan alleen de database **sqldbtutorialdb** bekijken.

   ![Verbinding maken als gebruiker1 met firewallregel1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

11. Vouw **sqldbtutorialdb** uit en vouw daarna **Tabellen** uit. U ziet dat gebruiker1 alleen is gemachtigd om één tabel te bekijken: de tabel **SalesLT.ProductCategory**. 

   ![Verbinding maken als gebruiker1 en objects1 weergeven](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

## <a name="create-a-new-user-as-dbowner-and-a-database-level-firewall-rule"></a>Een nieuwe gebruiker (db_owner) en een firewallregel op databaseniveau maken

Via de stappen in deze procedure leert u hoe u een gebruiker in een andere database maakt met de databaserolmachtigingen van db_owner en hoe u een firewall op databaseniveau maakt voor de andere database. Deze nieuwe gebruiker met de rol **db_owner** kan alleen verbinding maken met deze database en kan alleen deze database beheren.

1. Schakel naar de computer die is verbonden met SQL Database en gebruik daarvoor het serverbeheerdersaccount.
2. Open een queryvenster dat is verbonden met de **blankdb**-database en voer de volgende instructie uit om in de blankdb-database een gebruiker te maken met de naam blankdbadmin.

   ```
   CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. Voer in hetzelfde queryvenster de volgende instructie uit om de blankdbadmin-gebruiker toe te voegen aan de databaserol db_owner. Deze gebruiker kan nu alle acties uitvoeren die nodig zijn voor het beheren van de blankdb-database.

   ```
   ALTER ROLE db_owner ADD MEMBER blankdbadmin; 
   ```

4. Voer in hetzelfde queryvenster de volgende instructie uit om een firewall op databaseniveau te maken door [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) uit te voeren met behulp van het IP-adres uit stap 4 in de vorige procedure (of een bereik met IP-adressen voor gebruikers van deze database):

   ```
   EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Schakel naar een computer waarvoor u een firewallregel op databaseniveau hebt gemaakt, en maak verbinding met de blankdb-database met behulp van het blankdbadmin-gebruikersaccount.
6. Open een queryvenster voor de blankdb-database en voer de volgende instructie uit om in de blankdb-database een gebruiker te maken met de naam blankdbuser1.

   ```
   CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
   ```
 
7. Maak een extra firewallregel op databaseniveau voor deze gebruiker, als dit nodig is voor uw leeromgeving. Als u de firewallregel op databaseniveau echter hebt gemaakt met een IP-adresbereik, is dit mogelijk niet nodig.

## <a name="grant-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>dbmanager machtigingen toekennen en een firewallregel op serverniveau maken

Via de stappen in deze procedure leert u hoe u aanmeldingsgegevens en een gebruiker maakt in de hoofddatabase, met machtigingen voor het maken en beheren van de nieuwe gebruikersdatabases. U maakt ook een extra firewallregel op serverniveau met Transact-SQL met behulp van [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). 

> [!IMPORTANT]
>De eerste firewallregel op serverniveau moet altijd worden gemaakt in Azure (in Azure Portal, met PowerShell of met de REST-API).
>

> [!IMPORTANT]
> De serverbeheerder moet aanmeldingen maken in de hoofddatabase en een gebruikersaccount maken via een aanmelding om het maken van databasemachtigingen te kunnen delegeren naar een andere gebruiker. Bij het maken van aanmeldingen en het maken van gebruikers op basis van die aanmeldingen wordt de draagbaarheid van uw omgeving echter beperkt.
>

1. Schakel naar de computer die is verbonden met SQL Database en gebruik daarvoor het serverbeheerdersaccount.
2. Open een queryvenster dat is verbonden met de hoofddatabase en voer de volgende instructie uit om in de hoofddatabase een gebruiker te maken met de naam dbcreator.

   ```
   CREATE LOGIN dbcreator
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. In hetzelfde queryvenster, 

   ```
   CREATE USER dbcreator
   FROM LOGIN dbcreator;
   ```

3. Voer in hetzelfde queryvenster de volgende query uit om de dbcreator-gebruiker toe te voegen aan de databaserol dbmanager. Deze gebruiker kan databases maken en beheren die zijn gemaakt door de gebruiker.

   ```
   ALTER ROLE dbmanager ADD MEMBER dbcreator; 
   ```

4. Voer in hetzelfde queryvenster de volgende query uit om een firewall op serverniveau te maken door [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) uit te voeren met behulp van een IP-adres dat geschikt is voor uw omgeving:

   ```
   EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Schakel naar een computer waarvoor u een firewallregel op databaseniveau hebt gemaakt en maak verbinding met de blankdb-database met behulp van het blankdbadmin-gebruikersaccount.
6. Open een queryvenster voor de hoofddatabase en voer de volgende query uit om een database te maken met de naam foo.

   ```
   CREATE DATABASE FOO (EDITION = 'basic');
   ```
 7. U kunt deze database optioneel verwijderen om geld te besparen door de volgende instructie te volgen:

   ```
   DROP DATABASE FOO;
   ```

## <a name="complete-script"></a>Script voltooien

Als u aanmeldingen en gebruikers wilt maken, voegt u ze toe aan rollen, verleent u ze machtigingen, maakt u firewallregels op databaseniveau en op serverniveau en voert u de volgende instructies uit in de juiste databases op de server.

### <a name="master-database"></a>hoofddatabase
Voer deze instructies uit in de hoofddatabase me behulp van het serverbeheerdersaccount, waarbij u de juiste IP-adressen of het juiste IP-bereik toevoegt.

```
CREATE LOGIN dbcreator WITH PASSWORD = 'p@ssw0rd';
CREATE USER dbcreator FROM LOGIN dbcreator;
ALTER ROLE dbmanager ADD MEMBER dbcreator;
EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="sqldbtutorialdb-database"></a>sqldbtutorialdb-database
Voer deze instructies uit in de sqldbtutorialdb-database met behulp van het serverbeheerdersaccount, waarbij u de juiste IP-adressen of het juiste IP-bereik toevoegt.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="blankdb-database"></a>blankdb-database
Voer deze instructies uit in de blankdb-database met behulp van het serverbeheerdersaccount, waarbij u de juiste IP-adressen of het juiste IP-bereik toevoegt.

```
CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
ALTER ROLE db_owner ADD MEMBER blankdbadmin;
EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
```

## <a name="next-steps"></a>Volgende stappen
- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
- Zie [SQL Database-firewallregels](sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.
- Zie [Azure AD-verificatie en -autorisatie](sql-database-control-access-aad-authentication-get-started.md) voor een zelfstudie over het gebruik van Azure Active Directory-verificatie.




<!--HONumber=Feb17_HO3-->


