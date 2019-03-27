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
ms.reviewer: carlrab
manager: craigg
ms.date: 01/14/2019
ms.openlocfilehash: 82b85ffd685df52e702db15e5a5b57a53a3b4f64
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485999"
---
# <a name="tutorial-set-up-sql-data-sync-between-azure-sql-database-and-sql-server-on-premises"></a>Zelfstudie: SQL Data Sync tussen Azure SQL Database en SQL Server instellen met on-premises

In deze zelfstudie leert u hoe u Azure SQL Data Sync instellen door het maken van een groep voor synchronisatie met Azure SQL Database- en SQL Server-instanties. De synchronisatiegroep is ingesteld op aangepast geconfigureerd en gesynchroniseerd op de planning die u instelt.

De zelfstudie wordt ervan uitgegaan dat u hebt ten minste enige ervaring met SQL Database en SQL Server.

Zie voor een overzicht van SQL Data Sync [gegevens synchroniseren tussen cloud en on-premises databases met Azure SQL Data Sync](sql-database-sync-data.md).

Zie voor voorbeelden van PowerShell voor het configureren van SQL Data Sync [synchroniseren tussen Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md) of [een Azure SQL Database en een on-premises SQL Server-database](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!IMPORTANT]
> Azure SQL Data Sync biedt op dit moment **geen** ondersteuning voor beheerde exemplaren voor Azure SQL Database.

## <a name="create-sync-group"></a>Synchronisatiegroep maken

1. In de browser, Ga naar de Azure-portal. Zoek uw SQL-database op het dashboard of, selecteer de **SQL-databases** pictogram op de werkbalk en klik op de **SQL-databases** pagina, selecteert u de database die u wilt gebruiken als de hubdatabase voor het synchroniseren van gegevens.

    > [!NOTE]
    > De hub database centrale eindpunt een synchronisatie-topologie, waarin een groep voor synchronisatie met meerdere eindpunten van de database heeft. Alle andere lid databases met eindpunten in de groep voor synchronisatie, synchroniseren met de hubdatabase.

1. Op de **SQL-database** -pagina voor de geselecteerde database, selecteert **synchroniseren met andere databases**.

    ![Synchroniseren met andere databases-optie](media/sql-database-get-started-sql-data-sync/datasync-overview.png)

1. Op de **synchroniseren met andere databases** weergeeft, schakelt **nieuwe Synchronisatiegroep**. De **nieuwe synchronisatiegroep** pagina wordt geopend met **synchronisatiegroep maken (stap 1)** gemarkeerd.

   ![Stap 1-instellingen](media/sql-database-get-started-sql-data-sync/stepone.png)

   Op de **Gegevenssynchronisatiegroep maken** pagina, wijzigt u de volgende instellingen:

   | Instelling                        | Description |
   | ------------------------------ | ------------------------------------------------- |
   | **Naam van Synchronisatiegroep** | Voer een naam voor de nieuwe groep voor synchronisatie. Deze naam is niet hetzelfde als van de naam van de database zelf. |
   | **Metagegevensdatabase voor synchronisatie** | Kies een database (aanbevolen) te maken of een bestaande database wilt gebruiken.<br/><br/>Als u ervoor kiest **nieuwe database**, selecteer **nieuwe database maken.** Klik op de **SQL-Database** pagina, een naam en het configureren van de nieuwe database en selecteer **OK**.<br/><br/>Als u ervoor kiest **bestaande database gebruiken**, selecteert u de database uit de lijst. |
   | **Automatische synchronisatie** | Selecteer **op** of **uit**.<br/><br/>Als u ervoor kiest **op**, voer een getal in en selecteert u **seconden**, **minuten**, **uur**, of **dagen** in de **Synchronisatiefrequentie** sectie. |
   | **Conflictoplossing** | Selecteer **Hub win** of **lid win**.<br/><br/>**Hub win** betekent dat wanneer conflicten optreden, gegevens in de hubdatabase conflicterende gegevens in de liddatabase worden overschreven.<br/><br/>**Lid win** betekent dat wanneer conflicten optreden, gegevens in de liddatabase overschrijft conflicterende gegevens in de hubdatabase. |

   > [!NOTE]
   > Microsoft raadt aan om te maken van een nieuwe, lege database voor gebruik als de **metagegevensdatabase voor synchronisatie**. Gegevenssynchronisatie tabellen in deze database maakt en een frequente werkbelasting wordt uitgevoerd. Deze database wordt gedeeld als de **metagegevensdatabase voor synchronisatie** voor alle synchronisatiegroepen in een geselecteerde regio en u de database of de naam niet wijzigen zonder het verwijderen van alle synchronisatiegroepen en Synchronisatieagents in de regio.

   Selecteer **OK** en wacht tot de groep voor synchronisatie moet worden gemaakt en geïmplementeerd.

