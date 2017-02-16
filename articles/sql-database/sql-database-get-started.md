---
title: 'Azure Portal: Aan de slag met Azure SQL Database | Microsoft Docs'
description: Meer informatie over hoe u met behulp van Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en databases maakt. U leert ook hoe u met SQL Server Management Studio gegevens opvraagt uit databases.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: single databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/23/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2a85b3dc1078bad9e5e2fc0ce0bec7e994b29150
ms.openlocfilehash: 6da0bb371336e0d9662a7fd36187cdf4317c22ea


---
# <a name="sql-database-tutorial-get-started-with-azure-sql-database-servers-databases-and-firewall-rules-using-the-azure-portal-and-sql-server-management-studio"></a>SQL Database-zelfstudie: Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure Portal en SQL Server Management Studio

In deze aan de slag-zelfstudie leert u hoe u Azure Portal kunt gebruiken voor het volgende:

* Een nieuwe Azure-resourcegroep maken
* Een logische Azure SQL-server maken
* Eigenschappen van de logische Azure SQL-server bekijken
* Een serverfirewallregel maken
* De AdventureWorks LT-voorbeelddatabase als individuele database maken
* Eigenschappen van de Adventure Works LT-voorbeelddatabase bekijken in Azure

Daarnaast gebruikt u in deze zelfstudie de nieuwste versie van SQL Server Management Studio voor het volgende:

* Verbinding maken met de logische server en de bijbehorende hoofddatabase
* Query's uitvoeren voor de hoofddatabase
* Verbinding maken met de voorbeelddatabase
* Query's uitvoeren voor de voorbeelddatabase

Aan het eind van deze zelfstudie hebt u een voorbeelddatabase en een lege database gemaakt die worden uitgevoerd in een Azure-resourcegroep en zijn gekoppeld aan een logische server. U hebt dan ook een serverfirewallregel geconfigureerd waarmee de serverprincipal wordt aangemeld bij de server vanaf een opgegeven IP-adres (of IP-adresbereik). 

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).

