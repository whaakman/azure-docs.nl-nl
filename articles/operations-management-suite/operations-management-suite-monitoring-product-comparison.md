---
title: Microsoft monitoring-vergelijking | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) is van Microsoft-cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur.  In dit artikel worden de verschillende services van OMS beschreven en vindt u koppelingen naar gedetailleerde inhoud.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: a63ca0ad-61f8-425d-a48c-d87ba518c104
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
ms.openlocfilehash: b4201f105a87b0a41059c061eb37fb35d4514e02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-monitoring-product-comparison"></a>Microsoft monitoring-vergelijking
Dit artikel bevat een vergelijking tussen System Center Operations Manager (SCOM) en Log Analytics in Operations Management Suite (OMS) in termen van hun architectuur, de logica van hoe ze resources bewaken en hoe ze analyse van de gegevens die zij verzamelen uitvoeren.  Dit is om u te bieden een fundamenteel begrip van hun verschillen en relatieve sterke.  

## <a name="basic-architecture"></a>Basic-architectuur
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Alle SCOM-onderdelen worden geïnstalleerd in uw datacenter.  [Agents zijn geïnstalleerd](http://technet.microsoft.com/library/hh551142.aspx) op Windows- en Linux-machines die worden beheerd door SCOM.  Agents die verbinding maken met [beheerservers](https://technet.microsoft.com/library/hh301922.aspx) die communiceren met de SCOM-database en het datawarehouse.  Agents zijn afhankelijk van domeinverificatie verbinding maken met beheerservers.  Die buiten een vertrouwd domein kunnen verificatie uitvoeren of verbinding maken met een [gatewayserver](https://technet.microsoft.com/library/hh212823.aspx).

SCOM vereist twee SQL-databases, één voor de operationele gegevens en een andere datawarehouse ter ondersteuning van rapportages en analyses.  Een [Reporting Server](https://technet.microsoft.com/library/hh298611.aspx) SQL Reporting Services voor het rapporteren van gegevens uit het datawarehouse wordt uitgevoerd. 

SCOM kunt bewaken met behulp van management packs voor producten zoals cloudresources [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708), en [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Deze management packs gebruiken een of meer lokale agents als proxy voor detecterende cloudresources en actieve werkstromen voor het meten van de prestaties en beschikbaarheid.  Proxy-agents worden ook gebruikt om [netwerkapparaten bewaken](https://technet.microsoft.com/library/hh212935.aspx) en andere externe bronnen.

De Operations-Console is een Windows-toepassing die is verbonden met een van de beheerservers en kan de beheerder om te bekijken en analyseren van verzamelde gegevens en configureert u de SCOM-omgeving.  Een webconsole kan worden gehost op een IIS-server en analyse van gegevens via een browser biedt.

![SCOM-architectuur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics
De meeste OMS-onderdelen zijn in de Azure-cloud, zodat u kunt implementeren en met minimale kosten en administratieve werk beheren.  Alle gegevens die worden verzameld door Log Analytics wordt opgeslagen in de OMS-opslagplaats.

Log Analytics kunt verzamelen van gegevens uit een van drie bronnen:

* Fysieke en virtuele machines waarop Windows wordt uitgevoerd en de [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) - of Linux en de [Operations Management Suite-Agent voor Linux](https://technet.microsoft.com/library/mt622052.aspx).  Deze machines kunnen worden on-premises of virtuele machines in Azure of een andere cloud.
* Een Azure Storage-account met [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) gegevens verzameld door de functie Azure worker Webrol of virtuele machine.
* [Verbinding met een SCOM-beheergroep](https://technet.microsoft.com/library/mt484104.aspx).  In deze configuratie communiceren de agents met SCOM-beheerservers die ervoor zorgen dat de gegevens de SCOM-database waar deze vervolgens in het gegevensarchief OMS is geleverd.
  Beheerders verzamelde gegevens te analyseren en Log Analytics configureren met de OMS-portal toegankelijk vanuit elke browser die wordt gehost in Azure.  Mobiele apps te krijgen tot deze gegevens zijn beschikbaar voor de standaard-platforms.

![Log Analytics-architectuur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integratie van SCOM en Log Analytics
Wanneer SCOM wordt gebruikt als gegevensbron voor Log Analytics kunt u gebruikmaken van de functies van beide producten in een hybride omgeving bewaken.  U kunt bestaande SCOM-agents via de Operations-Console kan worden beheerd door OMS, naast actief blijft management packs uit SCOM configureren.  
Gegevens van een verbonden SCOM-beheergroep wordt geleverd met logboekanalyse met een van de vier methoden:

* Gebeurtenissen en prestatiegegevens worden verzameld door de agent en geleverd aan SCOM.  Beheerservers in SCOM levert vervolgens de gegevens met logboekanalyse.
* Sommige gebeurtenissen zoals IIS-logboeken en beveiligingsgebeurtenissen blijven van de agent rechtstreeks aan logboekanalyse worden geleverd.
* Sommige oplossingen wordt extra software te leveren aan de agent of vereisen dat de software worden geïnstalleerd om aanvullende gegevens te verzamelen.  Deze gegevens worden meestal verzonden rechtstreeks met logboekanalyse.
* Sommige oplossingen worden gegevens verzameld rechtstreeks vanuit de SCOM-beheerservers die niet afkomstig is van de agent.  Bijvoorbeeld, de [waarschuwing beheeroplossing](https://technet.microsoft.com/library/mt484092.aspx) waarschuwingen verzamelt uit SCOM nadat ze zijn gemaakt.

## <a name="monitoring-logic"></a>Bewakingslogica
SCOM en Log Analytics werkt met vergelijkbare gegevens verzameld van agents maar fundamentele verschillen in hoe ze definiëren en implementeren van de logica voor het verzamelen van gegevens en hoe ze de gegevens die ze verzamelen analyseren hebben.

### <a name="operations-manager"></a>Operations Manager
Monitoringlogica voor SCOM is geïmplementeerd in [beheerpakketten](https://technet.microsoft.com/library/hh457558.aspx) die logica voor het detecteren van onderdelen moeten worden bewaakt, bevat het meten van de status van deze onderdelen en voor het verzamelen van gegevens te analyseren.  Het bewaken van gegevens kan net zo eenvoudig als het verzamelen van een gebeurtenis of prestatiemeteritem, of deze complexe logica geïmplementeerd in een script kan gebruiken.  Management packs met volledige controle zijn beschikbaar voor een verscheidenheid aan [toepassingen van Microsoft en derden](http://go.microsoft.com/fwlink/?LinkId=82105) naast hardware- en apparaten.  U kunt [ontwerpen van uw eigen management packs](http://aka.ms/mpauthor) voor aangepaste toepassingen.

Management packs bevatten meerdere werkstromen die elk een afzonderlijke bewaking functie zoals steekproef van een prestatiemeteritem, de status van een service te controleren of een script uitvoeren uitvoert.  Elke werkstroom wordt onafhankelijk uitgevoerd en definieert een eigen zoals welke database worden geschreven naar resultaten en of er een waarschuwing wordt gegenereerd. 

U kunt details van de werkstroom zoals de frequentie die ze worden uitgevoerd, de drempelwaarde wanneer ze van mening zijn een fout opgetreden en de ernst van de waarschuwing die zij genereren overschrijven.  U kunt ook aanvullende functionaliteit geven door uw eigen werkstromen toe te voegen.

![Onderdrukkingen](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management packs zijn geïnstalleerd in de Operations Manager-database en wordt automatisch gedistribueerd naar agents door beheerservers.  Elke agent wordt automatisch management packs downloaden en laden van werkstromen die relevant zijn voor de toepassingen die ze hebben geïnstalleerd.  Gegevens die worden verzameld door de agent wordt doorgegeven naar de beheerserver voor invoegen in de SCOM-database en het datawarehouse.  De Operations-Console kunt u weergeven en analyseren van deze gegevens via aangepaste weergaven, dashboards en rapporten die zijn opgenomen in het management pack.

De verdeling van management packs wordt in het volgende diagram geïllustreerd.

![Management pack-stroom](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Gebeurtenis- en Prestatieverzameling
Log Analytics verzamelt de gebeurtenissen en prestatiemeteritems van agent-systemen bronnen zoals Windows-gebeurtenislogboek, IIS-logboeken en Syslog.  U kunt criteria waarvoor gegevens worden verzameld via de portal Log Analytics en maak vervolgens logboek query's voor het analyseren van de verzamelde gegevens definiëren.  Een reeks standaard criteria wordt gedefinieerd wanneer de OMS-werkruimte is gemaakt en u kunt extra gegevens voor specifieke toepassingen definiëren. 

![Het definiëren van gebeurtenislogboeken in Log Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Terwijl SCOM veel gedetailleerde werkstromen die meestal specifieke criteria definiëren voor gegevens en de actie die moet worden uitgevoerd in het antwoord heeft, heeft logboekanalyse meer algemene criteria voor het verzamelen van gegevens.  Logboek query's en oplossingen bieden meer gerichte criteria voor analyseren en stelt op specifieke gegevens in de cloud nadat deze is verzameld.

#### <a name="solutions"></a>Oplossingen
Oplossingen bieden aanvullende logica voor gegevens verzamelen en analyseren.  U kunt oplossingen om toe te voegen aan uw abonnement OMS uit de galerie oplossing selecteren.

![Galerie met oplossingen](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Oplossingen wordt voornamelijk uitgevoerd in de cloud biedt analyse van gebeurtenissen en prestatiemeteritems die worden verzameld in de OMS-opslagplaats.  Ze bepalen ook aanvullende gegevens moeten worden verzameld die kunnen worden geanalyseerd met logboek query's of door aanvullende gebruikersinterface van de oplossing in de OMS-dashboard. 

Bijvoorbeeld, de [oplossing bijhouden](https://technet.microsoft.com/library/mt484099.aspx) detecteert configuratie wijzigingen op de agent-systemen en schrijft gebeurtenissen in de OMS-opslagplaats die kunnen worden geanalyseerd met verschillende grafische weergaven om samen te vatten wijzigingen gedetecteerd.  U kunt inzoomen van de samengevatte weergave in het logboek-query's die de gedetailleerde gegevens verzameld door de oplossing weergeven.

U kunt selecteren welke oplossingen die u aan uw abonnement toevoegt, hebt u geen momenteel op de mogelijkheid om uw eigen oplossingen te maken.  U kunt de gebeurtenissen en prestatiemeteritems voor het verzamelen en aangepaste weergaven op basis van uw eigen logboek query's maken.

De bewakingslogica voor logboekanalyse wordt samengevat in het volgende diagram.

![Log Analytics-oplossing stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Statuscontrole
### <a name="operations-manager"></a>Operations Manager
SCOM kunt model van de verschillende onderdelen van een toepassing en een realtime status bieden voor elk.  Hiermee kunt u niet alleen fouten weergeven die zijn gedetecteerd en prestaties gedurende een bepaalde periode, maar ook om te valideren van de huidige status van een toepassing of het systeem en elk van de onderdelen op een bepaald moment.  Omdat wordt begrepen de perioden die een toepassing beschikbaar is, ondersteunt de health-engine in SCOM ook Service Level Agreements (SLA) die analyseren en rapporteren over de beschikbaarheid van een toepassing gedurende een bepaalde periode.

De weergave hieronder ziet u bijvoorbeeld de real-time status van SQL database-engines bewaakt door SCOM.  De status van elk van de databases voor een van de database-engines wordt weergegeven op de onderste helft van de weergave.

![Statusweergave](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

De Health Explorer voor een van de database-engines worden hieronder weergegeven met de monitors die worden gebruikt om de algehele status te bepalen.  Deze monitors zijn gedefinieerd in het management pack voor SQL en uitvoeren op alle SQL database-engines door SCOM gedetecteerd.

![Health explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Onderdelen op meerdere systemen kunnen worden gecombineerd voor het meten van de status van een gedistribueerde toepassing.  Dit kan zijn bijzonder nuttig voor line-of-business-toepassingen die meerdere gedistribueerde onderdelen bevatten.  U kunt een model dat de status van elk onderdeel meet dat rollup maken in de beschikbaarheid van de toepassing.

Active Directory is een voorbeeld van een management pack dat voorziet in een model voor het analyseren van de gedistribueerde onderdelen.  Het onderstaande voorbeelddiagram toont de status van de algehele omgeving en de relatie tussen forests, domeinen en domeincontrollers.  Elk van deze onderdelen bevat onderdelen en meerdere beeldschermen vergelijkbaar met het bovenstaande voorbeeld van SQL.

![De diagramweergave SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS niet bevatten een algemene engine model van toepassingen of de real-time status meten.  Afzonderlijke oplossingen kunnen de algehele status van bepaalde services op basis van de verzamelde gegevens beoordelen en aangepaste regels kunnen worden geïnstalleerd op de agent voor realtime analyses.  Omdat oplossingen worden uitgevoerd in de cloud met toegang tot de OMS-opslagplaats, kunnen ze vaak grondigere analyse dan gewoonlijk wordt uitgevoerd door management packs te bieden. 

Bijvoorbeeld, de [AD-evaluatie en beoordeling van de SQL-oplossingen](https://technet.microsoft.com/library/mt484102.aspx) verzamelde gegevens te analyseren en geef een classificatie voor verschillende aspecten van de omgeving.  Het bevat aanbevelingen voor verbeteringen die kunnen worden gemaakt voor het verbeteren van de beschikbaarheid en prestaties van de omgeving.

![Oplossing voor AD](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Data-analyse
SCOM en Log Analytics bieden verschillende functies om verzamelde gegevens te analyseren.  SCOM heeft weergaven en Dashboards in de Operations-Console voor het analyseren van recente gegevens in verschillende indelingen en rapporten voor de presentatie van gegevens uit het datawarehouse in tabelvorm.  Log Analytics biedt een volledig logboek querytaal en interface voor het analyseren van gegevens in de OMS-opslagplaats.  Wanneer SCOM wordt gebruikt als gegevensbron voor logboekanalyse, bevat de opslagplaats voor gegevens die door SCOM is verzameld, zodat de Log Analytics-hulpprogramma's kunnen worden gebruikt om gegevens van beide systemen te analyseren.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Weergaven
Weergaven in de Operations-Console kunnen u verschillende gegevenstypen die worden verzameld door SCOM weergeven in verschillende indelingen, doorgaans in tabelvorm voor gebeurtenissen, waarschuwingen en gegevens van de gebruikersstatus en grafieken voor prestatiegegevens van de regel.  Weergaven uitvoeren minimale analyse of consolidatie van de gegevens, maar kunnen u filteren op basis van bepaalde criteria. 

![Weergaven](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Management packs bieden doorgaans meerdere weergaven ondersteuning van de toepassing of het systeem die deze controleert.  Deze kan statusweergaven voor de verschillende objecten die het management pack detecteert, waarschuwingsweergaven voor gedetecteerde problemen en prestatieweergaven voor items bevatten.

Weergaven zijn met name geschikt voor het analyseren van de huidige status van de omgeving met inbegrip van waarschuwingen openen en de status van bewaakte systemen en objecten.  U kunt inzoomen op gebeurtenissen of prestatiegegevens ondersteuning van een bepaalde waarschuwing om de hoofdoorzaak onderzoeken. U kunt ook de prestaties en de status van de verschillende onderdelen van een toepassing voor het evalueren van de huidige status weergeven.

#### <a name="dashboards"></a>Dashboards
Dashboards in de Operations-Console wordt voornamelijk werken met dezelfde gegevens als maar meer aanpasbare en weergaven uitgebreidere visualisaties bevatten kunnen.  Een reeks standaard dashboards zijn beschikbaar u gemakkelijk kunt aanpassen voor uw eigen doeleinden.  U kunt ook een PowerShell-widget waarin gegevens uit een PowerShell-query zijn geretourneerd.

![Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Ontwikkelaars hebben de mogelijkheid aangepaste onderdelen toevoegen aan dashboards die ze in hun management packs opnemen.  Deze kunnen worden maximaal speciaal bedoeld voor een bepaalde toepassing zoals het dashboard in het SQL-management pack hieronder weergegeven.  Dit dashboard kan ook worden gebruikt als een sjabloon voor aangepaste versies.

![SQL-Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Rapporten
Rapporten in SCOM analyseren van gegevens uit het datawarehouse in tabelvorm.  Ze kunnen worden afgedrukt en gepland voor de automatische levering in verschillende bestandsindelingen, met inbegrip van PDF- en CSV- en Word.  Rapporten werken met gegevens uit het datawarehouse, zodat ze met name geschikt voor analyse van trends lange termijn.

Management packs wordt doorgaans aangepaste rapporten geven voor een bepaalde toepassing.  U kunt ook selecteren uit een bibliotheek met algemene rapporten die u aanpassen kunt voor uw eigen toepassingen of voor het uitvoeren van ad-hoc analyses.

Hieronder volgt een voorbeeld prestatierapport uit gegevens die door de Active Directory Management Pack worden verzameld weergegeven.

![Rapport](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Log Analytics is een [querytaal](https://technet.microsoft.com/library/mt484120.aspx) waarmee u kunt analyses uitvoeren op gegevens uit meerdere toepassingen zonder de noodzaak om een aangepaste weergave of rapport te maken.  Omdat OMS is geïmplementeerd in de cloud, worden de prestaties van query's en analyse van gegevens vallen niet onder de hardwarebeperkingen en query's, met inbegrip van miljoenen records snel kunt analyseren. 

Query's in logboekanalyse zijn ook de basis van andere functies.  U kunt een query opslaan, de resultaten exporteren naar Excel of dit wordt automatisch uitgevoerd met regelmatige tussenpozen en een waarschuwing genereren als de resultaten aan bepaalde criteria voldoen.  

![Logboek query stroom](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Hieronder volgt een voorbeeld van een Log Analytics-query.  In dit voorbeeld alle gebeurtenissen met de "gestart" in de naam worden geretourneerd en gegroepeerd door gebeurtenis-id.  De gebruiker gewoon de query geeft en Log Analytics dynamisch genereert de gebruikersinterface voor de analyse.  Selecteer een item in de lijst, wordt de gedetailleerde gebeurtenisgegevens geretourneerd.

![Logboek query](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Naast het bieden van ad-hoc analyse, query's in logboekanalyse kunnen worden opgeslagen voor toekomstig gebruik en ook toegevoegd aan uw [OMS dashboard](http://technet.microsoft.com/library/mt484090.aspx) zoals weergegeven in het volgende voorbeeld.

![OMS-dashboard](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Volgende stappen
* Implementeer [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Aanmelden voor [logboekanalyse](https://azure.microsoft.com/documentation/services/log-analytics).  

