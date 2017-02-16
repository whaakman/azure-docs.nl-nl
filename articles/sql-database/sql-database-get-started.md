---
title: 'Snel starten: uw eerste Azure SQL Database | Microsoft Docs'
description: Meer informatie over hoe u met behulp van Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en databases maakt. U leert ook SQL Server Management Studio gebruiken met Azure SQL Database.
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
ms.date: 02/04/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 535b16490bb601070c7d2a7135f3d69aa898be1f
ms.openlocfilehash: 10a128ea56ea014cf72036f71ec97202691bec94


---
# <a name="quick-start-tutorial-your-first-azure-sql-database"></a>Snel starten-zelfstudie: uw eerste Azure SQL Database

In deze zelfstudie leert u:

* [Een nieuwe logische server maken](sql-database-get-started.md#create-a-new-logical-sql-server) 
* [De eigenschappen van de logische server bekijken](sql-database-get-started.md#view-the-logical-server-properties) 
* [Een serverfirewallregel maken](sql-database-get-started.md#create-a-server-level-firewall-rule) 
* [Verbinding maken met de server via SSMS](sql-database-get-started.md#connect-to-the-server-with-ssms) 
* [Een database maken met voorbeeldgegevens](sql-database-get-started.md#create-a-database-with-sample-data) 
* [Eigenschappen van de database bekijken](sql-database-get-started.md#view-the-database-properties) 
* [Een query toepassen op de database in Azure Portal](sql-database-get-started.md#query-the-database-in-the-azure-portal) 
* [Verbinding maken met en een query toepassen op de database via SSMS](sql-database-get-started.md#connect-and-query-the-database-with-ssms) 
* [Een lege database maken met SSMS](sql-database-get-started.md#create-a-blank-database-with-ssms) 
* [Problemen met verbindingen oplossen](sql-database-get-started.md#troubleshoot-connectivity) 
* [Een database verwijderen](sql-database-get-started.md#delete-a-single-database) 


Aan het eind van deze zelfstudie hebt u een voorbeelddatabase en een lege database gemaakt die worden uitgevoerd in een Azure-resourcegroep en zijn gekoppeld aan een logische server. U hebt dan ook firewallregels op twp-serverniveau geconfigureerd waarmee de server-principal wordt aangemeld bij de server vanaf twee opgegeven IP-adressen. 

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).

> [!TIP]
> U kunt dezelfde taken uitvoeren met [C#](sql-database-get-started-csharp.md) of [PowerShell](sql-database-get-started-powershell.md).
>

## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

> [!NOTE]
> Deze zelfstudie helpt u inzicht te krijgen in de inhoud van de volgende onderwerpen: [Overzicht van de SQL Database-server](sql-database-server-overview.md), [Overzicht SQL-databases](sql-database-overview.md) en [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-with-your-azure-account"></a>Meld u bij Azure Portal aan
met uw [Azure-account](https://account.windowsazure.com/Home/Index) en volg deze stappen om verbinding te maken met Azure Portal.

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Een nieuwe logische SQL-server maken

Volg de stappen in deze procedure om met Azure Portal een nieuwe logische server te maken in de gewenste regio.

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
5. Geef in de tekstvakken **Wachtwoord** en **Wachtwoord bevestigen** een wachtwoord op voor het server-principal-aanmeldingsaccount. Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.
    
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

## <a name="view-the-logical-server-properties"></a>De eigenschappen van de logische server bekijken

Volg de stappen in deze procedure om de servereigenschappen te bekijken met Azure Portal. U hebt de volledig gekwalificeerde servernaam nodig om in een latere procedure verbinding te maken met de server. 

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

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

Volg de stappen in deze procedure om een nieuwe serverfirewallregel te maken met Azure Portal waarmee u in de volgende procedure verbinding kunt maken met uw server via SQL Server Management Studio.

1. Ga op de blade van de SQL-server naar Instellingen en klik op **Firewall** om de blade Firewall van de SQL-server te openen.

    ![sql-serverfirewall](./media/sql-database-get-started/sql-server-firewall.png)

2. Klik op **IP van client toevoegen** op de werkbalk.

    ![IP van client toevoegen](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > U kunt de SQL-databasefirewall op de server naar een enkel IP-adres of een reeks adressen openen. Als u de firewall opent, kunnen SQL-beheerders en -gebruikers zich aanmelden bij elke database op de server waarvoor ze geldige aanmeldgegevens hebben.
    >

4. Klik in de taakbalk op **Opslaan** om deze serverfirewallregel op te slaan. Klik vervolgens op **OK**.

    ![IP van client toevoegen](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Verbinding maken met de server via SSMS

Volg de stappen in deze procedure om verbinding te maken met de logische SQL-server met behulp van SQL Server Management Studio.

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

## <a name="create-a-database-with-sample-data"></a>Een database maken met voorbeeldgegevens

Volg de stappen in deze procedure om een database met voorbeeldgegevens te maken met behulp van Azure Portal. U maakt deze database gekoppeld aan de logische server die u eerder hebt gemaakt. Als de Basic-servicelaag niet beschikbaar is in de regio waarin u uw server hebt gemaakt, verwijdert u uw server en maakt u deze opnieuw in een andere regio. Zie de laatste procedure in deze zelfstudie voor het verwijderen.

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

## <a name="view-the-database-properties"></a>Eigenschappen van de database bekijken

Volg de stappen in deze procedure om een query op de database toe te passen met behulp van Azure Portal.

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

## <a name="query-the-database-in-the-azure-portal"></a>Een query toepassen op de database in Azure Portal

Volg de stappen in deze procedure om een query op een database toe te passen met behulp van de queryeditor in Azure Portal. De query toont de objecten in de database.

1. Klik op de blade SQL-databases op **Hulpprogramma's** in de werkbalk.

    ![hulpprogramma's](./media/sql-database-get-started/tools.png)
2. Klik op de blade Hulpprogramma's op **Queryeditor (preview)**.

    ![queryeditor](./media/sql-database-get-started/query-editor.png)
3. Schakel het selectievakje in om te bevestigen dat de queryeditor een previewfunctie is en klik vervolgens op **OK**.
4. Klik op de blade **Queryeditor** op **Aanmelden**.

    ![blade queryeditor](./media/sql-database-get-started/query-editor-blade.png)
5. Controleer het autorisatietype en de aanmelding en geef vervolgens het wachtwoord op voor deze aanmelding. 

    ![aanmelding queryeditor](./media/sql-database-get-started/query-editor-login.png)
6. Klik op **OK** om u aan te melden.
7. Wanneer er een aanmeldingsfout wordt weergegeven waarin staat dat uw client geen toestemming heeft om zich aan te melden omdat er een firewallregel ontbreekt voor het IP-adres van uw client, kopieert u het IP-adres van uw client in het foutvenster en maakt u een serverfirewallregel op de SQL-serverblade voor deze database.

    ![fout queryeditor](./media/sql-database-get-started/query-editor-error.png)
8. Herhaal de vorige zes stappen om u aan te melden bij uw database.
9. Typ na het aanmelden de volgende query in het queryvenster:

   ```select * from sys.objects```

    ![queryeditor-query](./media/sql-database-get-started/query-editor-query.png) 10 Klik op **Uitvoeren**.
11. Bekijk de resultaten van de query in het deelvenster **Resultaten**.

    ![resultaten queryeditor](./media/sql-database-get-started/query-editor-results.png)

## <a name="connect-and-query-the-database-with-ssms"></a>Verbinding maken met en een query toepassen op de database via SSMS

Volg de stappen in deze procedure om verbinding te maken met de database met behulp van SQL Server Management Studio en vervolgens een query toe te passen op de voorbeeldgegevens om de objecten in de database weer te geven.

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

## <a name="create-a-blank-database-with-ssms"></a>Een nieuwe lege database maken met SSMS

Volg de stappen in deze procedure om op de logische server een nieuwe database te maken met behulp van SQL Server Management Studio.

1. Klik in Objectverkenner met de rechtermuisknop op **Databases** en klik vervolgens op **Nieuwe database**.

    ![nieuwe lege database met ssms](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > U kunt met SSMS ook een databasescript maken waarmee u een nieuwe database kunt maken via Transact-SQL.
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

## <a name="troubleshoot-connectivity"></a>Problemen met verbindingen oplossen

> [!IMPORTANT]
> Als u problemen met de netwerkverbinding hebt, raadpleegt u [Verbindingsproblemen](sql-database-troubleshoot-common-connection-issues.md).
> 

## <a name="delete-a-single-database"></a>Een individuele database verwijderen

Volg de stappen in deze procedure om een individuele database te verwijderen met behulp van Azure Portal.

1. Klik op de blade voor uw SQL-database in Azure Portal op **Verwijderen**.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Klik op **Ja** om te bevestigen dat u de database permanent wilt verwijderen.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Tijdens de retentieperiode voor uw database kunt u deze herstellen vanuit de automatische back-ups die vanaf de server zijn gestart. U kunt databases uit de Standaard-editie binnen zeven dagen herstellen. Verwijder echter geen servers. Als u dit doet, kunt u de server en bijbehorende verwijderde databases niet herstellen. Zie [Learn about SQL Database backups](sql-database-automated-backups.md) (Informatie over SQL Database-back-ups) voor meer informatie over databaseback-ups. Zie [Database recovery](sql-database-recovery-using-backups.md) (Databaseherstel) voor meer informatie over het herstellen van een database vanuit back-ups. Zie [Restore a deleted Azure SQL database - Azure Portal](sql-database-restore-deleted-database-portal.md) (Een verwijderde Azure SQL-database herstellen - Azure Portal) voor een zelfstudie over het herstellen van een verwijderde database.
>


## <a name="next-steps"></a>Volgende stappen
Nu u deze zelfstudie hebt voltooid, wilt u mogelijk aanvullende zelfstudies volgen die aansluiten op wat u in deze zelfstudie hebt geleerd. 

- Zie [SQL-verificatie en -autorisatie](sql-database-control-access-sql-authentication-get-started.md) voor een zelfstudie om aan de slag te gaan met SQL Server-verificatie
- Zie [AAD-verificatie en -autorisatie](sql-database-control-access-aad-authentication-get-started.md) voor een zelfstudie om aan de slag te gaan met Azure Active Directory-verificatie
* Zie [Openbaar voorbeeld: interactieve query's voor SQL-databases](https://azure.microsoft.com/en-us/updates/azure-sql-database-public-preview-t-sql-editor/) als u een query wilt uitvoeren voor de voorbeelddatabase in Azure Portal
* Bent u bekend met Excel? Ontdek dan hoe u [verbinding kunt maken met een SQL Database in Azure via Excel](sql-database-connect-excel.md).
* Als u wilt gaan coderen, kiest u uw programmeertaal in [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md).
* Als u uw on-premises SQL Server-database wilt verplaatsen naar Azure, raadpleegt u [Een database migreren naar SQL Database](sql-database-cloud-migrate.md).
* Als u met het opdrachtregelprogramma BCP enkele gegevens vanuit een CSV-bestand in een nieuwe tabel wilt laden, leest u [Met BCP gegevens vanuit een CSV-bestand in SQL Database laden](sql-database-load-from-csv-with-bcp.md).
* Als u tabellen en andere objecten wilt maken, raadpleegt u het onderwerp 'Een tabel maken' in [Een tabel maken](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Aanvullende bronnen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor een technisch overzicht.
- Zie [Prijs van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor informatie over prijzen.




<!--HONumber=Feb17_HO1-->


