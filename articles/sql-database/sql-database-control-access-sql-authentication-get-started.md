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
ms.date: 01/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 356cc4c6d8e25d36880e4b12bf471326e61990c3
ms.openlocfilehash: 275a33567fa1472573bc8abc87948ad306e853f0


---
# <a name="sql-database-tutorial-sql-server-authentication-logins-and-user-accounts-database-roles-permissions-server-level-firewall-rules-and-database-level-firewall-rules"></a>SQL Server-zelfstudie: SQL Server-verificatie, aanmeldingen en gebruikersaccounts, databaserollen, machtigingen, en firewallregels op server- en databaseniveau
In deze aan de slag-zelfstudie leert u hoe u SQL Server Management Studio en Transact-SQL kunt gebruiken om te werken met SQL-verificatie, aanmeldingen, gebruikers en databaserollen voor toegang tot en machtigingen voor Azure SQL Database-servers en Azure SQL-databases. U leert hier hoe u de volgende handelingen kunt uitvoeren:

- Gebruikersmachtigingen in de hoofddatabase en in gebruikersdatabases bekijken
- Aanmeldingen en gebruikers maken op basis van SQL Server-verificatie
- Machtigingen verlenen aan gebruikers voor de hele server of specifiek voor een database
- Als een niet-beheerder aanmelden bij een gebruikersdatabase
- Firewallregels op databaseniveau maken voor databasegebruikers
- Firewallregels op serverniveau maken voor serverbeheerders

**Geschatte tijd**: deze zelfstudie duurt circa 45 minuten. (We gaan er hierbij vanuit dat u al aan de vereisten voldoet.)

## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* U hebt de zelfstudie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio](sql-database-get-started.md) of de equivalente [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie voltooid. Als dit niet het geval is, voltooit u eerst deze vereiste zelfstudie of voert u het PowerShell-script aan het einde van de [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie uit voordat u doorgaat.

