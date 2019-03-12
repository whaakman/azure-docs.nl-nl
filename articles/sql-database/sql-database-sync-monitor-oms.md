---
title: Azure SQL Data Sync bewaken met Azure Monitor-Logboeken | Microsoft Docs
description: Leer hoe u Azure SQL Data Sync bewaken met behulp van Azure Monitor-Logboeken
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 1417907bf9472137677a090906fa173c3d1ea571
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539289"
---
# <a name="monitor-sql-data-sync-with-azure-monitor-logs"></a>SQL Data Sync bewaken met Azure Monitor-Logboeken 

Als u wilt het activiteitenlogboek van SQL Data Sync controleren en opsporen van fouten en waarschuwingen, eerder moest u handmatig SQL Data Sync controleren in Azure portal of PowerShell of de REST-API gebruiken. Volg de stappen in dit artikel voor het configureren van een aangepaste oplossing waarmee de controle-ervaring gegevenssynchronisatie worden verbeterd. U kunt deze oplossing voor uw scenario.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

> [!IMPORTANT]
> Azure SQL Data Sync biedt op dit moment **geen** ondersteuning voor beheerde exemplaren voor Azure SQL Database.

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Dashboard van de controle voor alle synchronisatiegroepen 

U moet niet meer om te zoeken via de logboeken van elke Synchronisatiegroep afzonderlijk op zoek naar problemen. U kunt alle synchronisatiegroepen uit een van uw abonnementen op één plek bewaken met behulp van een aangepaste weergave van Azure Monitor. In deze weergave toont de informatie die belangrijk is voor klanten met SQL Data Sync.

