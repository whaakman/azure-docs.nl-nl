---
title: Bewaking van prestaties van een shard multitenant Azure SQL database in een multitenant SaaS-app | Microsoft Docs
description: Bewaken en beheren van de prestaties van shard multitenant Azure SQL database in een multitenant SaaS-app
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: 3e97f0635a856256dd08c29d33d8058be9c8d8b4
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-manage-performance-of-sharded-multi-tenant-azure-sql-database-in-a-multi-tenant-saas-app"></a>Bewaken en beheren van de prestaties van shard multitenant Azure SQL database in een multitenant SaaS-app

In deze zelfstudie worden de verschillende prestatie scenario's voor beheer in de SaaS-toepassingen gebruikt verkend. Met een load-generator te simuleren van activiteit in de shard multitenant-databases, worden de ingebouwde bewaking en waarschuwingen van de functies van SQL Database gedemonstreerd.

De app Wingtip Tickets SaaS multitenant Database maakt gebruik van een gedeelde multitenant-gegevens, waar u wilt (tenant) gegevens door tenant-ID is verdeeld over mogelijk meerdere databases. Net als bij veel andere SaaS-toepassingen, is het verwachte tenantworkloadpatroon onvoorspelbaar en sporadisch. Met andere woorden, kaartverkoop kan op ieder moment plaatsvinden. Als u wilt profiteren van dit patroon voor het gebruik van typische database, kunnen de databases omhoog en omlaag worden uitgebreid voor het optimaliseren van de kosten van een oplossing. Met dit type patroon is het belangrijk dat het Resourcegebruik van de database om ervoor te zorgen dat belasting redelijkerwijs verdeeld zijn over mogelijk meerdere databases bewaken. Ook moet u ervoor zorgen dat afzonderlijke databases voldoende bronnen en zijn niet kunt u door hun [DTU](sql-database-what-is-a-dtu.md) limieten. Deze zelfstudie behandelt manieren om te controleren en beheren van databases en hoe u corrigerende maatregelen nemen in reactie op de verschillen in de werkbelasting.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Informatie over het gebruik van een shard multitenant-database door het uitvoeren van een opgegeven load generator simuleren
> * De database bewaken tijdens het reageert op de toename van load
> * De database in reactie op de belasting van de hogere database opschalen
> * Een tenant in te richten in een één-tenant-database

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS multitenant Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-saas-performance-management-patterns"></a>Inleiding tot SaaS prestaties management patronen

Het beheren van de databaseprestaties bestaat uit het verzamelen en analyseren van prestatiegegevens en het reageren op deze gegevens door parameters aan te passen om een acceptabele reactietijd voor de toepassing te behouden. 

### <a name="performance-management-strategies"></a>Strategieën voor prestatiebeheer

* Om te voorkomen dat handmatig prestaties bewaken, is het meest effectief te **waarschuwingen die wordt geactiveerd wanneer databases buiten het normale adresbereiken afwijken instellen**.
* Om te reageren op korte termijn schommelingen in het prestatieniveau van een database, de **DTU niveau kan worden geschaald omhoog of omlaag**. Als deze fluctuatie op een basis reguliere of voorspelbare optreedt **schalen van de database kan worden gepland om te worden automatisch uitgevoerd**. U kunt bijvoorbeeld omlaag schalen wanneer de workload licht is, zoals 's nachts of tijdens het weekend.
* Om te reageren op langere schommelingen of wijzigingen in de tenants **individuele tenants kunnen worden verplaatst naar een andere database**.
* Om te reageren op korte termijn toename in *afzonderlijke* belasting van de tenant, **individuele tenants kunnen worden genomen uit een database en een afzonderlijke prestatieniveau toegewezen**. Als de belasting wordt beperkt, kan de tenant met de multitenant-database worden geretourneerd. Wanneer dit van tevoren bekend, kunnen tenants worden verplaatst pre-emptively om te controleren of dat de database heeft altijd de resources die nodig is, om te voorkomen dat gevolgen voor andere tenants in de multitenant-database. Als het om een voorspelbare vereiste gaat, zoals bij een locatie die met een toename in kaartverkoop te maken krijgt voor een populair evenement, kan dit beheergedrag in de toepassing worden geïntegreerd.

