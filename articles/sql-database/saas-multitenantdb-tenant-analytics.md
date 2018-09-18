---
title: Analytics-query's uitvoeren op Azure SQL-databases | Microsoft Docs
description: Cross-tenant-analytics query's met behulp van gegevens uit meerdere Azure SQL Database-databases hebt uitgepakt.
keywords: zelfstudie SQL
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: anjangsh
ms.reviewer: billgib, genemi
ms.openlocfilehash: d5ffc9ade54ea02f34d25e4b6b7a4c0027b7d7b7
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733930"
---
# <a name="cross-tenant-analytics-using-extracted-data"></a>Cross-tenant-analytics met behulp van de opgehaalde gegevens

In deze zelfstudie doorloopt u een volledige analytics-scenario. Het scenario laat zien hoe analytics bedrijven om slimme beslissingen kan inschakelen. Gegevens die zijn geëxtraheerd uit de shard-database gebruikt, u analytics gebruiken voor inzicht in de tenant-gedrag, met inbegrip van hun gebruik van de Wingtip Tickets SaaS-voorbeeldtoepassing. In dit scenario bestaat uit drie stappen: 

1.  **Gegevens ophalen** van elke tenantdatabase in een archief van de analyse.
2.  **De opgehaalde gegevens optimaliseren** voor analyseverwerking.
3.  Gebruik **Business Intelligence** hulpprogramma's om nuttige inzichten kunnen dienen als richtlijn besluitvorming. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Maak de tenant analytics opslaan om de gegevens in te extraheren.
> - Met elastische taken kunt gegevens ophalen uit elke tenantdatabase in het archief van analytics.
> - Optimaliseer de opgehaalde gegevens (reorganize in een ster-schema).
> - Query uitvoeren op de analytics-database.
> - Gebruik Power BI voor gegevensvisualisatie trends in de gegevens van de tenant markeren en aanbevelingen voor verbetering.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/architectureOverview.png)

## <a name="offline-tenant-analytics-pattern"></a>Offline tenant analytics-patroon

SaaS-toepassingen ontwikkelen hebben toegang tot een grote hoeveelheid tenantgegevens die zijn opgeslagen in de cloud. De gegevens biedt een rijke bron van informatie over de werking en het gebruik van uw toepassing en het gedrag van de tenants. Deze inzichten kunnen dienen als richtlijn ontwikkelen van functies, bruikbaarheid verbeteren en andere investeringen doen in de app en het platform.

Het is eenvoudig om toegang tot de gegevens voor alle tenants wanneer alle gegevens in een multitenant-database. Maar de toegang is complexer wanneer op schaal zijn gedistribueerd via duizenden databases. Eén manier kunt beheersen van de complexiteit is om op te halen van de gegevens naar een analytics-database of een datawarehouse. U vervolgens opvragen het datawarehouse voor het verzamelen van inzichten uit gegevens van de tickets van alle tenants.

Deze zelfstudie toont een scenario volledige analytics voor deze SaaS-voorbeeldtoepassing. Eerste, elastische taken worden gebruikt voor het plannen van de extractie van gegevens uit elke tenantdatabase. De gegevens worden verzonden naar een analytics-store. De store analytics kan zijn van een SQL-Database of een SQL Data Warehouse. Voor het ophalen van grote hoeveelheden gegevens, [Azure Data Factory](../data-factory/introduction.md) prees is.

