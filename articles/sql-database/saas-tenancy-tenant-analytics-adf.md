---
title: Analytics query's uitvoeren op de tenant-databases via de Azure SQL Data Warehouse | Microsoft Docs
description: Cross-tenant analytics query's met behulp van gegevens uit meerdere databases van Azure SQL Database hebt uitgepakt.
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
ms.topic: article
ms.date: 11/08/2017
ms.author: anjangsh
ms.openlocfilehash: dd51480f30187456104a0f9d72ec17082dc1e39e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>SaaS-analytics met Azure SQL Database, SQL Data Warehouse, Data Factory en Power BI verkennen

In deze zelfstudie maakt doorlopen u een end-to-end analytics scenario. Het scenario laat zien hoe analytics over gegevens van de tenant softwareleveranciers slimme beslissingen kunt maken. Gegevens die zijn geëxtraheerd uit de database van elke tenant gebruikt, kunt u analytics inzicht in het gedrag van de tenant, met inbegrip van hun gebruik van de voorbeeldtoepassing Wingtip Tickets SaaS. Dit scenario omvat drie stappen: 

1.  **Gegevens extraheren** vanaf elke tenant van de database naar een archief analytics, in dit geval een SQL Data Warehouse.
2.  **De opgehaalde gegevens optimaliseren** voor analytics verwerking.
3.  Gebruik **Business Intelligence** hulpprogramma's om handig inzicht die besluitvorming kunnen leiden. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> - Maak de tenant analytics opslaan voor het laden.
> - Azure Data Factory (ADF) gebruiken om gegevens te extraheren uit de database van elke tenant in het datawarehouse analytics.
> - De opgehaalde gegevens (reorganize in een ster-schema) optimaliseren.
> - Query uitvoeren op het datawarehouse analytics.
> - Gebruik Power BI voor gegevensvisualisatie te markeren trends in gegevens van de tenant en aanbeveling voor verbeteringen te doen.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Analytics over uitgepakte tenant gegevens

SaaS-toepassingen bevatten mogelijk grote hoeveelheden gegevens van de tenant in de cloud. Deze gegevens kan gebruikers een rijke bron van informatie over de werking en gebruik van uw toepassing en het gedrag van uw tenants. Deze inzichten kunnen leiden functie ontwikkeling, bruikbaarheid verbeteringen en andere investeringen in de apps en -platform.

Toegang tot de gegevens voor alle tenants is eenvoudig wanneer alle gegevens in een multitenant-database. Maar access complexere wanneer over duizenden databases op grote schaal gedistribueerd. Er is een manier om de complexiteit pak uitpakken van de gegevens naar een analytics-database of een datawarehouse voor query.

Deze zelfstudie toont een scenario analytics end-to-end voor de toepassing Wingtip Tickets. Eerst [Azure Data Factory (ADF)](../data-factory/introduction.md) tickets verkoop- en gerelateerde gegevens te extraheren uit de database van elke tenant wordt gebruikt als de orchestration-hulpprogramma. Deze gegevens wordt in faseringstabellen worden weergegeven in een archief analytics geladen. De store analytics kan zijn van een SQL-Database of een SQL Data Warehouse. Deze zelfstudie wordt gebruikgemaakt van [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) opslaan als de analytics.