## <a name="add-sync-members"></a>Synchronisatieleden toevoegen

Nadat de nieuwe groep voor synchronisatie wordt gemaakt en geïmplementeerd, **synchronisatieleden (stap 2) Voeg** is gemarkeerd op de **nieuwe synchronisatiegroep** pagina.

In de **Hubdatabase** sectie, bestaande referenties invoeren voor de SQL-Database-server waarop de hubdatabase zich bevindt. Voer niet *nieuwe* referenties in deze sectie.

![Stap 2-instellingen](media/sql-database-get-started-sql-data-sync/steptwo.png)

### <a name="to-add-an-azure-sql-database"></a>Toevoegen van een Azure SQL Database

In de **Liddatabase** sectie, optioneel een Azure SQL Database toevoegen aan de groep voor synchronisatie hiervoor **toevoegen van een Azure SQL Database**. De **Azure SQL-Database configureren** pagina wordt geopend.

  ![Stap 2: de database configureren](media/sql-database-get-started-sql-data-sync/steptwo-configure.png)

  Op de **Azure SQL-Database configureren** pagina, wijzigt u de volgende instellingen:

  | Instelling                       | Description |
  | ----------------------------- | ------------------------------------------------- |
  | **Naam van synchronisatielid** | Geef een naam voor het nieuwe synchronisatielid. Deze naam is hetzelfde als de naam van de database zelf. |
  | **Abonnement** | Selecteer de bijbehorende Azure-abonnement voor factureringsdoeleinden. |
  | **Azure SQL Server** | Selecteer de bestaande SQL Database-server. |
  | **Azure SQL Database** | Selecteer de bestaande SQL-database. |
  | **Synchronisatierichtingen** | Selecteer **bidirectionele synchronisatie**, **naar de Hub**, of **van de Hub**. |
  | **Gebruikersnaam** en **wachtwoord** | Voer de bestaande referenties voor de SQL-Database-server waarop de liddatabase zich bevindt. Voer niet *nieuwe* referenties in deze sectie. |

  Selecteer **OK** en wachten op het nieuwe synchronisatielid moet worden gemaakt en geïmplementeerd.

<a name="add-on-prem"></a>
### <a name="to-add-an-on-premises-sql-server-database"></a>Toevoegen van een on-premises SQL Server-database

In de **Liddatabase** sectie, optioneel een on-premises SQL-Server toevoegen aan de groep voor synchronisatie door te selecteren **toevoegen van een On-Premises Database**. De **On-Premises configureren** pagina wordt geopend, waar u de volgende zaken kunt doen:

1. Selecteer **de Synchronisatieagentgateway kiezen**. De **synchronisatieagent selecteren** pagina wordt geopend.

   ![Het maken van een sync-agent](media/sql-database-get-started-sql-data-sync/steptwo-agent.png)

