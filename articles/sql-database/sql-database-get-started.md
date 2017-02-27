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
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 166a9d7032bb75188a790bea1724aefd194dcefa
ms.openlocfilehash: 36afd5c8bccb080ae3aaf1b4975d317b9087a3b3


---
# <a name="create-connect-to-and-query-your-first-azure-sql-databases-in-the-azure-portal-and-using-ssms"></a>Uw eerste Azure SQL-databases maken, er verbinding mee maken en er query's op uitvoeren via Azure Portal en met SSMS

In deze zelfstudie leert u hoe u Azure SQL-databases maakt, hoe u er verbinding mee maakt en hoe u er query's op uitvoert via Azure Portal en met SQL Server Management Studio. Wanneer u deze zelfstudie hebt voltooid:

* Hebt u een resourcegroep gemaakt met een logische server, een firewallregel op serverniveau en twee databases.
* Weet u hoe u de server- en database-eigenschappen kunt bekijken in Azure Portal en met SQL Server Management Studio.
* Weet u hoe u in Azure Portal en met SQL Server Management Studio een query kunt uitvoeren op de database.

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).

> [!TIP]
> U leert ook hoe u een Azure SQL-database maakt, hoe u er verbinding mee maakt en hoe u er query's op uitvoert via [PowerShell](sql-database-get-started-powershell.md) of [C#](sql-database-get-started-csharp.md).
>

> [!NOTE]
> Deze zelfstudie helpt u inzicht te krijgen in de inhoud van de volgende onderwerpen: [Overzicht van de SQL Database-server](sql-database-server-overview.md), [Overzicht SQL-databases](sql-database-overview.md) en [Overzicht van de firewallregels voor Azure SQL Database](sql-database-firewall-configure.md). Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor informatie over de service SQL Database.
>  

## <a name="prerequisites"></a>Vereisten

