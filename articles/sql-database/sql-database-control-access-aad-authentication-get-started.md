---
title: 'AAD-authenticatie: firewalls, verificatie en toegang in Azure SQL Database | Microsoft Docs'
description: In deze aan de slag-zelfstudie leert u hoe u SQL Server Management Studio en Transact-SQL kunt gebruiken om te werken met firewallregels op server- en databaseniveau, Azure Active Directory-verificatie, aanmeldingen, gebruikers en rollen voor toegang tot en beheer van Azure SQL Database-servers en Azure SQL-databases.
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
ms.sourcegitcommit: 7d061c083b23de823d373c30f93cccfe1c856ba3
ms.openlocfilehash: 8a6dc7d3dca80782a55e13b53180b1542b61544b


---
# <a name="azure-ad-authentication-access-and-database-level-firewall-rules"></a>Azure AD-verificatie, toegang en firewallregels op databaseniveau
In deze zelfstudie leert u hoe u SQL Server Management Studio en Transact-SQL kunt gebruiken om te werken met Azure Active Directory-verificatie, aanmeldingen, gebruikers en databaserollen voor toegang tot en machtigingen voor Azure SQL Database-servers en Azure SQL-databases. U leert hier hoe u de volgende handelingen kunt uitvoeren:

- Gebruikersmachtigingen in de hoofddatabase en in gebruikersdatabases bekijken
- Aanmeldingen en gebruikers maken op basis van Azure Active Directory-verificatie
- Machtigingen verlenen aan gebruikers voor de hele server of specifiek voor een database
- Als een niet-beheerder aanmelden bij een gebruikersdatabase
- Firewallregels op databaseniveau maken voor databasegebruikers
- Firewallregels op serverniveau maken voor serverbeheerders

**Geschatte tijd**: deze zelfstudie duurt circa 45 minuten. (We gaan er hierbij vanuit dat u al aan de vereisten voldoet.)

## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](https://azure.microsoft.com/free/) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* U hebt de zelfstudie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio](sql-database-get-started.md) of de equivalente [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie voltooid. Als dit niet het geval is, voltooit u eerst deze vereiste zelfstudie of voert u het PowerShell-script aan het einde van de [PowerShell-versie](sql-database-get-started-powershell.md) van deze zelfstudie uit voordat u doorgaat.

   > [!NOTE]
   > Voltooiing van de gerelateerde zelfstudie voor SQL Server-verificatie ([SQL-verificatie, aanmeldingen en gebruikersaccounts, databaserollen, machtigingen, en firewallregels op server- en op databaseniveau](sql-database-control-access-sql-authentication-get-started.md)) is optioneel. Er worden in die zelfstudie echter concepten behandeld die hier niet worden herhaald. De procedures in deze zelfstudie die zijn gerelateerd aan firewalls op server- en databaseniveau, zijn niet vereist als u deze gerelateerde zelfstudie op dezelfde computer (met hetzelfde IP-adres) hebt voltooid. Deze procedures zijn daarom gemarkeerd als optioneel. In de schermafbeeldingen in deze zelfstudie wordt ervan uitgegaan dat u deze gerelateerde zelfstudie hebt voltooid. 
   >

* U hebt Azure Active Directory gemaakt en ingevuld. Zie [Uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Uw domeinnaam toevoegen in Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure ondersteunt nu federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Uw Azure AD-adreslijst beheren](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Azure AD beheren met Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) en [Poorten en protocollen waarvoor hybride identiteit is vereist](../active-directory/active-directory-aadconnect-ports.md) voor meer informatie.

