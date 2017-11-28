---
title: Analytics-query's uitvoeren op Azure SQL-databases | Microsoft Docs
description: Cross-tenant analytics query's met behulp van gegevens uit meerdere databases van Azure SQL Database hebt uitgepakt.
keywords: SQL-zelfstudie
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/08/2017
ms.author: anjangsh; billgib; genemi
ms.openlocfilehash: 549b6abf5728e50ee365f40326263d391e4b26fd
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="cross-tenant-analytics-using-extracted-data"></a>Cross-tenant analytics met gegevens opgehaald

In deze zelfstudie maakt doorlopen u een volledige analytics scenario. Het scenario laat zien hoe analytics bedrijven slimme beslissingen kunt inschakelen. Gegevens die zijn opgehaald uit de shard-database gebruikt, kunt u analytics inzicht in het gedrag van de tenant, met inbegrip van hun gebruik van de voorbeeldtoepassing Wingtip Tickets SaaS. Dit scenario omvat drie stappen: 

1.  **Gegevens extraheren** uit de database van elke tenant naar een winkel analytics.
2.  **De opgehaalde gegevens optimaliseren** voor analytics verwerking.
3.  Gebruik **Business Intelligence** hulpprogramma's om handig inzicht die besluitvorming kunnen leiden. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Maak de tenant analytics opslaan om op te halen van de gegevens in.
> - Elastische taken gebruiken om gegevens te extraheren uit de database van elke tenant in het archief analytics.
> - De opgehaalde gegevens (reorganize in een ster-schema) optimaliseren.
> - Query uitvoeren op de analytics-database.
> - Gebruik Power BI voor gegevensvisualisatie te markeren trends in gegevens van de tenant en aanbeveling voor verbeteringen te doen.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/architectureOverview.png)

## <a name="offline-tenant-analytics-pattern"></a>Offline tenant analytics patroon

SaaS-toepassingen die u ontwikkelt hebben toegang tot een enorme hoeveelheid tenant-gegevens die zijn opgeslagen in de cloud. De gegevens biedt een uitgebreide bron van informatie over de werking en het gebruik van uw toepassing en het gedrag van de tenants. Deze inzichten kunnen leiden functie ontwikkeling, bruikbaarheid verbeteringen en andere investeringen in de app en het platform.

Toegang tot de gegevens voor alle tenants is eenvoudig wanneer alle gegevens in een multitenant-database. Maar de toegang is complexer wanneer over duizenden databases op grote schaal gedistribueerd. Er is een manier om de complexiteit pak uitpakken van de gegevens naar een analytics-database of een datawarehouse. U vervolgens opvragen het datawarehouse voor het verzamelen van insights uit de tickets gegevens van alle tenants.

Deze zelfstudie toont een scenario voltooid analytics voor dit voorbeeld SaaS-toepassing. Eerste, elastische taken worden gebruikt voor het plannen van de extractie van gegevens uit de database van elke tenant. De gegevens worden verzonden naar een winkel analytics. De store analytics kan zijn van een SQL-Database of een SQL Data Warehouse. Voor het ophalen van grootschalige gegevens [Azure Data Factory](../data-factory/introduction.md) prees is.