Vervolgens de geëxtraheerde gegevens worden getransformeerd en geladen in een set [ster-schema](https://www.wikipedia.org/wiki/Star_schema) tabellen. De tabellen bestaan uit een centrale feitentabel plus gerelateerde dimensietabellen:

- De centrale feitentabel in de ster-schema bevat ticket gegevens.
- De dimensietabellen bevatten gegevens over plaatsen, gebeurtenissen, klanten en datums kopen.

Samen het midden- en dimensie tabellen inschakelen efficiënt analytische verwerking. De ster-schema dat wordt gebruikt in deze zelfstudie wordt weergegeven in de volgende afbeelding:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Ten slotte worden de ster-schema-tabellen opgevraagd. Queryresultaten worden weergegeven met Power BI visueel te markeren inzicht in het gedrag van de tenant en het gebruik van de toepassing. Met dit ster-schema, u query's uitvoeren die beschikbaar:

- Wie is aanschaf van tickets en vanaf welke u wilt.
- Patronen en trends in de verkoop van tickets.
- De relatieve populariteit van elke locatie vast.

Deze zelfstudie bevat eenvoudige voorbeelden van inzichten die van de gegevens Wingtip Tickets kunnen worden achterhaald. Begrijpen hoe elke locatie vast gebruikmaakt van de service kan ertoe leiden dat de leverancier Wingtip Tickets bij het nadenken over de verschillende serviceplannen bijvoorbeeld gericht op meer of minder actieve plaatsen. 

## <a name="setup"></a>Instellen

### <a name="prerequisites"></a>Vereisten

> [!NOTE]
> Deze zelfstudie maakt gebruik van functies van de Azure Data Factory die zich momenteel in een beperkte preview (parameterization gekoppelde service). Als u wenst te doen in deze zelfstudie, geeft u uw abonnements-ID [hier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). We sturen u een bevestiging zodra uw abonnement is ingeschakeld.

Voor het voltooien van deze zelfstudie moet u ervoor zorgen dat aan de volgende vereisten is voldaan:
- De toepassing Wingtip Tickets SaaS Database Per Tenant is geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md).
- De scripts Wingtip Tickets SaaS Database Per Tenant en de toepassing [broncode](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) worden gedownload van GitHub. Zie voor downloadinstructies ontvangen. Zorg ervoor dat *blokkering van het zip-bestand* voordat het uitpakken van de inhoud ervan.
- Power BI Desktop is geïnstalleerd. [Download Power BI Desktop](https://powerbi.microsoft.com/downloads/).
- De batch van extra tentants is ingericht, raadpleegt u de [ **inrichten tenants zelfstudie**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Gegevens voor de demo maken

Deze zelfstudie behandelt analytics via verkoopgegevens ticket. In deze stap maakt u ticket gegevens moeten genereren voor de tenants. In een latere stap, wordt deze gegevens worden opgehaald voor analyse. _Zorg ervoor dat u de batch van tenants ingericht_ (zoals eerder is beschreven) zodat u beschikt over onvoldoende gegevens beschikbaar stellen tal van verschillende koopgedrag ticket.

1. Open in PowerShell ISE *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo TenantAnalyticsDW.ps1*, en stel de volgende waarde:
    - **$DemoScenario** = **1** kopen van tickets voor gebeurtenissen op alle plaatsen
2. Druk op **F5** uitvoeren van het script en ticket geschiedenis voor het plaatsen aanschaffen maken. Met 20-tenants, het script genereert tienduizenden tickets en kan 10 minuten of langer duren.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>SQL datawarehouse, Data Factory, implementeren en Blob Storage 
In de app Wingtip Tickets transactionele gegevens van de tenants verdeeld over veel databases. Azure Data Factory (ADF) wordt gebruikt voor het indelen van de extraheren, laden en transformeren (ELT) van deze gegevens naar het datawarehouse. Om gegevens te laden in SQL Data Warehouse zo efficiënt mogelijk, ADF haalt u gegevens in bestanden tussenliggende blob en gebruikt vervolgens [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) de gegevens te laden in het datawarehouse.   

In deze stap implementeert u de aanvullende bronnen gebruikt in de zelfstudie: een SQL Data Warehouse aangeroepen _tenantanalytics_, een Azure Data Factory aangeroepen _dbtodwload -\<gebruiker\>_  , en een Azure storage-account met de naam _wingtipstaging\<gebruiker\>_. Het opslagaccount wordt gebruikt om tijdelijk bestanden geëxtraheerde gegevens als blobs voordat ze geladen in het datawarehouse worden. Deze stap implementeert de datawarehouse-schema ook en definieert de ADF-pijplijnen die het proces ELT indelen.
1. Open in PowerShell ISE *...\Learning Modules\Operational Analytics\Tenant Analytics DW\Demo TenantAnalyticsDW.ps1* en instellen:
    - **$DemoScenario** = **2** tenant analytics-datawarehouse, blob-opslag en gegevensfactory implementeren 
1. Druk op **F5** de demoscript uitvoeren en implementeren van de Azure-resources. 

Controleer nu de Azure-resources die u geïmplementeerd:
#### <a name="tenant-databases-and-analytics-store"></a>Tenant-databases en analyses store
Gebruik [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) verbinding maken met **tenants1-dpt -&lt;gebruiker&gt;**  en **catalogus-dpt -&lt;gebruiker&gt;**  servers. Vervang &lt;gebruiker&gt; met de waarde die wordt gebruikt wanneer u de app hebt geïmplementeerd. Gebruik aanmelding = *developer* en het wachtwoord = *P@ssword1*. Zie de [inleidende zelfstudie](saas-dbpertenant-wingtip-app-overview.md) voor meer informatie.

![Verbinding maken met SQL Database-server van SSMS](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

In Object Explorer:

1. Vouw de *tenants1-dpt -&lt;gebruiker&gt;*  server.
1. Vouw het knooppunt Databases uit en Zie de lijst met tenant-databases.
1. Vouw de *catalogus-dpt -&lt;gebruiker&gt;*  server.
1. Controleren of u zien dat de analytics opslaan met de volgende objecten:
    1. Tabellen **raw_Tickets**, **raw_Customers**, **raw_Events** en **raw_Venues** onbewerkte geëxtraheerde gegevens uit de tenant-databases.
    1. De ster-schema-tabellen zijn **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, en **dim_Dates** .
    1. De opgeslagen procedure **sp_transformExtractedData** wordt gebruikt voor de gegevens transformeren en laden in de tabellen ster-schema.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Blob Storage
1. In de [Azure Portal](https://ms.portal.azure.com), navigeer naar de resourcegroep die u hebt gebruikt voor het implementeren van de toepassing. Controleer of een opslagaccount **wingtipstaging\<gebruiker\>**  is toegevoegd.

  ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Klik op **wingtipstaging\<gebruiker\>**  storage-account om te verkennen van de aanwezige objecten.
1. Klik op **Blobs** tegel
1. Klik op de container **configfile**
1. Controleer **configfile** bevat een JSON-bestand aangeroepen **TableConfig.json**. Dit bestand bevat de bron- en doelserver tabelnamen, kolomnamen en tracering kolomnaam.

#### <a name="azure-data-factory-adf"></a>Azure Data Factory (ADF)
In de [Azure Portal](https://ms.portal.azure.com) in de resourcegroep, Controleer of een Azure Data Factory _dbtodwload -\<gebruiker\>_  is toegevoegd. 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

Deze sectie behandelt de gegevensfactory gemaakt. Volg onderstaande stappen voor het starten van de gegevensfactory:
1. Klik op de gegevensfactory aangeroepen in de portal **dbtodwload -\<gebruiker\>**.
2. Klik op **auteur & Monitor** tegel starten van de ontwerpfunctie voor Data Factory's in een afzonderlijke tabblad. 

## <a name="extract-load-and-transform-data"></a>Extraheren, laden, en gegevens transformeren
Azure Data Factory wordt gebruikt voor het organiseren van extractie, laden en transformatie van gegevens. In deze zelfstudie maakt u gegevens ophalen uit vier verschillende SQL-weergaven van elk van de tenant-databases: **rawTickets**, **rawCustomers**, **rawEvents**, en  **rawVenues**. Deze weergaven bevatten wetten-Id, zodat u gegevens vanaf elke locatie vast in het datawarehouse kan onderscheid. De gegevens in het bijbehorende faseringstabellen worden weergegeven in het datawarehouse wordt geladen: **raw_Tickets**, **raw_customers**, **raw_Events** en **raw_Venue**. Een opgeslagen procedure vervolgens transformeert de onbewerkte gegevens en vult u de tabellen ster-schema: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** , en **dim_Dates**.

In de vorige sectie, geïmplementeerd en de benodigde Azure-resources, met inbegrip van de gegevensfactory wordt geïnitialiseerd. De geïmplementeerde gegevensfactory bevat de pijplijnen, gegevenssets, gekoppelde services, enz., extraheren, laden en de tenant-gegevens transformeren. We gaan verkennen deze objecten verder en trigger de pijplijn om gegevens te verplaatsen van databases tenant naar het datawarehouse.

### <a name="data-factory-pipeline-overview"></a>Data factory-pijplijn overzicht
Deze sectie behandelt de objecten die zijn gemaakt in de gegevensfactory. De volgende afbeelding wordt de algemene werkstroom beschreven van de ADF-pijplijn in deze zelfstudie gebruikt. Als u liever de pijplijn later verkennen en eerst de resultaten te zien, gaat u naar de volgende sectie **activeren van de pijplijn uitvoeren**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Op de overzichtspagina overschakelen naar **auteur** tabblad in het linkerdeelvenster en zien of er drie zijn [pijplijnen](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) en drie [gegevenssets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) gemaakt.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

De drie geneste pijplijnen zijn: SQLDBToDW DBCopy en TableCopy.

**Pijplijn 1 - SQLDBToDW** Hiermee zoekt u de namen van de tenant databases die zijn opgeslagen in de catalogus-database (de tabelnaam van de: [__ShardManagement]. [ ShardsGlobal]) en voor elke tenant-database, voert u de **DBCopy** pijplijn. Na voltooiing, het opgegeven **sp_TransformExtractedData** opgeslagen procedure-schema wordt uitgevoerd. Deze opgeslagen procedure transformeert de geladen gegevens in de faseringstabellen en vult de tabellen ster-schema.

**Pijplijn 2 - DBCopy** Hiermee zoekt u de namen van de brontabellen en kolommen uit een configuratiebestand opgeslagen in blob storage.  De **TableCopy** pijplijn wordt vervolgens uitgevoerd voor elk van de vier tabellen: TicketFacts, CustomerFacts EventFacts en VenueFacts. De **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** activiteit wordt uitgevoerd voor alle databases voor 20 parallel. ADF kunt maximaal 20 lus iteraties parallel worden uitgevoerd. Overweeg te maken van meerdere pijplijnen voor meer databases.    

**Pijplijn 3 - TableCopy** gebruikt rij versienummers in SQL-Database (_rowversion_) voor het identificeren van rijen die zijn gewijzigd of bijgewerkt. Deze activiteit zoekt de begin- en de versie van de rij einde voor het uitpakken van rijen uit de brontabellen. De **CopyTracker** tabel uit de database van elke tenant houdt de laatste rij is opgehaald uit elke brontabel in elke uitvoering. Nieuwe of gewijzigde rijen worden gekopieerd naar de bijbehorende faseringstabellen worden weergegeven in het datawarehouse: **raw_Tickets**, **raw_Customers**, **raw_Venues**, en **raw_ Gebeurtenissen**. Ten slotte de laatste rijversie wordt opgeslagen in de **CopyTracker** tabel moet worden gebruikt als de eerste rijversie voor de volgende extractie. 

Er zijn ook drie parameters gekoppelde services de gegevensfactory met de SQL-Databases van de bron van die koppeling, het doel-SQL Data Warehouse en de tussenliggende Blob-opslag. In de **auteur** tabblad, klikt u op **verbindingen** om te verkennen van de gekoppelde services, zoals wordt weergegeven in de volgende afbeelding:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Overeenkomt met de drie gekoppelde services, wordt er drie gegevenssets die verwijzen naar gegevens die u in de pipeline-activiteiten als invoer of uitvoer gebruikt zijn. Bekijk elk van de gegevenssets in acht nemen verbindingen en parameters die worden gebruikt. _AzureBlob_ verwijst naar het configuratiebestand met de bron en doel-tabellen en kolommen, evenals de tracering kolom in elke bron.
  
### <a name="data-warehouse-pattern-overview"></a>Overzicht van datawarehouse-patroon
SQL Data Warehouse wordt gebruikt als het archief analytics aggregatie uitvoeren op de tenant-gegevens. In dit voorbeeld wordt PolyBase gebruikt om gegevens te laden in de SQL Data warehouse. Onbewerkte gegevens wordt in faseringstabellen waarvoor een identiteitskolom voor het bijhouden van rijen die in de ster-schema-tabellen zijn getransformeerd geladen. De volgende afbeelding toont het patroon laden: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Langzaam worden dimensie wijzigen (SCD) type 1 dimensietabellen gebruikt in dit voorbeeld. Elke dimensie heeft een vervangende sleutel gedefinieerd met behulp van een identiteitskolom. Als een best practice is de datum-dimensietabel vooraf ingestelde tijd te besparen. Voor de andere dimensietabellen tabel een maken als selecteren... Instructie (CTAS) wordt gebruikt voor het maken van een tijdelijke tabel met de bestaande gewijzigd en niet gewijzigd rijen, samen met de vervangende sleutels. Dit wordt gedaan met IDENTITY_INSERT = ON. Nieuwe rijen vervolgens worden ingevoegd in de tabel met IDENTITY_INSERT = OFF. Voor eenvoudig terugdraaien, de naam van de bestaande dimensietabel is gewijzigd en de naam van de tijdelijke tabel wordt gewijzigd om te worden van de nieuwe dimensietabel. Voordat elke uitvoering wordt de oude dimensietabel verwijderd.

Dimensietabellen worden geladen voordat de feitentabel. Deze volgorde zorgt ervoor dat voor elke binnenkomende fact alle dimensies waarnaar wordt verwezen al bestaat. Als de gegevens zijn geladen, de business-sleutel voor elke overeenkomende dimensie is gekoppeld en de bijbehorende vervangende sleutels worden toegevoegd aan elk feit.

De laatste stap van de transformatie gereed is voor de volgende uitvoering van de pijplijn staging gegevens worden verwijderd.
   
### <a name="trigger-the-pipeline-run"></a>Activeren van de pijplijn uitvoeren
Volg onderstaande stappen voor het uitvoeren van de volledige extraheren, laden en transformatie pijplijn voor alle databases in de tenant:
1. In de **auteur** tabblad van de ADF-gebruikersinterface, selecteer **SQLDBToDW** pijplijn in het linkerdeelvenster.
1. Klik op **Trigger** en van de opgehaalde menu klikt u op **Trigger nu**. Deze actie wordt de pijplijn onmiddellijk uitgevoerd. In een productiescenario zou u een tijdschema voor het uitvoeren van de pijplijn voor het vernieuwen van de gegevens op een planning definiëren.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. Op **pijplijn uitvoeren** pagina, klikt u op **voltooien**.
 
### <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.
1. In de gebruikersinterface van de ADF overschakelen naar de **Monitor** tabblad in het menu aan de linkerkant.
1. Klik op **vernieuwen** totdat de status van de pijplijn SQLDBToDW **geslaagd**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. Verbinding maken met het datawarehouse met SSMS en querytabellen ster-schema om te controleren dat de gegevens in deze tabellen zijn geladen.

Zodra de pijplijn is voltooid, de feitentabel bevat ticket verkoopgegevens voor alle plaatsen en de dimensietabellen worden ingevuld met de bijbehorende plaatsen, gebeurtenissen en klanten.

## <a name="data-exploration"></a>Gegevensverkenning

### <a name="visualize-tenant-data"></a>Tenant-gegevens visualiseren

De gegevens in de ster-schema biedt alle ticket verkoop gegevens die nodig zijn voor uw analyse. Grafisch weergegeven gegevens te visualiseren, kunt gemakkelijker trends te bekijken in grote gegevenssets. In deze sectie gebruikt u **Power BI** worden bewerkt en visualiseren van de tenant-gegevens in het datawarehouse.

Verbinding maken met Power BI, en voor het importeren van de weergaven die u eerder hebt gemaakt, gebruikt u de volgende stappen uit:

1. Starten van Power BI desktop.
2. Selecteer in het lint Home **gegevens ophalen**, en selecteer **meer...** in het menu.
3. In de **gegevens ophalen** Selecteer **Azure SQL Database**.
4. Voer de servernaam van uw in het venster van de aanmelding database (**catalogus-dpt -&lt;gebruiker&gt;. database.windows.net**). Selecteer **importeren** voor **gegevens Connectiviteitsmodus**, en klik vervolgens op **OK**. 

    ![Sign-in-naar-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Selecteer **Database** Voer gebruikersnaam in het linkerdeelvenster = *developer*, en Voer wachtwoord = *P@ssword1*. Klik op **Verbinden**.  

    ![database-aanmeldingspagina](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. In de **Navigator** deelvenster onder de analytics-database, selecteer de tabellen ster-schema: **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** en **dim_Dates**. Selecteer vervolgens **Load**. 

Gefeliciteerd! U heeft de gegevens in Power BI geladen. Nu verkennen interessante visualisaties om inzicht in uw tenants. Laten we zien hoe analytics enkele aanbevelingen gegevensgestuurde naar het managementteam Wingtip Tickets kunt bieden. De aanbevelingen kunnen u het model en de klant ervaring te optimaliseren.

Eerst analyseren verkoopgegevens om te zien van de variatie in gebruik voor het plaatsen ticket. Selecteer de opties worden weergegeven in Power BI om te tekenen van een staafdiagram van het totale aantal tickets verkocht door elke locatie vast. (Als gevolg van willekeurige variaties in de generator ticket uw resultaten kunnen afwijken.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

De voorgaande grafiek bevestigt dat het aantal tickets verkocht door elke locatie vast varieert. Plaatsen die meer tickets verkopen gebruikt uw service dan plaatsen die minder tickets verkopen. Mogelijk zijn er een kans om aan te passen resourcetoewijzing op basis van andere tenant behoeften.

Verder kunt u de gegevens om te zien hoe de verkoop ticket variëren na verloop van tijd analyseren. Selecteer de opties worden weergegeven in de volgende afbeelding in Power BI om te tekenen van het totale aantal tickets verkocht per dag voor een periode van 60 dagen.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

De voorgaande diagram toont dat ticket verkoop piek voor sommige plaatsen. Deze pieken versterking van het idee dat sommige plaatsen van de systeembronnen niet goed maken mogelijk. Er is tot nu toe zonder duidelijk patroon in wanneer de pieken optreden.

Volgende we het belang van deze Verkoop Piekdagen onderzoeken. Wanneer deze pieken optreden nadat tickets verkoop gaat? Selecteer de opties worden weergegeven in de volgende afbeelding in Power BI om tickets per dag is verkocht getekend.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

Deze grafiek toont dat sommige plaatsen grote aantallen tickets op de eerste dag van de verkoop verkopen. Als u de tickets gaat verkoop op deze plaatsen, lijkt er een gekke snel uitgevoerd. Deze burst van activiteit door enkele plaatsen mogelijk invloed op de service voor andere tenants.

U kunt inzoomen op de gegevens opnieuw te controleren of deze gekke snel uitgevoerd geldt voor alle gebeurtenissen die worden gehost door deze plaatsen. U hebt gezien in de vorige waarnemingspunten dat Contoso overleg Hall veel tickets wordt verkocht, en dat Contoso heeft ook een piek in ticket verkoop op bepaalde dagen. Uitproberen met Power BI-opties voor het uitzetten van cumulatieve ticket sales voor Contoso overleg Hall, te focussen op verkoop trends voor elk van de bijbehorende gebeurtenissen. Voer alle gebeurtenissen hetzelfde patroon verkoop? Probeer te produceren tekent zoals hieronder.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

Deze grafiek van cumulatieve ticket verkoop na verloop van tijd voor Contoso overleg Hall voor elke gebeurtenis toont het gekke enthousiasme gebeurt niet voor alle gebeurtenissen. De filteropties om te verkennen verkoop trends voor andere evenementen uitproberen.

De inzichten in ticket verkopen patronen kunnen leiden Wingtip kaartjes voor hun bedrijfsmodel optimaliseren. In plaats van alle huurders evenveel laden, mogelijk introduceert Wingtip Servicelagen met verschillende prestatieniveaus. Grotere plaatsen die nodig zijn om te verkopen meer tickets per dag kunnen worden aangeboden als een hogere laag met een hogere service level agreement (SLA). De plaatsen van uitvoering kunnen hebben hun databases in de groep met hogere limieten per database geplaatst. Elke servicelaag kan een toewijzing voor de per uur verkoop hebben met extra kosten in rekening gebracht voor het overschrijden van de toewijzing. Grotere plaatsen waarvoor periodieke lichtflitsen verkoop zouden kunnen profiteren van de hogere lagen en Wingtip Tickets kunt Verdien er geld mee hun service efficiënter.

Ondertussen klagen sommige klanten Wingtip Tickets dat ze moeilijk te verkopen voldoende tickets te rechtvaardigen de servicekosten. In deze inzichten is er wellicht een kans om de ticket verkoop voor attenderen plaatsen. Hogere verkoop zou een hogere waargenomen waarde van de service. Klik met de rechtermuisknop op fact_Tickets en selecteer **nieuwe meting**. Voer de volgende expressie voor de nieuwe meting aangeroepen **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Selecteer de volgende Visualisatieopties voor het uitzetten van de percentage tickets die door elke locatie vast om te bepalen van hun relatieve succes verkocht.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

De bovenstaande grafiek ziet u dat hoewel de meeste plaatsen meer dan 80% van hun ticket verkopen, sommige zijn moesten voor het vervullen van meer dan de helft van hun plaatsen. Uitproberen met de waarden goed maximum of minimum percentage van tickets voor elke locatie vast verkocht selecteren.

## <a name="embedding-analytics-in-your-apps"></a>Analytics insluiten in uw apps 
Deze zelfstudie is gericht op cross-tenant analytics gebruikt voor het verbeteren van de softwareleverancier kennis van hun tenants. Analytics biedt ook inzichten voor de _tenants_, zodat ze effectiever beheren van hun bedrijf kunnen zelf. 

In het voorbeeld Wingtip Tickets u eerder gedetecteerd dat ticket verkoop hebben vaak voorspelbare patronen te volgen. Deze inzicht kan worden gebruikt om te attenderen plaatsen versterking ticket verkoop. Er is mogelijk een mogelijkheid voor het gebruiken van machine learning-technieken te voorspellen ticket sales voor gebeurtenissen. De gevolgen van prijs kunnen ook worden gemodelleerd, zodat de gevolgen van het aanbieden van kortingen worden voorspeld. Power BI Embedded kan worden geïntegreerd in een management-toepassing voor gebeurtenis voor het visualiseren van voorspelde verkoop, met inbegrip van de impact van kortingen op totale seats verkocht en omzet op laag verkopen gebeurtenissen. Met Power BI Embedded, kunt u zelfs integreren daadwerkelijk de korting toepassen op de prijzen ticket, direct aan de visualisatie-ervaring.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Implementeer een SQL Data Warehouse gevuld met een sterschema voor tenant-analyses.
> * Azure Data Factory om gegevens te extraheren uit de database van elke tenant in het datawarehouse analytics gebruiken.
> * De opgehaalde gegevens (reorganize in een ster-schema) optimaliseren.
> * Query uitvoeren op het datawarehouse analytics. 
> * Power BI gebruiken voor het visualiseren van trends in gegevens over de tenants.

Gefeliciteerd!

## <a name="additional-resources"></a>Aanvullende resources

- Aanvullende [zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
