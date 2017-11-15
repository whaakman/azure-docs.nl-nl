---
title: Synchroniseren van Azure SQL-gegevens met logboekanalyse OMS bewaken | Microsoft Docs
description: Informatie over het synchroniseren van Azure SQL-gegevens met behulp van logboekanalyse OMS bewaken
services: sql-database
ms.date: 11/7/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ace0eb671556dc980836464a365731d6100eab25
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="monitor-sql-data-sync-preview-with-oms-log-analytics"></a>Controleprogramma SQL synchroniseren van gegevens (Preview) met OMS Log Analytics 

Controleer het activiteitenlogboek synchroniseren van de SQL-gegevens en fouten en waarschuwingen te detecteren, voorheen moest u handmatig synchroniseren van de SQL-gegevens controleren in de Azure portal of PowerShell of de REST-API gebruiken. Volg de stappen in dit artikel voor het configureren van een aangepaste oplossing waarmee het synchroniseren van gegevens implementatiebewakingservaring worden verbeterd. U kunt deze oplossing aanpassen aan uw scenario.

Zie voor een overzicht van de SQL-gegevenssynchronisatie [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Dashboard voor uw synchronisatiegroepen controle 

Niet langer hoeft te zoeken in de logboeken van elke groep voor synchronisatie afzonderlijk om te zoeken naar problemen. U kunt alle groepen van de synchronisatie van een van uw abonnementen op één plek bewaken met behulp van een aangepaste weergave van OMS (Operations Management Suite). Deze weergave geeft de informatie die van belang is voor klanten synchroniseren van de SQL-gegevens weer.

![Gegevens synchroniseren bewakingsdashboard](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Geautomatiseerd e-mailmeldingen

U moet niet langer Raadpleeg het logboek handmatig in de Azure-portal of via PowerShell of de REST-API. Met [OMS Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), kunt u waarschuwingen die rechtstreeks naar de e-mailadressen van de mensen die u zien wilt wanneer er een fout optreedt.

![Gegevens synchroniseren e-mailmeldingen](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Hoe u deze bewakingsfuncties instellen? 

Implementeer een aangepaste OMS bewakingsoplossing voor SQL-gegevenssynchronisatie in minder dan een uur door het volgende doen:

U moet drie onderdelen configureren:

-   Een PowerShell-runbook om te synchroniseren van gegevens van SQL-logboekgegevens feed met OMS.

-   Een OMS-logboekanalyse-waarschuwing voor e-mailmeldingen.

-   Een OMS-weergave voor bewaking.

### <a name="samples-to-download"></a>Voorbeelden voor het downloaden

Download de volgende twee voorbeelden:

-   [Gegevens synchroniseren logboek PowerShell-Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Gegevens synchroniseren logboek OMS bekijken](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u de volgende zaken die u hebt ingesteld:

-   Een Azure Automation-account

-   Log Analytics gekoppeld aan OMS-werkruimte

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>PowerShell-Runbook ophalen van de SQL-logboek voor gegevens synchroniseren 

Gebruik een PowerShell-runbook gehost in Azure Automation om de logboekgegevens van het synchroniseren van de SQL-gegevens ophalen en deze verzenden naar OMS. Een voorbeeld van een script is opgenomen. Als een vereiste moet u een Azure Automation-account hebben. Vervolgens moet u een runbook maken en plannen om uit te voeren. 

### <a name="create-a-runbook"></a>Een runbook maken

Zie voor meer informatie over het maken van een runbook [Mijn eerste PowerShell-runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Selecteer onder uw Azure Automation-account, de **Runbooks** tabblad onder procesautomatisering.

2.  Selecteer **een Runbook toevoegen** in de linkerbovenhoek van de pagina Runbooks.

3.  Selecteer **een bestaand Runbook importeren**.

4.  Onder **Runbook bestand**, gebruikt u de opgegeven `DataSyncLogPowerShellRunbook` bestand. Stel de **runbooktype** als `PowerShell`. Geef een naam voor het runbook.

5.  Selecteer **Maken**. U hebt nu een runbook.

6.  Selecteer onder uw Azure Automation-Account de **variabelen** tabblad onder gedeelde bronnen.

7.  Selecteer **toevoegen van een variabele** op de pagina variabelen. Maak een variabele voor het opslaan van de laatste uitvoertijd voor het runbook. Als u meerdere runbooks hebt, moet u een variabele voor elk runbook.

8.  Stel de naam van de variabele als `DataSyncLogLastUpdatedTime` en stel het Type datum/tijd.

9.  Selecteer het runbook en klik op de knop Bewerken boven aan de pagina.

10. Breng de wijzigingen die vereist zijn voor uw account en de configuratie van het synchroniseren van de SQL-gegevens. (Zie het voorbeeldscript voor meer gedetailleerde informatie.)

    1.  Azure informatie.

    2.  Informatie van de groep voor synchronisatie.

    3.  OMS-informatie. Deze informatie vinden op OMS-Portal | Instellingen | Verbonden bronnen. Zie voor meer informatie over het verzenden van gegevens met logboekanalyse [gegevens verzenden naar logboekanalyse met de HTTP-API van Data Collector (openbare preview)](../log-analytics/log-analytics-data-collector-api.md).

11. Het runbook worden uitgevoerd in het testvenster. Controleer of dat deze is geslaagd.

    Als u fouten hebt, moet u de meest recente PowerShell-module geïnstalleerd. U kunt installeren de nieuwste PowerShell-module in de **Modules galerie** in uw Automation-Account.

12. Klik op **publiceren**

### <a name="schedule-the-runbook"></a>Het runbook plannen

Het runbook plannen:

1.  Selecteer onder het runbook de **planningen** tabblad onder Resources.

2.  Selecteer **toevoegen van een planning** op de pagina schema.

3.  Selecteer **een planning aan uw runbook koppelen**.

4.  Selecteer **een nieuw schema maken.**

5.  Stel **terugkeerpatroon** terugkerend en stel het interval u wilt. Gebruik hetzelfde interval, in het script en in OMS.

6.  Selecteer **Maken**.

### <a name="check-the-automation"></a>Controleer de automatisering

Om te controleren of uw automation wordt uitgevoerd zoals verwacht, klikt u onder **overzicht** vinden voor uw automation-account, de **taak statistieken** weergeven onder **bewaking**. Deze weergave aan uw dashboard voor eenvoudige weergave vastmaken. Geslaagde wordt uitgevoerd van het runbook als 'Voltooid' weergeven en kan niet wordt uitgevoerd, weergegeven als 'Mislukt'.

## <a name="create-an-oms-log-reader-alert-for-email-notifications"></a>Een waarschuwing OMS Log Reader voor e-mailmeldingen maken

Als u wilt maken van een waarschuwing die gebruikmaakt van OMS Log Analytics, het volgende doen. U moet als een vereiste hebt gekoppeld aan een OMS-werkruimte logboekanalyse.

1.  Selecteer in de OMS-portal **logboek zoeken**.

2.  Een query maken om de fouten en waarschuwingen die door de groep voor synchronisatie Selecteer binnen het interval dat u hebt geselecteerd. Bijvoorbeeld:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Nadat de query is uitgevoerd, selecteert u de bel waarin staat dat **waarschuwing**.

4.  Onder **waarschuwing genereren op basis van**, selecteer **metriek meting**.

    1.  De totale waarde ingesteld op **groter is dan**.

    2.  Na **groter is dan**, voer de drempelwaarde voor de verstrijken voordat u meldingen ontvangen. Tijdelijke fouten worden in het synchroniseren van gegevens verwacht. Als u ruis, de drempelwaarde te ingesteld op 5.

5.  Onder **acties**stelt **e-mailmeldingen** op 'Ja'. Geef de gewenste e-mailontvangers.

6.  Klik op **Opslaan**. De opgegeven ontvangers wordt nu een e-mailmeldingen ontvangen wanneer er fouten optreden.

## <a name="create-an-oms-view-for-monitoring"></a>Een OMS-weergave maken voor de bewaking

Deze stap maakt een OMS-weergave voor het controleren van de opgegeven synchronisatiegroepen visueel. De weergave omvat verschillende onderdelen:

-   Een overzichttegel waarin het aantal fouten, successen en waarschuwingen alle synchronisatiegroepen hebben.

-   Een tegel voor alle groepen synchroniseren, geeft de telling van fouten en waarschuwingen per groep voor synchronisatie. Groepen zonder problemen worden niet weergegeven op deze tegel.

-   Een tegel voor elke groep voor synchronisatie, die het aantal fouten, is voltooid, en waarschuwingen en de recente foutberichten toont.

Voor het configureren van de OMS-weergave, het volgende doen:

1.  Selecteer het plusteken aan de linkerkant te openen op de startpagina OMS de **ontwerper**.

2.  Selecteer **importeren** op de bovenste balk van de ontwerpfunctie voor weergave. Selecteer vervolgens het voorbeeldbestand 'DataSyncLogOMSView'.

3.  De voorbeeldweergave is voor het beheren van twee synchronisatiegroepen. Bewerk deze weergave aanpassen aan uw scenario. Klik op **bewerken** en breng de volgende wijzigingen:

    1.  Nieuwe 'Ring & List'-objecten maken uit de galerie, indien nodig.

    2.  Werk de query's met uw gegevens in elke tegel.

        1.  Wijzig het interval TimeStamp_t naar wens op elke tegel.

        2.  Bijwerken op de tegels voor elke groep voor synchronisatie met de namen van de groep voor synchronisatie.

    3.  Update de titel op voor elke tegel.

4.  Klik op **opslaan** en de weergave is gereed.

## <a name="cost-of-this-solution"></a>Kosten van deze oplossing

Deze oplossing is in de meeste gevallen gratis.

**Azure Automation:** kan er een kosten die zijn gemaakt met de Azure Automation-account, afhankelijk van uw gebruik. De eerste 500 minuten van taakuitvoeringstijd per maand zijn gratis. Deze oplossing wordt in de meeste gevallen verwacht gebruik van minder dan 500 minuten per maand. Plannen om te voorkomen kosten, het runbook moet worden uitgevoerd bij een interval van twee uur of langer. Zie voor meer informatie [Automation prijzen](https://azure.microsoft.com/pricing/details/automation/).

**OMS Log Analytics:** mogelijk zijn er kosten verbonden met OMS afhankelijk van uw gebruik. De laag gratis omvat 500 MB aan opgenomen gegevens per dag. In de meeste gevallen heeft deze oplossing wordt verwacht voor het opnemen van minder dan 500 MB per dag. Als u wilt verlagen van het gebruik, de fout alleen-lezen filtering gebruiken opgenomen in het runbook. Als u meer dan 500 MB per dag worden gebruikt, moet u een upgrade uitvoeren naar de laag betaald om te voorkomen dat het risico van analytics stoppen wanneer de limiet is bereikt. Zie voor meer informatie [logboekanalyse prijzen](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Codevoorbeelden

Download de codevoorbeelden in dit artikel uit de volgende locaties beschreven:

-   [Gegevens synchroniseren logboek PowerShell-Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Gegevens synchroniseren logboek OMS bekijken](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met synchroniseren van Azure SQL-gegevens](sql-database-sync-data.md)
-   [Synchroniseren van Azure SQL-gegevens instellen](sql-database-get-started-sql-data-sync.md)
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL-gegevens](sql-database-best-practices-data-sync.md)
-   [Problemen oplossen met het synchroniseren van Azure SQL-gegevens](sql-database-troubleshoot-data-sync.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)
