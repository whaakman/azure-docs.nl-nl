---
title: 'SQL Database-verificatie en -autorisatie: toegang verlenen | Microsoft Docs'
description: Meer informatie over SQL Database-beveiligingsbeheer, in het bijzonder over hoe de databasetoegang en de aanmeldingsbeveiliging worden beheerd via het hoofdaccount op serverniveau.
keywords: sql-databasebeveiliging,beheer databasebeveiliging,aanmeldingsbeveiliging,databasebeveiliging,databasetoegang
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/14/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 498ee06fedf986a1b7fb4e362c4b5cb688f6a685
ms.openlocfilehash: 37cce678e0a9dfb081719cee4be7a04a838c1012


---
# <a name="sql-database-authentication-and-authorization-granting-access"></a>SQL Database-verificatie en -autorisatie: toegang verlenen
> [!div class="op_single_selector"]
> * [Zelfstudie Aan de slag](sql-database-get-started-security.md)
> * [Toegang verlenen](sql-database-manage-logins.md)
> 
> 

Begin hier voor een overzicht van de SQL Database-toegangsconcepten voor beheerders, niet-beheerders en rollen.

## <a name="unrestricted-administrative-accounts"></a>Onbeperkte beheerdersaccounts
Er zijn twee mogelijke beheerdersaccounts met onbeperkte machtigingen voor toegang tot de virtuele hoofddatabase en alle gebruikersdatabases. Deze accounts worden hoofdaccounts op serverniveau genoemd.

### <a name="azure-sql-database-subscriber-account"></a>Account Azure SQL Database-abonnee
Bij het maken van een logische SQL-instantie wordt er een account voor enkelvoudige aanmelding gemaakt, 'account van de SQL Database-abonnee' genoemd. Dit account maakt verbinding met behulp van SQL Server-verificatie (gebruikersnaam en wachtwoord). Het account fungeert als beheerder voor de logische serverinstantie en voor alle gebruikersdatabases die hieraan zijn gekoppeld. De machtigingen van het abonnee-account kunnen niet worden beperkt. Er kan slechts één van deze accounts bestaan.

