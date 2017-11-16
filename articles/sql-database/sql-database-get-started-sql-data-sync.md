---
title: Instellen van Azure SQL-gegevenssynchronisatie (Preview) | Microsoft Docs
description: Deze zelfstudie ziet u het instellen van Azure SQL-gegevenssynchronisatie (Preview)
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: b356bc9db9e883c2514953b516d6dd51c1807610
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="set-up-sql-data-sync-preview"></a>Instellen van het synchroniseren van de SQL-gegevens (Preview)
In deze zelfstudie leert u het instellen van het synchroniseren van Azure SQL-gegevens door te maken van een hybride-groep voor synchronisatie met Azure SQL Database- en SQL Server-exemplaren. De nieuwe groep voor synchronisatie is volledig geconfigureerd en gesynchroniseerd volgens de planning die u instelt.

Deze zelfstudie wordt ervan uitgegaan dat er ten minste enige ervaring met SQL-Database en SQL Server. 

Zie voor een overzicht van de SQL-gegevenssynchronisatie [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

Voor volledige PowerShell voorbeelden van het synchroniseren van de SQL-gegevens configureren, Zie de volgende artikelen:
-   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
-   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>Stap 1: het maken van de groep voor synchronisatie

### <a name="locate-the-data-sync-settings"></a>Zoek de instellingen van het synchroniseren van gegevens

1.  Ga in uw browser naar de Azure-portal.

2.  Zoek in de portal voor uw SQL-databases van het Dashboard of van het SQL-Databases-pictogram op de werkbalk.

    ![Lijst met Azure SQL-databases](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Op de **SQL-databases** pagina, selecteert u de bestaande SQL-database die u wilt gebruiken als de hub-database voor het synchroniseren van gegevens. De pagina van de SQL-database wordt geopend.

4.  Selecteer op de pagina SQL-database voor de geselecteerde database **synchroniseren met andere databases**. De pagina voor het synchroniseren van gegevens wordt geopend.

    ![Met de optie van andere databases synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Maak een nieuwe groep voor synchronisatie

1.  Selecteer op de pagina gegevenssynchronisatie **groep voor synchronisatie met nieuwe**. De **groep voor synchronisatie met nieuwe** pagina wordt geopend met stap 1 **groep maken-sync**, gemarkeerde. De **groep voor synchronisatie maken** pagina ook wordt geopend.

2.  Op de **groep voor synchronisatie maken** pagina, het volgende doen:

    1.  In de **Sync groepsnaam** en voer een naam voor de nieuwe groep voor synchronisatie.

    2.  In de **metagegevens synchronisatiedatabase** sectie, kies of maak een nieuwe database (aanbevolen) of een bestaande database gebruiken.

        > [!NOTE]
        > Microsoft raadt aan dat u een nieuwe, lege database om te gebruiken als de synchronisatiedatabase metagegevens maken. Synchroniseren van gegevens maakt tabellen in deze database en voert een regelmatige werkbelasting. Deze database wordt automatisch gedeeld als de synchronisatiedatabase metagegevens voor alle van de synchronisatie-groepen in de geselecteerde regio. U kunt de synchronisatiedatabase metagegevens of van de naam niet wijzigen zonder slepen en neerzetten.

        Als u hebt gekozen **nieuwe database**, selecteer **nieuwe database maken.** De **SQL-Database** pagina wordt geopend. Op de **SQL-Database** pagina, Geef een naam en het configureren van de nieuwe database. Selecteer vervolgens **OK**.

        Als u hebt gekozen **bestaande database gebruiken**, selecteert u de database uit de lijst.

    3.  In de **automatische synchronisatie** sectie, selecteert u eerst **op** of **uit**.

        Als u hebt gekozen **op**, in de **Synchronisatiefrequentie** sectie, voer een getal in en selecteert u seconden, minuten, uren of dagen.

        ![Geef de synchronisatiefrequentie](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  In de **conflictoplossing** sectie, selecteert u 'Hub wins' of "WINS-lid."

        ![Opgeven hoe conflicten worden opgelost](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Selecteer **OK** en wacht tot de nieuwe groep voor synchronisatie kan worden gemaakt en geïmplementeerd.

## <a name="step-2---add-sync-members"></a>Stap 2: synchronisatie leden toevoegen

Nadat de nieuwe groep voor synchronisatie is gemaakt en geïmplementeerd, stap 2 **sync leden toevoegen**, is gemarkeerd in de **groep voor synchronisatie met nieuwe** pagina.

In de **Hub Database** sectie, voert u de bestaande referenties voor de SQL-Database-server waarop de hub-database zich bevindt. Voer geen *nieuwe* referenties in deze sectie.

![Hub-database is toegevoegd aan de groep te synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Toevoegen van een Azure SQL Database

In de **Liddatabase** sectie optioneel een Azure SQL Database toevoegen aan de groep voor synchronisatie door te selecteren **toevoegen van een Azure-Database**. De **Azure-Database configureren** pagina wordt geopend.

Op de **Azure-Database configureren** pagina, het volgende doen:

1.  In de **Sync lidnaam** veld, Geef een naam op voor het nieuwe lid van de synchronisatie. Deze naam verschilt van de naam van de database zelf.

2.  In de **abonnement** veld, selecteert u de gekoppelde Azure-abonnement voor facturering.

3.  In de **Azure SQL Server** veld, selecteert u de bestaande SQL-databaseserver.

4.  In de **Azure SQL Database** veld, selecteert u de bestaande SQL-database.

5.  In de **Sync richtingen** veld, selecteer bidirectionele synchronisatie, naar de Hub of van de Hub.

    ![Het toevoegen van een nieuwe SQL-Database sync lid](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  In de **gebruikersnaam** en **wachtwoord** velden, geef de bestaande referenties voor de SQL-Database-server waarop de liddatabase zich bevindt. Voer geen *nieuwe* referenties in deze sectie.

7.  Selecteer **OK** en wachten op het nieuwe lid van de synchronisatie kan worden gemaakt en geïmplementeerd.

    ![Nieuw lid van de SQL-Database-synchronisatie is toegevoegd](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a>Toevoegen van een lokale SQL Server-database.

In de **Liddatabase** sectie eventueel een lokale SQL-Server toevoegen aan de groep voor synchronisatie door te selecteren **toevoegen van een On-Premises Database**. De **configureren On-Premises** pagina wordt geopend.

Op de **configureren On-Premises** pagina, het volgende doen:

1.  Selecteer **kiezen de Gateway van de Agent Sync**. De **Sync-Agent Selecteer** pagina wordt geopend.

    ![Kies de synchronisatie-agent-gateway](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Op de **kiezen de Gateway van de Agent Sync** pagina, kies of u wilt het gebruik van een bestaande agent of maak een nieuwe agent.

    Als u hebt gekozen **bestaande agents**, selecteert u de bestaande agent in de lijst.

    Als u hebt gekozen **maken van een nieuwe agent**, het volgende doen:

    1.  De clientsoftware van de synchronisatie-agent van de koppeling downloaden en installeren op de computer waarop de SQL-Server zich bevindt.
 
        > [!IMPORTANT]
        > U moet een uitgaande TCP-poort 1433 openen in de firewall te laten de clientagent communiceren met de server.


    2.  Voer een naam voor de agent.

    3.  Selecteer **maken en sleutel genereren**.

    4.  De agentcode naar het Klembord kopiëren.
        
        ![Maken van een nieuwe sync-agent](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Selecteer **OK** sluiten de **Sync-Agent Selecteer** pagina.

    6.  Zoek in de SQL Server-computer en voer de synchronisatie-Agent voor Client-app.

        ![De gegevens synchroniseren client agent-app](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  Selecteer in de app sync-agent **Agentcode indienen**. De **synchroniseren metagegevens databaseconfiguratie** dialoogvenster wordt geopend.

    8.  In de **synchroniseren metagegevens databaseconfiguratie** in het dialoogvenster Plakken in de agentcode gekopieerd van de Azure-portal. Geef ook de bestaande referenties voor de Azure SQL Database-server waarop de database met metagegevens zich bevindt. (Als u een nieuwe metagegevensdatabase gemaakt, wordt deze database is op dezelfde server als de database van de hub.) Selecteer **OK** en wacht tot de configuratie te voltooien.

        ![Voer de referenties van sleutel- en agent](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Als u een Firewallfout op dit moment krijgt, hebt u een firewallregel maken op Azure inkomend verkeer van de SQL Server-computer. U kunt de regel maken in de portal, maar soms is het eenvoudiger om deze te maken in SQL Server Management Studio (SSMS). In SSMS, probeert u verbinding maken met de hub-database in Azure. Voer de naam als \<hub_database_name\>. database.windows.net. Volg de stappen in het dialoogvenster voor het configureren van de firewallregel op Azure. Keer vervolgens terug naar de Agent voor synchronisatie van Client-app.

    9.  Klik in de app Client Sync-Agent op **registreren** registreren van een SQL Server-database met de agent. De **SQL Server-configuratiebestand** dialoogvenster wordt geopend.

        ![Toevoegen en configureren van een SQL Server-database](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. In de **SQL Server-configuratiebestand** dialoogvenster Kies of u verbinding maken met behulp van SQL Server-verificatie of Windows-verificatie. Als u SQL Server-verificatie kiest, voert u de bestaande referenties. Geef de naam van de SQL Server en de naam van de database die u wilt synchroniseren. Selecteer **verbinding testen** voor het testen van uw instellingen. Selecteer vervolgens **opslaan**. De geregistreerde database weergegeven in de lijst.

        ![SQL Server-database is nu geregistreerd.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. U kunt nu de synchronisatie-Agent voor Client-app sluiten.

    12. In de portal op de **configureren On-Premises** pagina **selecteert u de Database.** De **Database selecteren** pagina wordt geopend.

    13. Op de **Database selecteren** pagina in de **Sync lidnaam** veld, Geef een naam op voor het nieuwe lid van de synchronisatie. Deze naam verschilt van de naam van de database zelf. Selecteer de database uit de lijst. In de **Sync richtingen** veld, selecteer bidirectionele synchronisatie, naar de Hub of van de Hub.

        ![Selecteer de lokale-database op.](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Selecteer **OK** sluiten de **Database selecteren** pagina. Selecteer vervolgens **OK** sluiten de **configureren On-Premises** pagina en wachten op het nieuwe lid van de synchronisatie kan worden gemaakt en geïmplementeerd. Tot slot op **OK** sluiten de **sync leden selecteren** pagina.

        ![Op de lokale database toegevoegd aan de groep voor synchronisatie](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  Voor verbinding met het synchroniseren van de SQL-gegevens en de lokale agent, voegt u uw gebruikersnaam toe aan de rol `DataSync_Executor`. Synchroniseren van gegevens wordt deze rol op de SQL Server-exemplaar gemaakt.

## <a name="step-3---configure-sync-group"></a>Stap 3 - groep voor synchronisatie configureren

Nadat de nieuwe leden van de synchronisatie-groep worden gemaakt en geïmplementeerd, stap 3 **groep voor synchronisatie configureren**, is gemarkeerd in de **groep voor synchronisatie met nieuwe** pagina.

1.  Op de **tabellen** pagina, selecteert u een database uit de lijst met leden van de beveiligingsgroep synchronisatie en selecteer vervolgens **schema vernieuwen**.

2.  Selecteer de tabellen die u wilt synchroniseren in de lijst met beschikbare tabellen.

    ![Selecteer tabellen om te synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Standaard worden alle kolommen in de tabel geselecteerd. Als u niet dat alle kolommen synchroniseren wilt, schakelt u het selectievakje voor de kolommen die u niet wilt synchroniseren. Zorg ervoor dat de primaire-sleutelkolom geselecteerd laat.

    ![Velden selecteren om te synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Tot slot selecteert **opslaan**.

## <a name="faq-about-setup-and-configuration"></a>Veelgestelde vragen over de installatie en configuratie

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Hoe vaak kunt synchroniseren van gegevens mijn gegevens Synchroniseer? 
De minimale frequentie is om de vijf minuten.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>SQL gegevenssynchronisatie volledig maken en inrichten van tabellen?

Als de synchronisatie-schema-tabellen worden niet in de doeldatabase zijn gemaakt, wordt deze door synchroniseren van de SQL-gegevens (Preview) maken met de kolommen die u hebt geselecteerd. Echter, dit gedrag resulteert niet in een schema volledige fidelity om de volgende redenen:

-   Alleen de kolommen die u hebt geselecteerd, worden gemaakt in de doeltabel. Als sommige kolommen in de brontabellen geen deel uit van de groep voor synchronisatie maken, worden deze kolommen in de doeltabellen niet ingericht.

-   Indexen worden alleen gemaakt voor de geselecteerde kolommen. Als de tabel bronindex kolommen die geen deel uitmaken van de groep voor synchronisatie heeft, worden de indexen in de doeltabellen niet ingericht.

-   Indexen voor kolommen van het type XML zijn niet ingericht.

-   CHECK-beperkingen niet ingericht.

-   Bestaande triggers op de brontabellen zijn niet ingericht.

-   Weergaven en opgeslagen Procedures worden niet gemaakt op de doeldatabase.

Vanwege deze beperkingen raden we aan de volgende zaken:
-   Voor productieomgevingen, richt het schema volledig gehandhaafd zelf.
-   Voor het uitproberen van de service, wordt de functie automatisch inrichten van het synchroniseren van de SQL-gegevens (Preview) goed werkt.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Waarom zie ik tabellen die ik niet hebt gemaakt?  
Synchroniseren van gegevens maakt aan tabellen in de database voor het bijhouden. Deze niet verwijderen of werkt niet meer synchroniseren van gegevens.

### <a name="is-my-data-convergent-after-a-sync"></a>Mijn gegevens convergente is na een synchronisatie?

Dat hoeft niet. In een groep voor synchronisatie met een hub en drie spaken (A, B en C) zijn de synchronisaties Hub A, B Hub en Hub en c Als een wijziging wordt aangebracht in de database een *nadat* de Hub en een synchronisatie die wijzigen totdat de volgende sync-taak niet naar de database B of C-database worden geschreven.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Hoe krijg ik wijzigingen in het schema in een groep voor synchronisatie

U moet wijzigingen in het schema handmatig uitvoeren.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Hoe kan ik exporteren en importeren van een database met het synchroniseren van gegevens?
Na het exporteren van een database als een `.bacpac` bestand en importeer het bestand om een nieuwe database te maken, moet u de volgende twee taken uitvoeren om te synchroniseren van gegevens in de nieuwe database te gebruiken:
1.  Opschonen van de objecten synchroniseren van gegevens en de tabellen op de **nieuwe database** met behulp van [dit script](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Dit script worden alle vereiste objecten synchroniseren van gegevens uit de database verwijderd.
2.  Maak de groep voor synchronisatie met de nieuwe database opnieuw. Als u niet langer de oude groep voor synchronisatie, verwijderen.

## <a name="faq-about-the-client-agent"></a>Veelgestelde vragen over de clientagent

### <a name="why-do-i-need-a-client-agent"></a>Waarom moet ik een clientagent

Het synchroniseren van de SQL-gegevens (Preview)-service communiceert met SQL Server-databases via de clientagent. Deze beveiligingsfunctie voorkomt dat directe communicatie met databases achter een firewall. Wanneer het synchroniseren van de SQL-gegevens (Preview)-service communiceert met de agent wordt uitgevoerd met behulp van versleutelde verbindingen en -token van een unieke of *agentcode*. De SQL Server-databases verifiëren de agent met de tekenreeks en agent verbindingssleutel. Dit ontwerp biedt een hoog niveau van beveiliging voor uw gegevens.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Hoeveel exemplaren van de lokale agent UI kan worden uitgevoerd?

Er kan slechts één exemplaar van de gebruikersinterface worden uitgevoerd.

### <a name="how-can-i-change-my-service-account"></a>Hoe kan ik mijn serviceaccount wijzigen?

Nadat u een clientagent hebt geïnstalleerd, is de enige manier om de serviceaccount te wijzigen om te verwijderen en een nieuwe clientagent installeren met de nieuwe serviceaccount.

### <a name="how-do-i-change-my-agent-key"></a>Hoe kan ik mijn agentcode wijzigen?

Een sleutel van de agent kan slechts eenmaal worden gebruikt door een agent. Deze kan niet opnieuw worden gebruikt wanneer u verwijderen en opnieuw installeren van een nieuwe agent, en kunnen worden gebruikt door meerdere agents. Als u een nieuwe sleutel maken voor een bestaande agent moet, moet u ervoor dat dezelfde sleutel wordt geregistreerd met de clientagent en met de service SQL synchroniseren van gegevens (Preview) zijn.

### <a name="how-do-i-retire-a-client-agent"></a>Hoe ik een clientagent wilt intrekken?

Opnieuw genereren van de sleutel in de portal wilt onmiddellijk ongeldig te maken of buiten gebruik stellen van een agent, maar niet verzenden in de gebruikersinterface van de Agent. Opnieuw genereren van een sleutel wordt de vorige sleutel ongeacht als de bijbehorende agent online of offline is ongeldig.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>Hoe kan ik een clientagent verplaatsen naar een andere computer?

Als u de lokale agent uitvoeren vanaf een andere computer wilt dan de momenteel op, kunt u het volgende doen:

1. Installeer de agent op de gewenste computer.

2. Aanmelden bij de portal voor het synchroniseren van de SQL-gegevens (Preview) en een agent-sleutel opnieuw genereren voor de nieuwe agent.

3. De nieuwe agent UI gebruikt voor het verzenden van de nieuwe sleutel van de agent.

4. Wacht terwijl de clientagent downloadt de lijst met on-premises databases die eerder zijn geregistreerd.

5. Databasereferenties opgeven voor alle databases die worden weergegeven als niet bereikbaar. Deze databases moeten bereikbaar zijn vanaf de nieuwe computer waarop de agent is geïnstalleerd.

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! U kunt een groep voor synchronisatie met zowel een exemplaar van SQL-Database en SQL Server-database hebt gemaakt.

Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met synchroniseren van Azure SQL-gegevens](sql-database-sync-data.md)
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL-gegevens](sql-database-best-practices-data-sync.md)
-   [Monitor Azure SQL-gegevens synchroniseren met OMS Log Analytics](sql-database-sync-monitor-oms.md)
-   [Problemen oplossen met het synchroniseren van Azure SQL-gegevens](sql-database-troubleshoot-data-sync.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)
