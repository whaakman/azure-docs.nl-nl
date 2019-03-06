---
title: 'SaaS-app: Prestaties bewaken van veel Azure SQL-databases | Microsoft Docs'
description: Prestaties van Azure SQL-databases en pools in een multitenant SaaS-app controleren en beheren
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
ms.openlocfilehash: df54f9dd4047fffb578a1a95a2edc47cba711ba1
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433515"
---
# <a name="monitor-and-manage-performance-of-azure-sql-databases-and-pools-in-a-multi-tenant-saas-app"></a>Prestaties van Azure SQL-databases en pools in een multitenant SaaS-app controleren en beheren

In deze zelfstudie worden verschillende belangrijke scenario's die worden gebruikt in SaaS-toepassingen verkend. Met behulp van een load-generator voor het simuleren van activiteit in alle tenantdatabases, worden de ingebouwde bewaking en waarschuwingen van de functies van SQL-Database en elastische pools toegelicht.

De Wingtip Tickets SaaS Database Per Tenant-app gebruikt een gegevensmodel van één tenant, waarbij elke venue (tenant) een eigen database heeft. Net als bij veel andere SaaS-toepassingen, is het verwachte tenantworkloadpatroon onvoorspelbaar en sporadisch. Met andere woorden, kaartverkoop kan op ieder moment plaatsvinden. Als u wilt profiteren van dit typerende patroon, worden tenantdatabases geïmplementeerd in elastische pools. Elastische pools verlagen de kosten van een oplossing door resources over veel databases te verdelen. Bij dit type patroon is het belangrijk om het gebruik van database- en poolresources te controleren om ervoor te zorgen dat loads goed over pools zijn verdeeld. Ook moet u ervoor zorgen dat individuele databases voldoende resources hebben en dat pools niet hun [eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model)-limieten overschrijden. In deze zelfstudie worden manieren toegelicht om databases en pools te controleren en beheren. Ook wordt uitgelegd hoe u corrigerende maatregelen kunt nemen als reactie op wisselingen in de workload.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]

> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit


U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* De Wingtip Tickets SaaS Database Per Tenant-app is geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de toepassing Wingtip Tickets SaaS Database Per Tenant](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-saas-performance-management-patterns"></a>Kennismaking met prestatiebeheerpatronen voor SaaS

Het beheren van de databaseprestaties bestaat uit het verzamelen en analyseren van prestatiegegevens en het reageren op deze gegevens door parameters aan te passen om een acceptabele reactietijd voor de toepassing te behouden. Bij het hosten van meerdere tenants, zijn elastische pools een kosteneffectieve manier om te leveren en beheren van resources voor een groep databases met onvoorspelbare workloads. Met bepaalde workloadpatronen kan het al bij twee S3-databases voordelig zijn om ze door een pool te laten beheren.

![Een diagram van toepassing](./media/saas-dbpertenant-performance-monitoring/app-diagram.png)

Pools en de databases in pools moeten worden gecontroleerd om ervoor te zorgen dat ze binnen een acceptabel prestatiebereik blijven. De configuratie van de pool om te voldoen aan de behoeften van de cumulatieve workload van alle databases, ervoor te zorgen dat de groep edtu's geschikt is voor de algemene workload zijn af te stemmen. Pas de minimale en maximale eDTU-waarden per database aan naar de geschikte waarden voor de vereisten van uw specifieke toepassing.

### <a name="performance-management-strategies"></a>Strategieën voor prestatiebeheer

* Om te voorkomen dat handmatig prestaties moet bewaken, is het meest effectief aan **waarschuwingen instellen die wordt geactiveerd wanneer databases of pools normale bereik effectiefst**.
* Om te reageren op fluctuaties op korte termijn in het aggregaat compute-grootte van een groep, de **eDTU-niveau kunt omhoog of omlaag worden geschaald**. Als deze fluctuatie regelmatig of op voorspelbare momenten optreedt, **kan het schalen van de pool op automatisch worden ingesteld**. U kunt bijvoorbeeld omlaag schalen wanneer de workload licht is, zoals 's nachts of tijdens het weekend.
* U kunt ook **afzonderlijke databases naar andere pools verplaatsen** bij fluctuaties op de langere termijn of wijzigingen in het aantal databases.
* Om te reageren op korte termijn verhogingen in *afzonderlijke* belasting van de database **afzonderlijke databases kunnen worden genomen uit een pool en de grootte van een afzonderlijke computerknooppunten toegewezen**. Wanneer de load is verlaagd, kan de database naar de pool worden teruggezet. Wanneer dit vooraf bekend is, wordt databases kunnen worden voorzorg verplaatst om te controleren of dat de database heeft altijd de benodigde resources beschikken en om te voorkomen dat gevolgen voor andere databases in de groep. Als het om een voorspelbare vereiste gaat, zoals bij een locatie die met een toename in kaartverkoop te maken krijgt voor een populair evenement, kan dit beheergedrag in de toepassing worden geïntegreerd.

[Azure Portal](https://portal.azure.com) biedt ingebouwde functionaliteit voor bewaking en waarschuwingen voor de meeste resources. Voor SQL Database zijn bewaking en waarschuwingen beschikbaar voor databases en pools. Deze ingebouwde bewaking en waarschuwingen is resource-specifieke, dus dit is handig voor kleine aantallen resources, maar is niet erg handig als u werkt met veel resources.

Voor scenario's met hoge volumes, waarin u met veel resources werkt, [logboeken van Azure Monitor](saas-dbpertenant-log-analytics.md) kan worden gebruikt. Dit is een afzonderlijke Azure-service die analyse van verzonden diagnostische logboeken en telemetrie verzamelen in Log Analytics-werkruimte biedt. Logboeken in Azure Monitor kunnen telemetrische gegevens van veel services en worden gebruikt om te zoeken en waarschuwingen instellen.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>De scripts van de toepassing Wingtip Tickets SaaS Database Per Tenant ophalen

De Wingtip Tickets SaaS multitenant-Database-scripts en broncode van toepassing zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets SaaS-scripts.

## <a name="provision-additional-tenants"></a>Extra tenants inrichten

Hoewel pools al met twee S3-databases rendabel kunnen zijn, geldt dat hoe meer databases er in de pool zitten, hoe rendabeler het berekenen van gemiddelden wordt. U hebt voor deze zelfstudie ten minste 20 geïmplementeerde databases nodig om een goed inzicht te krijgen in de werking van het bewaken en beheren van prestaties op schaal.

Als u al een batch met tenants in een eerdere zelfstudie ingericht, gaat u naar de [gebruik simuleren op alle tenantdatabases](#simulate-usage-on-all-tenant-databases) sectie.

1. In de **PowerShell ISE**, openen... \\Learning Modules\\Performance Monitoring and Management\\*Demo-PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel het volgende in: **$DemoScenario** = **1**, **Batch met tenants inrichten**
1. Druk op **F5** om het script uit te voeren.

Het script implementeert 17 tenants in minder dan vijf minuten.

De *New-TenantBatch* script maakt gebruik van een geneste of gekoppelde set [Resource Manager](../azure-resource-manager/index.yml) sjablonen die een batch met tenants, wat standaard kopieert u de database maakt **basetenantdb**op de catalogusserver om databases voor de nieuwe tenant maken, klikt u vervolgens worden deze geregistreerd in de catalogus en ten slotte geïnitialiseerd met de tenant en het locatietype type. Dit komt overeen met de manier waarop die de app een nieuwe tenant inricht. Alle wijzigingen aan *basetenantdb* worden toegepast op alle nieuwe tenants ingericht. Zie de [Schemabeheer zelfstudie](saas-tenancy-schema-management.md) voor informatie over het aanbrengen van schemawijzigingen naar *bestaande* tenant-databases (met inbegrip van de *basetenantdb* database).

## <a name="simulate-usage-on-all-tenant-databases"></a>Gebruik simuleren op alle tenantdatabases

De *Demo-PerformanceMonitoringAndManagement.ps1* script is opgegeven dat een werkbelasting die wordt uitgevoerd op alle tenantdatabases simuleert. De belasting wordt gegenereerd met behulp van een van de beschikbare load-scenario's:

| Demo | Scenario |
|:--|:--|
| 2 | Genereren van belasting met normale intensiteit (ongeveer 40 dtu's) |
| 3 | Load genereren met langere en frequentere bursts per database|
| 4 | Load genereren met meer DTU-bursts per database (ongeveer 80 dtu's)|
| 5 | Genereren van een normale load en hoge load op één tenant (ongeveer 95 dtu's)|
| 6 | Niet-gebalanceerde load genereren voor meerdere pools|

De load-generator past een *synthetische* load alleen voor CPU toe op elke tenantdatabase. De generator start voor elke tenantdatabase een taak, die periodiek een opgeslagen procedure aanroept die de load genereert. De load-niveaus (in eDTU's), duur en intervallen zijn verschillend voor alle databases. Dit simuleert onvoorspelbare tenantactiviteit.

1. In de **PowerShell ISE**, openen... \\Learning Modules\\Performance Monitoring and Management\\*Demo-PerformanceMonitoringAndManagement.ps1*. Houd dit script open; u gaat verschillende scenario's uitvoeren tijdens deze zelfstudie.
1. Stel **$DemoScenario** = **2**, *belasting met normale intensiteit genereren*.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.

Wingtip Tickets SaaS Database Per Tenant is een SaaS-app en de werkelijke belasting van een SaaS-app is meestal sporadisch en onvoorspelbaar. Om dit te simuleren, produceert de load-generator een willekeurige workload die over alle tenants wordt verdeeld. Enkele minuten nodig zijn voor het patroon load te geven, dus de load-generator voor 3 tot 5 minuten voordat u de workload in de volgende secties worden uitgevoerd.

> [!IMPORTANT]
> De load-generator wordt uitgevoerd als een reeks taken in uw lokale PowerShell-sessie. Houd het tabblad *Demo-PerformanceMonitoringAndManagement.ps1* geopend. Als u het tabblad sluit of de computer onderbreekt, stopt de load-generator. De load-generator blijft in een *aanroepen van de taak* staat waarin deze belasting op alle nieuwe tenants die zijn ingericht genereert na de generator is gestart. Gebruik *Ctrl-C* te stoppen met het aanroepen van nieuwe projecten en het script af te sluiten. De load-generator blijft om uit te voeren, maar alleen op bestaande tenants.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Gebruik van resources met behulp van de Azure-portal bewaken

Open de portal aan de groep die de tenantdatabases bevat voor het controleren van het gebruik van bronnen die het resultaat is van de belasting wordt toegepast:

1. Open de [Azure-portal](https://portal.azure.com) en blader naar de *tenants1-dpt -&lt;gebruiker&gt;*  server.
1. Scrol naar beneden, zoek de elastische pools en klik op **Pool1**. Deze pool bevat alle tenantdatabases die tot nu toe zijn gemaakt.

Bekijk de **elastische groep controleren** en **Elastic database controleren** grafieken.

Het Resourcegebruik van de pool is het cumulatieve Databasegebruik voor alle databases in de groep. De database-grafiek toont de vijf meest belaste databases:

![database-grafiek](./media/saas-dbpertenant-performance-monitoring/pool1.png)

Omdat er andere databases in de groep dan de top vijf, toont het poolgebruik activiteit die niet wordt weergegeven in de bovenste vijf-databases van grafiek. Voor meer informatie klikt u op **databasebron**:

![databasebron](./media/saas-dbpertenant-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Prestatiewaarschuwingen voor de pool instellen

Een melding instellen op de pool die wordt geactiveerd op \>75% gebruik als volgt te werk:

1. Open *Pool1* (op de *tenants1-dpt -\<gebruiker\>*  server) in de [Azure-portal](https://portal.azure.com).
1. Klik op **Waarschuwingsregels** en klik vervolgens op **+ Waarschuwing toevoegen**:

   ![waarschuwing toevoegen](media/saas-dbpertenant-performance-monitoring/add-alert.png)

1. Geef een naam op, bijvoorbeeld **hoog DTU**.
1. Stel de volgende waarden in:
   * **Metriek = eDTU-percentage**
   * **Voorwaarde = groter dan**
   * **Drempel = 75**
   * **Periode = in de afgelopen 30 minuten**
1. Een e-mailadres toevoegen de *beheerder email(s)* vak en klikt u op **OK**.

   ![waarschuwing instellen](media/saas-dbpertenant-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Een zwaar belaste pool omhoog schalen

Als het cumulatieve loadniveau voor een pool in een dergelijke mate toeneemt dat de pool volledig wordt gebruikt en 100% eDTU-gebruik wordt bereikt, worden de prestaties van afzonderlijke databases beïnvloed. Hierdoor wordt de reactietijd voor query's mogelijk vertraagd voor alle databases in de pool.

**Op korte termijn**, kunt u omhoog schalen van de groep om aanvullende resources te leveren of databases verwijderen uit de pool (verplaatsen naar andere pools of uit de pool naar een zelfstandige servicelaag).

**Langere termijn**, u kunt query's of indexgebruik voor betere databaseprestaties. Afhankelijk van de gevoeligheid voor prestatieproblemen van de toepassing, is de aanbevolen procedure om een pool op te schalen voordat 100% eDTU-gebruik wordt bereikt. Gebruik een waarschuwing zodat u tijdig wordt gewaarschuwd.

U kunt een zwaar belaste pool simuleren door de load die de generator produceert, te verhogen. Doordat de databases vaker en voor meer het geval is, burst verhogen van de cumulatieve load voor de pool zonder te hoeven wijzigen van de vereisten van de afzonderlijke databases. U kunt de pool eenvoudig opschalen in de portal of vanuit PowerShell. In deze oefening wordt de portal gebruikt.

1. Stel *$DemoScenario* = **3**, _Load genereren met langere en frequentere bursts per database_ in om de intensiteit van de cumulatieve load voor de pool te verhogen zonder de piekbelasting die is vereist voor elke database te wijzigen.
1. Druk op **F5** om een load toe te passen op al uw tenantdatabases.

1. Ga naar **Pool1** in Azure portal.

Controleer de hogere groep eDTU-gebruik op de bovenste grafiek. Het duurt een paar minuten voor het nieuwe hogere load wordt geactiveerd, maar ziet u snel de toepassingen die beginnen met het maximale capaciteit bereikt en als de belasting vrij in het nieuwe patroon, wordt de pool snel overbelast.

1. Als u wilt de pool opschalen, klikt u op **groep configureren** aan de bovenkant van de **Pool1** pagina.
1. Pas de **groeps-eDTU** instelt op **100**. Als u de groeps-eDTU wijzigt, wordt de instelling per database niet gewijzigd (deze is nog steeds maximaal 50 eDTU's per database). U kunt de instellingen per database zien aan de rechterkant van de **groep configureren** pagina.
1. Klik op **opslaan** dien de aanvraag voor het schalen van de groep.

Ga terug naar **Pool1** > **overzicht** om de bewakingsgrafieken weer te geven. Controleer het effect van het leveren van de groep met meer bronnen (hoewel enkele databases en een willekeurige workload is het niet altijd gemakkelijk afdoende zien totdat u voor enige tijd uitvoert). Houd er rekening mee dat 100% bij de bovenste grafiek nu staat voor 100 eDTU's, terwijl voor de onderste grafiek 100% nog steeds 50 eDTU's betekent, omdat het maximum per database 50 eDTU's is.

Databases blijven gedurende het proces online en volledig beschikbaar. Op het laatste moment, wanneer elke database gereed is om te worden ingeschakeld met de nieuwe pool-eDTU's, worden alle actieve verbindingen verbroken. Om verbroken verbindingen opnieuw te proberen, moet altijd een toepassingscode worden geschreven zodat er opnieuw verbinding wordt gemaakt met de database in de omhoog geschaalde pool.

## <a name="load-balance-between-pools"></a>Taakverdeling tussen pools

In plaats van de pool omhoog te schalen, kunt u ook een tweede pool maken en databases hiernaartoe verplaatsen om de load tussen de twee pools te verdelen. Om dit te doen, moet de nieuwe pool op dezelfde server als de eerste pool worden gemaakt.

1. In de [Azure-portal](https://portal.azure.com), open de **tenants1-dpt -&lt;gebruiker&gt;**  server.
1. Klik op **+ nieuwe pool** een groep maken op de huidige server.
1. Op de **elastische pool** sjabloon:

    1. Stel **naam** naar *Pool2*.
    1. Laat de prijscategorie op **Standaardpool** staan.
    1. Klik op **Groep configureren**.
    1. Stel **groeps-eDTU** naar *50 eDTU*.
    1. Klik op **databases toevoegen** voor een overzicht van databases op de server die kan worden toegevoegd aan *Pool2*.
    1. Selecteer elke 10 databases die deze verplaatsen naar de nieuwe groep en klik vervolgens op **Selecteer**. Als u de load-generator uitvoert hebt, wordt de service al weet dat uw prestatieprofiel een grotere groep dan de standaardgrootte voor de 50 edtu's vereist en raadt aan te beginnen met een 100 eDTU-instelling.

    ![aanbeveling](media/saas-dbpertenant-performance-monitoring/configure-pool.png)

    1. Voor deze zelfstudie laat u de standaard op 50 edtu's en klikt u op **Selecteer** opnieuw.
    1. Selecteer **OK** om de nieuwe pool te maken en aan de geselecteerde databases hiernaartoe verplaatsen.

Het maken van de groep en verplaatsen van de databases duurt een paar minuten. Als u databases worden verplaatst dat ze online en volledig toegankelijk blijven totdat het laatste moment, waarna worden alle open verbindingen gesloten. Als u bepaalde logica voor opnieuw proberen hebt, wordt vervolgens clients verbinding met de database in de nieuwe pool.

Blader naar **Pool2** (op de *tenants1-dpt -\<gebruiker\>*  server) om te openen van de groep en zijn prestaties controleren. Als u dit niet ziet, wacht u voor het inrichten van de nieuwe pool te voltooien.

U ziet nu dat het gebruik van *Pool1* is verwijderd en dat *Pool2* nu op dezelfde manier wordt geladen.

## <a name="manage-performance-of-an-individual-database"></a>Prestaties van een individuele database beheren

Als een afzonderlijke database in een pool optreedt in een duurzame hoge belasting, afhankelijk van de configuratie van de pool, meestal het de resources in de pool overheersen en andere databases beïnvloeden. Als de activiteit enige tijd voortduurt is, kan de database tijdelijk worden verplaatst uit de pool. Hiermee wordt de database beschikken over de extra resources nodig heeft en worden geïsoleerd van de andere databases.

Deze oefening simuleert het effect van een hoge load voor Contoso Concert Hall wanneer kaartjes voor een populair concert in de verkoop gaan.

1. In de **PowerShell ISE**, open de... \\ *Demo-PerformanceMonitoringAndManagement.ps1* script.
1. Stel **$DemoScenario = 5, genereren een normale load en hoge load op één tenant (ongeveer 95 dtu's).**
1. Stel **$SingleTenantDatabaseName = contosoconcerthall** in
1. Voer het script uit met **F5**.


1. In de [Azure-portal](https://portal.azure.com), blader naar de lijst met databases op de *tenants1-dpt -\<gebruiker\>*  server. 
1. Klik op de **contosoconcerthall** database.
1. Klik op de groep die **contosoconcerthall** is in. Zoek de groep in de **elastische pool** sectie.

1. Inspecteer de **elastische groep controleren** grafiek en zoekt u de hogere groep eDTU-gebruik. Na een paar minuten moet de hogere load actief worden en zou de pool snel een gebruik van 100% moeten weergeven.
2. Inspecteer de **Elastic database controleren** weergeven, waarin de meest belaste databases in het afgelopen uur. De *contosoconcerthall* database moet binnenkort worden weergegeven als een van de vijf meest belaste databases.
3. **Klik op de Elastic database controleren** **grafiek** en het wordt geopend de **databasebron** pagina waar u een van de databases kunt bewaken. Hiermee kunt u isoleren van de weergave voor de *contosoconcerthall* database.
4. Klik in de lijst met databases op **contosoconcerthall**.
5. Klik op **prijscategorie (dtu's schalen)** openen de **prestatie configureren** pagina waar u een zelfstandige compute-grootte voor de database kunt instellen.
6. Klik op het tabblad **Standard** om de schaalopties in de categorie Standard te openen.
7. Schuif de **DTU schuifregelaar** rechts om te selecteren **100** dtu's. Dit komt overeen met de servicedoelstelling **S3**.
8. Klik op **toepassen** de database uit de pool verplaatsen en kunt u een *Standard S3* database.
9. Wanneer schalen voltooid is, is het effect op de database contosoconcerthall en Pool1 op de elastische pool en database-blades bewaken.

Zodra de hoge belasting van de database contosoconcerthall afneemt retourneer het aan de groep om de kosten te beperken. Als het niet duidelijk is wanneer dit gaat gebeuren, u kunt een melding instellen op de database die wordt geactiveerd wanneer het DTU-verbruik onder de per-database komt max in de pool. Het verplaatsen van een database naar een pool wordt beschreven in oefening 5.

## <a name="other-performance-management-patterns"></a>Andere patronen voor prestatiebeheer

**Preventief schalen** In de vorige oefening waar u het schalen van een geïsoleerde database verkend, wist u welke database om te zoeken. Als het beheer van Contoso Concert Hall hoogte had gesteld Wingtips van de aanstaande kaartverkoop, kan de database zijn verplaatst uit de pool voorzorg. Anders was er waarschijnlijk een waarschuwing voor de pool of de database nodig geweest om te zien wat er aan de hand was. U wilt dit liever niet te weten komen doordat andere tenants in de pool klagen over verminderde prestaties. Als de tenant kan voorspellen hoe lang deze aanvullende resources nodig heeft, kunt u een Azure Automation-runbook instellen om de database uit de pool te plaatsen en later weer terug de plaatsen volgens een ingesteld schema.

**Self-service schalen voor tenants** Omdat schalen een taak is die eenvoudig via de Management-API kan worden aangeroepen, kunt u gemakkelijk de mogelijkheid om tenantdatabases te schalen inbouwen in uw tenantgerichte toepassing en deze als een functie van uw SaaS-service aanbieden. Laat tenants bijvoorbeeld zelf omhoog en omlaag schalen beheren, mogelijk gekoppeld aan hun facturering.

**Een pool schalen omhoog en omlaag volgens een schema aan gebruikspatronen**

Wanneer het cumulatieve tenantgebruik een voorspelbaar patroon volgt, kunt u Azure Automation gebruiken om een pool volgens een schema omhoog en omlaag te schalen. Schaal een pool bijvoorbeeld omlaag na 18:00 uur en weer omhoog vóór 06:00 op dagen waarvan u weet dat er een afname in de benodigde resources is.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik op de tenantdatabases simuleren door het uitvoeren van een geleverde load-generator
> * De tenant-databases bewaken terwijl ze reageren op de toename in load
> * Omhoog schalen van de elastische pool als reactie op de verhoogde databaseload
> * Een tweede elastische pool inrichten om taken te verdelen voor de databaseactiviteit

[Zelfstudie Een individuele tenant herstellen](saas-dbpertenant-restore-single-tenant.md)


## <a name="additional-resources"></a>Aanvullende resources

* Aanvullende [zelfstudies voort op de implementatie van de toepassing Wingtip Tickets SaaS Database Per Tenant bouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische SQL-pools](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Logboeken in Azure Monitor](saas-dbpertenant-log-analytics.md) - instelling logboeken zelfstudie voor Azure Monitor instellen en gebruiken