> [!NOTE]
> Deze zelfstudie biedt informatie over de inhoud over deze onderwerpen: [SQL Database-toegang en -controle](sql-database-control-access.md), [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md), [Principals](https://msdn.microsoft.com/library/ms181127.aspx), [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx), [SQL Database-firewallregels](sql-database-firewall-configure.md) en [Azure Active Directory-verificatie](sql-database-aad-authentication.md). 
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Aanmelden bij Azure Portal met uw Azure-account
Gebruik uw [bestaande abonnement](https://account.windowsazure.com/Home/Index) en volg deze stappen om verbinding te maken met Azure Portal.

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="provision-an-azure-active-directory-admin-for-your-sql-logical-server"></a>Een Azure Active Directory-beheerder inrichten voor de logische SQL-server

In deze sectie van de zelfstudie kunt u informatie bekijken over de beveiligingsconfiguratie voor de logische server in Azure Portal.

1. Open de blade **SQL Server** voor de logische server en bekijk de informatie op de pagina **Overzicht**. U ziet dat er geen Azure Active Directory-beheerder is geconfigureerd.

   ![Serverbeheerdersaccount in Azure Portal](./media/sql-database-control-access-aad-authentication-get-started/sql_admin_portal.png)

2. Klik in het deelvenster **Essentials** op **Niet-geconfigureerd** om de blade **Active Directory-beheerder ** te openen.

   ![AAD-blade](./media/sql-database-control-access-aad-authentication-get-started/aad_blade.png)

3. Klik op **Beheerder instellen** om de blade **Beheerder toevoegen** te openen en selecteer vervolgens een Active Directory-gebruikers- of groepsaccount als de Active Directory-beheerder voor de server.

   ![AAD-beheerdersaccount selecteren](./media/sql-database-control-access-aad-authentication-get-started/aad_admin.png)

4. Klik op **Selecteren** en klik vervolgens op **Opslaan**.

   ![Geselecteerde AAD-beheerdersaccount opslaan](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_save.png)

> [!NOTE]
> Ga naar [Servers beheren](sql-database-manage-servers-portal.md) om de verbindingsinformatie voor deze server te bekijken. De volledige servernaam voor deze reeks zelfstudies is 'sqldbtutorialserver.database.windows.net'.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Verbinding maken met de SQL-server met SQL Server Management Studio (SSMS)

1. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) voor het downloaden en installeren van de meest recente versie van SSMS, als u dit nog niet hebt gedaan. U wordt in de meest recente versie van SSMS op de hoogte gesteld wanneer er een nieuwe versie kan worden gedownload, zodat u steeds met de nieuwste versie kunt werken.

2. Typ na het installeren **Microsoft SQL Server Management Studio** in het zoekvak van Windows en klik op **Enter** om SSMS te openen.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. Selecteer in het dialoogvenster **Verbinding maken met server** een van de Active Directory-verificatiemethoden en geef vervolgens de juiste verificatiegegevens op. Zie [Azure Active Directory-verificatie](sql-database-aad-authentication.md) en [SSMS-ondersteuning voor Azure AD MFA](sql-database-ssms-mfa-authentication.md) voor meer informatie.

   ![verbinding maken met de server via AAD](./media/sql-database-control-access-aad-authentication-get-started/connect_to_server_with_aad.png)

4. Voer de benodigde gegevens in om verbinding te maken met de SQL-server met behulp van SQL Server-verificatie en het serverbeheerdersaccount.

5. Klik op **Verbinden**.

   ![verbonden met de server via AAD](./media/sql-database-control-access-aad-authentication-get-started/connected_to_server_with_aad.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Het serverbeheerdersaccount en de bijbehorende machtigingen bekijken 
In deze sectie van de zelfstudie kunt u informatie bekijken over het serverbeheerdersaccount en de bijbehorende machtigingen in de hoofddatabase en in gebruikersdatabases.

1. Vouw in Objectverkenner achtereenvolgens **Databases** **Systeemdatabases**, **hoofd**, **Beveiliging** en **Gebruikers** uit. U ziet dat een gebruikersaccount is gemaakt in de hoofddatabase voor de Active Directory-beheerder. U ziet ook dat er geen aanmelding is gemaakt voor het Active Directory-beheerdersaccount.

   ![gebruikersaccount voor de AAD-beheerder in de hoofddatabase](./media/sql-database-control-access-aad-authentication-get-started/master_database_user_account_for_AAD_admin.png)

   > [!NOTE]
   > Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor informatie over de andere gebruikersaccounts die worden weergegeven.
   >

2. Klik in Objectverkenner met de rechtermuisknop op **hoofd** en klik vervolgens op **Nieuwe query** om een queryvenster te openen dat is verbonden met de hoofddatabase.
3. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert. U ziet dat user@microsoft.com wordt geretourneerd voor het gebruikersaccount dat deze query uitvoert. (Als we in een later stadium in deze procedure een query uitvoeren, krijgen we een ander resultaat.)

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de hoofddatabase](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_master_database.png)

4. Voer in het queryvenster de volgende query uit om informatie te krijgen over de machtigingen van de Active Directory-beheerder. U ziet dat de Active Directory-beheerder is gemachtigd om verbinding te maken met de hoofddatabase, om aanmeldingen en gebruikers te maken, om gegevens uit de tabel .sql_logins te selecteren en om gebruikers toe te voegen aan de databaserollen dbmanager en dbcreator. Dit zijn extra machtigingen, naast de machtigingen die zijn toegewezen aan de openbare rol waarvan alle gebruikers machtigingen overnemen (zoals machtigingen voor het selecteren van gegevens uit bepaalde tabellen). Zie [machtigingen](https://msdn.microsoft.com/library/ms191291.aspx) voor meer informatie.

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
   WHERE p.name = 'user@microsoft.com';
   ```

   ![machtigingen voor AAD-beheer in de hoofddatabase](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_master_database.png)

6. Vouw in Objectverkenner achtereenvolgens **blankdb**, **Beveiliging** en **Gebruikers** uit. U ziet dat er geen gebruikersaccount met de naam user@microsoft.com bestaat in deze database.

   ![gebruikersaccounts in blankdb](./media/sql-database-control-access-aad-authentication-get-started/user_accounts_in_blankdb.png)

7. Klik in Objectverkenner met de rechtermuisknop op **blankdb** en klik vervolgens op **Nieuwe query**.

8. Voer in het queryvenster de volgende query uit om informatie te krijgen over de gebruiker die de query uitvoert. U ziet dat dbo wordt geretourneerd voor het gebruikersaccount waarmee deze query wordt uitgevoerd. (De aanmelding van de serverbeheerder wordt standaard toegewezen aan het dbo-gebruikersaccount in elke gebruikersdatabase.)

   ```
   SELECT USER;
   ```

   ![gebruikersquery selecteren in de blankdb-database](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_blankdb_database.png)

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

   ![machtigingen voor serverbeheer in de blankdb-database](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_blankdb_database.png)

10. U kunt optioneel de vorige drie stappen herhalen voor de AdventureWorksLT-gebruikersdatabase.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>Een nieuwe gebruiker maken in de AdventureWorksLT-database met de bevoegdheid SELECT

In deze sectie van de zelfstudie maakt u een gebruikersaccount in de AdventureWorksLT-database op basis van de principal-naam van een Azure AD-gebruiker of weergavenaam voor een Azure AD-groep, test u de machtigingen van deze gebruiker als lid van de publieke rol, verleent u deze gebruiker de bevoegdheid SELECT en test u de machtigingen van deze gebruiker vervolgens opnieuw.

> [!NOTE]
> Gebruikers op databaseniveau ([ingesloten gebruikers](https://msdn.microsoft.com/library/ff929188.aspx)) verhogen de draagbaarheid van de database. Dit is een mogelijkheid die in latere zelfstudies wordt toegelicht.
>

1. Klik in Objectverkenner met de rechtermuisknop op **AdventureWorksLT** en klik vervolgens op **Nieuwe query** om een queryvenster te openen dat is verbonden met de AdventureWorksLT-database.
2. Voer de volgende instructie uit om een gebruikersaccount te maken in de AdventureWorksLT-database voor een gebruiker in het Microsoft-domein met de naam aaduser1.

   ```
   CREATE USER [aaduser1@microsoft.com]
   FROM EXTERNAL PROVIDER;
   ```
   ![nieuwe gebruiker aaduser1@microsoft.com AdventureWorksLT](./media/sql-database-control-access-aad-authentication-get-started/new_user_aaduser1@microsoft.com_aw.png)

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
   WHERE p.name = 'aaduser1@microsoft.com';
   ```

   ![nieuwe gebruikersmachtigingen in een gebruikersdatabase](./media/sql-database-control-access-aad-authentication-get-started/new_user_permissions_in_user_database.png)

4. Voer de volgende query's uit om als gebruiker1 een query uit te voeren voor een tabel in de AdventureWorksLT-database.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![geen bevoegdheid SELECT](./media/sql-database-control-access-aad-authentication-get-started/no_select_permissions.png)

5. Voer de volgende instructie uit om de bevoegdheid SELECT voor de ProductCategory-tabel in het SalesLT-schema te verlenen aan gebruiker1.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to [aaduser1@microsoft.com];
   ```

   ![bevoegdheid SELECT verlenen](./media/sql-database-control-access-aad-authentication-get-started/grant_select_permissions.png)

6. Voer de volgende query's uit om als gebruiker1 een query uit te voeren voor een tabel in de AdventureWorksLT-database.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![bevoegdheid SELECT](./media/sql-database-control-access-aad-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-adventureworkslt-database-users"></a>Een firewallregel op databaseniveau maken voor gebruikers van een AdventureWorksLT-database

> [!NOTE]
> U hoeft deze procedure niet uit te voeren als u een soortgelijke procedure hebt uitgevoerd in de gerelateerde zelfstudie voor SQL Server-verificatie, [SQL-verificatie en -autorisatie](sql-database-control-access-sql-authentication-get-started.md), en dezelfde computer met hetzelfde IP-adres gebruikt.
>

In deze sectie van de zelfstudie probeert u zich met behulp van het nieuwe gebruikersaccount aan te melden vanaf een computer met een ander IP-adres, maakt u als de serverbeheerder een firewallregel op databaseniveau en meldt u zich vervolgens aan met deze nieuwe firewallregel op databaseniveau. 

> [!NOTE]
> [Firewallregels op databaseniveau](sql-database-firewall-configure.md) verhogen de draagbaarheid van de database. Dit is een mogelijkheid die in latere zelfstudies wordt toegelicht.
>

1. Open SQL Server Management Studio op een andere computer, waarvoor u nog geen firewallregel op serverniveau hebt gemaakt.

   > [!IMPORTANT]
   > Gebruik altijd de nieuwste versie van SSMS uit [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx). 
   >

2. Voer in het venster **Verbinding maken met server** de servernaam en verificatiegegevens in om verbinding te maken met het aaduser1@microsoft.com-account met behulp van SQL Server-verificatie. 
    
   ![Verbinding maken als aaduser1@microsoft.com zonder firewallregel1](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule1.png)

3. Klik op **Opties** om de database op te geven waarmee u verbinding wilt maken. Typ vervolgens **AdventureWorksLT** in de vervolgkeuzelijst **Verbinding maken met database** op het tabblad **Eigenschappen van verbinding**.
   
   ![Verbinding maken als aadgebruiker1 zonder firewallregel2](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule2.png)

4. Klik op **Verbinden**. Er wordt een dialoogvenster weergegeven waarin wordt gemeld dat voor de computer waarmee u verbinding wilt maken met SQL Database, geen firewallregel voor toegang tot de database is ingesteld. Welke van twee mogelijke dialoogvensters u te zien krijgt, is afhankelijk van de stappen voor firewalls die u eerder hebt genomen. Meestal wordt echter het eerste dialoogvenster weergegeven.

   ![Verbinding maken als gebruiker1 zonder firewallregel3](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule3.png)

   ![Verbinding maken als gebruiker1 zonder firewallregel4](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule4.png)

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

   ![firewallregel4 op databaseniveau toevoegen](./media/sql-database-control-access-aad-authentication-get-started/aaduser1_add_rule_aw.png)

8. Schakel weer naar de andere computer en klik op **Verbinding maken** in het dialoogvenster **Verbinding maken met server** om verbinding te maken met AdventureWorksLT als aadgebruiker1. 

9. Vouw in Objectverkenner achtereenvolgens **Databases**, **AdventureWorksLT** en **Tabellen** uit. U ziet dat gebruiker1 alleen is gemachtigd om één tabel te bekijken: de tabel **SalesLT.ProductCategory**. 

10. Klik in Object Explorer met de rechtermuisknop op **SalesLT.ProductCategory** en klik op **Bovenste 1000 rijen selecteren**.   

## <a name="next-steps"></a>Volgende stappen
- Zie [SQL Database-toegang en -beheer](sql-database-control-access.md) voor een overzicht van toegang en beheer in SQL Database.
- Zie [Aanmeldingen, gebruikers en databaserollen](sql-database-manage-logins.md) voor een overzicht van aanmeldingen, gebruikers en databaserollen in SQL Database.
- Zie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) voor meer informatie over database-principals.
- Zie [Databaserollen](https://msdn.microsoft.com/library/ms189121.aspx) voor meer informatie over databaserollen.
- Zie [SQL Database-firewallregels](sql-database-firewall-configure.md) voor meer informatie over de firewallregels in SQL Database.




<!--HONumber=Feb17_HO3-->


