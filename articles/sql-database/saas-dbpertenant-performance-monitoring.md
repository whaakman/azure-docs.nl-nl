---
title: Bewaking van prestaties van veel Azure SQL-databases in een multitenant SaaS-app | Microsoft Docs
description: Bewaken en beheren van de prestaties van Azure SQL-databases en opslaggroepen in een multitenant SaaS-app
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: sstein
ms.openlocfilehash: 289f1f99b1661e499fa7132887e2f65e086ad689
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-manage-performance-of-azure-sql-databases-and-pools-in-a-multi-tenant-saas-app"></a>Bewaken en beheren van de prestaties van Azure SQL-databases en opslaggroepen in een multitenant SaaS-app

In deze zelfstudie worden de verschillende prestatie scenario's voor beheer in de SaaS-toepassingen gebruikt verkend. Met behulp van een load-generator om te simuleren activiteit voor alle databases van de tenant, worden de ingebouwde bewaking en waarschuwingen van de functies van SQL-Database en elastische pools gedemonstreerd.

De app Wingtip Tickets SaaS Database Per Tenant gebruikmaakt van een één-tenant-gegevensmodel waar elke locatie vast (tenant) hun eigen database heeft. Net als bij veel andere SaaS-toepassingen, is het verwachte tenantworkloadpatroon onvoorspelbaar en sporadisch. Met andere woorden, kaartverkoop kan op ieder moment plaatsvinden. Tenantdatabases worden geïmplementeerd in pools voor Elastic Database om te profiteren van dit typerende patroon voor databasegebruik. Elastische pools verlagen de kosten van een oplossing door resources over veel databases te verdelen. Bij dit type patroon is het belangrijk om het gebruik van database- en poolresources te controleren om ervoor te zorgen dat loads goed over pools zijn verdeeld. Ook moet u ervoor zorgen dat individuele databases voldoende resources hebben en dat pools niet hun [eDTU](sql-database-what-is-a-dtu.md)-limieten overschrijden. In deze zelfstudie worden manieren toegelicht om databases en pools te controleren en beheren. Ook wordt uitgelegd hoe u corrigerende maatregelen kunt nemen als reactie op wisselingen in de workload.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De app Wingtip Tickets SaaS Database Per Tenant wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de toepassing Wingtip Tickets SaaS Database Per Tenant verkennen](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-saas-performance-management-patterns"></a>Inleiding tot SaaS prestaties management patronen

Het beheren van de databaseprestaties bestaat uit het verzamelen en analyseren van prestatiegegevens en het reageren op deze gegevens door parameters aan te passen om een acceptabele reactietijd voor de toepassing te behouden. Wanneer u meerdere tenants host, zijn pools voor Elastic Database een rendabele manier om resources te bieden en te beheren voor een groep databases met onvoorspelbare workloads. Met bepaalde workloadpatronen kan het al bij twee S3-databases voordelig zijn om ze door een pool te laten beheren.

![diagram van de toepassing](./media/saas-dbpertenant-performance-monitoring/app-diagram.png)

Groepen en de databases in groepen, moeten worden gecontroleerd om te controleren of dat deze binnen acceptabele bereik van de prestaties van blijven. De configuratie van toepassingen om te voldoen aan de behoeften van de cumulatieve werkbelasting van alle databases, waarbij u ervoor zorgt dat de groepen met edtu's geschikt voor de algehele werkbelasting zijn optimaliseren. Pas de minimale en maximale eDTU-waarden per database aan naar de geschikte waarden voor de vereisten van uw specifieke toepassing.

### <a name="performance-management-strategies"></a>Strategieën voor prestatiebeheer

* Om te voorkomen dat handmatig prestaties bewaken, is het meest effectief te **waarschuwingen die wordt geactiveerd wanneer databases of groepen buiten het normale adresbereiken afwijken instellen**.
* Het **eDTU-niveau van een pool kan omhoog of omlaag worden geschaald** om te reageren op fluctuaties op de korte termijn in het cumulatieve prestatieniveau van een pool. Als deze fluctuatie regelmatig of op voorspelbare momenten optreedt, **kan het schalen van de pool op automatisch worden ingesteld**. U kunt bijvoorbeeld omlaag schalen wanneer de workload licht is, zoals 's nachts of tijdens het weekend.
* U kunt ook **afzonderlijke databases naar andere pools verplaatsen** bij fluctuaties op de langere termijn of wijzigingen in het aantal databases.
* Om te reageren op korte termijn toename in *afzonderlijke* belasting van de database **afzonderlijke databases kunnen worden genomen buiten een pool en een afzonderlijke prestatieniveau toegewezen**. Wanneer de load is verlaagd, kan de database naar de pool worden teruggezet. Wanneer dit van tevoren bekend, kunnen databases worden verplaatst pre-emptively om te controleren of dat de database heeft altijd de resources die nodig is, om te voorkomen dat gevolgen voor andere databases in de pool. Als het om een voorspelbare vereiste gaat, zoals bij een locatie die met een toename in kaartverkoop te maken krijgt voor een populair evenement, kan dit beheergedrag in de toepassing worden geïntegreerd.

[Azure Portal](https://portal.azure.com) biedt ingebouwde functionaliteit voor bewaking en waarschuwingen voor de meeste resources. Voor SQL Database zijn bewaking en waarschuwingen beschikbaar voor databases en pools. Deze ingebouwde bewaking en waarschuwingen is resource-specifieke, zodat dit is handig voor kleine hoeveelheden van resources, maar is niet erg handig als u werkt met veel resources.

Voor grote hoeveelheden scenario's waarin u met veel resources werkt, [logboekanalyse (OMS)](saas-dbpertenant-log-analytics.md) kan worden gebruikt. Dit is een afzonderlijke Azure-service die in analyse via verzonden diagnostische logboeken en telemetrie verzameld in een log analytics-werkruimte voorziet. Log Analytics telemetriegegevens kan verzamelen van veel services en worden gebruikt om te vragen en waarschuwingen instellen.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De toepassingsscripts Wingtip Tickets SaaS Database Per Tenant ophalen

De scripts Wingtip Tickets SaaS multitenant Database en de broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de scripts Wingtip Tickets SaaS deblokkeren.

## <a name="provision-additional-tenants"></a>Extra tenants inrichten

Hoewel pools al met twee S3-databases rendabel kunnen zijn, geldt dat hoe meer databases er in de pool zitten, hoe rendabeler het berekenen van gemiddelden wordt. U hebt voor deze zelfstudie ten minste 20 geïmplementeerde databases nodig om een goed inzicht te krijgen in de werking van het bewaken en beheren van prestaties op schaal.

Als u al een batch van tenants in een eerdere zelfstudie voorzien, gaat u naar de [simuleren-gebruik op alle databases van de tenant](#simulate-usage-on-all-tenant-databases) sectie.

1. In de **PowerShell ISE**open... \\Learning-Modules\\prestatiebewaking en beheer\\*Demo PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel het volgende in: **$DemoScenario** = **1**, **Batch met tenants inrichten**
1. Druk op **F5** om het script uit te voeren.

Het script implementeert 17 tenants in minder dan vijf minuten.

De *nieuw TenantBatch* script maakt gebruik van een geneste of gekoppelde set [Resource Manager](../azure-resource-manager/index.md) sjablonen die een batch-tenants, die standaard kopieert u de database maken **basetenantdb** op de catalogusserver de nieuwe tenant om databases te maken, klikt u vervolgens registreert deze in de catalogus, en ten slotte als wordt geïnitialiseerd met de tenant naam en u wilt. Dit komt overeen met de manier waarop die de app voorziet in een nieuwe tenant. Wijzigingen in *basetenantdb* worden toegepast op alle nieuwe tenants daarna ingericht. Zie de [Schemabeheer zelfstudie](saas-tenancy-schema-management.md) om te zien hoe u wijzigingen in het schema voor *bestaande* tenant-databases (inclusief de *basetenantdb* database).

## <a name="simulate-usage-on-all-tenant-databases"></a>Gebruik simuleren op alle tenantdatabases

De *Demo PerformanceMonitoringAndManagement.ps1* script is opgegeven dat een werkbelasting die wordt uitgevoerd op alle databases van de tenant simuleert. De belasting wordt gegenereerd met een van de beschikbare load-scenario's:

| Demo | Scenario |
|:--|:--|
| 2 | Belasting met normale intensiteit genereren (ongeveer 40 DTU's) |
| 3 | Load genereren met langere en frequentere bursts per database|
| 4 | Load genereren met meer DTU-bursts per database (ongeveer 80 DTU's)|
| 5 | Een normale load en hoge load genereren op één tenant (ongeveer 95 DTU's)|
| 6 | Niet-gebalanceerde load genereren voor meerdere pools|

De load-generator past een *synthetische* load alleen voor CPU toe op elke tenantdatabase. De generator start voor elke tenantdatabase een taak, die periodiek een opgeslagen procedure aanroept die de load genereert. De load-niveaus (in eDTU's), duur en intervallen zijn verschillend voor alle databases. Dit simuleert onvoorspelbare tenantactiviteit.

1. In de **PowerShell ISE**open... \\Learning-Modules\\prestatiebewaking en beheer\\*Demo PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel **$DemoScenario** = **2**, *genereren normale intensiteit load*.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.

Wingtip Tickets SaaS Database Per Tenant is een SaaS-app en de werkelijke belasting van een SaaS-app is doorgaans sporadisch en onvoorspelbare. Om dit te simuleren, produceert de load-generator een willekeurige workload die over alle tenants wordt verdeeld. Enkele minuten nodig zijn voor de load-patroon te komen, dus het uitvoeren van de load-generator voor 3-5 minuten voordat u probeert om te controleren van de belasting in de volgende secties.

> [!IMPORTANT]
> De load-generator wordt uitgevoerd als een reeks taken in uw lokale PowerShell-sessie. Houd het tabblad *Demo-PerformanceMonitoringAndManagement.ps1* geopend. Als u het tabblad sluit of de computer onderbreekt, stopt de load-generator. De load-generator blijft in een *aanroepen van de taak* status waar belasting van een nieuwe tenants die zijn ingericht genereert nadat de generator is gestart. Gebruik *Ctrl-C* aanroepen van nieuwe taken stoppen en afsluiten van het script. De load-generator wordt nog uitgevoerd, maar alleen op bestaande tenants.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Resourcegebruik monitor met de Azure portal

Open de portal voor de groep met de tenant-databases voor het controleren van het gebruik van bronnen die het resultaat van de belasting die wordt toegepast:

1. Open de [Azure-portal](https://portal.azure.com) en blader naar de *tenants1-dpt -&lt;gebruiker&gt;*  server.
1. Scrol naar beneden, zoek de elastische pools en klik op **Pool1**. Deze pool bevat alle tenantdatabases die tot nu toe zijn gemaakt.

Houd rekening met de **elastische pool bewaking** en **elastische database bewaken** grafieken.

Het Resourcegebruik van de pool is het gebruik van statistische database voor alle databases in de pool. De database-diagram toont de vijf gegevensbeheer databases:

![database-grafiek](./media/saas-dbpertenant-performance-monitoring/pool1.png)

Omdat er meer databases in de groep afgezien van de bovenste vijf, wordt het gebruik van de groep de activiteit die niet wordt weergegeven in de bovenste vijf databases-grafiek. Klik voor meer informatie **Database Resourcegebruik**:

![Resourcegebruik van de database](./media/saas-dbpertenant-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Prestatiewaarschuwingen voor de pool instellen

Een waarschuwing instellen voor de toepassingen die u activeert op \>75% gebruik als volgt:

1. Open *Pool1* (op het *tenants1-dpt -\<gebruiker\>*  server) in de [Azure-portal](https://portal.azure.com).
1. Klik op **Waarschuwingsregels** en klik vervolgens op **+ Waarschuwing toevoegen**:

   ![waarschuwing toevoegen](media/saas-dbpertenant-performance-monitoring/add-alert.png)

1. Geef een naam op, bijvoorbeeld **hoog DTU**.
1. Stel de volgende waarden in:
   * **Metriek = eDTU-percentage**
   * **Conditie = groter zijn dan**
   * **Drempelwaarde = 75**
   * **Period = in de afgelopen 30 minuten**
1. Een e-mailadres toevoegen de *aanvullende beheerder email(s)* vak en klikt u op **OK**.

   ![waarschuwing instellen](media/saas-dbpertenant-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Een zwaar belaste pool omhoog schalen

Als het cumulatieve loadniveau voor een pool in een dergelijke mate toeneemt dat de pool volledig wordt gebruikt en 100% eDTU-gebruik wordt bereikt, worden de prestaties van afzonderlijke databases beïnvloed. Hierdoor wordt de reactietijd voor query's mogelijk vertraagd voor alle databases in de pool.

**Korte termijn**, Overweeg de groep te voorzien in aanvullende resources schaalt of verwijderen van databases uit de groep (verplaatsen naar andere groepen of buiten de groep aan een zelfstandige servicelaag).

**Lange termijn**, u kunt query's of index-gebruik voor het verbeteren van de prestaties van de database. Afhankelijk van de gevoeligheid voor prestatieproblemen van de toepassing, is de aanbevolen procedure om een pool op te schalen voordat 100% eDTU-gebruik wordt bereikt. Gebruik een waarschuwing zodat u tijdig wordt gewaarschuwd.

U kunt een zwaar belaste pool simuleren door de load die de generator produceert, te verhogen. De databases naar burst vaker en voor langer, waardoor de cumulatieve belasting van de groep zonder de vereisten van de afzonderlijke databases verhogen. U kunt de pool eenvoudig opschalen in de portal of vanuit PowerShell. In deze oefening wordt de portal gebruikt.

1. Stel *$DemoScenario* = **3**, _Load genereren met langere en frequentere bursts per database_ in om de intensiteit van de cumulatieve load voor de pool te verhogen zonder de piekbelasting die is vereist voor elke database te wijzigen.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.

1. Ga naar **Pool1** in de Azure portal.

De toegenomen pool-eDTU-gebruik op de bovenste grafiek bewaken. Het duurt enkele minuten duren voordat de nieuwe hogere belasting te starten, maar ziet u snel de toepassingen die beginnen met de maximale capaciteit bereikt, en als de belasting steadies in het nieuwe patroon, wordt de groep snel overloads.

1. Als u wilt schalen van de groep, klikt u op **pool configureren** boven aan de **Pool1** pagina.
1. Pas de **groeps-eDTU** instelt op **100**. Als u de groeps-eDTU wijzigt, wordt de instelling per database niet gewijzigd (deze is nog steeds maximaal 50 eDTU's per database). U kunt de instellingen per database zien aan de rechterkant van de **pool configureren** pagina.
1. Klik op **opslaan** de aanvraag voor het schalen van de groep.

Ga terug naar **Pool1** > **overzicht** om de bewaking grafieken weer te geven. Controleer het effect van het bieden van de groep met meer bronnen (Hoewel u met enkele databases en een willekeurige belasting is het niet altijd duidelijk afdoende totdat u enige tijd uitvoert). Houd er rekening mee dat 100% bij de bovenste grafiek nu staat voor 100 eDTU's, terwijl voor de onderste grafiek 100% nog steeds 50 eDTU's betekent, omdat het maximum per database 50 eDTU's is.

Databases blijven gedurende het proces online en volledig beschikbaar. Op het laatste moment, wanneer elke database gereed is om te worden ingeschakeld met de nieuwe pool-eDTU's, worden alle actieve verbindingen verbroken. Om verbroken verbindingen opnieuw te proberen, moet altijd een toepassingscode worden geschreven zodat er opnieuw verbinding wordt gemaakt met de database in de omhoog geschaalde pool.

## <a name="load-balance-between-pools"></a>Taakverdeling tussen mediagroepen

In plaats van de pool omhoog te schalen, kunt u ook een tweede pool maken en databases hiernaartoe verplaatsen om de load tussen de twee pools te verdelen. Om dit te doen, moet de nieuwe pool op dezelfde server als de eerste pool worden gemaakt.

1. In de [Azure-portal](https://portal.azure.com), open de **tenants1-dpt -&lt;gebruiker&gt;**  server.
1. Klik op **+ nieuwe pool** voor het maken van een groep op de huidige server.
1. Op de **pool voor elastische database** sjabloon:

    1. Stel **naam** naar *Pool2*.
    1. Laat de prijscategorie op **Standaardpool** staan.
    1. Klik op **Groep configureren**.
    1. Stel **groeps-eDTU** naar *50 eDTU*.
    1. Klik op **databases toevoegen** voor een overzicht van databases op de server die kan worden toegevoegd aan *Pool2*.
    1. Selecteer elke 10 databases verplaats deze naar de nieuwe groep en klik vervolgens op **Selecteer**. Als u hebt de load-generator uitgevoerd, wordt de service al weet dat uw profiel prestaties een grotere groep dan de standaardgrootte voor de 50 eDTU vereist en raadt aan om te beginnen met een 100 eDTU-instelling.

    ![Aanbeveling](media/saas-dbpertenant-performance-monitoring/configure-pool.png)

    1. Voor deze zelfstudie laat de standaardwaarde op 50 edtu's en klik op **Selecteer** opnieuw.
    1. Selecteer **OK** om de nieuwe groep te maken en de geselecteerde databases verplaatsen in de App.

Maken van de groep en de databases verplaatsen duurt een paar minuten. Terwijl databases worden verplaatst dat deze online is en toegankelijk blijven totdat het laatste tijdstip, waarna worden alle geopende verbindingen gesloten. Als u een aantal Pogingslogica hebt, clients wordt vervolgens verbinding maken met de database in de nieuwe groep.

Blader naar **Pool2** (op het *tenants1-dpt -\<gebruiker\>*  server) te openen van de groep zijn prestaties controleren. Als u deze niet ziet, wacht u totdat het inrichten van de nieuwe groep te voltooien.

U ziet nu dat Resourcegebruik op *Pool1* is verwijderd en dat *Pool2* nu op dezelfde manier is geladen.

## <a name="manage-performance-of-a-single-database"></a>Prestaties van één database beheren

Als een individuele database in een pool langere tijd een hogere load heeft, kan het de resources in de pool overheersen en andere databases beïnvloeden, afhankelijk van de poolconfiguratie. Als de activiteit kan enige tijd blijven, kan de database tijdelijk worden verplaatst buiten de groep. Hierdoor kan de database om de extra bronnen nodig heeft en geïsoleerd van de andere databases.

Deze oefening simuleert het effect van een hoge load voor Contoso Concert Hall wanneer kaartjes voor een populair concert in de verkoop gaan.

1. In de **PowerShell ISE**, open de... \\ *Demo PerformanceMonitoringAndManagement.ps1* script.
1. Stel het volgende in: **$DemoScenario = 5, Een normale load en hoge load genereren op één tenant (ongeveer 95 DTU's).**
1. Stel **$SingleTenantDatabaseName = contosoconcerthall** in
1. Voer het script uit met **F5**.


1. In de [Azure-portal](https://portal.azure.com), blader naar de lijst met databases op de *tenants1-dpt -\<gebruiker\>*  server. 
1. Klik op de **contosoconcerthall** database.
1. Klik op de groep die **contosoconcerthall** is in. Zoek de pool in de **pool voor elastische database** sectie.

1. Inspecteer de **elastische pool bewaking** grafiek en zoekt u de verbeterde pool-eDTU-gebruik. Na een paar minuten moet de hogere load actief worden en zou de pool snel een gebruik van 100% moeten weergeven.
2. Inspecteer de **elastische database bewaken** weergeven, waarin de populairste databases in het afgelopen uur. De *contosoconcerthall* database moet binnenkort worden weergegeven als een van de vijf gegevensbeheer databases.
3. **Klik op de bewaking van de elastische database** **grafiek** en deze wordt geopend. de **Database Resourcegebruik** pagina waar u een van de databases kunt bewaken. Hiermee kunt u isoleren van de weergave voor de *contosoconcerthall* database.
4. Klik in de lijst met databases op **contosoconcerthall**.
5. Klik op **prijscategorie (schaal dtu's)** openen de **prestaties configureren** pagina waarin u een zelfstandige prestatieniveau voor de database kunt instellen.
6. Klik op het tabblad **Standard** om de schaalopties in de categorie Standard te openen.
7. Schuif de **DTU schuifregelaar** rechts om te selecteren **100** dtu's. Dit komt overeen met de servicedoelstelling Opmerking **S3**.
8. Klik op **toepassen** naar de database uit de groep verplaatst, waardoor het een *Standard-S3* database.
9. Zodra de schaal is voltooid, is het effect op de database contosoconcerthall en Pool1 op de elastische pool en database blades bewaken.

Zodra de hoge belasting van de database contosoconcerthall subsidies retourneer deze aan de groep om de kosten te beperken. Als het is onduidelijk wanneer dat gebeurt u kan een waarschuwing instellen voor de database die wordt geactiveerd wanneer de DTU-gebruik onder per database zakt max op de groep. Het verplaatsen van een database naar een pool wordt beschreven in oefening 5.

## <a name="other-performance-management-patterns"></a>Andere prestaties management patronen

**Preventieve schalen** In de vorige oefening waar u een geïsoleerde database schalen verkend, wist u welke database om te zoeken. Als u het beheer van Contoso overleg Hall had op de hoogte Wingtips van de aanstaande ticket verkoop, kan de database zijn verplaatst buiten de pool pre-emptively. Anders was er waarschijnlijk een waarschuwing voor de pool of de database nodig geweest om te zien wat er aan de hand was. U wilt dit liever niet te weten komen doordat andere tenants in de pool klagen over verminderde prestaties. Als de tenant kan voorspellen hoe lang deze aanvullende resources nodig heeft, kunt u een Azure Automation-runbook instellen om de database uit de pool te plaatsen en later weer terug de plaatsen volgens een ingesteld schema.

**Self-service schalen voor tenants** Omdat schalen een taak is die eenvoudig via de Management-API kan worden aangeroepen, kunt u gemakkelijk de mogelijkheid om tenantdatabases te schalen inbouwen in uw tenantgerichte toepassing en deze als een functie van uw SaaS-service aanbieden. Laat tenants bijvoorbeeld zelf omhoog en omlaag schalen beheren, mogelijk gekoppeld aan hun facturering.

**Een pool schalen omhoog en omlaag volgens een schema moet worden gezocht gebruikspatronen**

Wanneer het cumulatieve tenantgebruik een voorspelbaar patroon volgt, kunt u Azure Automation gebruiken om een pool volgens een schema omhoog en omlaag te schalen. Schaal een pool bijvoorbeeld omlaag na 18:00 uur en weer omhoog vóór 06:00 op dagen waarvan u weet dat er een afname in de benodigde resources is.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit

[Zelfstudie Een individuele tenant herstellen](saas-dbpertenant-restore-single-tenant.md)


## <a name="additional-resources"></a>Aanvullende bronnen

* Aanvullende [zelfstudies waarin voort op de toepassingsimplementatie Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische SQL-pools](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](saas-dbpertenant-log-analytics.md): zelfstudie Log Analytics instellen en gebruiken