1. Op de **kiest u de Sync-Agent** pagina, kies of u wilt gebruikmaken van een bestaande agent of maken van een agent.

   Als u ervoor kiest **bestaande agents**, selecteert u de bestaande agent uit de lijst.

   Als u ervoor kiest **maken van een nieuwe agent**, doe dan het volgende:

   1. De data sync-agent downloaden via de koppeling en installeer deze op de computer waar de SQL-Server zich bevindt. U kunt ook de agent rechtstreeks vanuit downloaden [SQL Azure Data Sync-Agent](https://www.microsoft.com/download/details.aspx?id=27693).

      > [!IMPORTANT]
      > U moet uitgaande TCP-poort 1433 openen in de firewall zodat de clientagent communiceren met de server.

   1. Voer een naam voor de agent.

   1. Selecteer **maken en genereren van de sleutel** en de agentsleutel kopiëren naar het Klembord.

   1. Selecteer **OK** sluiten de **synchronisatieagent selecteren** pagina.

1. Zoek in de SQL Server-computer en de synchronisatieagent van Client-app uitvoeren.

   ![De gegevens synchroniseren agent-client-app](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    1. Selecteer in de app sync-agent **Agentsleutel indienen**. De **synchroniseren metagegevens databaseconfiguratie** in het dialoogvenster wordt geopend.

    1. In de **synchroniseren metagegevens databaseconfiguratie** in het dialoogvenster Plakken in de agentsleutel die is opgehaald uit de Azure-portal. Geef ook de bestaande referenties voor de Azure SQL Database-server waarop de metagegevensdatabase zich bevindt. (Als u een database met metagegevens gemaakt, wordt deze database zich op dezelfde server als de hubdatabase.) Selecteer **OK** en wacht tot de configuratie te voltooien.

        ![Voer de referenties van sleutel en server agent](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Als er een Firewallfout optreedt, maakt u een firewallregel op Azure voor het toestaan van binnenkomend verkeer van de SQL Server-computer. U kunt de regel handmatig maken in de portal of in SQL Server Management Studio (SSMS). In SSMS verbinding maken met de hubdatabase in Azure door te voeren van de naam < hub_database_name >. database.windows.net.

    1. Selecteer **registreren** voor het registreren van een SQL Server-database met de agent. De **SQL Server-configuratie** in het dialoogvenster wordt geopend.

        ![Toevoegen en configureren van een SQL Server-database](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    1. In de **SQL Server-configuratie** dialoogvenster vak, kiest u verbinding maken met behulp van SQL Server-verificatie of Windows-verificatie. Als u SQL Server-verificatie kiest, voert u de bestaande referenties. Geef de naam van de SQL Server en de naam van de database die u wilt synchroniseren en selecteer **verbinding testen** voor het testen van uw instellingen. Selecteer vervolgens **opslaan** en de geregistreerde database wordt weergegeven in de lijst.

        ![SQL Server-database is nu geregistreerd](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    1. Sluit de app van de Agent voor het synchroniseren van de Client.

1. In de portal op de **On-Premises configureren** weergeeft, schakelt **selecteert u de Database**.

1. Op de **Database selecteren** pagina, in de **Synchronisatielidnaam** veld, Geef een naam op voor het nieuwe synchronisatielid. Deze naam is niet hetzelfde als van de naam van de database zelf. Selecteer de database uit de lijst. In de **Synchronisatierichtingen** veld **bidirectionele synchronisatie**, **naar de Hub**, of **van de Hub**.

    ![Selecteer de on-premises-database](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

1. Selecteer **OK** sluiten de **Database selecteren** pagina. Selecteer vervolgens **OK** sluiten de **On-Premises configureren** pagina en wachten op het nieuwe synchronisatielid moet worden gemaakt en geïmplementeerd. Selecteer ten slotte **OK** sluiten de **synchronisatieleden selecteren** pagina.

> [!NOTE]
> Voor verbinding met SQL Data Sync en de lokale agent, kunt u uw gebruikersnaam toevoegt aan de rol *DataSync_Executor*. Data Sync maakt deze rol op de SQL Server-exemplaar.

## <a name="configure-sync-group"></a>Synchronisatiegroep configureren

Nadat de nieuwe leden van de synchronisatie-groep worden gemaakt en geïmplementeerd, **synchronisatiegroep configureren (stap 3)** is gemarkeerd in de **nieuwe synchronisatiegroep** pagina.

![Stap 3-instellingen](media/sql-database-get-started-sql-data-sync/stepthree.png)

1. Op de **tabellen** pagina, selecteert u een database uit de lijst met leden van synchronisatie en selecteer **schema vernieuwen**.

1. In de lijst, selecteer de tabellen die u wilt synchroniseren. Standaard alle kolommen zijn geselecteerd, dus schakel het selectievakje voor de kolommen die u niet wilt synchroniseren. Zorg ervoor dat de primaire-sleutelkolom ingeschakeld laten.

1. Selecteer **Opslaan**.

1. Standaard worden databases zijn niet gesynchroniseerd tot geplande of handmatig uitvoeren. Als u wilt een handmatige synchronisatie uitvoeren, gaat u naar uw SQL-database in Azure portal, selecteer **synchroniseren met andere databases**, en selecteer de groep voor synchronisatie. De **gegevenssynchronisatie** pagina wordt geopend. Selecteer **Synchroniseren**.

    ![Handmatige synchronisatie](media/sql-database-get-started-sql-data-sync/datasync-sync.png)

## <a name="faq"></a>Veelgestelde vragen

**Hoe vaak kan gegevenssynchronisatie mijn gegevens Synchroniseer?**

De minimale duur tussen synchronisaties is vijf minuten.

**SQL Data Sync volledig tabellen maken?**

Als synchronisatie schema tabellen in de doeldatabase ontbreken, worden deze in SQL Data Sync gemaakt met de kolommen die u hebt geselecteerd. Dit wordt niet echter resulteren in een hoogwaardige-schema voor de volgende redenen:

- Alleen de geselecteerde kolommen worden gemaakt in de doeltabel. Kolommen die niet is ingeschakeld, worden genegeerd.
- Alleen geselecteerde kolom indexen worden gemaakt in de doeltabel. Voor kolommen die niet is ingeschakeld, worden deze indexen worden genegeerd.
- Indexen voor kolommen van het XML-gegevenstype worden niet gemaakt.
- CHECK-beperkingen zijn niet gemaakt.
- Triggers op de brontabellen worden niet gemaakt.
- Weergaven en opgeslagen procedures worden niet gemaakt.

Vanwege deze beperkingen raden we aan de volgende zaken:

- Voor productieomgevingen het schema hoogwaardige zelf maken.
- Bij het experimenteren met de service, gebruikt u de functie voor automatische inrichting.

**Waarom zie ik tabellen die ik heb gemaakt?**

Data Sync maakt aanvullende tabellen in de database voor het bijhouden. Deze niet verwijderen of werkt niet meer synchroniseren.

**Zijn mijn gegevens convergente na een synchronisatie?**

Dat hoeft niet. Een groep voor synchronisatie met een hub en drie knooppunten (A, B en C) nemen waar synchronisaties zijn Hub tot A, B-Hub en Hub en c Als een wijziging wordt aangebracht in de database een *nadat* de Hub kunt u een synchronisatie, die wijziging is niet naar de database B of C-database tot en met de volgende sync-taak geschreven.

**Hoe krijg ik wijzigingen in het schema in een groep voor synchronisatie**

Controleer en alle wijzigingen in het schema handmatig doorgegeven.

1. De wijzigingen in het schema handmatig naar de hub en repliceren naar alle synchronisatieleden van de.
1. Werk het synchronisatieschema.

Voor het toevoegen van nieuwe tabellen en kolommen:

Nieuwe tabellen en kolommen niet van invloed zijn op de huidige synchronisatie en gegevenssynchronisatie genegeerd totdat ze worden toegevoegd aan het synchronisatieschema. Bij het toevoegen van nieuwe objecten in de database, volgt u de volgorde:

1. Nieuwe tabellen of kolommen toevoegen aan de hub en aan alle synchronisatieleden van de.
1. Nieuwe tabellen of kolommen toevoegen aan het synchronisatieschema.
1. Beginnen met het invoegen van waarden in de nieuwe tabellen en kolommen.

Voor het wijzigen van het gegevenstype van een kolom:

Wanneer u het gegevenstype van een bestaande kolom wijzigt, wordt Data Sync blijft werken als de nieuwe waarden aanpassen aan het oorspronkelijke type gedefinieerd in het synchronisatieschema. Bijvoorbeeld, als u het type in de brondatabase van wijzigen **int** naar **bigint**, Data Sync blijft werken totdat u een waarde te groot voor de **int** gegevenstype. De schemawijziging handmatig repliceren naar de hub en op alle synchronisatieleden voor het voltooien van de wijziging, en klik vervolgens het synchronisatieschema bijwerken.

**Hoe kan ik exporteren en importeren van een database met het synchroniseren van gegevens?**

Na het exporteren van een database als een *.bacpac* -bestand en importeer het bestand een database maken, dient u het volgende om te synchroniseren van gegevens in de nieuwe database gebruiken:

1. Opschonen van de Data Sync-objecten en aanvullende tabellen op de nieuwe database met behulp van [met dit script](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Het script worden alle vereiste Data Sync-objecten uit de database verwijderd.
1. Maak de groep voor synchronisatie met de nieuwe database opnieuw. Als u de oude groep voor synchronisatie niet meer nodig hebt, verwijderen.

**Waar vind ik informatie over de clientagent**

Zie voor veelgestelde vragen over de clientagent [Agent Veelgestelde vragen over](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! U kunt een groep voor synchronisatie met zowel een exemplaar van SQL Database en een SQL Server-database hebt gemaakt.

Zie de volgende onderwerpen voor meer informatie over SQL Data Sync:

- [Data Sync-Agent voor Azure SQL Data Sync](sql-database-data-sync-agent.md)
- [Aanbevolen procedures](sql-database-best-practices-data-sync.md) en [over het oplossen van problemen met Azure SQL Data Sync](sql-database-troubleshoot-data-sync.md)
- [SQL Data Sync bewaken met Azure Monitor-Logboeken](sql-database-sync-monitor-oms.md)
- [Het synchronisatieschema bijwerken met behulp van Transact-SQL](sql-database-update-sync-schema.md) of [PowerShell](scripts/sql-database-sync-update-schema.md)

Zie de volgende onderwerpen voor meer informatie over SQL Database:

- [Wat is de Azure SQL Database-service?](sql-database-technical-overview.md)
- [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