Vervolgens wordt de geaggregeerde gegevens in een set is vernietigd [ster-schema](https://www.wikipedia.org/wiki/Star_schema) tabellen. De tabellen bestaan uit een centrale feitentabel plus gerelateerde dimensietabellen:

- De centrale feitentabel in de ster-schema bevat ticket gegevens.
- De dimensietabellen bevatten gegevens over evenementen, evenementen, klanten en datums kopen.

Samen het midden- en dimensie tabellen inschakelen efficiënte analytische verwerking. De ster-schema in deze zelfstudie gebruikt, wordt weergegeven in de volgende afbeelding:
 
![StarSchema](media/saas-multitenantdb-tenant-analytics/StarSchema.png)

Ten slotte worden de ster-schema-tabellen opgevraagd. De queryresultaten worden visueel weergegeven om u te lichten in het gedrag van de tenant en het gebruik van de toepassing. Met deze ster-schema, kunt u query's die helpen bij het detecteren van items zoals het volgende uitvoeren:

- Wie is tickets kopen en vanaf welke locatie.
- Verborgen patronen en trends in de volgende gebieden:
    - De verkopen van tickets.
    - De relatieve populariteit van elke locatie.

Inzicht krijgen in hoe consistent elke tenant wordt gebruik van de service biedt de mogelijkheid om te maken van service-plannen die geschikt zijn voor hun behoeften. Deze zelfstudie biedt eenvoudige voorbeelden van inzichten die kunnen worden afgelezen uit gegevens van de tenant.

## <a name="setup"></a>Instellen

### <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

- De toepassing Wingtip Tickets SaaS multitenant-Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant-Database verkennen](saas-multitenantdb-get-started-deploy.md)
- Het Wingtip SaaS-scripts en de toepassing [broncode](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) worden gedownload uit GitHub. Zorg ervoor dat u *blokkering van het zip-bestand* voordat het uitpakken van de inhoud ervan. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.
- Power BI Desktop is geïnstalleerd. [Power BI Desktop downloaden](https://powerbi.microsoft.com/downloads/)
- De batch verschillende tenants tegelijk is ingericht, raadpleegt u de [ **de zelfstudie tenants inrichten**](saas-multitenantdb-provision-and-catalog.md).
- Een agent voor taken en taken zijn gemaakt. Zie de juiste stappen in de [ **Schema management zelfstudie**](saas-multitenantdb-schema-management.md#create-a-job-agent-database-and-new-job-agent).

### <a name="create-data-for-the-demo"></a>Maken van gegevens voor de demo

In deze zelfstudie, wordt analyse uitgevoerd op gegevens van internetverkopen ticket. In de huidige stap kunt u gegevens van de ticket genereren voor alle tenants.  Deze gegevens worden later opgehaald voor analyse. *Zorg ervoor dat u de batch met tenants hebt ingericht zoals eerder beschreven, zodat u een zinvolle hoeveelheid gegevens hebt*. Een voldoende hoeveelheid gegevens beschikbaar maakt een scala aan verschillende patronen van kaartverkoop bij.

1. In **PowerShell ISE**Open *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1*, en stel de volgende waarde:
    - **$DemoScenario** = **1** tickets kopen voor voorstellingen bij alle theaters
2. Druk op **F5** uitvoeren van het script en geschiedenis voor elke gebeurtenis in elke venue van kaartverkoop te maken.  Het script wordt uitgevoerd voor enkele minuten voor het genereren van tienduizenden tickets.

### <a name="deploy-the-analytics-store"></a>De store analytics implementeren
Er zijn vaak talloze transactionele shard-databases die samen alle gegevens van de tenant bevatten. U moet de gegevens van de tenant van de shard-database samenvoegen in één analytics store. De aggregatie kan efficiënt query van de gegevens. In deze zelfstudie wordt een Azure SQL Database-database gebruikt voor het opslaan van de cumulatieve gegevens.

In de volgende stappen uit, implementeert u de store analytics, met de naam **tenantanalytics**. U implementeert ook vooraf gedefinieerde tabellen die later in de zelfstudie zijn ingevuld:
1. Open in PowerShell ISE, *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1* 
2. Stel de variabele $DemoScenario in het script moet overeenkomen met uw eigen keuze aan analytics store. SQL-database zonder columnstore wordt aanbevolen voor educatieve doeleinden.
    - Instellen voor het gebruik van SQL-database zonder columnstore **$DemoScenario** = **2**
    - Als u wilt gebruiken SQL database met columnstore, **$DemoScenario** = **3**  
3. Druk op **F5** de demoscript uit te voeren (die roept de *implementeren TenantAnalytics<XX>.ps1* script) waarmee wordt gemaakt van de tenant analytics-store. 

Nu dat u hebt de toepassing is geïmplementeerd en ingevuld interessante gegevens van de tenant, [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) om verbinding te **tenants1-mt -\<gebruiker\>**  en **catalogus-mt -\<gebruiker\>**  servers met behulp van aanmelding = *developer*, wachtwoord = *P@ssword1*.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/ssmsSignIn.png)

In de Objectverkenner, voert u de volgende stappen uit:

1. Vouw de *tenants1-mt -\<gebruiker\>*  server.
2. Vouw het knooppunt Databases uit en Zie *tenants1* -database met meerdere tenants.
3. Vouw de *catalogus-mt -\<gebruiker\>*  server.
4. Controleer of dat u de analytics-store en de jobaccount-database ziet.

Zie de volgende items in de database in de Objectverkenner SSMS door het uitbreiden van het knooppunt van het analytics-store:

- Tabellen **TicketsRawData** en **EventsRawData** onbewerkte opgehaalde gegevens van de tenant-databases bevatten.
- De ster-schema-tabellen zijn **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, en **dim_Dates** .
- De **sp_ShredRawExtractedData** opgeslagen procedure wordt gebruikt voor het vullen van de ster-schema-tabellen uit de tabellen onbewerkte gegevens.

![tenantAnalytics](media/saas-multitenantdb-tenant-analytics/tenantAnalytics.png)

## <a name="data-extraction"></a>Ophalen van gegevens 

### <a name="create-target-groups"></a>Doelgroepen maken 

Voordat u doorgaat, zorg ervoor dat u de taak-account en de jobaccount-database hebt geïmplementeerd. In de volgende reeks stappen, elastische taken gebruikt om gegevens te extraheren uit de database voor shard-tenants, en voor het opslaan van de gegevens in de store analytics. Vervolgens wordt de tweede taak shreds van de gegevens en slaat ze op in tabellen in de ster-schema. Deze twee taken uitvoeren op twee verschillende doelgroepen, namelijk **TenantGroup** en **AnalyticsGroup**. De taak voor uitpakken wordt uitgevoerd op basis van de TenantGroup, die de tenantdatabases bevat. De vernietigen taak wordt uitgevoerd op basis van de AnalyticsGroup, waarin alleen de analytics-store. De doelgroepen maken met behulp van de volgende stappen uit:

1. In SSMS verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\>.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ TargetGroups.sql* 
3. Wijzig de @User variabele aan de bovenkant van het script vervangen <User> met de gebruikerswaarde die wordt gebruikt tijdens de implementatie van de toepassing Wingtip Tickets SaaS multitenant-Database.
4. Druk op **F5** om uit te voeren van het script dat wordt gemaakt van de twee doelgroepen.

### <a name="extract-raw-data-from-all-tenants"></a>Onbewerkte gegevens ophalen uit alle tenants

Transacties kunnen vaker optreden voor *ticket en de klant* gegevens dan voor *gebeurtenis en het rechtsgebied* gegevens. Overweeg daarom ticket en klantgegevens afzonderlijk en vaker dan u gebeurtenis en het rechtsgebied gegevens extraheren uitpakken. In deze sectie kunt u definiëren en twee afzonderlijke taken plannen:

- Ticket en klanten gegevens ophalen.
- Gebeurtenis en het rechtsgebied gegevens ophalen.

Elke taak de gegevens worden uitgepakt en wordt het in het archief van analytics. Een afzonderlijke taak shreds er de opgehaalde gegevens in de analytics ster-schema.

1. In SSMS verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\> server.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ExtractTickets.sql*.
3. Wijzigen @User aan de bovenkant van het script en vervang <User> met de naam van de gebruiker die wordt gebruikt tijdens de implementatie van de toepassing Wingtip Tickets SaaS multitenant-Database. 
4. Druk op **F5** om uit te voeren van het script dat wordt gemaakt en voert de taak die tickets en klanten gegevens uit elke tenantdatabase ophaalt. De taak slaat de gegevens in het archief van analytics.
5. Query uitvoeren op de tabel TicketsRawData in de database tenantanalytics, om ervoor te zorgen dat de tabel worden ingevuld met gegevens van de tickets van alle tenants.

![ticketExtracts](media/saas-multitenantdb-tenant-analytics/ticketExtracts.png)

Herhaal de voorgaande stappen, met uitzondering van deze tijd vervangen **\ExtractTickets.sql** met **\ExtractVenuesEvents.sql** in stap 2.

De taak met succes is uitgevoerd, vult de tabel EventsRawData in de store analytics met nieuwe gebeurtenissen en venues informatie van alle tenants. 

## <a name="data-reorganization"></a>Gegevens opnieuw rangschikken

### <a name="shred-extracted-data-to-populate-star-schema-tables"></a>Versnipperen opgehaalde gegevens zijn ingevuld ster-schema-tabellen

De volgende stap is het versnipperen de uitgepakte onbewerkte gegevens in een set met tabellen die zijn geoptimaliseerd voor analysequery's. Een ster-schema wordt gebruikt. Een centrale feitentabel bevat afzonderlijke ticket verkoopgegevens. Dimensietabellen worden ingevuld met gegevens over evenementen, evenementen, klanten en datums kopen. 

In deze sectie van de zelfstudie, die u kunt definiëren en voert een taak die de uitgepakte onbewerkte gegevens met de gegevens in de tabellen ster-schema worden samengevoegd. Nadat de samenvoegen-taak is voltooid, de onbewerkte gegevens worden verwijderd, waardoor de tabellen klaar om te worden gevuld met de volgende gegevens van de tenant taak uitpakken.

1. In SSMS verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\>.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ShredRawExtractedData.sql*.
3. Druk op **F5** opgeslagen procedure uit in de store analytics om uit te voeren van het script voor het definiëren van een taak die de sp_ShredRawExtractedData aanroept.
4. Geef voldoende tijd voor de taak is uitgevoerd.
    - Controleer de **Lifecycle** kolom van de tabel jobs.jobs_execution voor de status van taak. Zorg ervoor dat de taak **geslaagd** voordat u doorgaat. Een geslaagde uitvoering weergegeven gegevens die vergelijkbaar is met het volgende diagram:

![shreddingJob](media/saas-multitenantdb-tenant-analytics/shreddingJob.PNG)

## <a name="data-exploration"></a>Gegevens verkennen

### <a name="visualize-tenant-data"></a>Tenantgegevens visualiseren

De gegevens in de tabel ster-schema biedt alle de ticket verkoopgegevens die nodig zijn voor uw analyse. Als u wilt maken het gemakkelijker om te zien van trends in grote gegevenssets, moet u grafisch visualiseren.  In deze sectie leert u hoe u **Power BI** om te bewerken en visualiseren van de gegevens van de tenant u hebt uitgepakt en georganiseerd.

Gebruik de volgende stappen uit om te verbinden met Power BI, en voor het importeren van de weergaven die u eerder hebt gemaakt:

1. Start Power BI desktop.
2. Selecteer in het lint Start **gegevens ophalen**, en selecteer **meer...** in het menu.
3. In de **gegevens ophalen** venster, selecteer Azure SQL Database.
4. Voer in het aanmeldingsvenster van de database, de servernaam (catalogus-mt -\<gebruiker\>. database.windows.net). Selecteer **importeren** voor **Gegevensverbindingsmodus**, en klik vervolgens op OK. 

    ![powerBISignIn](media/saas-multitenantdb-tenant-analytics/powerBISignIn.PNG)

5. Selecteer **Database** in het linkerdeelvenster, voert u de gebruikersnaam van de = *developer*, en Voer wachtwoord = *P@ssword1*. Klik op **Verbinden**.  

    ![DatabaseSignIn](media/saas-multitenantdb-tenant-analytics/databaseSignIn.PNG)

6. In de **Navigator** deelvenster onder de analytics-database, selecteert u de tabellen ster-schema: fact_Tickets, dim_Events, dim_Venues, dim_Customers en dim_Dates. Selecteer vervolgens **Load**. 

Gefeliciteerd! U hebt de gegevens zijn geladen in Power BI. U kunt nu beginnen verkennen interessante visualisaties zodat u inzicht in uw tenants. De volgende stapsgewijze instructies voor hoe analytics kunt u gegevensgestuurde aanbevelingen te doen om het Wingtip Tickets-business-team. De aanbevelingen kunnen helpen met het optimaliseren van het model en de klant ervaring.

Begint u met verkoopgegevens om te zien van de variatie in gebruik voor de venues ticket analyseren. De volgende opties selecteren in Power BI om te tekenen van een staafdiagram van het totale aantal tickets die door elke venue verkocht. Vanwege een willekeurige variatie in de generator ticket, kunnen uw resultaten afwijken.
 
![TotalTicketsByVenues](media/saas-multitenantdb-tenant-analytics/TotalTicketsByVenues.PNG)

De voorgaande diagram wordt bevestigd dat het aantal tickets die door elke venue verkocht varieert. Venues die meer tickets kunnen verkopen gebruikt uw service strenger dan venues die minder tickets kunnen verkopen. Mogelijk zijn er een kans om aan te passen van de toewijzing van resources op basis van behoeften van de andere tenant.

Verder kunt u de gegevens om te zien hoe kaartverkoop variëren na verloop van tijd analyseren. De volgende opties selecteren in Power BI om te tekenen het totale aantal tickets die worden verkocht per dag gedurende een periode van 60 dagen.
 
![SaleVersusDate](media/saas-multitenantdb-tenant-analytics/SaleVersusDate.PNG)

De voorgaande grafiek wordt weergegeven dat ticket verkoop piek voor sommige plaatsen. Deze pieken versterking van het idee dat sommige venues mogelijk worden verbruikt systeembronnen niet goed. Er is tot nu toe zonder duidelijk patroon in wanneer de pieken optreden.

Vervolgens wilt u de betekenis van deze Verkoop Piekdagen verder kunnen onderzoeken. Wanneer deze pieken optreden nadat kaartjes voor verkoop? Als u wilt tekenen tickets die worden verkocht per dag, de volgende opties te selecteren in Power BI.

![SaleDayDistribution](media/saas-multitenantdb-tenant-analytics/SaleDistributionPerDay.PNG)

De voorgaande diagram laat zien dat sommige venues veel tickets op de eerste dag van de verkoop verkopen. Zodra de kaartjes voor verkoop op deze venues, lijkt er een gekke spits. Deze burst van activiteit door een paar venues heeft mogelijk invloed op de service voor andere tenants.

U kunt inzoomen op de gegevens opnieuw om te zien of deze gekke beheergedrag geldt voor alle gebeurtenissen die door deze venues worden gehost. In vorige grafieken, dat zich heeft voorgedaan dat Contoso Concert Hall verkoopt veel tickets en die Contoso heeft ook een piek in kaartverkoop op bepaalde dagen. Experimenteren met Power BI-opties voor het tekenen van cumulatieve kaartverkoop voor Contoso Concert Hall, gericht op trends die grote verkoop voor elk van de bijbehorende gebeurtenissen. Volg alle gebeurtenissen hetzelfde patroon verkoop?

![ContosoSales](media/saas-multitenantdb-tenant-analytics/EventSaleTrends.PNG)

De voorgaande diagram voor Contoso Concert Hall laat zien dat de gekke beheergedrag niet is gebeurd voor alle gebeurtenissen. Experimenteren met de filteropties om te zien van verkoop trends voor andere evenementen.

Het inzicht te krijgen in patronen verkopen ticket kunnen Wingtip Tickets te optimaliseren van hun bedrijfsmodel leiden. In plaats van alle tenants even factureren, misschien introduceert Wingtip Servicelagen met verschillende grootten. Grotere venues die nodig hebt om te verkopen meer tickets per dag kunnen worden aangeboden als een hogere laag met een hogere service level agreement (SLA). Deze evenementen hebt kunnen hun databases in de groep met hogere limieten voor per database resource geplaatst. Elke servicelaag hebt kan een toewijzing van verkoop per uur, met extra kosten in rekening gebracht voor het overschrijden van de toewijzing. Grotere venues waarvoor periodieke pieken van verkoop veel voordeel hebben van de hogere lagen en Wingtip Tickets efficiënter inkomsten kunt genereren van hun service.

Ondertussen klagen sommige Wingtip Tickets-klanten dat ze het moeilijk te verkopen voldoende tickets te rechtvaardigen, de servicekosten. In deze inzichten er het is wellicht een kans om kaartverkoop voor bij het uitvoeren van venues te verbeteren. Hogere omzet zou verhoogt de waargenomen waarde van de service. Klik met de rechtermuisknop op fact_Tickets en selecteer **nieuwe meting**. Voer de volgende expressie voor de nieuwe meting met de naam **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Selecteer de volgende Visualisatieopties voor het tekenen van de percentage tickets die door elke venue om te bepalen van hun relatieve succes verkocht.

![analyticsViews](media/saas-multitenantdb-tenant-analytics/AvgTicketsByVenues.PNG)

De voorgaande diagram laat zien dat hoewel de meeste venues meer dan 80% van de tickets verkopen, in sommige problemen ondervinden in te vullen van meer dan de helft van de seats. Experimenteren met de waarden en maximum of minimum percentage van tickets die worden verkocht voor elke locatie selecteren.

Eerder verdiept u uw analyse om te detecteren dat kaartverkoop meestal voorspelbare patronen volgen. Deze detectie kunt mogelijk Wingtip Tickets help venues boost kaartverkoop attenderen door dynamische prijzen aanbevelen. Deze detectie kan de mogelijkheid om te worden machine learning-technieken om te voorspellen kaartverkoop voor elke gebeurtenis onthullen. Voorspellingen kunnen ook worden gemaakt voor de impact op de omzet van kortingen op kaartverkoop-aanbieding. Power BI Embedded kan worden geïntegreerd in een event management-toepassing. De integratie kan helpen bij het visualiseren van de voorspelde verkoop- en het effect van verschillende kortingen. De toepassing kan helpen bij het ontwerpen van een optimale korting rechtstreeks vanuit de analytics-weergave worden toegepast.

U hebt de ontwikkeling van gegevens van de tenant van de toepassing Wingtip Tickets SaaS multitenant-Database in acht genomen. U kunt andere manieren om die de app zakelijke beslissingen te nemen voor leveranciers van SaaS-toepassingen informeren kan willen. Leveranciers kunnen beter geschikt zijn voor de behoeften van hun tenants. In deze zelfstudie is de u hopelijk uitgerust met hulpprogramma's die nodig zijn voor analyses uitvoeren op gegevens van de tenant voor die te maken van uw bedrijf om gegevensgestuurde beslissingen te nemen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> - Een tenant analytics-database met vooraf gedefinieerde sterschema tabellen geïmplementeerd
> - Elastische taken gebruikt om gegevens te extraheren uit alle tenantdatabase
> - De opgehaalde gegevens samenvoegen in tabellen in een ster-schema die is ontworpen voor analyses
> - Query uitvoeren op een analytics-database 
> - Power BI voor gegevensvisualisatie gebruiken om te observeren trends in de gegevens van de tenant 

Gefeliciteerd!

## <a name="additional-resources"></a>Aanvullende resources

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials). -->
- [Elastische taken](sql-database-elastic-jobs-overview.md).
