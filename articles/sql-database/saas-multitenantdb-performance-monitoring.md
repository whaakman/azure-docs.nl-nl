---
title: Prestaties van een shard multitenant Azure SQL database in een multitenant SaaS-app bewaken | Microsoft Docs
description: Prestaties van shard Azure SQL-database met meerdere tenants in een multitenant SaaS-app controleren en beheren
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 5be6acc28932cb3c7f0481b18cbcffae27c3ce13
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002371"
---
# <a name="monitor-and-manage-performance-of-sharded-multi-tenant-azure-sql-database-in-a-multi-tenant-saas-app"></a>Prestaties van shard Azure SQL-database met meerdere tenants in een multitenant SaaS-app controleren en beheren

In deze zelfstudie worden verschillende belangrijke scenario's die worden gebruikt in SaaS-toepassingen verkend. Met behulp van een load-generator voor het simuleren van activiteit in shard multitenant-databases, worden de ingebouwde bewaking en waarschuwingen van de functies van SQL Database geïllustreerd.

De app Wingtip Tickets SaaS multitenant-Database maakt gebruik van een model met gedeelde gegevens van meerdere tenants, waar gegevens van de venue (tenant) worden gedistribueerd door tenant-ID voor mogelijk meerdere databases. Net als bij veel andere SaaS-toepassingen, is het verwachte tenantworkloadpatroon onvoorspelbaar en sporadisch. Met andere woorden, kaartverkoop kan op ieder moment plaatsvinden. Als u wilt profiteren van dit typerende patroon, kunnen databases omhoog en omlaag worden geschaald voor het optimaliseren van de kosten van een oplossing. Met dit type patroon is het belangrijk om het Resourcegebruik van de database om ervoor te zorgen dat belastingen redelijkerwijs worden verdeeld over mogelijk meerdere databases te bewaken. Ook moet u ervoor zorgen dat individuele databases voldoende resources hebben en worden zo hun [DTU](sql-database-purchase-models.md#dtu-based-purchasing-model) limieten. Deze zelfstudie worden manieren om te controleren en beheren van databases, en hoe u corrigerende maatregelen nemen in reactie op wisselingen in de workload.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Gebruik op een shard multitenant-database door het uitvoeren van een geleverde load-generator simuleren
> * Bewaken van de database terwijl er wordt gereageerd op de toename in load
> * Schalen van de database in reactie op de verhoogde databaseload
> * Een tenant in te richten in een database met één tenant

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS multitenant-Database wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS multitenant-Database verkennen](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-saas-performance-management-patterns"></a>Kennismaking met prestatiebeheerpatronen voor SaaS

Het beheren van de databaseprestaties bestaat uit het verzamelen en analyseren van prestatiegegevens en het reageren op deze gegevens door parameters aan te passen om een acceptabele reactietijd voor de toepassing te behouden. 

### <a name="performance-management-strategies"></a>Strategieën voor prestatiebeheer

* Om te voorkomen dat handmatig prestaties moet bewaken, is het meest effectief aan **waarschuwingen instellen die wordt geactiveerd wanneer databases buiten het normale bereik afwijken**.
* Om te reageren op korte termijn fluctuaties in de compute-grootte van een database, de **DTU-niveau kunt omhoog of omlaag worden geschaald**. Als deze fluctuatie op de basis van een regelmatig of op voorspelbare optreedt **schalen van de database kan worden gepland ter automatisch uitgevoerd**. U kunt bijvoorbeeld omlaag schalen wanneer de workload licht is, zoals 's nachts of tijdens het weekend.
* Om te reageren op fluctuaties op de langere termijn of wijzigingen in de tenants **afzonderlijke tenants kunnen worden verplaatst naar een andere database**.
* Om te reageren op korte termijn verhogingen in *afzonderlijke* tenant laden, **afzonderlijke tenants kunnen worden genomen uit een database en de grootte van een afzonderlijke computerknooppunten toegewezen**. Zodra de load is verlaagd, kan de tenant met de multitenant-database worden geretourneerd. Wanneer dit vooraf bekend is, wordt tenants kunnen worden voorzorg verplaatst om te controleren of dat de database heeft altijd de benodigde resources beschikken en om te voorkomen dat gevolgen voor andere tenants in de database met meerdere tenants. Als het om een voorspelbare vereiste gaat, zoals bij een locatie die met een toename in kaartverkoop te maken krijgt voor een populair evenement, kan dit beheergedrag in de toepassing worden geïntegreerd.

[Azure Portal](https://portal.azure.com) biedt ingebouwde functionaliteit voor bewaking en waarschuwingen voor de meeste resources. Bewaking en waarschuwingen is beschikbaar voor databases voor SQL-Database. Deze ingebouwde bewaking en waarschuwingen is resource-specifieke, dus dit is handig voor kleine aantallen resources, maar is niet handig als u werkt met veel resources.

Voor scenario's met hoge volumes, waarin u met veel resources werkt, [Log Analytics](https://azure.microsoft.com/services/log-analytics/) kan worden gebruikt. Dit is een afzonderlijke Azure-service die analyse van verzonden diagnostische logboeken en telemetrie die zijn verzameld in een log analytics-werkruimte biedt. Log Analytics kunnen telemetrische gegevens van veel services en worden gebruikt om te zoeken en waarschuwingen instellen.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Download de broncode van de toepassing Wingtip Tickets SaaS multitenant-Database en -scripts

De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="provision-additional-tenants"></a>Extra tenants inrichten

Voor een goed begrip van hoe prestaties controleren en beheren op schaal werkt, moet in deze zelfstudie u meerdere tenants hebt in een multitenant shard-database.

Als u al een batch tenants hebt ingericht in een eerdere zelfstudie, gaat u naar de [gebruik simuleren op alle tenantdatabases](#simulate-usage-on-all-tenant-databases) sectie.

1. In de **PowerShell ISE**, openen... \\Learning Modules\\Performance Monitoring and Management\\*Demo-PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel het volgende in: **$DemoScenario** = **1**, _Batch met tenants inrichten_
1. Druk op **F5** om het script uit te voeren.

Het script implementeert 17 tenants in de database met meerdere tenants in een paar minuten. 

De *New-TenantBatch* script maakt u nieuwe tenants met unieke tenant sleutels binnen de shard multitenant-database en geïnitialiseerd met de tenant-type voor de naam en locatie. Dit komt overeen met de manier waarop die de app een nieuwe tenant inricht. 

## <a name="simulate-usage-on-all-tenant-databases"></a>Gebruik simuleren op alle tenantdatabases

De *Demo-PerformanceMonitoringAndManagement.ps1* script is opgegeven dat een werkbelasting op de database met meerdere tenants simuleert. De belasting wordt gegenereerd met behulp van een van de beschikbare load-scenario's:

| Demo | Scenario |
|:--|:--|
| 2 | Belasting met normale intensiteit genereren (ongeveer 30 DTU) |
| 3 | Load genereren met langere bursts per tenant|
| 4 | Load genereren met meer DTU-bursts per tenant (ongeveer 70 DTU)|
| 5 | Een hoge intensiteit genereren (ongeveer 90 DTU) belasting op één tenant plus een normale intensiteit van alle andere tenants |

De load-generator past een *synthetische* load alleen voor CPU toe op elke tenantdatabase. De generator start voor elke tenantdatabase een taak, die periodiek een opgeslagen procedure aanroept die de load genereert. De load-niveaus (in dtu's), duur en intervallen zijn verschillend voor alle databases, simuleert onvoorspelbare tenantactiviteit.

1. In de **PowerShell ISE**, openen... \\Learning Modules\\Performance Monitoring and Management\\*Demo-PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel **$DemoScenario** = **2**, _belasting met normale intensiteit genereren_
1. Druk op **F5** naar een belasting van toepassing op alle uw tenants.

Wingtip Tickets SaaS multitenant-Database is een SaaS-app en de werkelijke belasting van een SaaS-app is meestal sporadisch en onvoorspelbaar. Om dit te simuleren, produceert de load-generator een willekeurige workload die over alle tenants wordt verdeeld. Enkele minuten nodig zijn voor het patroon load te geven, dus de load-generator voor 3 tot 5 minuten voordat u de workload in de volgende secties worden uitgevoerd.

> [!IMPORTANT]
> De load-generator wordt uitgevoerd als een reeks taken in een nieuw PowerShell-venster. Als u de sessie sluit, stopt de load-generator. De load-generator blijft in een *aanroepen van de taak* staat waarin deze belasting op alle nieuwe tenants die zijn ingericht genereert na de generator is gestart. Gebruik *Ctrl-C* te stoppen met het aanroepen van nieuwe projecten en het script af te sluiten. De load-generator blijft om uit te voeren, maar alleen op bestaande tenants.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Gebruik van resources met behulp van de Azure-portal bewaken

Voor het controleren van het gebruik van bronnen die het resultaat is van de belasting wordt toegepast, open de portal naar de database met meerdere tenants **tenants1**, met de tenants:

1. Open de [Azure-portal](https://portal.azure.com) en blader naar de server *tenants1-mt -&lt;gebruiker&gt;*.
1. Schuif naar beneden, zoek van databases en klikt u op **tenants1**. Deze shard multitenant-database bevat alle tenants die tot nu toe zijn gemaakt.

![database-grafiek](./media/saas-multitenantdb-performance-monitoring/multitenantdb.png)

Bekijk de **DTU** grafiek.

## <a name="set-performance-alerts-on-the-database"></a>Prestatiewaarschuwingen instellen voor de database

Een melding instellen op de database die wordt geactiveerd \>75% gebruik als volgt te werk:

1. Open de *tenants1* database (op de *tenants1-mt -&lt;gebruiker&gt;*  server) in de [Azure-portal](https://portal.azure.com).
1. Klik op **Waarschuwingsregels** en klik vervolgens op **+ Waarschuwing toevoegen**:

   ![waarschuwing toevoegen](media/saas-multitenantdb-performance-monitoring/add-alert.png)

1. Geef een naam op, bijvoorbeeld **hoog DTU**.
1. Stel de volgende waarden in:
   * **Metrische gegevens = DTU-percentage**
   * **Voorwaarde = groter dan**
   * **Drempel = 75**.
   * **Periode = in de afgelopen 30 minuten**
1. Een e-mailadres toevoegen de *beheerder email(s)* vak en klikt u op **OK**.

   ![waarschuwing instellen](media/saas-multitenantdb-performance-monitoring/set-alert.png)

## <a name="scale-up-a-busy-database"></a>Schalen van een bezette database

Als het niveau van de belasting van een database op het punt deze maximaliseert de database en 100% DTU-gebruik wordt bereikt toeneemt, wordt klikt u vervolgens prestaties van de database beïnvloed, mogelijk trage reactietijden van de query.

**Korte termijn**, kunt u omhoog schalen van de database om aanvullende resources te leveren, of tenants verwijderen uit de multitenant-database (verplaatsen buiten de multitenant-database naar een zelfstandige database).

**Langere termijn**, u kunt query's of indexgebruik voor betere databaseprestaties. Afhankelijk van de toepassing de gevoeligheid voor prestatieproblemen van problemen met de aanbevolen procedure een database om omhoog te schalen voordat 100% DTU-gebruik is bereikt. Gebruik een waarschuwing zodat u tijdig wordt gewaarschuwd.

U kunt een bezette database simuleren door het verhogen van de belasting die de generator produceert. De tenants om uit te breiden vaker en voor meer het geval is, waardoor de belasting van de database met meerdere tenants verhogen zonder te hoeven wijzigen van de vereisten van de afzonderlijke tenants. Omhoog schalen van de database is eenvoudig uitgevoerd in de portal of vanuit PowerShell. In deze oefening wordt de portal gebruikt.

1. Stel *$DemoScenario* = **3**, _load genereren met langere en frequentere bursts per database_ te verhogen van de intensiteit van de cumulatieve load genereren op de de database zonder de piekbelasting die is vereist voor elke tenant.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.
1. Ga naar de **tenants1** database in Azure portal.

Het toegenomen DTU-gebruik op de bovenste grafiek-database bewaken. Het duurt een paar minuten voor het nieuwe hogere load wordt geactiveerd, maar ziet u snel de database die beginnen met het maximale capaciteit bereikt en als de belasting vrij in het nieuwe patroon, wordt de database snel overbelast.

1. Als u wilt schalen van de database, klikt u op **prijscategorie (dtu's schalen)** in de instellingenblade.
1. Pas de **DTU** instelt op **100**. 
1. Klik op **toepassen** dien de aanvraag voor het schalen van de database.

Ga terug naar **tenants1** > **overzicht** om de bewakingsgrafieken weer te geven. Controleer het effect van het leveren van de database met meer bronnen (Hoewel met enkele tenants en een willekeurige workload is het niet altijd gemakkelijk afdoende zien totdat u voor enige tijd uitvoert). Terwijl u kijkt naar het Beer grafieken er rekening mee dat 100% op staat de bovenste grafiek nu voor 100 dtu's, terwijl op de onderste grafiek 100% nog steeds is ingesteld op 50 dtu's.

Databases blijven gedurende het proces online en volledig beschikbaar. Toepassingscode altijd verbroken verbindingen opnieuw moet worden geschreven en dus wordt opnieuw verbinding maken met de database.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Een nieuwe tenant in een eigen database inrichten 

De shard multitenant-model kunt u kiezen of u een nieuwe tenant in een multitenant-database samen met andere tenants in te richten of voor het inrichten van de tenant in een database met een eigen. Door het inrichten van een tenant in een eigen database, profiteert het bedrijf van de isolatie van de afzonderlijke database, zodat u kunt de prestaties van deze tenant onafhankelijk van andere beheren en terug te zetten die tenant onafhankelijk van anderen, enzovoort. U kunt bijvoorbeeld, gratis proefversie of reguliere klanten in een multitenant-database en premium-klanten in afzonderlijke databases plaatst.  Als geïsoleerde één tenant-databases worden gemaakt, kunnen ze nog steeds worden beheerd samen in een elastische pool het optimaliseren van resourcekosten.

Als u een nieuwe tenant in een eigen database al hebt ingericht, gaat u de volgende stappen.

1. In de **PowerShell ISE**, openen... \\Learning Modules\\ProvisionTenants\\*Demo-ProvisionTenants.ps1*. 
1. Wijzigen **$TenantName = "Salix Salsa"** en **$VenueType = "dansen"**
1. Stel **$Scenario** = **2**, _tenant in een nieuwe database met één tenant in te richten_
1. Druk op **F5** om het script uit te voeren.

Het script wordt deze tenant in een afzonderlijke database in te richten, de database en de tenant te registreren bij de catalogus en open vervolgens de pagina gebeurtenissen van de tenant in de browser. Vernieuw de Events Hub-pagina en ziet u 'Salix Salsa' als een locatie die is toegevoegd.

## <a name="manage-performance-of-an-individual-database"></a>Prestaties van een individuele database beheren

Als één tenant in een multitenant-database optreedt in een duurzame hoge belasting, kan het vaak een groot gedeelte uit de database-resources en invloed hebben op andere tenants in dezelfde database. Als de activiteit enige tijd voortduurt is, kan de tenant tijdelijk worden verplaatst uit de database en in een eigen één tenant-database. Hiermee wordt de tenant beschikken over de extra resources nodig heeft en volledig geïsoleerd van de andere tenants.

In deze oefening simuleert het effect van Salix Salsa ondervindt een hoge belasting wanneer kaartjes voor verkoop voor een populair evenement.

1. Open de... \\ *Demo-PerformanceMonitoringAndManagement.ps1* script.
1. Stel **$DemoScenario = 5**, _genereren van een normale load en hoge load op één tenant (ongeveer 90 DTU)._
1. Stel **$SingleTenantName Salix Salsa =**
1. Voer het script uit met **F5**.

Ga naar de portal en Ga naar **salixsalsa** > **overzicht** om de bewakingsgrafieken weer te geven. 

## <a name="other-performance-management-patterns"></a>Andere patronen voor prestatiebeheer

**Self-service schalen voor tenant**

Omdat schalen een taak eenvoudig kunt aangeroepen via de beheer-API, u eenvoudig de mogelijkheid om tenantdatabases te schalen in uw tenantgerichte toepassing bouwen en deze als een functie van uw SaaS-service te bieden. Laat tenants bijvoorbeeld zelf omhoog en omlaag schalen beheren, mogelijk gekoppeld aan hun facturering.

**Een database schalen omhoog en omlaag volgens een schema aan gebruikspatronen**

Wanneer het cumulatieve tenantgebruik een voorspelbaar patroon volgt, kunt u Azure Automation een database omhoog of omlaag schalen volgens een schema. Schalen van een database bijvoorbeeld omlaag na 18: 00 uur en van opnieuw vóór 06: 00 op werkdagen waarop u weet dat er een afname in de benodigde resources.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik op een shard multitenant-database door het uitvoeren van een geleverde load-generator simuleren
> * Bewaken van de database terwijl er wordt gereageerd op de toename in load
> * Schalen van de database in reactie op de verhoogde databaseload
> * Een tenant in te richten in een database met één tenant

## <a name="additional-resources"></a>Aanvullende resources

<!--* [Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Azure Automation](../automation/automation-intro.md)