![Gegevens synchroniseren bewakingsdashboard](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Geautomatiseerde e-mailmeldingen

U moet niet meer om te controleren of het logboek handmatig in Azure portal of via PowerShell of de REST-API. Met [logboeken van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), kunt u waarschuwingen die rechtstreeks naar de e-mailadressen van de mensen die u zien wilt wanneer er een fout optreedt.

![Data Sync-e-mailmeldingen](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Hoe kan u deze bewakingsfuncties instellen? 

Implementeer een aangepast Azure Monitor registreert bewakingsoplossing voor SQL Data Sync in minder dan een uur door het uitvoeren van de volgende handelingen:

U moet drie onderdelen configureren:

-   Een PowerShell-runbook om gegevens van een SQL Data Sync naar Azure Monitor-Logboeken.

-   Een Azure Monitor-waarschuwing voor e-mailmeldingen.

-   Een Azure Monitor-weergave voor de bewaking.

### <a name="samples-to-download"></a>Voorbeelden downloaden

Download de volgende twee voorbeelden:

-   [Gegevens synchroniseren Log PowerShell-Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Gegevens synchroniseren met Azure Monitor weergeven](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u hebt ingesteld met de volgende zaken:

-   Een Azure Automation-account

-   Log Analytics-werkruimte

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>PowerShell-Runbook om op te halen van SQL Data Sync-logboek 

Gebruik een PowerShell-runbook die wordt gehost in Azure Automation voor het ophalen van de gegevens van een SQL Data Sync en deze verzenden naar Azure Monitor-Logboeken. Een voorbeeld van een script is opgenomen. Als een vereiste moet u een Azure Automation-account hebben. Vervolgens moet u een runbook maken en plannen om uit te voeren. 

### <a name="create-a-runbook"></a>Een runbook maken

Zie voor meer informatie over het maken van een runbook [Mijn eerste PowerShell-runbook](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Selecteer onder uw Azure Automation-account, de **Runbooks** tabblad onder de automatisering van bedrijfsprocessen.

2.  Selecteer **een Runbook toevoegen** in de linkerbovenhoek van de pagina Runbooks.

3.  Selecteer **een bestaand Runbook importeren**.

4.  Onder **Runbook-bestand**, gebruikt u de opgegeven `DataSyncLogPowerShellRunbook` bestand. Stel de **runbooktype** als `PowerShell`. Geef een naam voor het runbook.

5.  Selecteer **Maken**. U hebt nu een runbook.

6.  Selecteer onder uw Azure Automation-Account, de **variabelen** tabblad onder gedeelde bronnen.

7.  Selecteer **toevoegen van een variabele** op de pagina variabelen. Maak een variabele voor het opslaan van de laatste uitvoeringstijd voor het runbook. Als u meerdere runbooks hebt, moet u een variabele voor elk runbook.

8.  Stel de naam van de variabele als `DataSyncLogLastUpdatedTime` en het Type datum/tijd.

9.  Selecteer het runbook en klik op de knop Bewerken boven aan de pagina.

10. Breng de wijzigingen die vereist zijn voor uw account en de configuratie van uw SQL Data Sync. (Zie het voorbeeld van een script voor meer gedetailleerde informatie.)

    1.  Azure-gegevens.

    2.  Groep voor synchronisatie met gegevens.

    3.  Azure Monitor registreert informatie. Deze informatie vinden in Azure Portal | Instellingen | Verbonden bronnen. Zie voor meer informatie over het verzenden van gegevens naar Azure Monitor-Logboeken, [gegevens verzenden naar Azure Monitor-logboeken met de HTTP Data Collector-API (preview)](../azure-monitor/platform/data-collector-api.md).

11. Het runbook uitvoeren in het deelvenster. Controleer of dat deze is voltooid.

    Als u fouten hebt, zorg ervoor dat u hebt de meest recente PowerShell-module geïnstalleerd. U kunt de meest recente PowerShell-module in de **Modulegalerie** in uw Automation-Account.

12. Klik op **publiceren**

### <a name="schedule-the-runbook"></a>Het runbook plannen

Het runbook plannen:

1.  Selecteer onder het runbook de **planningen** tabblad onder Resources.

2.  Selecteer **toevoegen van een schema** op de pagina schema's.

3.  Selecteer **een planning koppelen aan uw runbook**.

4.  Selecteer **een nieuw schema maken.**

5.  Stel **terugkeerpatroon** terugkerend en stel het interval u wilt gebruiken. Gebruik hetzelfde interval, in het script en in Azure Monitor-Logboeken.

6.  Selecteer **Maken**.

### <a name="check-the-automation"></a>Controleer de automatisering

Om te controleren of uw automation wordt uitgevoerd zoals verwacht, onder **overzicht** vinden voor uw automation-account, de **taakstatistieken** weergeven onder **bewaking**. Zet deze weergave aan uw dashboard voor eenvoudige weergave vast. Geslaagde uitvoeringen van het runbook als 'Voltooid' weergeven en mislukte uitvoeringen weergegeven als 'Mislukt'.

## <a name="create-an-azure-monitor-reader-alert-for-email-notifications"></a>Een waarschuwing met Azure Monitor-lezer voor e-mailmeldingen maken

Voor het maken van een waarschuwing die gebruikmaakt van Azure Monitor-Logboeken, kunt u de volgende dingen doen. Als een vereiste moet u logboeken van Azure Monitor is gekoppeld met een Log Analytics-werkruimte.

1.  Selecteer in de Azure portal, **zoeken in logboeken**.

2.  Maak een query voor het selecteren van de fouten en waarschuwingen door synchronisatiegroep binnen het interval dat u hebt geselecteerd. Bijvoorbeeld:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Nadat de query is uitgevoerd, selecteert u de bel met de melding dat **waarschuwing**.

4.  Onder **waarschuwing genereren op basis van**, selecteer **meting van metrische gegevens**.

    1.  De statistische waarde instelt op **groter is dan**.

    2.  Na **groter is dan**, geef de drempelwaarde voor het verstrijken voordat u berichten wilt ontvangen. Tijdelijke fouten worden verwacht in de gegevenssynchronisatie. Stel de drempelwaarde 5 ruis, verminderen.

5.  Onder **acties**, stel **e-mailmelding** op "Ja". Voer de gewenste e-mailontvangers.

6.  Klik op **Opslaan**. De opgegeven ontvangers wordt nu een e-mailmeldingen ontvangen wanneer er fouten optreden.

## <a name="create-an-azure-monitor-view-for-monitoring"></a>Een weergave met een Azure Monitor voor het bewaken van maken

Deze stap maakt u een overzicht van Azure Monitor voor het controleren van de opgegeven synchronisatiegroepen visueel. De weergave bevat verschillende onderdelen:

-   Een overzichtstegel ziet u hoeveel fouten, voltooide bewerkingen en waarschuwingen voor alle synchronisatiegroepen hebben.

-   Een tegel voor alle synchronisatiegroepen, waarin de telling van fouten en waarschuwingen per groep voor synchronisatie. Groepen zonder problemen worden niet weergegeven op deze tegel.

-   Een tegel voor elke groep voor synchronisatie, waarin het aantal fouten, is voltooid, en waarschuwingen en de recente foutberichten worden weergegeven.

Voor het configureren van de weergave van Azure Monitor, moet u de volgende dingen doen:

1.  Selecteer het plusteken aan de linkerkant om te openen op de startpagina van Log Analytics-werkruimte de **weergaveontwerper**.

2.  Selecteer **importeren** op de bovenste balk van de ontwerper. Selecteer vervolgens het voorbeeldbestand 'DataSyncLogOMSView'.

3.  De voorbeeldweergave is voor het beheren van twee synchronisatiegroepen. Bewerken in deze weergave voor uw scenario. Klik op **bewerken** en breng de volgende wijzigingen:

    1.  Maak nieuwe 'Ring en lijst' objecten vanuit de galerie zo nodig.

    2.  Werk de query's met uw gegevens in elke tegel.

        1.  Op elke tegel, het interval TimeStamp_t naar wens worden gewijzigd.

        2.  Op de tegels voor elke groep voor synchronisatie met de namen van de groep voor synchronisatie bijwerken

    3.  Update de titel op elke tegel.

4.  Klik op **opslaan** en de weergave is gereed.

## <a name="cost-of-this-solution"></a>Kosten van deze oplossing

In de meeste gevallen is deze oplossing gratis.

**Azure Automation:** Mogelijk zijn er kosten in rekening gebracht met de Azure Automation-account, afhankelijk van het gebruik. De eerste 500 minuten aan taakuitvoertijd per maand zijn gratis. Deze oplossing wordt in de meeste gevallen verwacht gebruik van minder dan 500 minuten per maand. Om kosten te voorkomen, moet u het runbook om uit te voeren met een interval van twee uur of meer plannen. Zie voor meer informatie, [Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/).

**Logboeken in Azure Monitor:** Er is mogelijk een kosten die gepaard gaan met Azure Monitor-Logboeken, afhankelijk van uw gebruik. De laag gratis omvat 500 MB opgenomen gegevens per dag. Deze oplossing wordt in de meeste gevallen verwacht voor het opnemen van minder dan 500 MB per dag. Om te reduceren het verbruik, gebruik van de alleen-fout filtering opgenomen in het runbook. Als u meer dan 500 MB per dag gebruikt, moet u een upgrade uitvoeren naar de prijscategorie betaald om te voorkomen dat het risico van analytics stoppen na het verstrijken van de beperking. Zie voor meer informatie, [Azure Monitor-logboeken prijzen](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Codevoorbeelden

Download de codevoorbeelden in dit artikel uit de volgende locaties beschreven:

-   [Gegevens synchroniseren Log PowerShell-Runbook](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Gegevens synchroniseren met Azure Monitor weergeven](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over SQL Data Sync:

-   Overzicht: [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md)
-   Data Sync instellen
    - In de portal - [Zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises](sql-database-get-started-sql-data-sync.md)
    - Met PowerShell
        -  [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Data Sync-agent: [Data Sync-agent voor Azure SQL Data Sync](sql-database-data-sync-agent.md)
-   Best practices: [Best practices voor Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   Problemen oplossen: [Problemen met Azure SQL Data Sync oplossen](sql-database-troubleshoot-data-sync.md)
-   Het synchronisatieschema bijwerken
    -   Met Transact-SQL: [De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren](sql-database-update-sync-schema.md)
    -   Met PowerShell: [PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande synchronisatiegroep](scripts/sql-database-sync-update-schema.md)

Zie de volgende onderwerpen voor meer informatie over SQL Database:

-   [Wat is de Azure SQL Database-service?](sql-database-technical-overview.md)
-   [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