> [!NOTE]
> Deze zelfstudie biedt informatie over de inhoud over deze onderwerpen: [SQL Database-toegang en -controle](sql-database-control-access.md), [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md), [Principals](https://msdn.microsoft.com/library/ms181127.aspx), [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) en [SQL Database-firewallregels](sql-database-firewall-configure.md).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Aanmelden bij Azure Portal met uw Azure-account
Gebruik uw [bestaande abonnement](https://account.windowsazure.com/Home/Index) en volg deze stappen om verbinding te maken met Azure Portal.

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-information-about-the-security-configuration-for-your-logical-server"></a>Informatie bekijken over de beveiligingsconfiguratie voor de logische server

In deze sectie van de zelfstudie kunt u informatie bekijken over de beveiligingsconfiguratie voor de logische server in Azure Portal.

1. Open de blade **SQL Server** voor de logische server en bekijk de informatie op de pagina **Overzicht**.

   ![Serverbeheerdersaccount in Azure Portal](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Noteer de naam van het serverbeheerdersaccount voor de logische server. Als u het wachtwoord niet meer weet, klikt u op **Wachtwoord opnieuw instellen** om een nieuw wachtwoord in te stellen.

> [!NOTE]
> Ga naar [Serverinstellingen weergeven of bijwerken](sql-database-view-update-server-settings.md) om de verbindingsinformatie voor deze server te bekijken. De volledige servernaam voor deze reeks zelfstudies is 'sqldbtutorialserver.database.windows.net'.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Verbinding maken met de SQL-server met SQL Server Management Studio (SSMS)

1. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) voor het downloaden en installeren van de meest recente versie van SSMS, als u dit nog niet hebt gedaan. U wordt in de meest recente versie van SSMS op de hoogte gesteld wanneer er een nieuwe versie kan worden gedownload, zodat u steeds met de nieuwste versie kunt werken.

2. Typ na het installeren **Microsoft SQL Server Management Studio** in het zoekvak van Windows en klik op **Enter** om SSMS te openen.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. Voer in het dialoogvenster **Verbinding maken met server** de nodige gegevens in om verbinding te maken met uw SQL-server met behulp van SQL Server-verificatie en het serverbeheerdersaccount.

   ![verbinding maken met server](./media/sql-database-get-started/connect-to-server.png)

4. Klik op **Verbinden**.

   ![verbonden met server](./media/sql-database-get-started/connected-to-server.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Het serverbeheerdersaccount en de bijbehorende machtigingen bekijken 
In deze sectie van de zelfstudie kunt u informatie bekijken over het serverbeheerdersaccount en de bijbehorende machtigingen in de hoofddatabase en in gebruikersdatabases.

1. Vouw in Objectverkenner achtereenvolgens **Beveiliging** en **Aanmeldingen** uit om de bestaande aanmeldingen in de Azure SQL Database-server te bekijken. U ziet een aanmelding voor het serverbeheerdersaccount die is opgegeven tijdens het inrichten: de sqladmin-aanmelding voor deze reeks zelfstudies.

   ![Aanmeldgegevens van serverbeheerder](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

2. Vouw in Objectverkenner achtereenvolgens **Databases** **Systeemdatabases**, **hoofd**, **Beveiliging** en **Gebruikers** uit. U ziet dat in de hoofddatabase een gebruikersaccount is gemaakt voor het serverbeheerdersaccount met dezelfde naam voor het gebruikersaccount als de aanmelding. (De namen hoeven niet overeen te komen, maar dit wordt wel aangeraden om verwarring te voorkomen.)

   ![Gebruikersaccount voor de serverbeheerder in de hoofddatabase](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor informatie over de andere gebruikersaccounts die worden weergegeven.
   >

3. Klik in Objectverkenner met de rechtermuisknop op **hoofd** en klik vervolgens op **Nieuwe query** om een queryvenster te openen dat is verbonden met de hoofddatabase.
4. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert. U ziet dat sqladmin wordt geretourneerd voor het gebruikersaccount dat deze query uitvoert. (Als we in een later stadium in deze procedure een query uitvoeren, krijgen we een ander resultaat.)

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de hoofddatabase](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

5. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van de sqladmin-gebruiker. U ziet dat sqladmin is gemachtigd om verbinding te maken met de hoofddatabase, om aanmeldingen en gebruikers te maken, om gegevens uit de tabel .sql_logins te selecteren en om gebruikers toe te voegen aan de databaserollen dbmanager en dbcreator. Dit zijn extra machtigingen, naast de machtigingen die zijn toegewezen aan de openbare rol waarvan alle gebruikers machtigingen overnemen (zoals machtigingen voor het selecteren van gegevens uit bepaalde tabellen). Zie [machtigingen](https://msdn.microsoft.com/library/ms191291.aspx) voor meer informatie.

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

6. Vouw in Objectverkenner achtereenvolgens **blankdb**, **Beveiliging** en **Gebruikers** uit. U ziet dat er geen gebruikersaccount met de naam sqladmin is aangeroepen in deze database.

   ![gebruikersaccounts in blankdb](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

7. Klik in Objectverkenner met de rechtermuisknop op **blankdb** en klik vervolgens op **Nieuwe query**.

8. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert. U ziet dat dbo wordt geretourneerd voor het gebruikersaccount waarmee deze query wordt uitgevoerd. (De aanmelding van de serverbeheerder wordt standaard toegewezen aan het dbo-gebruikersaccount in elke gebruikersdatabase.)

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de blankdb-database](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

9. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van de dbo-gebruiker. U ziet dat dbo lid van de publieke rol en ook lid is van de vaste databaserol db_owner. Zie [Rollen op databaseniveau](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie.

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

10. U kunt optioneel de vorige drie stappen herhalen voor de AdventureWorksLT-gebruikersdatabase.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>Een nieuwe gebruiker maken in de AdventureWorksLT-database met de bevoegdheid SELECT

In deze sectie van de zelfstudie maakt u een gebruikersaccount in de AdventureWorksLT-database, test u de machtigingen van deze gebruiker als lid van de publieke rol, verleent u deze gebruiker de bevoegdheid SELECT en test u de machtigingen van deze gebruiker vervolgen opnieuw.

> [!NOTE]
> Gebruikers op databaseniveau ([ingesloten gebruikers](https://msdn.microsoft.com/library/ff929188.aspx)) verhogen de draagbaarheid van de database. Dit is een mogelijkheid die in latere zelfstudies wordt toegelicht.
>

1. Klik in Objectverkenner met de rechtermuisknop op **AdventureWorksLT** en klik vervolgens op **Nieuwe query** om een queryvenster te openen dat is verbonden met de AdventureWorksLT-database.
2. Voer de volgende instructie uit om in de AdventureWorksLT-database een gebruiker te maken met de naam gebruiker1.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![nieuwe gebruiker gebruiker1 AdventureWorksLT](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van gebruiker1. U ziet dat de enige machtigingen van gebruiker1 de machtigingen zijn die zijn overgenomen van de openbare rol.

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

4. Voer de volgende query's uit om als gebruiker1 een query uit te voeren voor een tabel in de AdventureWorksLT-database.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![geen bevoegdheid SELECT](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

5. Voer de volgende instructie uit om de bevoegdheid SELECT voor de ProductCategory-tabel in het SalesLT-schema te verlenen aan gebruiker1.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![bevoegdheid SELECT verlenen](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. Voer de volgende query's uit om als gebruiker1 een query uit te voeren voor een tabel in de AdventureWorksLT-database.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![bevoegdheid SELECT](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-an-adventureworkslt-database-user"></a>Een firewallregel op databaseniveau maken voor een gebruiker van een AdventureWorksLT-database

In deze sectie van de zelfstudie probeert u zich aan te melden vanaf een computer met een ander IP-adres, maakt u als de serverbeheerder een firewallregel op databaseniveau en meldt u zich vervolgens aan met deze nieuwe firewallregel op databaseniveau. 

> [!NOTE]
> [Firewallregels op databaseniveau](sql-database-firewall-configure.md) verhogen de draagbaarheid van de database. Dit is een mogelijkheid die in latere zelfstudies wordt toegelicht.
>

1. Open SQL Server Management Studio op een andere computer, waarvoor u nog geen firewallregel op serverniveau hebt gemaakt.

   > [!IMPORTANT]
   > Gebruik altijd de nieuwste versie van SSMS uit [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx). 
   >

2. Voer in het venster **Verbinding maken met server** de servernaam en verificatiegegevens in om verbinding te maken met het gebruiker1-account met behulp van SQL Server-verificatie. 
    
   ![Verbinding maken als gebruiker1 zonder firewallregel1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. Klik op **Opties** om de database op te geven waarmee u verbinding wilt maken. Typ vervolgens **AdventureWorksLT** in de vervolgkeuzelijst **Verbinding maken met database** op het tabblad **Eigenschappen van verbinding**.
   
   ![Verbinding maken als gebruiker1 zonder firewallregel2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. Klik op **Verbinden**. Er wordt een dialoogvenster weergegeven waarin wordt gemeld dat voor de computer waarmee u verbinding wilt maken met SQL Database, geen firewallregel voor toegang tot de database is ingesteld. Welke van twee mogelijke dialoogvensters u te zien krijgt, is afhankelijk van de stappen voor firewalls die u eerder hebt genomen. Meestal wordt echter het eerste dialoogvenster weergegeven.

   ![Verbinding maken als gebruiker1 zonder firewallregel3](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule3.png)

   ![Verbinding maken als gebruiker1 zonder firewallregel4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)

   > [!NOTE]
   > De nieuwste versies van SSMS omvatten de functionaliteit waarmee eigenaars van abonnementen en bijdragers zich kunnen aanmelden bij Microsoft Azure en een firewallregel op serverniveau kunnen maken.
   > 

4. Kopieer het client-IP-adres uit dit dialoogvenster voor gebruik in stap 7.
5. Klik op **Annuleren** maar sluit het dialoogvenster **Verbinding maken met server** niet.
6. Schakel terug naar een computer waarvoor u al een firewallregel op serverniveau hebt gemaakt en maak verbinding met de server met behulp van het serverbeheerdersaccount.
7. Voer in een nieuw queryvenster dat is verbonden met de AdventureWorksLT-database als serverbeheerder, de volgende instructie uit om een firewall op databaseniveau te maken door [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) uit te voeren met behulp van het IP-adres uit stap 4:

   ```
   EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![firewallregel toevoegen](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

8. Schakel weer naar de andere computer en klik op **Verbinding maken** in het dialoogvenster **Verbinding maken met server** om verbinding te maken met AdventureWorksLT als gebruiker1. 

   ![Verbinding maken als gebruiker1 met firewallregel1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

9. Vouw in Objectverkenner achtereenvolgens **Databases**, **AdventureWorksLT** en **Tabellen** uit. U ziet dat gebruiker1 alleen is gemachtigd om één tabel te bekijken: de tabel **SalesLT.ProductCategory**. 

   ![Verbinding maken als gebruiker1 en objects1 weergeven](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

10. Klik in Object Explorer met de rechtermuisknop op **SalesLT.ProductCategory** en klik op **Bovenste 1000 rijen selecteren**.   

   ![gebruiker1 query1](./media/sql-database-control-access-sql-authentication-get-started/user1_query1.png)

   ![resultaten voor gebruiker1 query1](./media/sql-database-control-access-sql-authentication-get-started/user1_query1_results.png)

## <a name="create-a-new-user-in-the-blankdb-database-with-dbowner-database-role-permissions-and-a-database-level-firewall-rule"></a>Maak een nieuwe gebruiker in de blankdb-database met de databaserolmachtigingen voor db_owner en een firewallregel op databaseniveau

In deze sectie van de zelfstudie maakt u een gebruiker in de blankdb-database met de rolmachtigingen voor db_owner. Ook maakt u een firewall op databaseniveau voor deze database met behulp van het serverbeheerdersaccount. 

1. Schakel naar de computer die is verbonden met SQL Database, met behulp van het serverbeheerdersaccount.
2. Open een queryvenster dat is verbonden met de blankdb-database, en voer de volgende instructie uit om in de blankdb-database een gebruiker te maken met de naam blankdbadmin.

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
 
7. Maak een extra firewallregel op databaseniveau voor deze gebruiker, als dit nodig is voor uw leeromgeving. 

## <a name="create-a-new-login-and-user-in-the-master-database-with-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>Maak een nieuwe aanmelding en gebruiker in de hoofddatabase met dbmanager-machtigingen en maak een firewallregel op serverniveau

In deze sectie van de zelfstudie maakt u een aanmelding en gebruiker in de hoofddatabase met machtigingen voor het maken en beheren van de nieuwe gebruikersdatabases. U maakt ook een extra firewallregel op serverniveau met Transact-SQL met behulp van [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx).

> [!NOTE]
> De eigenaar van het serverbeheerdersaccount moet aanmeldingen maken in de hoofddatabase en een gebruikersaccount maken via een aanmelding, om het maken van databasemachtigingen te kunnen delegeren naar een andere gebruiker. Het maken van aanmeldingen en van gebruikers via aanmeldingen vermindert echter de draagbaarheid van de omgeving. De gevolgen hiervan worden besproken in latere zelfstudies, onder andere hoe u zich kunt voorbereiden en moet handelen, als onderdeel van de planning voor rampherstel.
>

1. Schakel naar de computer die is verbonden met SQL Database, met behulp van het serverbeheerdersaccount.
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

4. Voer in hetzelfde queryvenster de volgende query uit om een firewall op serverniveau te maken door [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) uit te voeren met behulp van een IP-adres dat geschikt is voor uw omgeving:

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

### <a name="adventureworkslt-database"></a>AdventureWorksLT-database
Voer deze instructies uit in de AdventureWorksLT-database met behulp van het serverbeheerdersaccount, waarbij u de juiste IP-adressen of het juiste IP-bereik toevoegt.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
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
- Voor een zelfstudie met SQL Server-verificatie raadpleegt u [SQL Database tutorial: SQL Server authentication, logins and user accounts, database roles, permissions, server-level firewall rules, and database-level firewall rules](sql-database-control-access-sql-authentication-get-started.md) (SQL Database zelfstudie: SQL Server-verificatie, aanmeldingen en gebruikersaccounts, databaserollen, machtigingen, firewallregels op serverniveau en firewallregels op databaseniveau).




<!--HONumber=Jan17_HO3-->


