---
title: De prestaties van een SQL Database SaaS-app bewaken | Microsoft Docs
description: De prestaties van de Azure SQL Database-voorbeeld-app Wingtip Tickets (WTP) bewaken en beheren
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: af9511978718af10c97bee6af3a2835c9d2c1ff4
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="monitor-performance-of-the-wtp-sample-saas-application"></a>Prestaties bewaken van de SaaS-voorbeeldtoepassing WTP

In deze zelfstudie worden de ingebouwde functies voor bewaken en waarschuwen van SQL Database en elastische pools toegelicht. Daarna worden verschillende belangrijke scenario's voor prestatiebeheer in SaaS-toepassingen beschreven.

De Wingtip Tickets-app gebruikt een gegevensmodel met één tenant, waarbij elke locatie (tenant) een eigen database heeft. Net als bij veel andere SaaS-toepassingen, is het verwachte tenantworkloadpatroon onvoorspelbaar en sporadisch. Met andere woorden, kaartverkoop kan op ieder moment plaatsvinden. Tenantdatabases worden geïmplementeerd in pools voor Elastic Database om te profiteren van dit typerende patroon voor databasegebruik. Elastische pools verlagen de kosten van een oplossing door resources over veel databases te verdelen. Bij dit type patroon is het belangrijk om het gebruik van database- en poolresources te controleren om ervoor te zorgen dat loads goed over pools zijn verdeeld. Ook moet u ervoor zorgen dat individuele databases voldoende resources hebben en dat pools niet hun [eDTU](sql-database-what-is-a-dtu.md)-limieten overschrijden. In deze zelfstudie worden manieren toegelicht om databases en pools te controleren en beheren. Ook wordt uitgelegd hoe u corrigerende maatregelen kunt nemen als reactie op wisselingen in de workload.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De WTP-app is geïmplementeerd. Zie [De WTP SaaS-toepassing implementeren en verkennen](sql-database-saas-tutorial.md) om dit in minder dan vijf minuten te doen
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-saas-performance-management-patterns"></a>Kennismaking met prestatiebeheerpatronen voor SaaS

Het beheren van de databaseprestaties bestaat uit het verzamelen en analyseren van prestatiegegevens en het reageren op deze gegevens door parameters aan te passen om een acceptabele reactietijd voor de toepassing te behouden. Wanneer u meerdere tenants host, zijn pools voor Elastic Database een rendabele manier om resources te bieden en te beheren voor een groep databases met onvoorspelbare workloads. Met bepaalde workloadpatronen kan het al bij twee S3-databases voordelig zijn om ze door een pool te laten beheren. Een pool deelt niet alleen de kosten van resources,maar kan er ook voor zorgen dat het voortdurend controleren en volgen van afzonderlijke databases niet meer nodig is.