[Azure Portal](https://portal.azure.com) biedt ingebouwde functionaliteit voor bewaking en waarschuwingen voor de meeste resources. Bewaking en waarschuwingen is beschikbaar voor databases voor SQL-Database. Deze ingebouwde bewaking en waarschuwingen is resource-specifieke, zodat het is gemakkelijk te gebruiken voor een klein aantal resources, maar is niet handig bij het werken met veel resources.

Voor grote hoeveelheden scenario's waarin u met veel resources werkt, [logboekanalyse (OMS)](https://azure.microsoft.com/services/log-analytics/) kan worden gebruikt. Dit is een afzonderlijke Azure-service die in analyse via verzonden diagnostische logboeken en telemetrie verzameld in een log analytics-werkruimte voorziet. Log Analytics telemetriegegevens kan verzamelen van veel services en worden gebruikt om te vragen en waarschuwingen instellen.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Ophalen van de broncode van de Database Wingtip Tickets SaaS-multitenant-toepassing en scripts

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="provision-additional-tenants"></a>Extra tenants inrichten

Voor een goed begrip van de bewaking van toepassingsprestaties en beheer werking op grote schaal moet in deze zelfstudie u beschikken over meerdere tenants in een shard multitenant-database.

Als u al een batch van tenants in een eerdere zelfstudie ingericht hebt, gaat u naar de [simuleren-gebruik op alle databases van de tenant](#simulate-usage-on-all-tenant-databases) sectie.

1. In de **PowerShell ISE**open... \\Learning-Modules\\prestatiebewaking en beheer\\*Demo PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel het volgende in: **$DemoScenario** = **1**, _Batch met tenants inrichten_
1. Druk op **F5** om het script uit te voeren.

Het script implementeert 17 tenants in de multitenant-database in een paar minuten. 

De *nieuw TenantBatch* script maakt nieuwe tenants met unieke tenant sleutels binnen de shard multitenant-database en initialiseert u ze met de tenant naam en u wilt. Dit komt overeen met de manier waarop die de app voorziet in een nieuwe tenant. 

## <a name="simulate-usage-on-all-tenant-databases"></a>Gebruik simuleren op alle tenantdatabases

De *Demo PerformanceMonitoringAndManagement.ps1* script is opgegeven dat een werkbelasting uitvoeren in de database multitenant simuleert. De belasting wordt gegenereerd met een van de beschikbare load-scenario's:

| Demo | Scenario |
|:--|:--|
| 2 | Laden van de normale intensiteit genereren (ongeveer 30 DTU) |
| 3 | Load met langer bursts per tenant genereren|
| 4 | Genereren van load met hogere DTU bursts per tenant (ongeveer 70 DTU)|
| 5 | Een hoge intensiteit genereren (ongeveer 90 DTU) op een enkele tenant plus een normale intensiteit van de belasting van alle andere tenants |

De load-generator past een *synthetische* load alleen voor CPU toe op elke tenantdatabase. De generator start voor elke tenantdatabase een taak, die periodiek een opgeslagen procedure aanroept die de load genereert. De belastingsniveaus (in dtu's), duur en intervallen voor alle databases, onvoorspelbare tenant activiteit simuleren verspreid.

1. In de **PowerShell ISE**open... \\Learning-Modules\\prestatiebewaking en beheer\\*Demo PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel **$DemoScenario** = **2**, _genereren normale intensiteit laden_
1. Druk op **F5** belasting toepassen op alle uw tenants.

Wingtip Tickets SaaS multitenant Database is een SaaS-app en de werkelijke belasting van een SaaS-app is doorgaans sporadisch en onvoorspelbaar. Om dit te simuleren, produceert de load-generator een willekeurige workload die over alle tenants wordt verdeeld. Enkele minuten nodig zijn voor de load-patroon te komen, dus het uitvoeren van de load-generator voor 3-5 minuten voordat u probeert om te controleren van de belasting in de volgende secties.

> [!IMPORTANT]
> De load-generator wordt uitgevoerd als een reeks taken in een nieuw PowerShell-venster. Als u de sessie sluit, wordt de load-generator stopt. De load-generator blijft in een *aanroepen van de taak* status waar belasting van een nieuwe tenants die zijn ingericht genereert nadat de generator is gestart. Gebruik *Ctrl-C* aanroepen van nieuwe taken stoppen en afsluiten van het script. De load-generator wordt nog uitgevoerd, maar alleen op bestaande tenants.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Resourcegebruik monitor met de Azure portal

Voor het bewaken van het gebruik van bronnen die het resultaat van de belasting die wordt toegepast, opent u de portal naar de database multitenant **tenants1**, met de tenants:

1. Open de [Azure-portal](https://portal.azure.com) en blader naar de server *tenants1-mt -&lt;gebruiker&gt;*.
1. Schuif naar beneden en zoek de databases en klikt u op **tenants1**. Deze shard multitenant-database bevat de tenants die zijn gemaakt.

![database-grafiek](./media/saas-multitenantdb-performance-monitoring/multitenantdb.png)

Houd rekening met de **DTU** grafiek.

## <a name="set-performance-alerts-on-the-database"></a>Prestatiewaarschuwingen instellen voor de database

Een waarschuwing instellen voor de database die wordt geactiveerd op \>75% gebruik als volgt:

1. Open de *tenants1* database (op het *tenants1-mt -&lt;gebruiker&gt;*  server) in de [Azure-portal](https://portal.azure.com).
1. Klik op **Waarschuwingsregels** en klik vervolgens op **+ Waarschuwing toevoegen**:

   ![waarschuwing toevoegen](media/saas-multitenantdb-performance-monitoring/add-alert.png)

1. Geef een naam op, bijvoorbeeld **hoog DTU**.
1. Stel de volgende waarden in:
   * **Metriek = DTU-percentage**
   * **Conditie = groter zijn dan**
   * **Drempel = 75**.
   * **Period = in de afgelopen 30 minuten**
1. Een e-mailadres toevoegen de *aanvullende beheerder email(s)* vak en klikt u op **OK**.

   ![waarschuwing instellen](media/saas-multitenantdb-performance-monitoring/set-alert.png)

## <a name="scale-up-a-busy-database"></a>Een bezet database opschalen

Als het niveau van de belasting op een database op het punt dit maximaliseert de database en 100% DTU-gebruik is bereikt toeneemt, wordt klikt u vervolgens prestaties van de database beïnvloed, mogelijk vertraging reactietijden van de query.

**Korte termijn**, overweeg schalen van de database om te voorzien in aanvullende resources, of tenants te verwijderen uit de multitenant-database (verplaatsen uit de multitenant-database naar een zelfstandige database).

**Lange termijn**, u kunt query's of index-gebruik voor het verbeteren van de prestaties van de database. Problemen met de aanbevolen procedure om een database opschalen voordat het DTU-gebruik van 100% bereikt, afhankelijk van de gevoeligheid voor de prestaties van de toepassing. Gebruik een waarschuwing zodat u tijdig wordt gewaarschuwd.

U kunt een bezet database door te verhogen van de belasting die wordt geproduceerd door de generator simuleren. De tenants voor burst vaker en voor langer, waardoor de belasting van de database multitenant vergroten zonder de vereisten van de afzonderlijke tenants. Schalen van de database is eenvoudig worden uitgevoerd in de portal of PowerShell. In deze oefening wordt de portal gebruikt.

1. Stel *$DemoScenario* = **3**, _load genereren met langer en vaker bursts per database_ te verhogen van de intensiteit van de cumulatieve belasting op de de database zonder de piekbelasting vereist voor elke tenant.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.
1. Ga naar de **tenants1** database in de Azure portal.

De toegenomen database DTU-gebruik op de bovenste grafiek bewaken. Het duurt enkele minuten duren voordat de nieuwe hogere belasting te starten, maar ziet u snel de database die beginnen met de maximale capaciteit bereikt, en als de belasting steadies in het nieuwe patroon, wordt de database snel overloads.

1. Als u wilt schalen van de database, klikt u op **prijscategorie (schaal dtu's)** op de instellingenblade.
1. Pas de **DTU** instelt op **100**. 
1. Klik op **toepassen** de aanvraag voor het schalen van de database.

Ga terug naar **tenants1** > **overzicht** om de bewaking grafieken weer te geven. Controleer het effect van het bieden van de database met meer bronnen (Hoewel u met enkele tenants en een willekeurige belasting is het niet altijd duidelijk afdoende totdat u enige tijd uitvoert). Terwijl u bekijkt de grafieken voorzien zijn er rekening mee dat 100% op vertegenwoordigt de bovenste grafiek nu 100 dtu's, terwijl in de grafiek lager is 100% nog steeds 50 dtu's is.

Databases blijven gedurende het proces online en volledig beschikbaar. Toepassingscode altijd verbroken verbindingen proberen opnieuw te worden geschreven en dus verbinding met de database wordt hersteld.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Een nieuwe tenant in de eigen database inrichten 

De shard multi-tenant model, kunt u kiezen of u voor het inrichten van een nieuwe tenant in een multitenant-database, samen met andere tenants of voor het inrichten van de tenant in een database met een eigen. Door het inrichten van een tenant in de eigen database kunt profiteren van de isolatie inherent aan de afzonderlijke database, zodat u kunt de prestaties van deze tenant onafhankelijk van andere gebruikers te beheren, herstellen die tenant onafhankelijk van andere, enzovoort. U kunt bijvoorbeeld gratis proefversie of reguliere klanten in een multitenant-database en premium-klanten in afzonderlijke databases plaatst.  Als geïsoleerde één tenant databases worden gemaakt, kunnen ze nog steeds worden beheerd samen in een elastische pool resourcekosten optimaliseren.

Als u een nieuwe tenant in de eigen database al ingericht, slaat u de volgende stappen.

1. In de **PowerShell ISE**open... \\Learning-Modules\\ProvisionTenants\\*Demo ProvisionTenants.ps1*. 
1. Wijzig **$TenantName = 'Salix Salsa'** en **$VenueType = 'dansen'**
1. Stel **$Scenario** = **2**, _inrichten van een tenant in een nieuwe database voor één tenant_
1. Druk op **F5** om het script uit te voeren.

Het script wordt deze tenant in een aparte database inrichten, de database en de tenant te registreren bij de catalogus en open vervolgens pagina van de tenant-gebeurtenissen in de browser. Vernieuw de pagina gebeurtenissen Hub en u ziet 'Salix Salsa' als u wilt een is toegevoegd.

## <a name="manage-performance-of-a-single-database"></a>Prestaties van één database beheren

Als een enkel tenant in een multitenant-database optreedt in een continue hoge belasting, kan het vaak een groot gedeelte uit de databaseresources en invloed hebben op andere tenants in dezelfde database. Als de activiteit kan enige tijd blijven, kan de tenant tijdelijk worden verplaatst uit de database en in een eigen database voor één tenant. Hierdoor kan de tenant om de extra bronnen nodig heeft en volledig geïsoleerd van de andere tenants hebben.

In deze oefening simuleert het effect van Salix Salsa overbelast wanneer tickets verkoop voor een populaire gebeurtenis.

1. Open de... \\ *Demo PerformanceMonitoringAndManagement.ps1* script.
1. Stel **$DemoScenario = 5**, _genereren van een normale belasting plus een hoge belasting van een enkele tenant (ongeveer 90 DTU)._
1. Stel **$SingleTenantName Salix Salsa =**
1. Voer het script uit met **F5**.

Ga naar de portal en navigeer naar **salixsalsa** > **overzicht** om de bewaking grafieken weer te geven. 

## <a name="other-performance-management-patterns"></a>Andere prestaties management patronen

**Tenant selfservice schalen**

Omdat schalen van een taak eenvoudig kunt aangeroepen via de beheer-API, u eenvoudig de mogelijkheid te schalen tenant databases in uw tenant gerichte-toepassing bouwen en aanbieden als onderdeel van uw SaaS-service. Laat tenants bijvoorbeeld zelf omhoog en omlaag schalen beheren, mogelijk gekoppeld aan hun facturering.

**Een database schalen omhoog en omlaag volgens een schema moet worden gezocht gebruikspatronen**

Wanneer het gebruik van de cumulatieve tenant volgt voorspelbare gebruikspatronen, kunt u Azure Automation omhoog en omlaag schalen van een database op een planning. Bijvoorbeeld, een database omlaag schalen na 18: 00 uur en up weer aan voordat u 06: 00 op weekdagen wanneer u weet er is een afname in resourcevereisten.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Informatie over het gebruik van een shard multitenant-database door het uitvoeren van een opgegeven load generator simuleren
> * De database bewaken tijdens het reageert op de toename van load
> * De database in reactie op de belasting van de hogere database opschalen
> * Een tenant in te richten in een één-tenant-database

## <a name="additional-resources"></a>Aanvullende bronnen

<!--* [Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Azure Automation](../automation/automation-intro.md)