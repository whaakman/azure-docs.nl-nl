---
title: Analytics-query's uitvoeren op basis van de tenant-databases via de Azure SQL Data Warehouse | Microsoft Docs
description: Cross-tenant-analytics query's met behulp van gegevens uit meerdere Azure SQL Database-databases hebt uitgepakt.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: ''
author: anumjs
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: anjangsh
ms.openlocfilehash: 9dad675188782f3feb769e47230aa7b0cb42b10e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45737206"
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>SaaS-analytics met Azure SQL Database, SQL Data Warehouse, Data Factory en Power BI verkennen

In deze zelfstudie doorloopt u een end-to-end-analytics-scenario. Het scenario laat zien hoe analyses over de gegevens van de tenant softwareleveranciers om slimme beslissingen kunt maken. Gegevens opgehaald uit elke tenantdatabase gebruikt, u analytics gebruiken voor inzicht in de tenant-gedrag, met inbegrip van hun gebruik van de Wingtip Tickets SaaS-voorbeeldtoepassing. In dit scenario bestaat uit drie stappen: 

1.  **Gegevens ophalen** vanaf elke tenant-database in een store analytics in dit geval een SQL Data Warehouse.
2.  **De opgehaalde gegevens optimaliseren** voor analyseverwerking.
3.  Gebruik **Business Intelligence** hulpprogramma's om nuttige inzichten kunnen dienen als richtlijn besluitvorming. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Maak de tenant analytics opslaan voor het laden.
> - Azure Data Factory (ADF) gebruiken om gegevens te extraheren uit elke tenantdatabase in het datawarehouse analytics.
> - Optimaliseer de opgehaalde gegevens (reorganize in een ster-schema).
> - Query uitvoeren op de analytics-datawarehouse.
> - Gebruik Power BI voor gegevensvisualisatie trends in de gegevens van de tenant markeren en aanbevelingen voor verbetering.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Analyses over de gegevens van de uitgepakte tenant

SaaS-toepassingen bevatten een mogelijk enorme hoeveelheid gegevens van de tenant in de cloud. Deze gegevens kan bieden een rijke bron van informatie over de werking en het gebruik van uw toepassing en het gedrag van uw tenants. Deze inzichten kunnen dienen als richtlijn ontwikkelen van functies, bruikbaarheid verbeteren en andere investeringen doen in de apps en -platform.

Het is eenvoudig om toegang tot de gegevens voor alle tenants wanneer alle gegevens in een multitenant-database. Maar toegang is complexer wanneer op schaal zijn gedistribueerd via duizenden databases. Eén manier kunt beheersen van de complexiteit is om op te halen van de gegevens naar een analytics-database of een datawarehouse voor de query.

Deze zelfstudie toont een end-to-end-analytics-scenario voor de toepassing Wingtip Tickets. Eerste, [Azure Data Factory (ADF)](../data-factory/introduction.md) wordt gebruikt als de orchestration-hulpprogramma tickets verkoop- en gerelateerde gegevens te extraheren uit elke tenantdatabase. Deze gegevens wordt in faseringstabellen in een store analytics geladen. De store analytics kan zijn van een SQL-Database of een SQL Data Warehouse. Deze zelfstudie wordt gebruikgemaakt van [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) opslaan als de analyse.