![media](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Pools en de databases in pools moeten nog wel worden gecontroleerd om ervoor te zorgen dat ze binnen een acceptabel prestatiebereik blijven. Stel de configuratie van de pool af op de vereisten van de cumulatieve workload om ervoor te zorgen dat de eDTU's van de pools geschikt zijn voor de algemene workload. Pas de minimale en maximale eDTU-waarden per database aan naar de geschikte waarden voor de vereisten van uw specifieke toepassing.

### <a name="performance-management-strategies"></a>Strategieën voor prestatiebeheer

* Als u wilt voorkomen dat u handmatig prestaties moet bewaken, is **waarschuwingen instellen die worden geactiveerd wanneer databases of pools van het normale bereik afwijken** het effectiefst.
* Het **eDTU-niveau van een pool kan omhoog of omlaag worden geschaald** om te reageren op fluctuaties op de korte termijn in het cumulatieve prestatieniveau van een pool. Als deze fluctuatie regelmatig of op voorspelbare momenten optreedt, **kan het schalen van de pool op automatisch worden ingesteld**. U kunt bijvoorbeeld omlaag schalen wanneer de workload licht is, zoals 's nachts of tijdens het weekend.
* U kunt ook **afzonderlijke databases naar andere pools verplaatsen** bij fluctuaties op de langere termijn of wijzigingen in het aantal databases.
* **Afzonderlijke databases kunnen uit een pool worden gehaald en een afzonderlijk prestatieniveau toegewezen krijgen** voor een bepaalde duur als reactie op verhogingen op de korte termijn in de load van *afzonderlijke* databases. Wanneer de load is verlaagd, kan de database naar de pool worden teruggezet. Als dit vooraf bekend is, kunnen databases uit voorzorg worden verplaatst om ervoor te zorgen dat ze altijd over de benodigde resources beschikken en om gevolgen voor de andere databases in de pool te voorkomen. Als het om een voorspelbare vereiste gaat, zoals bij een locatie die met een toename in kaartverkoop te maken krijgt voor een populair evenement, kan dit beheergedrag in de toepassing worden geïntegreerd.

[Azure Portal](https://portal.azure.com) biedt ingebouwde functionaliteit voor bewaking en waarschuwingen voor de meeste resources. Voor SQL Database zijn bewaking en waarschuwingen beschikbaar voor databases en pools. De ingebouwde functionaliteit voor bewaking en waarschuwingen is resource-gebonden. Daardoor is het handig voor een klein aantal resources, maar niet erg handig als u met veel resources werkt.

Voor scenario's met grote volumes kan Log Analytics (ook bekend als OMS) worden gebruikt. Dit is een afzonderlijke Azure-service die analyse biedt van verzonden diagnostische logboeken en telemetrische gegevens die zijn verzameld in een werkruimte voor logboekanalyse. In een dergelijke ruimte kunnen telemetrische gegevens van allerlei services worden verzameld om op basis hiervan query's uit te voeren en waarschuwingen te genereren.

## <a name="get-the-wingtip-application-scripts"></a>De scripts van de Wingtip-toepassing downloaden

De scripts en broncode van de Wingtip Tickets-toepassing zijn beschikbaar in de GitHub-opslagplaats [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). U vindt de scriptbestanden in de map [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Download de map **Learning Modules** naar de lokale computer en behoud de mapstructuur.

## <a name="provision-additional-tenants"></a>Extra tenants inrichten

Hoewel pools al met twee S3-databases rendabel kunnen zijn, geldt dat hoe meer databases er in de pool zitten, hoe rendabeler het berekenen van gemiddelden wordt. U hebt voor deze zelfstudie ten minste 20 geïmplementeerde databases nodig om een goed inzicht te krijgen in de werking van het bewaken en beheren van prestaties op schaal.

Als u al een batch tenants hebt ingericht in een eerdere zelfstudie, kunt u het gedeelte [Gebruik simuleren op alle tenantdatabases](#simulate-usage-on-all-tenant-databases) overslaan.

1. Open \\Learning Modules\\Provision and Catalog\\*Demo PerformanceMonitoringAndManagement.ps1* in de **PowerShell ISE**. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel het volgende in: **$DemoScenario** = **1**, **Batch met tenants inrichten**
1. Druk op **F5** om het script uit te voeren.

Het script implementeert 17 tenants in minder dan vijf minuten.

Het nieuwe script *New-TenantBatch* gebruikt een geneste of gekoppelde set [Resource Manager](../azure-resource-manager/index.md)-sjablonen die een batch tenants maakt. Hierbij wordt standaard de database **baseTenantDb** gekopieerd op de catalogusserver om nieuwe tenantdatabases te maken. Vervolgens worden deze geregistreerd in de catalogus en ten slotte geïnitialiseerd met de tenantnaam en het locatietype. Dit komt overeen met de manier waarop de WTP-app een nieuwe tenant inricht. Wijzigingen die in *baseTenantDB* worden aangebracht, worden toegepast op alle nieuwe tenants die hierna worden ingericht. Zie de zelfstudie [Schema Management](sql-database-saas-tutorial-schema-management.md) (Schemabeheer) voor het aanbrengen van schemawijzigingen in *bestaande* tenantdatabases (inclusief de *golden* database).

## <a name="simulate-different-usage-patterns-by-generating-different-load-types"></a>Verschillende gebruikspatronen simuleren door verschillende loadtypes te genereren

Het script *Demo-PerformanceMonitoringAndManagement.ps1* start de load-generator met behulp van een van de beschikbare *loadtypes*:

| Demo | Scenario |
|:--|:--|
| 2 | Belasting met normale intensiteit genereren (ongeveer 40 DTU's) |
| 3 | Load genereren met langere en frequentere bursts per database|
| 4 | Load genereren met meer DTU-bursts per database (ongeveer 80 DTU's)|
| 5 | Een normale load en hoge load genereren op één tenant (ongeveer 95 DTU's)|
| 6 | Niet-gebalanceerde load genereren voor meerdere pools|

## <a name="simulate-usage-on-all-tenant-databases"></a>Gebruik simuleren op alle tenantdatabases

De load-generator past een *synthetische* load alleen voor CPU toe op elke tenantdatabase. De generator start voor elke tenantdatabase een taak, die periodiek een opgeslagen procedure aanroept die de load genereert. De load-niveaus (in eDTU's), duur en intervallen zijn verschillend voor alle databases. Dit simuleert onvoorspelbare tenantactiviteit.

1. Stel het volgende in: **$DemoScenario** = **2**, *Een load van normale intensiteit genereren*.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.

Vanwege de sporadische aard van de load, moet de generator 10 tot 20 minuten worden uitgevoerd, zodat de activiteit een stabiele status kan bereiken en een duidelijk patroon kan ontwikkelen.

> [!IMPORTANT]
> De load-generator wordt uitgevoerd als een reeks taken in uw lokale PowerShell-sessie. Houd het tabblad *Demo-PerformanceMonitoringAndManagement.ps1* geopend. Als u het tabblad sluit of de computer onderbreekt, stopt de load-generator.

## <a name="monitor-resource-usage-using-the-portal"></a>Gebruik van resources bewaken met de portal

Open de portal naar de pool die de tenantdatabases bevat om het resourcegebruik bij te houden dat ontstaat door het toepassen van de load.

1. Open [Azure Portal](https://portal.azure.com) en blader naar de server tenants1-&lt;USER&gt;.
1. U zou de lijst met tenantdatabases moeten zien, met inbegrip van de nieuwe batch databases.
1. Scrol naar beneden, zoek de elastische pools en klik op **Pool1**. Deze pool bevat alle tenantdatabases die tot nu toe zijn gemaakt.
1. Vouw de pool-blade die wordt geopend uit en bekijk de grafiek voor het poolgebruik en de grafiek voor het gebruik van de meest belaste databases.

Het poolgebruik is in feite het cumulatieve databasegebruik voor alle databases in de pool. De grafiek voor databasegebruik toont de vijf meest belaste databases:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Omdat er naast de top vijf nog andere databases in de pool zitten, toont het poolgebruik activiteit die niet wordt weergegeven in de grafiek voor de vijf meest belaste databases. Klik op **Gebruik van de databasebron** om meer gegevens te zien:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Prestatiewaarschuwingen voor de pool instellen

Stel als volgt een waarschuwing in voor de pool die wordt geactiveerd \>bij 75% gebruik gedurende vijf minuten:

1. Open *Pool1* (op de server *tenants1-\<user\>*) in [Azure Portal](https://portal.azure.com).
1. Klik op **Waarschuwingsregels** en klik vervolgens op **+ Waarschuwing toevoegen**:

   ![waarschuwing toevoegen](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Geef een naam op, bijvoorbeeld **hoog DTU**. 
1. Stel de volgende waarden in:
   * **Metriek = eDTU-percentage**
   * **Voorwaarde = groter dan**.
   * **Drempel = 75**.
   * **Periode = In de afgelopen 30 minuten**.

   ![waarschuwing instellen](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)

U kunt instellen dat meldingen worden verzonden naar het e-mailadres van uw Azure-account en eventueel naar andere e-mailadressen (dit laatste wordt niet aanbevolen, tenzij u de eigenaar van het gebruikte abonnement bent).

> [!NOTE]
> Aangezien de waarschuwing alleen wordt gegeven wanneer de drempelwaarde gedurende de laatste 30 minuten is overschreden, moet de load-generator meer dan 30 minuten worden uitgevoerd om de waarschuwing te testen.


## <a name="scale-up-a-busy-pool"></a>Een zwaar belaste pool omhoog schalen

Als het cumulatieve loadniveau voor een pool in een dergelijke mate toeneemt dat de pool volledig wordt gebruikt en 100% eDTU-gebruik wordt bereikt, worden de prestaties van afzonderlijke databases beïnvloed. Hierdoor wordt de reactietijd voor query's mogelijk vertraagd voor alle databases in de pool.

Op de korte termijn kunt u de pool opschalen om extra resources te bieden of databases uit de pool verwijderen (door ze naar andere pools te verplaatsen of ze vanuit de pool in een zelfstandige servicelaag te plaatsen).

Op de langere termijn kunt u query's of indexgebruik optimaliseren voor betere databaseprestaties. Afhankelijk van de gevoeligheid voor prestatieproblemen van de toepassing, is de aanbevolen procedure om een pool op te schalen voordat 100% eDTU-gebruik wordt bereikt. Gebruik een waarschuwing zodat u tijdig wordt gewaarschuwd.

U kunt een zwaar belaste pool simuleren door de load die de generator produceert, te verhogen. Doordat de databases vaker en langer maximaal worden gebruikt, wordt de cumulatieve load voor de pool verhoogd zonder dat de vereisten van de afzonderlijke databases worden gewijzigd. U kunt de pool eenvoudig opschalen in de portal of vanuit PowerShell. In deze oefening wordt de portal gebruikt.

1. Stel *$DemoScenario* = **3**, _Load genereren met langere en frequentere bursts per database_ in om de intensiteit van de cumulatieve load voor de pool te verhogen zonder de piekbelasting die is vereist voor elke database te wijzigen.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.


1. **Open de pool-blade** **voor tenants1/Pool1**.


1. Controleer het verhoogde DTU-verbruik voor de pool in de bovenste grafiek. Het duurt enkele minuten voordat de nieuwe, hogere load wordt geactiveerd, maar de pool zou vrij snel 100% gebruik moeten bereiken. Zodra de load een nieuw patroon vormt, wordt de pool snel overbelast.


1. Klik op **Groep configureren** om de pool omhoog te schalen
1. Stel de schuifregelaar **Groeps-eDTU** in op 100 (we raden u aan niet hoger te gaan om de kosten te beperken). Let op hoe de cumulatieve opslag die beschikbaar is voor alle databases in de pool, aangeduid met **Groep-GB**, gekoppeld is aan de eDTU-instelling en ook stijgt. Als u de groeps-eDTU wijzigt, wordt de instelling per database niet gewijzigd (deze is nog steeds maximaal 50 eDTU's per database). De instellingen per database staan aan de rechterkant van de blade **Groep configureren**.
1. Klik op **Opslaan** om de aanvraag te verzenden. De wijziging duurt doorgaans 3 tot 5 minuten voor een standaardpool.

Ga terug naar het **Pool1** > **Overzicht** om de bewakingsgrafieken weer te geven. Bewaak het effect van de toevoeging van meer resources aan de pool (met weinig databases en een willekeurige load is het niet altijd duidelijk te bepalen). Houd er rekening mee dat 100% bij de bovenste grafiek nu staat voor 100 eDTU's, terwijl voor de onderste grafiek 100% nog steeds 50 eDTU's betekent, omdat het maximum per database 50 eDTU's is.

Databases blijven gedurende het proces online en volledig beschikbaar. Op het laatste moment, wanneer elke database gereed is om te worden ingeschakeld met de nieuwe pool-eDTU's, worden alle actieve verbindingen verbroken. Om verbroken verbindingen opnieuw te proberen, moet altijd een toepassingscode worden geschreven zodat er opnieuw verbinding wordt gemaakt met de database in de omhoog geschaalde pool.

## <a name="create-a-second-pool-and-load-balance-databases-to-handle-increased-aggregate-load"></a>Maak een tweede pool en verdeel taken voor de databases om de toegenomen cumulatieve load te verwerken

In plaats van de pool omhoog te schalen, kunt u ook een tweede pool maken en databases hiernaartoe verplaatsen om de load tussen de twee pools te verdelen. Om dit te doen, moet de nieuwe pool op dezelfde server als de eerste pool worden gemaakt.

1. Open de **serverblade voor de server customers1-&lt;USER&gt;**. Als u zich in een database of pool-blade bevindt, kunt u een vervolgkeuzelijst voor het besturingselement Essentials openen en de servernaam als snelkoppeling selecteren.
1. Klik op **+ Nieuwe pool** om een pool op de huidige server te maken
1. Op de nieuwe sjabloon voor pools voor Elastic Database:

    1. stel **naam = Pool2** in.
    1. Laat de prijscategorie op **Standaardpool** staan.
    1. Klik op **Groep configureren**.
    1. De blade Groep configureren wordt geopend. Stel hierin het volgende in: **Groeps-eDTU = 50 DTU's**.
    1. Klik op de opdracht **Databases toevoegen** voor een lijst van databases op deze server die niet in de huidige pool staan.
    1. U moet de helft van de databases (10 van 20) in deze lijst **inschakelen** om ze naar de nieuwe pool te verplaatsen. Klik vervolgens op **Selecteren**.
    1. Klik opnieuw op **Selecteren** om de configuratiewijzigingen te accepteren. Bekijk de geschatte kosten voor één maand gebruik met de geselecteerde opties.
    1. Selecteer **OK** om de nieuwe pool te maken met de nieuwe configuratie en om de databases te verplaatsen.

Het maken van de pool en het verplaatsen van de databases duurt enkele minuten. Elke database die wordt verplaatst, blijft online en volledig toegankelijk tot op het laatste moment. Hierna worden alle open verbindingen gesloten. Wanneer een client opnieuw verbinding probeert te maken, wordt er verbinding gemaakt met de database in de nieuwe pool.

Zodra de pool is gemaakt, wordt deze weergegeven op de serverblade customers1. Klik op de naam van de pool om de pool-blade te openen en de prestaties te controleren.

U zou moeten zien dat het resourcegebruik voor Pool1 is gedaald en dat Pool2 evenveel is belast.

## <a name="manage-an-increased-load-on-a-single-database"></a>Een hogere load voor een individuele database beheren

Als een individuele database in een pool langere tijd een hogere load heeft, kan het de resources in de pool overheersen en andere databases beïnvloeden, afhankelijk van de poolconfiguratie. Als de activiteit mogelijk nog enige tijd voortduurt, kan de database tijdelijk uit de pool worden geplaatst. Hiermee kan de database meer resources worden gegeven dan de andere databases in de pool en kan de database van de andere databases worden geïsoleerd. Deze oefening simuleert het effect van een hoge load voor Contoso Concert Hall wanneer kaartjes voor een populair concert in de verkoop gaan.

1. In het script …\\**Demo-PerformanceManagementAndMonitoring**.ps1
1. Stel het volgende in: **$DemoScenario = 5, Een normale load en hoge load genereren op één tenant (ongeveer 95 DTU's).**
1. Stel **$SingleTenantDatabaseName = contosoconcerthall** in
1. Voer het script uit met **F5**.
1. **Open de pool-blade** **voor Customers1/Pool1**.
1. Bekijk de weergave **Elastische groep controleren** bovenaan de blade en zoek naar het toegenomen DTU-gebruik voor de pool. Na een paar minuten moet de hogere load actief worden en zou de pool snel een gebruik van 100% moeten weergeven.
1. Controleer ook de weergave **Elastic Database controleren** waarin de meest belaste databases in het afgelopen uur worden weergegeven. De database contosoconcerthall zou snel moeten worden weergegeven als een van de vijf meest belaste databases.
1. **Klik op de **grafiek** Elastic Database controleren**. De blade **Gebruik van de databasebron** wordt geopend. Hier kunt u elke database controleren. Hiermee kunt u de weergave voor de database contosoconcerthall isoleren.
1. **Klik op contosoconcerthall** in de lijst met databases. De database-blade wordt geopend.
1. Klik op **Prijscategorie (DTU's schalen)** in het contextmenu om de blade **Prestatie configureren** te openen. Hier kunt u een geïsoleerd prestatieniveau voor de database instellen.
1. Klik op het tabblad **Standard** om de schaalopties in de categorie Standard te openen.
1. Schuif de schuifregelaar **DTU** naar rechts om 100 DTU's te selecteren. Dit komt overeen met de servicedoelstelling, **S3**, die tussen haakjes wordt weergegeven tussen de meters DTU en Opslaggrootte.
1. Klik op **Toepassen** om de database uit de pool te plaatsen en er een standaard S3-database van te maken.
1. Zodra de implementatie is voltooid, kunt u het effect op de database contosoconcert hall en de pool waaruit deze was verwijderd bewaken op de blades Elastische pool en Database.

Zodra de hoger dan normale load van de database contosoconcerthall afneemt, plaatst u deze direct terug in de pool om de kosten te beperken. Als het onduidelijk is wanneer dit gaat gebeuren, kunt u een waarschuwing voor de database instellen die wordt geactiveerd wanneer het DTU-verbruik onder het maximum per database voor de pool zakt. Het verplaatsen van een database naar een pool wordt beschreven in oefening 5.

## <a name="other-performance-management-patterns"></a>Andere patronen voor prestatiebeheer

**Preventief schalen** In oefening 6 waarin u hebt geleerd hoe u een geïsoleerde database kunt schalen, wist u welke database u moest zoeken. Als het management van Contoso Concert Hall WTP op de hoogte had gesteld van de aanstaande kaartverkoop, had de database preventief uit de pool kunnen worden geplaatst. Anders was er waarschijnlijk een waarschuwing voor de pool of de database nodig geweest om te zien wat er aan de hand was. U wilt dit liever niet te weten komen doordat andere tenants in de pool klagen over verminderde prestaties. Als de tenant kan voorspellen hoe lang deze aanvullende resources nodig heeft, kunt u een Azure Automation-runbook instellen om de database uit de pool te plaatsen en later weer terug de plaatsen volgens een ingesteld schema.

**Self-service schalen voor tenants** Omdat schalen een taak is die eenvoudig via de Management-API kan worden aangeroepen, kunt u gemakkelijk de mogelijkheid om tenantdatabases te schalen inbouwen in uw tenantgerichte toepassing en deze als een functie van uw SaaS-service aanbieden. Laat tenants bijvoorbeeld zelf omhoog en omlaag schalen beheren, mogelijk gekoppeld aan hun facturering.

### <a name="scaling-a-pool-up-and-down-on-a-schedule-to-match-usage-patterns"></a>Een pool omhoog en omlaag schalen volgens een schema om aan gebruikspatronen te voldoen

Wanneer het cumulatieve tenantgebruik een voorspelbaar patroon volgt, kunt u Azure Automation gebruiken om een pool volgens een schema omhoog en omlaag te schalen. Schaal een pool bijvoorbeeld omlaag na 18:00 uur en weer omhoog vóór 06:00 op dagen waarvan u weet dat er een afname in de benodigde resources is.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit

[Zelfstudie Een individuele tenant herstellen](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Aanvullende bronnen

* [Aanvullende zelfstudies die voortbouwen op de eerste implementatie van WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastische SQL-pools](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md): zelfstudie Log Analytics instellen en gebruiken