* **Een Azure-account**. U kunt [een gratis Azure-account openen](https://azure.microsoft.com/free/) of [uw voordelen als Visual Studio-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Azure-machtigingen voor maken**. U moet met Azure Portal verbinding kunnen maken met een account dat is gekoppeld aan de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* **SQL Server Management Studio**. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) voor het downloaden en installeren van de meest recente versie van SQL Server Management Studio (SSMS). Gebruik altijd de nieuwste versie van SSMS bij het verbinden met Azure SQL Database; er worden namelijk voortdurend nieuwe mogelijkheden geïntroduceerd.

### <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

De stappen in deze procedure bevatten informatie over het verbinden met Azure Portal middels uw [Azure-account](https://account.windowsazure.com/Home/Index).

1. Open de browser van uw keuze en maak verbinding met [Azure Portal](https://portal.azure.com/).
2. Meld u aan bij [Azure Portal](https://portal.azure.com/).
3. Zodra de **aanmeldingspagina** wordt weergegeven, typt u de referenties voor uw abonnement.
   
   ![Aanmelden](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Een nieuwe logische SQL-server maken

In de stappen in deze procedure ziet u hoe u een logische server maakt in Azure Portal in de gewenste regio. Een logische server is een object waarin u SQL-databases maakt en een object waarin u firewallregels maakt om gebruikers toestemming te geven om verbinding te maken via de Azure SQL Database-firewall. 

1. Klik op **Nieuw**, typ **sql server** en klik vervolgens op **ENTER**.

    ![logische sql-server](./media/sql-database-get-started/logical-sql-server.png)
2. Klik op **SQL-server (logische server)**.
   
    ![maken-logische sql-server](./media/sql-database-get-started/create-logical-sql-server.png)
3. Klik op **Maken** om alleen de blade van de nieuwe SQL-server (logische server) te openen.

    ![nieuw-logische sql-server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Geef in het tekstvak **Servernaam** een geldige naam op voor de nieuwe logische server. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![nieuwe servernaam](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > De volledig gekwalificeerde naam voor uw nieuwe server moet wereldwijd uniek zijn en ziet er als volgt uit: **<uw_servernaam>.database.windows.net**. U kunt deze volledig gekwalificeerde servernaam verderop in deze zelfstudie verbinden met uw server en databases.
    >
    
4. Geef in het tekstvak **Aanmeldgegevens van serverbeheerder** een gebruikersnaam op voor de aanmelding voor SQL-verificatie voor deze server. Deze aanmelding wordt de principal-aanmelding op serverniveau genoemd. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![Aanmeldgegevens SQL-beheerder](./media/sql-database-get-started/sql-admin-login.png)
5. Geef in de tekstvakken **Wachtwoord** en **Wachtwoord bevestigen** een wachtwoord op voor het server-principal-aanmeldingsaccount. Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.
    
    ![Wachtwoord SQL-beheerder](./media/sql-database-get-started/sql-admin-password.png)
6. Selecteer in het vervolgkeuzemenu **Abonnement** een abonnement waarmee u toestemming hebt om objecten te maken.

    ![abonnement](./media/sql-database-get-started/subscription.png)
7. Onder het tekstvak **Resourcegroep** selecteert u **Nieuw** en geeft u een geldige naam op voor de nieuwe resourcegroep. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![nieuwe resourcegroep](./media/sql-database-get-started/new-resource-group.png)

8. In het tekstvak **Locatie** selecteert u een datacenter waarin u de logische server wilt maken.
    
    ![serverlocatie](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > Het tekstvak voor **Toegang van Azure-services tot server toestaan** kan niet worden gewijzigd op deze blade. U kunt deze instelling op de serverfirewallblade wijzigen. Zie [Aan de slag met beveiliging](sql-database-control-access-sql-authentication-get-started.md) voor meer informatie.
    >
    
9. Schakel het selectievakje bij **Vastmaken aan dashboard** in.

10. Klik op **Maken** om dit script te implementeren in Azure om een logische server te maken.

    ![knop Maken](./media/sql-database-get-started/create.png)

11. Wanneer uw server is gemaakt, controleert u de eigenschappen van de server die standaard worden weergegeven. 

    ![blade sql-server](./media/sql-database-get-started/sql-server-blade.png)
12. Klik op **Eigenschappen** om aanvullende eigenschappen van de logische SQL-server weer te geven.

    ![sql-servereigenschappen](./media/sql-database-get-started/sql-server-properties.png)
13. Kopieer de volledige geldige servernaam naar het klembord, zodat u deze later tijdens deze zelfstudie kunt gebruiken.

    ![volledige naam sql-server](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

In de stappen in deze procedure ziet u hoe u een firewallregel op serverniveau maakt in Azure Portal. Met een Azure SQL Database-firewall worden standaard externe verbindingen met uw logische server en de databases voorkomen. Als u verbinding wilt maken met uw server, moet u een firewallregel maken voor het IP-adres van de computer die u in de volgende procedure gaat gebruiken om verbinding te maken. Zie [Overzicht van de firewallregels voor SQL Database](sql-database-firewall-configure.md) voor meer informatie.

1. Klik op de blade van de SQL-server op **Firewall** om de blade Firewall van de SQL-server te openen. Het IP-adres van de clientcomputer wordt weergegeven.

    ![sql-serverfirewall](./media/sql-database-get-started/sql-server-firewall.png)

2. Klik op **IP van client toevoegen** op de werkbalk om een firewallregel te maken voor uw huidige IP-adres.

    ![IP van client toevoegen](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > U kunt een firewallregel maken voor één IP-adres of voor een volledige reeks adressen. Als u de firewall opent, kunnen SQL-beheerders en -gebruikers zich aanmelden bij elke database op de server waarvoor ze geldige aanmeldgegevens hebben.
    >

4. Klik in de werkbalk op **Opslaan** om deze firewallregel op serverniveau op te slaan. Klik vervolgens op **OK** in het dialoogvenster Voltooid.

    ![voltooid](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Verbinding maken met de server via SSMS

Volg de stappen in deze procedure om verbinding te maken met de logische SQL-server met behulp van SQL Server Management Studio. SSMS is het primaire hulpprogramma dat DBA's gebruiken voor het beheren van SQL-servers en -databases.

1. Open SQL Server Management Studio (typ **Microsoft SQL Server Management Studio** in het zoekvak van Windows en klik op **Enter** om SSMS te openen).

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. In het dialoogvenster **Verbinding maken met server** voert u de volledig gekwalificeerde naam in uit de vorige procedure. Selecteer SQL Server-verificatie en geef de gebruikersnaam en het wachtwoord op die u tijdens het inrichten van de server hebt opgegeven.

    ![verbinding maken met server](./media/sql-database-get-started/connect-to-server.png)
4. Klik op **Verbinden** om verbinding te maken en open Objectverkenner in SSMS.

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
    > Zie [Aan de slag met SQL-verificatie](sql-database-control-access-sql-authentication-get-started.md) om aan de slag te gaan met SQL-beveiliging
    >

## <a name="create-a-database-with-sample-data"></a>Een database maken met voorbeeldgegevens

Met de stappen in deze procedure leert u hoe u een database met voorbeeldgegevens maakt in Azure Portal. Deze database wordt gekoppeld aan de logische server die u eerder hebt gemaakt. 

1. Klik in Azure Portal op de standaardblade op **SQL-databases**.

    ![sql-databases](./media/sql-database-get-started/new-sql-database.png)
2. Klik op de blade SQL-databases op **Toevoegen**. 

    ![sql-database toevoegen](./media/sql-database-get-started/add-sql-database.png)

    ![sql database-blade](./media/sql-database-get-started/sql-database-blade.png)
3. Geef in het tekstvak **Databasenaam** een geldige databasenaam op.

    ![sql-databasenaam](./media/sql-database-get-started/sql-database-name.png)
4. Bij **Bron selecteren** selecteert u **Voorbeeld (AdventureWorksLT)**.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
5. Bij **Server** controleert u of uw server is geselecteerd. Wanneer u een database aan een server toevoegt, kunt u ervoor kiezen om deze als individuele database toe te voegen (standaard) of om deze aan een elastische pool toe te voegen. Zie [Elastische pools](sql-database-elastic-pool.md) voor meer informatie over elastische pools.

6. Bij **Prijscategorie** wijzigt u de prijscategorie naar **Basic** en klikt u op **Selecteren**. U kunt de prijscategorie desgewenst verhogen, maar het wordt aangeraden om voor deze zelfstudie de laagste prijscategorie te gebruiken.

    ![prijscategorie](./media/sql-database-get-started/pricing-tier.png)
7. Schakel **Vastmaken aan dashboard** in en klik op **Maken**.

    ![knop Maken](./media/sql-database-get-started/create.png)

8. Wanneer de database is gemaakt, bekijkt u de eigenschappen in Azure Portal. In hiernavolgende zelfstudies komt u meer te weten over de beschikbare opties op deze blade. 

    ![blade nieuwe voorbeelddatabase](./media/sql-database-get-started/new-sample-db-blade.png)

## <a name="query-the-database-in-the-azure-portal"></a>Een query toepassen op de database in Azure Portal

In de stappen in deze procedure ziet u hoe u rechtstreeks vanuit Azure Portal een query uitvoert in de database. 

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
7. Na het verifiëren voert u de volgende query in in het queryvenster. Klik dan op **Uitvoeren**.

   ```select * from sys.objects```

    ![query editor query](./media/sql-database-get-started/query-editor-query.png)

8. Bekijk de resultaten van de query in het deelvenster **Resultaten**.

    ![resultaten queryeditor](./media/sql-database-get-started/query-editor-results.png)

## <a name="query-the-database-with-ssms"></a>Een query uitvoeren voor de database via SSMS

In de stappen in deze procedure ziet u hoe u verbinding maakt met de database met behulp van SQL Server Management Studio en hoe u vervolgens een query toepast op de voorbeeldgegevens om de objecten in de database weer te geven.

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

Volg de stappen in deze procedure om een nieuwe database te maken met behulp van SQL Server Management Studio.

1. Klik in Objectverkenner met de rechtermuisknop op **Databases** en klik vervolgens op **Nieuwe database**.

    ![nieuwe lege database met ssms](./media/sql-database-get-started/new-blank-database-ssms.png)

2. Geef in het dialoogvenster **Nieuwe database** een naam op voor de database in het tekstvak Databasenaam. 

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

U ontvangt foutberichten wanneer het verbinden met Azure SQL Database mislukt. De verbindingsproblemen kunnen worden veroorzaakt door de herconfiguratie van de SQL Azure-database, door de firewallinstellingen, door een verbindingstime-out of door onjuiste aanmeldingsgegevens. Zie [Verbindingsproblemen oplossen met Microsoft Azure SQL Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) voor hulp bij het oplossen van verbindingsproblemen.

## <a name="delete-a-single-database-in-the-azure-portal"></a>Een individuele database verwijderen in Azure Portal

Volg de stappen in deze procedure om een individuele database te verwijderen met behulp van Azure Portal.

1. Klik op de blade SQL-databases in Azure Portal op de database die u wilt verwijderen. 
2.  Klik op **Verwijderen** bij uw SQL-database.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Klik op **Ja** om te bevestigen dat u de database permanent wilt verwijderen.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Tijdens de retentieperiode voor uw database kunt u deze herstellen vanuit de automatische back-ups die vanaf de server zijn gestart (mits u de server zelf niet verwijdert). U kunt databases uit de Standaard-editie binnen zeven dagen herstellen. Bij alle andere edities kunt u ze binnen 35 dagen herstellen. Als u de server zelf ook verwijdert, kunt u de server en bijbehorende verwijderde databases niet herstellen. Zie [Learn about SQL Database backups](sql-database-automated-backups.md) (Informatie over SQL Database-back-ups) voor meer informatie over databaseback-ups. Zie [Database recovery](sql-database-recovery-using-backups.md) (Databaseherstel) voor meer informatie over het herstellen van een database vanuit back-ups. Zie [Restore a deleted Azure SQL database - Azure portal](sql-database-restore-deleted-database-portal.md) (Een verwijderde Azure SQL-database herstellen - Azure Portal) voor instructies voor het herstellen van een verwijderde database.
>


## <a name="next-steps"></a>Volgende stappen
Nu u deze zelfstudie hebt voltooid, wilt u mogelijk aanvullende zelfstudies volgen die aansluiten op wat u in deze zelfstudie hebt geleerd. 

- Zie [SQL-verificatie en -autorisatie](sql-database-control-access-sql-authentication-get-started.md) voor een zelfstudie om aan de slag te gaan met SQL Server-verificatie
- Zie [AAD-verificatie en -autorisatie](sql-database-control-access-aad-authentication-get-started.md) voor een zelfstudie om aan de slag te gaan met Azure Active Directory-verificatie
* Zie [Openbaar voorbeeld: interactieve query's voor SQL-databases](https://azure.microsoft.com/updates/azure-sql-database-public-preview-t-sql-editor/) als u een query wilt uitvoeren voor de voorbeelddatabase in Azure Portal
* Bent u bekend met Excel? Ontdek dan hoe u [verbinding kunt maken met een SQL Database in Azure via Excel](sql-database-connect-excel.md).
* Als u wilt gaan coderen, kiest u uw programmeertaal in [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md).
* Als u uw on-premises SQL Server-database wilt verplaatsen naar Azure, raadpleegt u [Een database migreren naar SQL Database](sql-database-cloud-migrate.md).
* Als u met het opdrachtregelprogramma BCP enkele gegevens vanuit een CSV-bestand in een nieuwe tabel wilt laden, leest u [Met BCP gegevens vanuit een CSV-bestand in SQL Database laden](sql-database-load-from-csv-with-bcp.md).
* Als u tabellen en andere objecten wilt maken, raadpleegt u het onderwerp 'Een tabel maken' in [Een tabel maken](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Aanvullende bronnen

- Zie [Wat is SQL Database?](sql-database-technical-overview.md) voor een technisch overzicht.
- Zie [Prijs van SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor informatie over prijzen.




<!--HONumber=Feb17_HO3-->