Vervolgens wordt de geaggregeerde gegevens in een set is vernietigd [ster-schema](https://www.wikipedia.org/wiki/Star_schema) tabellen. De tabellen bestaan uit een centrale feitentabel plus gerelateerde dimensietabellen:

- De centrale feitentabel in de ster-schema bevat ticket gegevens.
- De dimensietabellen bevatten gegevens over plaatsen, gebeurtenissen, klanten en datums kopen.

Samen het midden- en dimensie tabellen inschakelen efficiënt analytische verwerking. De ster-schema dat wordt gebruikt in deze zelfstudie wordt weergegeven in de volgende afbeelding:
 
![StarSchema](media/saas-multitenantdb-tenant-analytics/StarSchema.png)

Ten slotte worden de ster-schema-tabellen opgevraagd. De queryresultaten worden visueel weergegeven om uit te lichten inzicht in het gedrag van de tenant en het gebruik van de toepassing. Met dit ster-schema, kunt u query's waarmee u achterhalen welke items als volgt uitvoeren:

- Wie is aanschaf van tickets en vanaf welke u wilt.
- Verborgen patronen en trends in de volgende gebieden:
    - De verkoop van tickets.
    - De relatieve populariteit van elke locatie vast.

Begrijpen hoe consistent elke tenant wordt via de service biedt de mogelijkheid voor het maken van de service-abonnementen om te voorzien in hun behoeften. Deze zelfstudie bevat eenvoudige voorbeelden van inzichten die kunnen worden achterhaald van gegevens van de tenant.

## <a name="setup"></a>Instellen

### <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:

- De toepassing Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)
- De Wingtip SaaS-scripts en de toepassing [broncode](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) worden gedownload van GitHub. Zorg ervoor dat *blokkering van het zip-bestand* voordat het uitpakken van de inhoud ervan. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.
- Power BI Desktop is geïnstalleerd. [Download Power BI Desktop](https://powerbi.microsoft.com/downloads/)
- De batch van extra tentants is ingericht, raadpleegt u de [ **inrichten tenants zelfstudie**](saas-multitenantdb-provision-and-catalog.md).
- Een taak account en accountdatabase taak hebt gemaakt. Zie de juiste stappen in de [ **Schema management zelfstudie**](saas-multitenantdb-schema-management.md#create-a-job-account-database-and-new-job-account).

### <a name="create-data-for-the-demo"></a>Gegevens voor de demo maken

In deze zelfstudie wordt analyse uitgevoerd op de verkoopgegevens ticket. In de huidige stap kunt u gegevens van de ticket genereren voor de tenants.  Deze gegevens worden later opgehaald voor analyse. *Zorg ervoor dat u hebt ingericht, de batch van tenants zoals eerder beschreven, zodat u een zinvolle hoeveelheid gegevens hebt*. Een voldoende grote hoeveelheid gegevens kan een aantal verschillende ticket aanschaffen patronen worden blootgesteld.

1. In **PowerShell ISE**Open *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1*, en stel de volgende waarde:
    - **$DemoScenario** = **1** kopen van tickets voor gebeurtenissen op alle plaatsen
2. Druk op **F5** uitvoeren van het script en ticket aanschaffen van een geschiedenis voor elke gebeurtenis in elke wilt maken.  Het script kan enkele minuten voor het genereren van tienduizenden tickets worden uitgevoerd.

### <a name="deploy-the-analytics-store"></a>De store analytics implementeren
Er zijn vaak meerdere transactionele shard databases die samen alle gegevens van de tenant bevatten. U moet de gegevens van de tenant uit de shard-database samenvoegen in één analytics archief. De aggregatie kan efficiënt query van de gegevens. In deze zelfstudie wordt een Azure SQL Database-database gebruikt voor het opslaan van de verzamelde gegevens.

In de volgende stappen die u implementeert de store analytics, die is aangeroepen **tenantanalytics**. U implementeren vooraf gedefinieerde tabellen die zijn gevuld verderop in de zelfstudie ook:
1. Open in PowerShell ISE *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1* 
2. Stel de variabele $DemoScenario in het script moet overeenkomen met uw keuze van analytics store. Voor het leren doeleinden, wordt SQL-database zonder columnstore aanbevolen.
    - Instellen voor het gebruik van SQL-database zonder columnstore **$DemoScenario** = **2**
    - Met SQL-database met columnstore, stelt **$DemoScenario** = **3**  
3. Druk op **F5** de demoscript uit te voeren (die roept de *implementeren TenantAnalytics<XX>.ps1* script) die de tenant analytics store maakt. 

Nu dat u hebt de toepassing is geïmplementeerd en ingevuld interessante gegevens van de tenant, [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) om verbinding te **tenants1-mt -\<gebruiker\>**  en **catalogus-mt -\<gebruiker\>**  servers met behulp van aanmelding = *developer*, wachtwoord =  *P@ssword1* .

![architectureOverView](media/saas-multitenantdb-tenant-analytics/ssmsSignIn.png)

In de Objectverkenner, moet u de volgende stappen uitvoeren:

1. Vouw de *tenants1-mt -\<gebruiker\>*  server.
2. Vouw het knooppunt Databases uit en Zie *tenants1* database met meerdere tenants.
3. Vouw de *catalogus-mt -\<gebruiker\>*  server.
4. Controleer of u de store analytics en de database jobaccount zien.

Zie de volgende items in de database in de Objectverkenner SSMS door het uitbreiden van het knooppunt analytics-store:

- Tabellen **TicketsRawData** en **EventsRawData** onbewerkte geëxtraheerde gegevens uit de tenant-databases.
- De ster-schema-tabellen zijn **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, en **dim_Dates** .
- De **sp_ShredRawExtractedData** opgeslagen procedure wordt gebruikt voor het vullen van de ster-schema-tabellen uit de onbewerkte gegevenstabellen.

![tenantAnalytics](media/saas-multitenantdb-tenant-analytics/tenantAnalytics.png)

## <a name="data-extraction"></a>Ophalen van gegevens 

### <a name="create-target-groups"></a>Doelgroepen maken 

Voordat u doorgaat, zorg ervoor dat u de taak account en jobaccount database hebt geïmplementeerd. In de volgende reeks stappen elastische taken gebruikt om gegevens te extraheren uit de database shard tenants en voor het opslaan van de gegevens in het archief analytics. Vervolgens wordt de tweede taak shreds van de gegevens en opgeslagen in tabellen in de ster-schema. Deze twee taken uitvoeren op twee verschillende doelgroepen namelijk **TenantGroup** en **AnalyticsGroup**. De taak wordt uitgevoerd tegen de TenantGroup waarin alle databases in de tenant. De vernietigen taak uitgevoerd tegen de AnalyticsGroup die het analytics-archief bevat. De doelgroepen maken met behulp van de volgende stappen uit:

1. In SSMS, verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\>.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ TargetGroups.sql* 
3. Wijzig de @User variabele aan de bovenkant van het script vervangen <User> met de waarde van de gebruiker gebruikt wanneer u de toepassing Wingtip Tickets SaaS multitenant Database geïmplementeerd.
4. Druk op **F5** om uit te voeren van het script dat wordt gemaakt van de twee doelgroepen.

### <a name="extract-raw-data-from-all-tenants"></a>Onbewerkte gegevens van alle tenants ophalen

Transacties optreden vaker voor *ticket en de klant* gegevens dan voor *gebeurtenis en u wilt* gegevens. Overweeg daarom ticket en klantgegevens afzonderlijk en vaker dan het extraheren van gebeurtenis-en u wilt uitpakken. In deze sectie kunt u definiëren en twee afzonderlijke taken plannen:

- Ticket- en -gegevens ophalen.
- Gebeurtenis-en u wilt extraheren.

Elke taak haalt gegevens en verzendt deze in het archief analytics. Een afzonderlijke taak shreds er geëxtraheerde gegevens in het analytics ster-schema.

1. In SSMS, verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\> server.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ExtractTickets.sql*.
3. Wijzig @User aan de bovenkant van het script en vervang <User> met de naam van de gebruiker gebruikt wanneer u de toepassing Wingtip Tickets SaaS multitenant Database geïmplementeerd. 
4. Druk op **F5** om uit te voeren van het script dat wordt gemaakt en wordt de taak die tickets en klanten gegevens uit de database van elke tenant ophaalt wordt uitgevoerd. De taak slaat de gegevens in het archief analytics.
5. De tabel TicketsRawData in de database tenantanalytics om ervoor te zorgen dat de tabel is gevuld met informatie van tickets van alle tenants opvragen.

![ticketExtracts](media/saas-multitenantdb-tenant-analytics/ticketExtracts.png)

Herhaal de stappen, maar deze tijd vervangen **\ExtractTickets.sql** met **\ExtractVenuesEvents.sql** in stap 2.

Waarop de taak wordt uitgevoerd, wordt de tabel EventsRawData in het archief analytics met nieuwe gebeurtenissen en evenementen gegevens uit alle huurders gevuld. 

## <a name="data-reorganization"></a>Gegevens rangschikken

### <a name="shred-extracted-data-to-populate-star-schema-tables"></a>Versnipperen opgehaalde gegevens zijn ingevuld tabellen ster-schema

De volgende stap is het versnipperen de uitgepakte onbewerkte gegevens in een set met tabellen die zijn geoptimaliseerd voor analytics query's. Een ster-schema wordt gebruikt. Een centrale feitentabel bevat afzonderlijke ticket verkoopgegevens. Dimensietabellen worden ingevuld met gegevens over plaatsen, gebeurtenissen, klanten en datums kopen. 

In deze sectie van de zelfstudie definiëren en een taak uitvoeren die de uitgepakte onbewerkte gegevens met de gegevens in de ster-schema-tabellen worden samengevoegd. Nadat de merge-taak is voltooid, de onbewerkte gegevens worden verwijderd, verlaat de tabellen gereed om te worden gevuld door de volgende gegevens voor de tenant taak uitpakken.

1. In SSMS, verbinding maken met de **jobaccount** database in de catalogus-mt -\<gebruiker\>.
2. Open in SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ShredRawExtractedData.sql*.
3. Druk op **F5** voor het script uitvoeren om te definiëren van een taak die de sp_ShredRawExtractedData roept opgeslagen procedure uit in het archief analytics.
4. Geef voldoende tijd voor de taak te kunnen uitvoeren.
    - Controleer de **Lifecycle** kolom van de tabel jobs.jobs_execution voor de status van taak. Zorg ervoor dat de taak **geslaagd** voordat u doorgaat. Een geslaagde uitvoering gegevens vergelijkbaar met het volgende diagram worden weergegeven:

![shreddingJob](media/saas-multitenantdb-tenant-analytics/shreddingJob.PNG)

## <a name="data-exploration"></a>Gegevensverkenning

### <a name="visualize-tenant-data"></a>Tenant-gegevens visualiseren

De gegevens in de tabel ster-schema bieden alle de ticket verkoopgegevens die nodig zijn voor uw analyse. Om het gemakkelijker om te zien van trends in grote gegevenssets maken, moet u grafisch visualiseren.  In dit gedeelte vindt u informatie over het gebruik **Power BI** worden bewerkt en visualiseren van de tenant gegevens die u hebt uitgepakt en geordend.

Verbinding maken met Power BI, en voor het importeren van de weergaven die u eerder hebt gemaakt, gebruikt u de volgende stappen uit:

1. Starten van Power BI desktop.
2. Selecteer in het lint Home **gegevens ophalen**, en selecteer **meer...** in het menu.
3. In de **gegevens ophalen** venster, selecteer Azure SQL Database.
4. Voer de servernaam van uw in het venster van de aanmelding database (catalogus-mt -\<gebruiker\>. database.windows.net). Selecteer **importeren** voor **Connectiviteitsmodus gegevens**, en klik op OK. 

    ![powerBISignIn](media/saas-multitenantdb-tenant-analytics/powerBISignIn.PNG)

5. Selecteer **Database** Voer gebruikersnaam in het linkerdeelvenster = *developer*, en Voer wachtwoord =  *P@ssword1* . Klik op **Verbinden**.  

    ![DatabaseSignIn](media/saas-multitenantdb-tenant-analytics/databaseSignIn.PNG)

6. In de **Navigator** deelvenster onder de analytics-database, selecteer de tabellen ster-schema: fact_Tickets, dim_Events, dim_Venues, dim_Customers en dim_Dates. Selecteer vervolgens **Load**. 

Gefeliciteerd. U hebt de gegevens in Power BI geladen. U kunt nu starten verkennen interessante visualisaties zodat u inzicht in uw tenants. Naast helpt u bij hoe analytics kunt u gegevensgestuurde aanbevelingen voor het managementteam Wingtip Tickets. De aanbevelingen kunnen u het model en de klant ervaring te optimaliseren.

Begint u met de analyse van ticket verkoopgegevens om te zien van de variatie in gebruik over de plaatsen. De volgende opties selecteren in Power BI om te tekenen van een staafdiagram van het totale aantal tickets verkocht door elke locatie vast. Als gevolg van willekeurige variaties in de generator ticket kunnen uw resultaten afwijken.
 
![TotalTicketsByVenues](media/saas-multitenantdb-tenant-analytics/TotalTicketsByVenues.PNG)

De voorgaande grafiek bevestigt dat het aantal tickets verkocht door elke locatie vast varieert. Plaatsen die meer tickets verkopen gebruikt uw service dan plaatsen die minder tickets verkopen. Mogelijk zijn er een kans om aan te passen resourcetoewijzing op basis van andere tenant behoeften.

Verder kunt u de gegevens om te zien hoe de verkoop ticket variëren na verloop van tijd analyseren. De volgende opties selecteren in Power BI om te tekenen van het totale aantal tickets verkocht per dag voor een periode van 60 dagen.
 
![SaleVersusDate](media/saas-multitenantdb-tenant-analytics/SaleVersusDate.PNG)

Het voorgaande diagram toont dat ticket verkoop piek voor sommige plaatsen. Deze pieken versterking van het idee dat sommige plaatsen van de systeembronnen niet goed maken mogelijk. Er is tot nu toe zonder duidelijk patroon in wanneer de pieken optreden.

Vervolgens wilt u verder onderzoek de betekenis van deze Verkoop Piekdagen. Wanneer deze pieken optreden nadat tickets verkoop gaat? Als u wilt getekend tickets per dag is verkocht, moet u de volgende opties selecteren in Power BI.

![SaleDayDistribution](media/saas-multitenantdb-tenant-analytics/SaleDistributionPerDay.PNG)

De voorgaande grafiek ziet u dat sommige plaatsen veel tickets op de eerste dag van de verkoop verkopen. Als u de tickets gaat verkoop op deze plaatsen, lijkt er een gekke snel uitgevoerd. Deze burst van activiteit door enkele plaatsen mogelijk invloed op de service voor andere tenants.

U kunt inzoomen op de gegevens opnieuw te controleren of deze gekke snel uitgevoerd geldt voor alle gebeurtenissen die worden gehost door deze plaatsen. In vorige waarnemingspunten waargenomen u dat Contoso overleg Hall veel tickets verkocht, en dat Contoso heeft ook een piek in ticket verkoop op bepaalde dagen. Uitproberen met Power BI-opties voor het uitzetten van cumulatieve ticket sales voor Contoso overleg Hall, te focussen op verkoop trends voor elk van de bijbehorende gebeurtenissen. Voer alle gebeurtenissen hetzelfde patroon verkoop?

![ContosoSales](media/saas-multitenantdb-tenant-analytics/EventSaleTrends.PNG)

De voorgaande tekent voor Contoso overleg Hall toont het gekke enthousiasme gebeurt niet voor alle gebeurtenissen. De filteropties voor een overzicht van trends voor andere evenementen verkoop uitproberen.

De inzichten in ticket verkopen patronen kunnen leiden Wingtip kaartjes voor hun bedrijfsmodel optimaliseren. In plaats van alle huurders evenveel laden, mogelijk introduceert Wingtip Servicelagen met verschillende prestatieniveaus. Grotere plaatsen die nodig zijn om te verkopen meer tickets per dag kunnen worden aangeboden als een hogere laag met een hogere service level agreement (SLA). De plaatsen van uitvoering kunnen hebben hun databases in de groep met hogere limieten per database geplaatst. Elke servicelaag kan een toewijzing voor de per uur verkoop hebben met extra kosten in rekening gebracht voor het overschrijden van de toewijzing. Grotere plaatsen waarvoor periodieke lichtflitsen verkoop zouden kunnen profiteren van de hogere lagen en Wingtip Tickets kunt Verdien er geld mee hun service efficiënter.

Ondertussen klagen sommige klanten Wingtip Tickets dat ze moeilijk te verkopen voldoende tickets te rechtvaardigen de servicekosten. In deze inzichten is er wellicht een kans om de verkoop van de ticket voor onder het uitvoeren van plaatsen. Hogere verkoop zou een hogere waargenomen waarde van de service. Klik met de rechtermuisknop op fact_Tickets en selecteer **nieuwe meting**. Voer de volgende expressie voor de nieuwe meting aangeroepen **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Selecteer de volgende Visualisatieopties voor het uitzetten van de percentage tickets die door elke locatie vast om te bepalen van hun relatieve succes verkocht.

![analyticsViews](media/saas-multitenantdb-tenant-analytics/AvgTicketsByVenues.PNG)

De voorgaande grafiek ziet u dat hoewel de meeste plaatsen meer dan 80% van hun ticket verkopen, sommige zijn moesten voor het vervullen van meer dan de helft plaatsen. Uitproberen met de waarden goed maximum of minimum percentage van tickets voor elke locatie vast verkocht selecteren.

Eerder verdiept u uw analyse om te detecteren dat ticket verkoop hebben vaak voorspelbare patronen te volgen. Deze detectie kunt mogelijk Wingtip Tickets help plaatsen versterking ticket verkoop attenderen door dynamische prijzen aanbevelen. Deze detectie kan onthullen gelegenheid alvast machine learning-technieken te voorspellen ticket verkoop voor elke gebeurtenis. Voorspellingen kunnen ook worden gemaakt voor de gevolgen voor de omzet van biedt kortingen op ticket verkoop. Power BI Embedded kan worden geïntegreerd in de management-toepassing voor een gebeurtenis. De integratie kan helpen bij het visualiseren van voorspelde verkoop en het effect van verschillende kortingen. De toepassing kan helpen bij het ontwerpen van een optimale korting rechtstreeks vanuit de weergave analytics worden toegepast.

U hebt waargenomen trends in gegevens van de tenant uit de Database Wingtip Tickets SaaS-multitenant-toepassing. U kunt andere manieren die de app zakelijke beslissingen te nemen voor leveranciers van SaaS-toepassingen informeren kan willen. Leveranciers kunnen beter geschikt zijn voor de behoeften van hun tenants. Deze zelfstudie is de u ideale geval uitgerust met hulpprogramma's die nodig zijn voor het uitvoeren van analyses op tenant gegevens zorgen dat uw bedrijf om gegevensgestuurde beslissingen te nemen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> - Een tenant analytics-database met vooraf gedefinieerde sterschema tabellen geïmplementeerd
> - Elastische taken gebruikt om gegevens te extraheren uit alle van de tenant-database
> - De opgehaalde gegevens samenvoegen in tabellen in een ster-schema ontworpen voor analyses
> - Query analytics-database 
> - Power BI voor gegevensvisualisatie gebruiken om te zien van trends in gegevens van de tenant 

Gefeliciteerd.

## <a name="additional-resources"></a>Aanvullende bronnen

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials). -->
- [Elastische taken](sql-database-elastic-jobs-overview.md).