### <a name="azure-active-directory-administrator"></a>Azure Active Directory-beheerder
Er kan ook één Azure Active Directory-account worden geconfigureerd als beheerder. Dit account kan betrekking hebben op een individuele Azure AD-gebruiker, maar ook op een Azure AD-groep die meerdere Azure AD-gebruikers bevat. Configuratie van een Azure AD-beheerder is optioneel, maar er moet een Azure AD-beheerder worden geconfigureerd als u wilt dat Azure AD-accounts via Windows-verificatie verbinding maken met SQL Database. Voor meer informatie over het configureren van toegang tot Azure Active Directory raadpleegt u [Verbinding maken met SQL Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) en [SSMS-ondersteuning voor Azure AD MFA met SQL Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

### <a name="configuring-the-firewall"></a>De firewall configureren
Wanneer de firewall op serverniveau is geconfigureerd, kunnen het account van de Azure SQL Database-abonnee en het Azure Active Directory-account verbinding maken met de masterdatabase en alle gebruikersdatabases. De firewall op serverniveau kan worden geconfigureerd via de portal. Nadat er een verbinding tot stand is gebracht, kunnen er ook aanvullende firewallregels op serverniveau worden geconfigureerd met behulp van de Transact-SQL-instructie [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). Voor meer informatie over firewallregels op serverniveau raadpleegt u [How to: Configure an Azure SQL server firewall using the Azure portal](sql-database-configure-firewall-settings.md) (Procedure: Een Azure SQL-serverfirewall configureren via Azure Portal).

### <a name="administrator-access-path"></a>Toegangspad beheerder
Wanneer de firewall op serverniveau correct is geconfigureerd, kunnen het account van de SQL Database-abonnee en de Azure Active Directory SQL Server-beheerders verbinding maken met clienthulpprogramma's zoals SQL Server Management Studio of SQL Server Data Tools. Alleen de nieuwste hulpprogramma's bieden alle functies en mogelijkheden. Het volgende diagram toont een standaardconfiguratie voor de twee beheerdersaccounts.
    ![Toegangspad beheerder](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Wanneer u een open poort in de firewall op serverniveau gebruikt, kunnen beheerders verbinding maken met elke SQL-database.

### <a name="connecting-to-a-database-by-using-sql-server-management-studio"></a>Verbinding maken met een database via SQL Server Management Studio
Voor een overzicht van het maken van verbinding via SQL Server Management Studio raadpleegt u [Verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld T-SQL-query uitvoeren](sql-database-connect-query-ssms.md).

> [!IMPORTANT]
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="additional-special-accounts"></a>Aanvullende speciale accounts
SQL Database biedt twee beperkte beheerdersrollen in de virtuele masterdatabase waaraan gebruikersaccounts kunnen worden toegevoegd.

### <a name="database-creators"></a>Databasemakers
De beheerdersaccounts kunnen nieuwe databases maken. Voor het maken van een extra account dat databases kan maken, moet u een gebruiker in de master maken en deze gebruiker toevoegen aan de speciale databaserol **dbmanager**. De gebruiker kan een ingesloten databasegebruiker zijn, maar ook een gebruiker gebaseerd op een SQL Server-aanmelding in de virtuele masterdatabase.

1. Gebruik een beheerdersaccount om verbinding te maken met de virtuele masterdatabase.
2. Optionele stap: maak een aanmelding voor SQL-verificatie met de instructie [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx). Voorbeeldinstructie:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Gebruik een sterk wachtwoord bij het maken van aanmeldgegevens of een ingesloten databasegebruiker. Zie [Sterke wachtwoorden](https://msdn.microsoft.com/library/ms161962.aspx) voor meer informatie.
   > 
   > 
   
   Voor betere prestaties worden aanmeldingen (principals op serverniveau) tijdelijk in het cachegeheugen op databaseniveau opgeslagen. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx) als u de verificatiecache wilt vernieuwen.
3. Maak een gebruiker in de virtuele masterdatabase met behulp van de instructie [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx). De gebruiker kan een ingesloten databasegebruiker op basis van Azure Active Directory-verificatie zijn (als u uw omgeving hebt geconfigureerd voor Azure AD-verificatie), maar ook een ingesloten databasegebruiker op basis van SQL Server-verificatie of een gebruiker op basis van SQL Server-verificatie met aanmelding voor SQL Server-verificatie (gemaakt in de vorige stap). Voorbeeldinstructies:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```
4. Voeg de nieuwe gebruiker toe aan de databaserol **dbmanager** met behulp van de instructie [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx). Voorbeeldinstructies:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > Dbmanager is een databaserol in de virtuele masterdatabase, zodat u alleen een gebruiker aan de rol dbmanager kunt toevoegen. U kunt geen aanmelding op serverniveau toevoegen aan een rol op databaseniveau .
   > 
   > 
5. Configureer, indien nodig, de firewall op serverniveau, zodat de nieuwe gebruiker verbinding kan maken.

De gebruiker kan nu verbinding maken met de virtuele masterdatabase en is in staat nieuwe databases te maken. Het account dat de database maakt, wordt eigenaar van de database.

### <a name="login-managers"></a>Aanmelding managers
Desgewenst kunt u dezelfde stappen doorlopen (een aanmelding en een gebruiker maken en een gebruiker toevoegen aan de rol **loginmanager**), zodat een gebruiker nieuwe aanmeldingen kan maken in de virtuele master. Dit is doorgaans niet nodig omdat Microsoft toepassing aanbeveelt van ingesloten databasegebruikers die gebruikmaken van verificatie op databaseniveau, in plaats van toepassing van gebruikers op basis van aanmelding. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie.

## <a name="non-administrator-users"></a>Niet-beheerders
Niet-beheerdersaccounts hebben doorgaans geen toegang nodig tot de virtuele masterdatabase. Maak ingesloten databasegebruikers op databaseniveau met de instructie [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx). De gebruiker kan een ingesloten databasegebruiker op basis van Azure Active Directory-verificatie zijn (als u uw omgeving hebt geconfigureerd voor Azure AD-verificatie), maar ook een ingesloten databasegebruiker op basis van SQL Server-verificatie of een gebruiker op basis van SQL Server-verificatie met aanmelding voor SQL Server-verificatie (gemaakt in de vorige stap). Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie. 

Als u gebruikers wilt maken, maakt u verbinding met de database en voert u de instructies uit die in de volgende voorbeelden worden getoond:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

In eerste instantie kan slechts een van de beheerders of de eigenaar van de database gebruikers maken. Om extra gebruikers te machtigen voor het maken van nieuwe gebruikers, verleent u de geselecteerde gebruikers de `ALTER ANY USER`-toestemming met behulp van een instructie zoals:

```
GRANT ALTER ANY USER TO Mary;
```

Om extra gebruikers volledig beheer van de database te geven, moet u ze lid maken van de vaste databaserol **db_owner** met behulp van de `ALTER ROLE`-instructie.

> [!NOTE]
> De belangrijkste reden om databasegebruikers te maken op basis van aanmelding, is wanneer u SQL Server-verificatiegebruikers hebt die toegang tot meerdere databases nodig hebben. Gebruikers op basis van aanmelding zijn gekoppeld aan de aanmelding. Voor deze aanmelding wordt slechts één wachtwoord bijgehouden. Ingesloten databasegebruikers in individuele databases zijn individuele entiteiten en hebben elk een eigen wachtwoord. Dit kan verwarrend zijn voor ingesloten databasegebruikers die hun wachtwoorden niet identiek hebben gehouden.
> 
> 

### <a name="configuring-the-database-level-firewall"></a>De firewall op databaseniveau configureren
U doet er verstandig aan niet-beheerders alleen via de firewall toegang te verlenen tot de databases die ze gebruiken. In plaats van het machtigen van hun IP-adressen via de firewall op serverniveau en hun toegang te verlenen tot alle databases, kunt u de instructie [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) gebruiken om de firewall op databaseniveau te configureren. De firewall op databaseniveau kan niet worden geconfigureerd met behulp van de portal.

### <a name="non-administrator-access-path"></a>Toegangspad niet-beheerder
Wanneer de firewall op databaseniveau correct is geconfigureerd, kunnen databasegebruikers verbinding maken met de hulp van clienthulpprogramma's zoals SQL Server Management Studio of SQL Server Data Tools. Alleen de nieuwste hulpprogramma's bieden alle functies en mogelijkheden. Het volgende diagram toont een standaardtoegangspad voor niet-beheerders.
![Toegangspad niet-beheerder](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Groepen en rollen
Voor efficiënt toegangsbeheer gebruikt u machtigingen die zijn toegewezen aan groepen en rollen in plaats van aan individuele gebruikers. Voorbeelden bij het gebruik van Azure Active Directory-verificatie:

* Plaats Active Directory-gebruikers in een Azure Active Directory-groep. Maak voor de groep een ingesloten databasegebruiker. Plaats een of meer databasegebruikers in een databaserol. Wijs vervolgens machtigingen toe aan deze databaserol.

Bij het gebruik van SQL Server-verificatie:

* Maak ingesloten databasegebruikers in de database. Plaats een of meer databasegebruikers in een databaserol. Wijs vervolgens machtigingen toe aan deze databaserol.

Bij de databaserollen kan het gaan om de ingebouwde rollen als **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** en **db_denydatareader**. **db_owner** wordt doorgaans gebruikt voor het verlenen van volledige machtigingen aan slechts enkele gebruikers. De andere vaste databaserollen zijn handig voor het snel verkrijgen van een eenvoudige database voor ontwikkeldoeleinden, maar worden niet aanbevolen voor de meeste productiedatabases. De vaste databaserol **db_datareader** verleent bijvoorbeeld leestoegang tot alle tabellen in de database, wat doorgaans meer is dan strikt noodzakelijk. Het is veel beter de instructie [CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx) te gebruiken om uw eigen gebruikergedefinieerde databaserollen te maken en zorgvuldig elke rol de minimale machtigingen te verlenen die nodig zijn voor de gerelateerde zakelijke behoeften. Als een gebruiker lid is van meerdere rollen, worden de machtigingen van alle rollen samengevoegd.

## <a name="permissions"></a>Machtigingen
Er zijn meer dan 100 machtigingen die afzonderlijk kunnen worden verleend of geweigerd in SQL Database. Veel van deze machtigingen zijn genest. De machtiging `UPDATE` voor een schema bevat bijvoorbeeld de machtiging `UPDATE` voor elke tabel binnen dat schema. Net als bij de meeste machtigingssystemen gaat de weigering van een machtiging vóór toestemming. Vanwege de geneste aard en het aantal machtigingen kan een nauwkeurig onderzoek nodig zijn om een geschikt machtigingssysteem te ontwerpen voor een goede bescherming van uw database. Start met de lijst van machtigingen in [Machtigingen (Database-engine)](https://msdn.microsoft.com/library/ms191291.aspx) en controleer de [afbeelding op postergrootte](http://go.microsoft.com/fwlink/?LinkId=229142) van de machtigingen.

## <a name="next-steps"></a>Volgende stappen
[Uw SQL-database beveiligen](sql-database-security.md)

[Een tabel maken\(Zelfstudie\)](https://msdn.microsoft.com/library/ms365315.aspx)

[Gegevens in een tabel invoegen en bijwerken \(Zelfstudie\)](https://msdn.microsoft.com/library/ms365309.aspx)

[Gegevens in een tabel lezen \(Zelfstudie\)](https://msdn.microsoft.com/library/ms365310.aspx)

[Weergaven en opgeslagen procedures maken](https://msdn.microsoft.com/library/ms365311.aspx)

[Toegang verlenen tot een databaseobject](https://msdn.microsoft.com/library/ms365327.aspx)

## <a name="additional-resources"></a>Aanvullende bronnen
[Uw SQL-database beveiligen](sql-database-security.md)

[Security Center for SQL Server Database Engine and Azure SQL Database](https://msdn.microsoft.com/library/bb510589.aspx) (Security Center voor SQL Server Database Engine en Azure SQL Database) 




<!--HONumber=Dec16_HO3-->