Vervolgens de uitgepakte gegevens worden getransformeerd en geladen in een set [ster-schema](https://www.wikipedia.org/wiki/Star_schema) tabellen. De tabellen bestaan uit een centrale feitentabel plus gerelateerde dimensietabellen:

- De centrale feitentabel in de ster-schema bevat ticket gegevens.
- De dimensietabellen bevatten gegevens over evenementen, evenementen, klanten en datums kopen.

Samen het midden- en dimensie tabellen inschakelen efficiënte analytische verwerking. De ster-schema in deze zelfstudie gebruikt, wordt weergegeven in de volgende afbeelding:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Ten slotte worden de ster-schema-tabellen opgevraagd. Queryresultaten weergegeven visueel met Power BI te lichten in het gedrag van de tenant en het gebruik van de toepassing. Met deze ster-schema, moet u query's die beschikbaar maken uitvoeren:

- Wie is tickets kopen en vanaf welke locatie.
- Patronen en trends in de verkoop van tickets.
- De relatieve populariteit van elke locatie.

Deze zelfstudie biedt eenvoudige voorbeelden van inzichten die kunnen worden afgelezen uit de Wingtip Tickets-gegevens. Informatie over hoe elke venue gebruikmaakt van de service kan leiden tot de leverancier van de Wingtip Tickets om na te denken over verschillende service-plannen die zijn gericht op meer of minder actieve venues bijvoorbeeld. 

## <a name="setup"></a>Instellen

### <a name="prerequisites"></a>Vereisten

> [!NOTE]
> In deze zelfstudie maakt gebruik van functies van Azure Data Factory die zich momenteel in een beperkte Preview-versie (parameterisering van de gekoppelde service). Als u doen in deze zelfstudie wilt, geeft u uw abonnements-ID [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). We sturen u een bevestiging zodra uw abonnement is ingeschakeld.

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:
- De toepassing Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md).
- De Wingtip Tickets SaaS Database Per Tenant scripts en de toepassing [broncode](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) worden gedownload uit GitHub. Zie voor downloadinstructies ontvangen. Zorg ervoor dat u *blokkering van het zip-bestand* voordat het uitpakken van de inhoud ervan.
- Power BI Desktop is geïnstalleerd. [Power BI Desktop downloaden](https://powerbi.microsoft.com/downloads/).
- De batch verschillende tenants tegelijk is ingericht, raadpleegt u de [ **de zelfstudie tenants inrichten**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Maken van gegevens voor de demo

Deze zelfstudie worden analytics via verkoopgegevens ticket. In deze stap kunt u gegevens van de ticket genereren voor alle tenants. Deze gegevens worden geëxtraheerd in een latere stap voor analyse. _Zorg ervoor dat u de batch met tenants ingericht_ ticket aankooppatronen (zoals eerder is beschreven) zodat er voldoende gegevens om een scala aan verschillende zichtbaar te maken.

1. Open in PowerShell ISE, *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1*, en stel de volgende waarde:
    - **$DemoScenario** = **1** tickets kopen voor voorstellingen bij alle theaters
2. Druk op **F5** uitvoeren van het script en geschiedenis voor alle theaters van kaartverkoop te maken. Met 20-tenants, het script genereert tienduizenden tickets en 10 minuten of langer kan duren.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>SQL datawarehouse, Data Factory, implementeren en Blob Storage 
In de app Wingtip Tickets transactionele gegevens van de tenants verdeeld over verschillende databases. Azure Data Factory (ADF) wordt gebruikt voor het indelen van de Extract, Load en Transform (ELT) van deze gegevens in het datawarehouse. Om gegevens te laden in SQL Data Warehouse meest efficiënt, ADF haalt u gegevens in de tussenliggende blob-bestanden en gebruikt vervolgens [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) de gegevens laden in het datawarehouse.   

In deze stap implementeert u de extra resources die tijdens deze zelfstudie: een SQL Data Warehouse met de naam _tenantanalytics_, een Azure-Gegevensfactory met de naam _dbtodwload -\<gebruiker\>_  , en een Azure storage-account met de naam _wingtipstaging\<gebruiker\>_. Het opslagaccount dat wordt gebruikt om tijdelijk uitgepakte bestanden als blobs voordat ze geladen in het datawarehouse worden. Deze stap implementeert u de datawarehouse-schema ook en definieert de ADF-pijplijnen die de ELT-proces indelen.
1. Open in PowerShell ISE, *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo-TenantAnalyticsDW.ps1* en ingesteld:
    - **$DemoScenario** = **2** tenant analytics-datawarehouse, blob-opslag en data factory implementeren 
1. Druk op **F5** de demoscript uitvoeren en implementeren van de Azure-resources. 

Bekijk nu de Azure-resources die u geïmplementeerd:
#### <a name="tenant-databases-and-analytics-store"></a>Tenant-databases en analytics store
Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) verbinding maken met **tenants1-dpt -&lt;gebruiker&gt;**  en **catalogus-dpt -&lt;gebruiker&gt;**  servers. Vervang &lt;gebruiker&gt; met de waarde die wordt gebruikt tijdens de implementatie van de app. Gebruik aanmelding = *developer* en het wachtwoord = *P@ssword1*. Zie de [inleidende zelfstudie](saas-dbpertenant-wingtip-app-overview.md) voor meer informatie.

![Verbinding maken met SQL Database-server via SSMS](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

In Object Explorer:

1. Vouw de *tenants1-dpt -&lt;gebruiker&gt;*  server.
1. Vouw het knooppunt Databases uit en bekijk de lijst van de tenant-databases.
1. Vouw de *catalogus-dpt -&lt;gebruiker&gt;*  server.
1. Controleer of dat u ziet dat de analytics opslaan met de volgende objecten:
    1. Tabellen **raw_Tickets**, **raw_Customers**, **raw_Events** en **raw_Venues** onbewerkte opgehaalde gegevens van de tenant-databases bevatten.
    1. De ster-schema-tabellen zijn **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, en **dim_Dates** .
    1. De opgeslagen procedure **sp_transformExtractedData** wordt gebruikt om de gegevens te transformeren en laden in de tabellen ster-schema.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Blob Storage
1. In de [Azure Portal](https://ms.portal.azure.com), gaat u naar de resourcegroep die u hebt gebruikt voor het implementeren van de toepassing. Controleer of dat er een opslagaccount met de naam **wingtipstaging\<gebruiker\>**  is toegevoegd.

  ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Klik op **wingtipstaging\<gebruiker\>**  storage-account om te verkennen van de objecten die aanwezig zijn.
1. Klik op **Blobs** tegel
1. Klik op de container **configfile**
1. Controleer **configfile** bevat een JSON-bestand met de naam **TableConfig.json**. Dit bestand bevat de bron- en tabelnamen, kolomnamen en beheer-kolomnaam.

#### <a name="azure-data-factory-adf"></a>Azure Data Factory (ADF)
In de [Azure Portal](https://ms.portal.azure.com) Controleer in de resourcegroep, of dat er een Azure-Gegevensfactory met de naam _dbtodwload -\<gebruiker\>_  is toegevoegd. 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

In deze sectie behandelt de data factory hebt gemaakt. Volg onderstaande stappen voor het starten van de data factory:
1. Klik in de portal op de data factory met de naam **dbtodwload -\<gebruiker\>**.
2. Klik op **Author & Monitor** tegel om te starten van de ontwerpfunctie voor Data Factory's in een afzonderlijk tabblad. 

## <a name="extract-load-and-transform-data"></a>Extraheren, laden, en gegevens transformeren
Azure Data Factory wordt gebruikt voor het indelen van extraheren, laden en transformatie van gegevens. In deze zelfstudie hebt u gegevens ophalen uit vier verschillende SQL-weergaven uit elk van de tenant-databases: **rawTickets**, **rawCustomers**, **rawEvents**, en  **rawVenues**. Deze weergaven bevatten locatie-Id, zodat u gegevens vanaf elke locatie in het datawarehouse onderscheid kunt maken. De gegevens in de bijbehorende tijdelijke tabellen in het datawarehouse wordt geladen: **raw_Tickets**, **raw_customers**, **raw_Events** en **raw_Venue**. Een opgeslagen procedure vervolgens de onbewerkte gegevens worden getransformeerd en vult u de tabellen ster-schema: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** , en **dim_Dates**.

In de vorige sectie hebt geïmplementeerd en de benodigde Azure-resources, met inbegrip van de data factory geïnitialiseerd. De geïmplementeerde data factory bevat de pijplijnen, gegevenssets, gekoppelde services, enz., extraheren, laden en transformeren van de gegevens van de tenant. Laten we deze objecten verder en trigger de pijplijn gegevens van de tenant-databases verplaatsen naar het datawarehouse verkennen.

### <a name="data-factory-pipeline-overview"></a>Overzicht van Data factory-pijplijn
In deze sectie behandelt de objecten die zijn gemaakt in de data factory. De volgende afbeelding wordt de algehele werkstroom beschreven van de ADF-pijplijn in deze zelfstudie gebruikt. Als u liever later verkennen van de pijplijn en de resultaten eerst zien, gaat u naar de volgende sectie **de pijplijnuitvoering activeren**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Schakel over naar de pagina overzicht van **auteur** tabblad in het linkerdeelvenster en u ziet dat er drie zijn [pijplijnen](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) en drie [gegevenssets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) gemaakt.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

De drie geneste pijplijnen zijn: SQLDBToDW DBCopy en TableCopy.

**Pijplijn 1 - SQLDBToDW** zoekt de namen van de tenant-databases die zijn opgeslagen in de catalogusdatabase (de naam van tabel: [__ShardManagement]. [ ShardsGlobal]) en voor elke tenantdatabase voert de **DBCopy** pijplijn. Na voltooiing, de opgegeven **sp_TransformExtractedData** opgeslagen procedure-schema wordt uitgevoerd. Deze opgeslagen procedure de geladen gegevens in de faseringstabellen worden getransformeerd en vult de ster-schema-tabellen.

**Pijplijn 2 - DBCopy** zoekt de namen van de brontabellen en kolommen uit een configuratiebestand opgeslagen in blob storage.  De **TableCopy** pijplijn wordt vervolgens uitgevoerd voor elk van de vier tabellen: TicketFacts, CustomerFacts EventFacts en VenueFacts. De **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** activiteit wordt uitgevoerd voor alle 20 databases parallel. ADF kunt maximaal 20 lus iteraties parallel worden uitgevoerd. Houd rekening met het maken van meerdere pijplijnen voor meer databases.    

**Pijplijn 3 - TableCopy** gebruikt versienummers rij in SQL-Database (_rowversion_) voor het identificeren van rijen die zijn gewijzigd of bijgewerkt. Deze activiteit zoekt de begin- en de versie van de rij end om rijen uit de brontabellen. De **CopyTracker** tabel die zijn opgeslagen in elke tenantdatabase houdt de laatste rij opgehaald uit elke brontabel in elke uitvoering. Nieuwe of gewijzigde rijen worden gekopieerd naar de bijbehorende tijdelijke tabellen in het datawarehouse: **raw_Tickets**, **raw_Customers**, **raw_Venues**, en **raw_ Gebeurtenissen**. Ten slotte de laatste versie van de rij wordt opgeslagen in de **CopyTracker** tabel moet worden gebruikt als de versie van de eerste rij voor de volgende extractie. 

Er zijn ook drie parameters gekoppelde services die koppeling van de data factory met de bron-SQL-Databases, het beoogde SQL Data Warehouse en de tussenliggende Blob-opslag. In de **auteur** tabblad, klikt u op **verbindingen** verkennen van de gekoppelde services, zoals wordt weergegeven in de volgende afbeelding:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Overeenkomt met de drie gekoppelde services, zijn er drie gegevenssets die verwijzen naar gegevens die u in de pipeline-activiteiten als invoer of uitvoer gebruikt. Verkennen elk van de gegevenssets kunnen observeren verbindingen en parameters die worden gebruikt. _AzureBlob_ verwijst naar het configuratiebestand met bron- en -tabellen en kolommen, evenals de beheer-kolom in elke bron.
  
### <a name="data-warehouse-pattern-overview"></a>Overzicht van datawarehouse-patroon
SQL Data Warehouse wordt gebruikt als de analytics-store om uit te voeren van aggregatie op de gegevens van de tenant. In dit voorbeeld wordt de PolyBase om gegevens te laden in de SQL Data warehouse gebruikt. Onbewerkte gegevens wordt in faseringstabellen waarvoor een identiteitskolom voor het bijhouden van rijen die in de tabellen ster-schema zijn getransformeerd geladen. De volgende afbeelding ziet u het patroon laden: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Langzaam worden wijzigen dimensie (SCD) type 1 dimensietabellen gebruikt in dit voorbeeld. Elke dimensie is een surrogaatsleutel gedefinieerd met behulp van een identiteitskolom. Als een best practice is de tabel van de dimensie datum vooraf gevulde om tijd te besparen. Voor de andere dimensietabellen tabel een maken als selecteren... (CTAS)-instructie wordt gebruikt voor het maken van een tijdelijke tabel met de bestaande gewijzigd en niet gewijzigd rijen, samen met de vervangend sleutels. Dit wordt gedaan met IDENTITY_INSERT = ON. Nieuwe rijen worden vervolgens ingevoegd in de tabel met IDENTITY_INSERT = OFF. Voor eenvoudige terugdraaien, de naam van de bestaande dimensietabel is gewijzigd en de tijdelijke tabel is gewijzigd om te worden van de nieuwe dimensietabel. Voordat elke uitvoering, is de oude dimensietabel verwijderd.

Dimensietabellen worden geladen voordat de feitentabel. Deze volgorde zorgt ervoor dat voor elke binnenkomende feit, alle dimensies waarnaar wordt verwezen al bestaan. Als de gegevens worden geladen, wordt voldaan aan de zakelijke-sleutel voor elke bijbehorende dimensie en de bijbehorende vervangend sleutels worden toegevoegd aan elk feit.

De laatste stap van de transformatie Hiermee verwijdert u de tijdelijke gegevens gereed voor de volgende uitvoering van de pijplijn.
   
### <a name="trigger-the-pipeline-run"></a>De pijplijnuitvoering activeren
Volg onderstaande stappen voor het uitvoeren van de volledige extraheren, laden en transformeren pijplijn voor alle tenantdatabases:
1. In de **auteur** tabblad van de ADF-gebruikersinterface, selecteer **SQLDBToDW** pijplijn in het linkerdeelvenster.
1. Klik op **Trigger** en van de opgehaalde menu klikt u op **nu activeren**. Deze actie wordt de pijplijn wordt onmiddellijk uitgevoerd. U zou een tijdschema voor het uitvoeren van de pijplijn voor het vernieuwen van de gegevens volgens een schema definiëren in een productiescenario voor.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. Op **Pijplijnuitvoering** pagina, klikt u op **voltooien**.
 
### <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.
1. In de gebruikersinterface van ADF overschakelen naar de **Monitor** tabblad in het menu aan de linkerkant.
1. Klik op **vernieuwen** totdat SQLDBToDW pijplijnstatus **geslaagd**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. Verbinding maken met het datawarehouse met SMMS en query uitvoeren op de ster-schema-tabellen om te controleren dat de gegevens in deze tabellen zijn geladen.

Als de pijplijn is voltooid, de feitentabel bevat verkoopgegevens voor alle theaters ticket en de dimensietabellen worden ingevuld met de bijbehorende venues, gebeurtenissen en klanten.

## <a name="data-exploration"></a>Gegevens verkennen

### <a name="visualize-tenant-data"></a>Tenantgegevens visualiseren

De gegevens in de ster-schema biedt alle de ticket verkoopgegevens die nodig zijn voor uw analyse. Grafisch weergegeven gegevens te visualiseren, maakt het gemakkelijker om te zien van trends in grote gegevenssets. In deze sectie maakt u **Power BI** om te bewerken en visualiseren van de gegevens van de tenant in het datawarehouse.

Gebruik de volgende stappen uit om te verbinden met Power BI, en voor het importeren van de weergaven die u eerder hebt gemaakt:

1. Start Power BI desktop.
2. Selecteer in het lint Start **gegevens ophalen**, en selecteer **meer...** in het menu.
3. In de **gegevens ophalen** venster **Azure SQL Database**.
4. Voer in het aanmeldingsvenster van de database, de servernaam (**catalogus-dpt -&lt;gebruiker&gt;. database.windows.net**). Selecteer **importeren** voor **Gegevensverbindingsmodus**, en klik vervolgens op **OK**. 

    ![Sign-in-naar-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Selecteer **Database** in het linkerdeelvenster, voert u de gebruikersnaam van de = *developer*, en Voer wachtwoord = *P@ssword1*. Klik op **Verbinden**.  

    ![database-aanmelding](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. In de **Navigator** deelvenster onder de analytics-database, selecteert u de tabellen ster-schema: **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** en **dim_Dates**. Selecteer vervolgens **Load**. 

Gefeliciteerd! U de gegevens zijn geladen in Power BI. Bekijk interessante visualisaties inzicht krijgen in uw tenants. Laten we eens kijken hoe analytics u enkele aanbevelingen gegevensgestuurde naar het managementteam Wingtip Tickets krijgt te worden gebruikt. De aanbevelingen kunnen helpen met het optimaliseren van het model en de klant ervaring.

Begin met een ticket verkoopgegevens om te zien van de variatie in gebruik voor de venues analyse. Selecteer de opties wordt weergegeven in Power BI om te tekenen van een staafdiagram van het totale aantal tickets die door elke venue verkocht. (Vanwege willekeurige variatie in de generator ticket, uw resultaten afwijken.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

De voorgaande diagram wordt bevestigd dat het aantal tickets die door elke venue verkocht varieert. Venues die meer tickets kunnen verkopen gebruikt uw service strenger dan venues die minder tickets kunnen verkopen. Mogelijk zijn er een kans om aan te passen van de toewijzing van resources op basis van behoeften van de andere tenant.

Verder kunt u de gegevens om te zien hoe kaartverkoop variëren na verloop van tijd analyseren. Selecteer de opties weergegeven in de volgende afbeelding in Power BI om te tekenen het totale aantal tickets die worden verkocht per dag gedurende een periode van 60 dagen.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

Het voorgaande diagram laat zien dat ticket verkoop piek voor sommige plaatsen. Deze pieken versterking van het idee dat sommige venues mogelijk worden verbruikt systeembronnen niet goed. Er is tot nu toe zonder duidelijk patroon in wanneer de pieken optreden.

Volgende we onderzoeken het belang van deze Piekdagen verkoop. Wanneer deze pieken optreden nadat kaartjes voor verkoop? Als u wilt tekenen tickets die worden verkocht per dag, selecteer de opties die worden weergegeven in de volgende afbeelding in Power BI.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

In dit diagram ziet u dat sommige venues grote aantallen tickets op de eerste dag van de verkoop verkopen. Zodra de kaartjes voor verkoop op deze venues, lijkt er een gekke spits. Deze burst van activiteit door een paar venues heeft mogelijk invloed op de service voor andere tenants.

U kunt inzoomen op de gegevens opnieuw om te zien of deze gekke beheergedrag geldt voor alle gebeurtenissen die door deze venues worden gehost. In vorige grafieken, hebt u gezien dat Contoso Concert Hall verkoopt veel tickets en die Contoso heeft ook een piek in kaartverkoop op bepaalde dagen. Experimenteren met Power BI-opties voor het tekenen van cumulatieve kaartverkoop voor Contoso Concert Hall, gericht op trends die grote verkoop voor elk van de bijbehorende gebeurtenissen. Volg alle gebeurtenissen hetzelfde patroon verkoop? Voor het produceren van een grafiek, zoals hieronder uitproberen.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

In dit diagram cumulatieve kaartverkoop na verloop van tijd voor Contoso Concert Hall voor elke gebeurtenis bevat de gekke beheergedrag gebeurt niet voor alle gebeurtenissen. Experimenteren met de filteropties verkoop trends voor andere evenementen verkennen.

Het inzicht te krijgen in patronen verkopen ticket kunnen Wingtip Tickets te optimaliseren van hun bedrijfsmodel leiden. In plaats van alle tenants even factureren, misschien introduceert Wingtip Servicelagen met verschillende grootten. Grotere venues die nodig hebt om te verkopen meer tickets per dag kunnen worden aangeboden als een hogere laag met een hogere service level agreement (SLA). Deze evenementen hebt kunnen hun databases in de groep met hogere limieten voor per database resource geplaatst. Elke servicelaag hebt kan een toewijzing van verkoop per uur, met extra kosten in rekening gebracht voor het overschrijden van de toewijzing. Grotere venues waarvoor periodieke pieken van verkoop veel voordeel hebben van de hogere lagen en Wingtip Tickets efficiënter inkomsten kunt genereren van hun service.

Ondertussen klagen sommige Wingtip Tickets-klanten dat ze het moeilijk te verkopen voldoende tickets te rechtvaardigen, de servicekosten. In deze inzichten er het is wellicht een kans om kaartverkoop voor attenderen venues te verbeteren. Hogere omzet zou verhoogt de waargenomen waarde van de service. Klik met de rechtermuisknop op fact_Tickets en selecteer **nieuwe meting**. Voer de volgende expressie voor de nieuwe meting met de naam **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Selecteer de volgende Visualisatieopties voor het tekenen van de percentage tickets die door elke venue om te bepalen van hun relatieve succes verkocht.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

Het bovenstaande diagram ziet u dat hoewel de meeste venues meer dan 80% van de tickets verkopen, in sommige problemen ondervinden in te vullen van meer dan de helft van hun seats. Experimenteren met de waarden en maximum of minimum percentage van tickets die worden verkocht voor elke locatie selecteren.

## <a name="embedding-analytics-in-your-apps"></a>Het insluiten van analyses in uw apps 
In deze zelfstudie is gericht op cross-tenant-analytics gebruikt voor het verbeteren van de leverancier van de software begrip van hun tenants. Analytics ook inzicht om te kunnen geven de _tenants_, zodat ze effectief beheren van hun bedrijf kunnen zelf. 

In het voorbeeld Wingtip Tickets u eerder hebt gevonden dat kaartverkoop meestal voorspelbare patronen volgen. Dit inzicht kan worden gebruikt om te attenderen venues boost kaartverkoop. Er is mogelijk een kans om te worden machine learning-technieken om te voorspellen kaartverkoop voor gebeurtenissen. De gevolgen van prijs kunnen ook worden gemodelleerd, zodat de impact van aanbieding kortingen worden voorspeld. Power BI Embedded kan worden geïntegreerd in een management-toepassing voor gebeurtenis voor het visualiseren van de voorspelde verkoop, met inbegrip van de impact van kortingen op de totale seats die worden verkocht en omzet van gebeurtenissen met lage verkopen. U kunt zelfs integreren daadwerkelijk de korting toegepast op de ticket-prijzen, rechtstreeks in de visualisatie-ervaring met Power BI Embedded.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een SQL Data Warehouse is gevuld met een stervormig schema voor tenant-analytics implementeren.
> * Gebruik Azure Data Factory om gegevens ophalen uit elke tenantdatabase in het datawarehouse analytics.
> * Optimaliseer de opgehaalde gegevens (reorganize in een ster-schema).
> * Query uitvoeren op de analytics-datawarehouse. 
> * Power BI gebruiken voor het visualiseren van trends in de gegevens voor alle tenants.

Gefeliciteerd!

## <a name="additional-resources"></a>Aanvullende resources

- Aanvullende [zelfstudies voort op de Wingtip SaaS-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