> [!TIP]
> U kunt dezelfde taken uitvoeren in een aan de slag-zelfstudie met [C#](sql-database-get-started-csharp.md) of [PowerShell](sql-database-get-started-powershell.md).
>

## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

> [!NOTE]
> Deze zelfstudie helpt u inzicht te krijgen in de inhoud van de volgende onderwerpen: [Overzicht van de SQL Database-server](sql-database-server-overview.md), [Overzicht SQL-databases](sql-database-overview.md) en [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Aanmelden bij Azure Portal met uw Azure-account
Gebruik uw [bestaande abonnement](https://account.windowsazure.com/Home/Index) en volg deze stappen om verbinding te maken met Azure Portal.

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Een nieuwe logische SQL-server maken in Azure Portal

1. Klik op **Nieuw**, typ **sql server** en klik vervolgens op **ENTER**.

    ![logische sql-server](./media/sql-database-get-started/logical-sql-server.png)
2. Klik op **SQL-server (logische server)**.
   
    ![maken-logische sql-server](./media/sql-database-get-started/create-logical-sql-server.png)
3. Klik op **Maken** om de blade van de nieuwe SQL-server (logische server) te openen.

    ![nieuw-logische sql-server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Geef in het tekstvak Servernaam een geldige naam op voor de nieuwe logische server. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![nieuwe servernaam](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > De volledige geldige naam voor uw nieuwe server is <uw_servernaam>.database.windows.net.
    >
    
4. Geef in het tekstvak Aanmeldgegevens van serverbeheerder een gebruikersnaam op voor de aanmelding voor SQL-verificatie voor deze server. Deze aanmelding wordt de principal-aanmelding op serverniveau genoemd. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![Aanmeldgegevens SQL-beheerder](./media/sql-database-get-started/sql-admin-login.png)
5. Geef in de tekstvakken **Wachtwoord** en **Wachtwoord bevestigen** een wachtwoord op voor het serverprincipal-aanmeldingsaccount. Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.
    
    ![Wachtwoord SQL-beheerder](./media/sql-database-get-started/sql-admin-password.png)
6. Selecteer een abonnement waarmee u toestemming hebt om objecten te maken.

    ![abonnement](./media/sql-database-get-started/subscription.png)
7. Selecteer in het tekstvak Resourcegroep **Nieuwe maken** en geef vervolgens in hetzelfde tekstvak een geldige naam op voor de nieuwe resourcegroep. U kunt ook een bestaande resourcegroep gebruiken. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![nieuwe resourcegroep](./media/sql-database-get-started/new-resource-group.png)

8. Selecteer in het tekstvak **Locatie** een geschikt datacentrum voor uw locatie, bijvoorbeeld 'Australië - oost'.
    
    ![serverlocatie](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > Het tekstvak voor **Toegang van Azure-services tot server toestaan** kan niet worden gewijzigd op deze blade. U kunt deze instelling op de serverfirewallblade wijzigen. Zie [Aan de slag met beveiliging](sql-database-control-access-sql-authentication-get-started.md) voor meer informatie.
    >
    
9. Klik op **Create**.

    ![knop Maken](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-sql-server-properties-in-the-azure-portal"></a>De eigenschappen van de logische SQL-server bekijken in Azure Portal

1. Klik in Azure Portal op **Meer services**.

    ![meer services](./media/sql-database-get-started/more-services.png)
2. Typ **SQL** in het tekstvak Filteren en klik vervolgens op de ster voor SQL-servers om SQL-servers als favoriet in te stellen in Azure. 

    ![favoriet instellen](./media/sql-database-get-started/favorite.png)
3. Klik op de standaardblade op **SQL-servers** om een lijst met SQL-servers in uw Azure-abonnement te openen. 

    ![nieuwe sql-server](./media/sql-database-get-started/new-sql-server.png)

4. Klik op de nieuwe SQL-server om de bijbehorende eigenschappen in Azure Portal weer te geven. In hiernavolgende zelfstudies komt u meer te weten over de beschikbare opties op deze blade.

    ![blade sql-server](./media/sql-database-get-started/sql-server-blade.png)
5. Ga naar Instellingen en klik op **Eigenschappen** om de verschillende eigenschappen van de logische SQL-server weer te geven.

    ![sql-servereigenschappen](./media/sql-database-get-started/sql-server-properties.png)
6. Kopieer de volledige geldige servernaam naar het klembord, zodat u deze later tijdens deze zelfstudie kunt gebruiken.

    ![volledige naam sql-server](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Ga op de blade van de SQL-server naar Instellingen en klik op **Firewall** om de blade Firewall van de SQL-server te openen.

    ![sql-serverfirewall](./media/sql-database-get-started/sql-server-firewall.png)

2. Controleer of het weergegeven IP-adres van de client uw IP-adres is in een internetbrowser naar keuze (vraag "Wat is mijn IP-adres?"). Soms komen ze niet overeen. Dit kan verschillende redenen hebben.

    ![uw IP-adres](./media/sql-database-get-started/your-ip-address.png)

3. Klik als de IP-adressen overeenkomen, op **IP van client toevoegen** in de taakbalk.

    ![IP van client toevoegen](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > U kunt de SQL-databasefirewall op de server naar een enkel IP-adres of een reeks adressen openen. Als u de firewall opent, kunnen SQL-beheerders en -gebruikers zich aanmelden bij elke database op de server waarvoor ze geldige aanmeldgegevens hebben.
    >

4. Klik in de taakbalk op **Opslaan** om deze serverfirewallregel op te slaan. Klik vervolgens op **OK**.

    ![IP van client toevoegen](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Verbinding maken met de SQL-server met SQL Server Management Studio (SSMS)

1. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) voor het downloaden en installeren van de meest recente versie van SSMS, als u dit nog niet hebt gedaan. U wordt in de meest recente versie van SSMS op de hoogte gesteld wanneer er een nieuwe versie kan worden gedownload, zodat u steeds met de nieuwste versie kunt werken.

2. Typ na het installeren **Microsoft SQL Server Management Studio** in het zoekvak van Windows en klik op **Enter** om SSMS te openen:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. Voer in het dialoogvenster Verbinding maken met server de nodige gegevens in om verbinding te maken met uw SQL-server met behulp van SQL Server-verificatie.

    ![verbinding maken met server](./media/sql-database-get-started/connect-to-server.png)
4. Klik op **Verbinden**.

    ![verbonden met server](./media/sql-database-get-started/connected-to-server.png)
5. Vouw in Objectverkenner achtereenvolgens **Databases** **Systeemdatabases** en **hoofd** uit om objecten in de hoofddatabase weer te geven.

    ![hoofddatabase](./media/sql-database-get-started/master-database.png)
6. Klik met de rechtermuisknop op **hoofd** en klik vervolgens op **Nieuwe query**.

    ![query hoofddatabase](./media/sql-database-get-started/query-master-database.png)

8. Typ de volgende query in het queryvenster:

   ```select * from sys.objects```

9.  Klik in de taakbalk op **Uitvoeren** om een lijst met alle systeemobjecten in de hoofddatabase weer te geven.

    ![query systeemobjecten hoofddatabase](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > Zie [Aan de slag met SQL-beveiliging](sql-database-control-access-sql-authentication-get-started.md) om meer over SQL-beveiliging te weten te komen
    >

## <a name="create-new-database-in-the-azure-portal-using-adventure-works-lt-sample"></a>Nieuwe database maken in Azure Portal met behulp van Adventure Works LT-voorbeeld

1. Klik in Azure Portal op de standaardblade op **SQL-databases**.

    ![sql-databases](./media/sql-database-get-started/new-sql-database.png)
2. Klik op de blade SQL-databases op **Toevoegen**.

    ![sql-database toevoegen](./media/sql-database-get-started/add-sql-database.png)
3. Controleer op de blade SQL-databases de ingevulde gegevens.

    ![sql database-blade](./media/sql-database-get-started/sql-database-blade.png)
4. Geef een geldige databasenaam op.

    ![sql-databasenaam](./media/sql-database-get-started/sql-database-name.png)
5. Klik onder Bron selecteren op **Voorbeeld** en klik vervolgens onder Voorbeeld selecteren op **AdventureWorksLT [V12]**.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
6. Geef onder Server een gebruikersnaam en wachtwoord op voor de serverbeheerder.

    ![serveraanmeldgegevens](./media/sql-database-get-started/server-credentials.png)

    > [!NOTE]
    > Wanneer u een database aan een server toevoegt, kunt u ervoor kiezen om deze als individuele database toe te voegen (standaard) of om deze aan een elastische pool toe te voegen. Zie [Elastische pools](sql-database-elastic-pool.md) voor meer informatie over elastische pools.
    >

7. Wijzig de prijscategorie onder Prijscategorie in **Standaard**. U kunt de prijscategorie desgewenst laten verhogen, maar we raden u aan in deze zelfstudie de laagste prijscategorie te gebruiken.

    ![prijscategorie](./media/sql-database-get-started/pricing-tier.png)
8. Klik op **Create**.

    ![knop Maken](./media/sql-database-get-started/create.png)

## <a name="view-database-properties-in-the-azure-portal"></a>Database-eigenschappen weergeven in Azure Portal

1. Klik op de blade SQL-databases op uw nieuwe database om de eigenschappen ervan weer te geven in Azure Portal. In hiernavolgende zelfstudies komt u meer te weten over de beschikbare opties op deze blade. 

    ![blade nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-blade.png)
2. Klik op **Eigenschappen** om meer gegevens over uw database weer te geven.

    ![eigenschappen nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-properties.png)

3. Klik op **Databaseverbindingsreeksen tonen**.

    ![verbindingsreeksen nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-connection-strings.png)
4. Klik op **Overzicht** en klik vervolgens in het deelvenster Essentials op de naam van uw server.
    
    ![essentials-deelvenster nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-essentials-pane.png)
5. U ziet uw nieuwe database in het deelvenster Essentials van uw server staan.

    ![essentials-deelvenster nieuwe voorbeelddatabase op server](./media/sql-database-get-started/new-sample-db-server-essentials-pane.png)

## <a name="connect-and-query-sample-database-using-sql-server-management-studio"></a>Voorbeelddatabases koppelen en er query's op uitvoeren met SQL Server Management Studio

1. Schakel naar SQL Server Management Studio en klik in Objectverkenner op **Databases**. Klik vervolgens op **Vernieuwen** in de taakbalk om de voorbeelddatabase weer te geven.

    ![nieuwe voorbeelddatabase met ssms](./media/sql-database-get-started/new-sample-db-ssms.png)
2. Vouw in Objectverkenner uw nieuwe database uit om de bijbehorende objecten weer te geven.

    ![objecten nieuwe voorbeelddatabase met ssms](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Klik met de rechtermuisknop op uw voorbeelddatabase en klik vervolgens op **Nieuwe query**.

    ![query nieuwe voorbeelddatabase met ssms](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. Typ de volgende query in het queryvenster:

   ```select * from sys.objects```
   
9.  Klik in de taakbalk op **Uitvoeren** om een lijst met alle systeemobjecten in de voorbeelddatabase weer te geven.

    ![systeemobjecten query nieuwe voorbeelddatabase met ssms](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-new-blank-database-using-sql-server-management-studio"></a>Een nieuwe lege database maken met SQL Server Management Studio

1. Klik in Objectverkenner met de rechtermuisknop op **Databases** en klik vervolgens op **Nieuwe database**.

    ![nieuwe lege database met ssms](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > U kunt met SSMS ook een databasescript maken waarmee u een nieuwe database kunt maken met Transact-SQL.
    >

2. Geef in het dialoogvenster Nieuwe database een naam op voor de database in het tekstvak Databasenaam. 

    ![naam nieuwe lege database met ssms](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. Klik in het dialoogvenster Nieuwe database op **Opties** en wijzig de editie in **Standaard**.

    ![opties nieuwe lege database met ssms](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Bekijk in het dialoogvenster de andere opties die u kunt aanpassen voor een Azure SQL-database. Zie [Database maken](https://msdn.microsoft.com/library/dn268335.aspx) voor meer informatie over deze opties.
    >

4. Klik op **OK** om een lege database te maken.
5. Wanneer dit is voltooid, vernieuwt u het databaseknooppunt in Objectverkenner om de nieuwe lege database weer te geven. 

    ![nieuwe lege database in objectverkenner](./media/sql-database-get-started/new-blank-database-object-explorer.png)

> [!TIP]
> U kunt geld besparen tijdens de zelfstudie door databases die u niet gebruikt, te verwijderen. U kunt databases uit de Standaard-editie binnen zeven dagen herstellen. Verwijder echter geen servers. Als u dit doet, kunt u de server en bijbehorende verwijderde databases niet herstellen.
>


## <a name="next-steps"></a>Volgende stappen
Nu u deze zelfstudie hebt voltooid, wilt u mogelijk aanvullende zelfstudies volgen die aansluiten op wat u in deze zelfstudie hebt geleerd. 

* Zie [Aan de slag met beveiliging](sql-database-control-access-sql-authentication-get-started.md) voor meer informatie over de beveiliging van Azure SQL Database.
* Bent u bekend met Excel? Ontdek dan hoe u [verbinding kunt maken met een SQL Database in Azure via Excel](sql-database-connect-excel.md).
* Als u wilt gaan coderen, kiest u uw programmeertaal in [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md).
* Als u uw on-premises SQL Server-database wilt verplaatsen naar Azure, raadpleegt u [Een database migreren naar SQL Database](sql-database-cloud-migrate.md).
* Als u met het opdrachtregelprogramma BCP enkele gegevens vanuit een csv-bestand in een nieuwe tabel wilt laden, leest u [Met BCP gegevens vanuit een csv-bestand in SQL Database laden](sql-database-load-from-csv-with-bcp.md).
* Als u tabellen en andere objecten wilt maken, raadpleegt u het onderwerp 'Een tabel maken' in [Een tabel maken](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Aanvullende bronnen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor een technisch overzicht.
- Zie [Prijs van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor informatie over prijzen.




<!--HONumber=Jan17_HO3-->


