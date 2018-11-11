---
title: Azure SQL Data Sync instellen | Microsoft Docs
description: Deze zelfstudie leert u hoe u Azure SQL Data Sync instellen
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 0a248ec5137a6de43910b1d11184dfeda18601f5
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51280344"
---
# <a name="tutorial-set-up-sql-data-sync-to-sync-data-between-azure-sql-database-and-sql-server-on-premises"></a>Zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises

In deze zelfstudie leert u hoe u Azure SQL Data Sync instellen door het maken van een hybride synchronisatiegroep met zowel Azure SQL Database en SQL Server-exemplaren. De nieuwe groep voor synchronisatie volledig is geconfigureerd en gesynchroniseerd op de planning die u instelt.

In deze zelfstudie wordt ervan uitgegaan dat u ten minste enige ervaring met SQL-Database en SQL Server hebt.

Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

Voor volledige PowerShell-voorbeelden die laten hoe u SQL Data Sync configureert zien, Zie de volgende artikelen:

- [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
- [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>Stap 1: maken van de groep voor synchronisatie

### <a name="locate-the-data-sync-settings"></a>Ga naar de Data Sync-instellingen

1. Navigeer in uw browser naar de Azure-portal.

2. Zoek in de portal voor uw SQL-databases uit uw Dashboard of het pictogram van de SQL-Databases op de werkbalk.

    ![Lijst met Azure SQL-databases](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3. Op de **SQL-databases** pagina, selecteert u de bestaande SQL-database die u wilt gebruiken als de hubdatabase voor het synchroniseren van gegevens. De pagina van de SQL-database wordt geopend.

    De hubdatabase is het centrale eindpunt van de synchronisatie-topologie, waarin een groep voor synchronisatie met meerdere eindpunten van de database heeft. Alle andere database-eindpunten in de dezelfde groep - dat wil zeggen, alle lid databases - synchronisatie-synchronisatie met de hubdatabase.

4. Selecteer op de pagina SQL database voor de geselecteerde database **synchroniseren met andere databases**. De pagina Data Sync wordt geopend.

    ![Synchroniseren met andere databases-optie](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Maak een nieuwe Synchronisatiegroep

1. Selecteer op de pagina Data Sync **nieuwe Synchronisatiegroep**. De **nieuwe synchronisatiegroep** pagina wordt geopend bij stap 1, **synchronisatiegroep maken**, gemarkeerd. De **Gegevenssynchronisatiegroep maken** pagina ook wordt geopend.

2. Op de **Gegevenssynchronisatiegroep maken** pagina, doe dan het volgende:

   1. In de **Synchronisatiegroepsnaam** en voer een naam op voor de nieuwe groep voor synchronisatie.

   2. In de **metagegevensdatabase voor synchronisatie** sectie, kiest u of u wilt maken van een nieuwe database (aanbevolen) of een bestaande database wilt gebruiken.

        > [!NOTE]
        > Microsoft raadt aan dat u maakt een nieuwe, lege database om te gebruiken als de metagegevensdatabase voor synchronisatie. Gegevenssynchronisatie tabellen in deze database maakt en een frequente werkbelasting wordt uitgevoerd. Deze database wordt automatisch gedeeld als de metagegevensdatabase voor synchronisatie voor al uw groepen synchroniseren in de geselecteerde regio. U kunt de metagegevensdatabase voor synchronisatie of de naam niet wijzigen zonder het verwijderen van alle synchronisatiegroepen en Synchronisatieagents in de regio.

        Als u ervoor hebt gekozen **nieuwe database**, selecteer **nieuwe database maken.** De **SQL-Database** pagina wordt geopend. Op de **SQL-Database** pagina, een naam en het configureren van de nieuwe database. Selecteer vervolgens **OK**.

        Als u ervoor hebt gekozen **bestaande database gebruiken**, selecteert u de database uit de lijst.

   3. In de **automatische synchronisatie** sectie, selecteert u eerst **op** of **uit**.

        Als u ervoor hebt gekozen **op**, in de **Synchronisatiefrequentie** sectie en voer een getal in seconden, minuten, uren of dagen selecteren.

        ![Geef de frequentie van synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

   4. In de **conflictoplossing** sectie, selecteert u "Hub wins" of "WINS-lid."

        'Hub wins' betekent dat, wanneer er een conflict optreedt, de gegevens in de hubdatabase de conflicterende gegevens in de liddatabase overschrijft. 'Lid wins' betekent dat, wanneer er een conflict optreedt, de gegevens in de database van de conflicterende gegevens in de hubdatabase overschrijft.

       ![Opgeven hoe beleidsconflicten worden opgelost](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

   5. Selecteer **OK** en wacht tot de nieuwe groep voor synchronisatie moet worden gemaakt en geïmplementeerd.

## <a name="step-2---add-sync-members"></a>Stap 2: synchronisatieleden toevoegen

Nadat de nieuwe groep voor synchronisatie wordt gemaakt en geïmplementeerd, stap 2, **synchronisatieleden toevoegen**, is gemarkeerd in de **nieuwe synchronisatiegroep** pagina.

In de **Hubdatabase** sectie, voert u de bestaande referenties voor de SQL-Database-server waarop de hubdatabase zich bevindt. Voer niet *nieuwe* referenties in deze sectie.

![De hubdatabase is toegevoegd aan de synchronisatiegroep](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Een Azure SQL-Database toevoegen

In de **Liddatabase** sectie, optioneel een Azure SQL Database toevoegen aan de groep voor synchronisatie hiervoor **toevoegen van een Azure-Database**. De **Azure-Database configureren** pagina wordt geopend.

Op de **Azure-Database configureren** pagina, doe dan het volgende:

1. In de **Synchronisatielidnaam** veld, Geef een naam op voor het nieuwe synchronisatielid. Deze naam is niet hetzelfde als van de naam van de database zelf.

2. In de **abonnement** veld, selecteert u het bijbehorende Azure-abonnement voor factureringsdoeleinden.

3. In de **Azure SQL-Server** veld, selecteert u de bestaande SQL database-server.

4. In de **Azure SQL Database** veld, selecteert u de bestaande SQL-database.

5. In de **Synchronisatierichtingen** veld, selecteer bidirectionele synchronisatie, naar de Hub of van de Hub.

    ![Toevoegen van een nieuwe SQL-Database-synchronisatielid](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6. In de **gebruikersnaam** en **wachtwoord** velden, voert u de bestaande referenties voor de SQL-Database-server waarop de liddatabase zich bevindt. Voer niet *nieuwe* referenties in deze sectie.

7. Selecteer **OK** en wachten op het nieuwe synchronisatielid moet worden gemaakt en geïmplementeerd.

    ![Nieuwe SQL-Database synchronisatielid is toegevoegd](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a> Een on-premises SQL Server-database toevoegen

In de **Liddatabase** sectie, optioneel een on-premises SQL-Server toevoegen aan de groep voor synchronisatie door te selecteren **toevoegen van een On-Premises Database**. De **On-Premises configureren** pagina wordt geopend.

Op de **On-Premises configureren** pagina, doe dan het volgende:

1. Selecteer **de Synchronisatieagentgateway kiezen**. De **synchronisatieagent selecteren** pagina wordt geopend.

    ![De synchronisatieagentgateway kiezen](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2. Op de **de Synchronisatieagentgateway kiezen** pagina, kies of u wilt gebruikmaken van een bestaande agent of een nieuwe agent maken.

    Als u ervoor hebt gekozen **bestaande agents**, selecteert u de bestaande agent uit de lijst.

    Als u ervoor hebt gekozen **maken van een nieuwe agent**, doe dan het volgende:

   1. De clientsoftware van de sync-agent downloaden via de koppeling die is opgegeven en installeer deze op de computer waar de SQL-Server zich bevindt. U kunt ook de sync-agent van de gegevens rechtstreeks vanuit downloaden [SQL Azure Data Sync-Agent](https://www.microsoft.com/download/details.aspx?id=27693).

        > [!IMPORTANT]
        > U moet uitgaande TCP-poort 1433 openen in de firewall zodat de clientagent communiceren met de server.

   2. Voer een naam voor de agent.

   3. Selecteer **maken en genereren van sleutel**.

   4. De agentsleutel kopiëren naar het Klembord.

        ![Het maken van een nieuwe synchronisatieagent](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

   5. Selecteer **OK** sluiten de **synchronisatieagent selecteren** pagina.

   6. Zoek in de SQL Server-computer en de synchronisatieagent van Client-app uitvoeren.

        ![De gegevens synchroniseren agent-client-app](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

   7. Selecteer in de app sync-agent **Agentsleutel indienen**. De **synchroniseren metagegevens databaseconfiguratie** in het dialoogvenster wordt geopend.

   8. In de **synchroniseren metagegevens databaseconfiguratie** in het dialoogvenster Plakken in de agentsleutel die is opgehaald uit de Azure-portal. Geef ook de bestaande referenties voor de Azure SQL Database-server waarop de metagegevensdatabase zich bevindt. (Als u een nieuwe database met metagegevens gemaakt, wordt deze database zich op dezelfde server als de hubdatabase.) Selecteer **OK** en wacht tot de configuratie te voltooien.

        ![Voer de referenties van sleutel en server agent](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Als u een firewall-fout optreedt op dit moment, hebt u een firewallregel maken in Azure voor het toestaan van binnenkomend verkeer van de SQL Server-computer. U kunt de regel maken in de portal, maar wellicht vindt u het eenvoudiger om deze te maken in SQL Server Management Studio (SSMS). In SSMS, probeert u verbinding maakt met de hubdatabase in Azure. Voer de naam < hub_database_name >. database.windows.net. Volg de stappen in het dialoogvenster voor het configureren van de Azure-firewallregel. Ga vervolgens terug naar de Agent voor Clientsynchronisatie-app.

   9. Klik in de app de Agent voor Clientsynchronisatie **registreren** voor het registreren van een SQL Server-database met de agent. De **SQL Server-configuratie** in het dialoogvenster wordt geopend.

        ![Toevoegen en configureren van een SQL Server-database](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

   10. In de **SQL Server-configuratie** dialoogvenster vak, kies of u verbinding maakt met behulp van SQL Server-verificatie of Windows-verificatie. Als u SQL Server-verificatie hebt gekozen, voert u de bestaande referenties. Geef de naam van de SQL Server en de naam van de database die u wilt synchroniseren. Selecteer **verbinding testen** voor het testen van uw instellingen. Selecteer vervolgens **Opslaan**. De geregistreerde database wordt weergegeven in de lijst.

        ![SQL Server-database is nu geregistreerd](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

   11. U kunt de Agent voor Clientsynchronisatie app nu sluiten.

   12. In de portal op de **On-Premises configureren** weergeeft, schakelt **selecteert u de Database.** De **Database selecteren** pagina wordt geopend.

   13. Op de **Database selecteren** pagina, in de **Synchronisatielidnaam** veld, Geef een naam op voor het nieuwe synchronisatielid. Deze naam is niet hetzelfde als van de naam van de database zelf. Selecteer de database uit de lijst. In de **Synchronisatierichtingen** veld, selecteer bidirectionele synchronisatie, naar de Hub of van de Hub.

        ![Selecteer de on-premises-database](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

   14. Selecteer **OK** sluiten de **Database selecteren** pagina. Selecteer vervolgens **OK** sluiten de **On-Premises configureren** pagina en wachten op het nieuwe synchronisatielid moet worden gemaakt en geïmplementeerd. Klik tot slot, **OK** sluiten de **synchronisatieleden selecteren** pagina.

        ![Op de lokale database toegevoegd aan de groep voor synchronisatie](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3. Voor verbinding met SQL Data Sync en de lokale agent, kunt u uw gebruikersnaam toevoegt aan de rol `DataSync_Executor`. Data Sync maakt deze rol op de SQL Server-exemplaar.

## <a name="step-3---configure-sync-group"></a>Stap 3: synchronisatiegroep configureren

Nadat de nieuwe leden van de synchronisatie-groep worden gemaakt en geïmplementeerd, stap 3, **synchronisatiegroep configureren**, is gemarkeerd in de **nieuwe synchronisatiegroep** pagina.

1. Op de **tabellen** pagina, selecteert u een database uit de lijst met leden van synchronisatie en selecteer vervolgens **schema vernieuwen**.

2. Selecteer de tabellen die u wilt synchroniseren in de lijst met beschikbare tabellen.

    ![Tabellen selecteren om te synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3. Standaard worden alle kolommen in de tabel geselecteerd. Als u niet wilt synchroniseren van alle kolommen, schakelt u het selectievakje voor de kolommen die u niet wilt synchroniseren. Zorg ervoor dat de primaire-sleutelkolom ingeschakeld laten.

    ![Velden selecteren om te synchroniseren](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4. Selecteer ten slotte **opslaan**.

## <a name="faq-about-setup-and-configuration"></a>Veelgestelde vragen over de installatie en configuratie

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Hoe vaak kan gegevenssynchronisatie mijn gegevens Synchroniseer

De minimale frequentie is om de vijf minuten.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>SQL Data Sync volledig maken en inrichten van tabellen

Als de synchronisatie-schema-tabellen niet al in de doeldatabase gemaakt worden, worden deze in SQL Data Sync gemaakt met de kolommen die u hebt geselecteerd. Maar resulteert dit gedrag niet in een schema vol vertrouwen om de volgende redenen:

- Alleen de kolommen die u hebt geselecteerd, worden gemaakt in de doeltabel. Als sommige kolommen in de brontabellen geen deel uit van de groep voor synchronisatie maken, kan deze kolommen niet in de doeltabellen zijn ingericht.
- Indexen worden gemaakt voor de geselecteerde kolommen. Als de index van de bron-tabel kolommen die geen deel uitmaken van de groep voor synchronisatie heeft, kan deze indexen niet in de doeltabellen zijn ingericht.
- Indexen voor kolommen van het gegevenstype XML zijn niet ingericht.
- CHECK-beperkingen zijn niet ingericht.
- Bestaande triggers op de tabellen in gegevensbronnen zijn niet ingericht.
- Weergaven en opgeslagen procedures worden niet gemaakt op de doeldatabase.

Vanwege deze beperkingen raden we aan de volgende zaken:

- Voor productieomgevingen, richt het schema hoogwaardige zelf.
- Voor het uitproberen van de service, wordt de functie voor automatische inrichting van SQL Data Sync goed werkt.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Waarom zie ik tabellen die ik niet hebben gemaakt

Data Sync maakt side tabellen in de database voor het bijhouden. Ze niet verwijderen of werkt niet meer synchroniseren.

### <a name="is-my-data-convergent-after-a-sync"></a>Zijn mijn gegevens convergente na een synchronisatie

Dat hoeft niet. In een groep voor synchronisatie met een hub en drie knooppunten (A, B en C) zijn de synchronisaties Hub tot A, B-Hub en Hub en c Als een wijziging wordt aangebracht in de database een *nadat* de Hub kunt u een synchronisatie wijzigen totdat de volgende sync-taak niet naar de database B of C-database geschreven.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Hoe krijg ik wijzigingen in het schema in een groep voor synchronisatie

U moet maken en alle wijzigingen in het schema handmatig doorgegeven.

1. De wijzigingen in het schema handmatig naar de hub en repliceren naar alle synchronisatieleden van de.
2. Werk het synchronisatieschema.

**Nieuwe tabellen en kolommen toe te voegen**.

De huidige synchronisatie niet van invloed op nieuwe tabellen en kolommen. Data Sync wordt de nieuwe tabellen en kolommen genegeerd totdat u ze aan het synchronisatieschema toegevoegd. Wanneer u nieuwe objecten in de database toevoegt, is dit de aanbevolen volgorde te volgen:

1. De nieuwe tabellen of kolommen toevoegen aan de hub en aan alle synchronisatieleden van de.
2. De nieuwe tabellen of kolommen toevoegen aan het synchronisatieschema.
3. Begin met het invoegen van waarden in de nieuwe tabellen en kolommen.

**Wijzigen van het gegevenstype van een kolom**.

Wanneer u het gegevenstype van een bestaande kolom wijzigt, wordt Data Sync blijft werken als de nieuwe waarden aanpassen aan het oorspronkelijke type gedefinieerd in het synchronisatieschema. Bijvoorbeeld, als u het type in de brondatabase van wijzigen **int** naar **bigint**, Data Sync blijft werken totdat u een waarde die is te groot voor de **int** gegevenstype . De schemawijziging handmatig repliceren naar de hub en op alle synchronisatieleden voor het voltooien van de wijziging en werk vervolgens het synchronisatieschema.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Hoe kan ik exporteren en importeren van een database met het synchroniseren van gegevens

Na het exporteren van een database als een `.bacpac` bestand en importeer het bestand als een nieuwe database wilt maken, hebt u de volgende twee taken uitvoeren om te synchroniseren van gegevens in de nieuwe database gebruiken:

1. Opschonen van de Data Sync-objecten en side tabellen op de **nieuwe database** met behulp van [met dit script](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Met dit script worden alle van de vereiste Data Sync-objecten uit de database verwijderd.
2. Maak de groep voor synchronisatie met de nieuwe database opnieuw. Als u de oude groep voor synchronisatie niet meer nodig hebt, verwijderen.

## <a name="faq-about-the-client-agent"></a>Veelgestelde vragen over de clientagent

Zie voor veelgestelde vragen over de clientagent [Agent Veelgestelde vragen over](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! U kunt een groep voor synchronisatie met zowel een exemplaar van SQL Database en een SQL Server-database hebt gemaakt.

Zie de volgende onderwerpen voor meer informatie over SQL Data Sync:

- [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync (Preview)](sql-database-sync-data.md)
- [Aanbevolen procedures voor Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
- [Azure SQL Data Sync bewaken met Log Analytics](sql-database-sync-monitor-oms.md)
- [Problemen oplossen met Azure SQL Data Sync](sql-database-troubleshoot-data-sync.md)

- Voer PowerShell-voorbeelden uit die laten zien hoe u SQL Data Sync configureert:
  - [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
  - [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)

- [De documentatie over de REST-API van SQL Data Sync downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie de volgende onderwerpen voor meer informatie over SQL Database:

- [Wat is de Azure SQL Database-service?](sql-database-technical-overview.md)
- [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
