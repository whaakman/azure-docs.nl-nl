---
title: Azure-toepassingen en bronnen bewaken | Microsoft Docs
description: Overzicht van de verschillende Microsoft-services en functionaliteit die aan een strategie voor een volledige controle voor uw Azure-services en toepassingen bijdragen.
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: ffd9a6f75a549b246a04adc5480e988b1622c5ca
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Bewaking van de Azure-toepassingen en bronnen

Bewaking wordt het verzamelen en analyseren van gegevens om te bepalen van de prestaties, status en beschikbaarheid van uw zakelijke toepassingen en de resources die deze afhankelijk is. Een strategie voor een effectieve controle krijgt u inzicht in de gedetailleerde werking van de verschillende onderdelen van uw toepassing en uw bedrijfstijd verbeteren door het verwittigen van kritieke problemen proactief zodat u deze oplossen kunt voordat ze problemen.

Azure bevat meerdere services die afzonderlijk uitvoeren van een bepaalde rol of de taak in de ruimte bewaking en leveren samen een uitgebreide oplossing voor het verzamelen, analyseren en fungeert voor telemetrie van uw toepassing en de onderliggende Azure-resources Deze ondersteuning.  Ze kunnen ook worden gebruikt voor het bewaken van kritieke on-premises bronnen om te voorzien van een hybride omgeving bewaken.   Kennis van de hulpprogramma's en gegevens die beschikbaar zijn, is de eerste stap bij het ontwikkelen van een strategie voor een volledige controle voor uw toepassing. 

Het volgende diagram toont een conceptueel overzicht van de verschillende onderdelen die samenwerken om te voorzien in bewaking van Azure-resources.  Elk van deze wordt beschreven in de volgende secties met koppelingen naar gedetailleerde technische informatie.

![Bewakingsoverzicht](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Basiscontrole
Basic monitoring biedt fundamentele vereist bewaking over Azure-resources.  Deze services minimale configuratie vereisen en verzamelen van core telemetrie die wordt gebruikt door de premium-services controleren.    

### <a name="azure-monitor"></a>Azure Monitor
[Monitor voor Azure](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) kunt basic bewaking voor Azure service doordat de verzameling van [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), en [diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Bijvoorbeeld, het activiteitenlogboek laat u weten wanneer nieuwe resources zijn gemaakt of gewijzigd.  Metrische gegevens beschikbaar zijn die prestatiestatistieken bieden voor verschillende bronnen en ook het besturingssysteem op een virtuele machine.  U kunt deze gegevens weergeven met een van de explorers in de Azure portal, verzenden naar Log Analytics voor trends en gedetailleerde analyse of waarschuwingsregels proactief om u te waarschuwen van kritieke problemen maken.

### <a name="service-health"></a>Service Health
De status van uw toepassing, is afhankelijk van de Azure-services die afhankelijk zijn van.  [Status van de Azure-Service](../service-health/service-health-overview.md) identificeert eventuele problemen met Azure-services die mogelijk gevolgen voor uw toepassing en helpt u van plan voor het onderhoud van een planning bent.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) voortdurend bewaakt uw resource configuratie en gebruik de telemetrie voor u aangepast aanbevelingen op basis van best practices.  Na deze aanbevelingen u helpen te verbeteren de prestaties, beveiliging en beschikbaarheid van de resources die uw toepassingen ondersteunen.


## <a name="premium-monitoring-services"></a>Premium-bewakingsservices
De volgende Azure-services bieden uitgebreide mogelijkheden voor het verzamelen en analyseren van bewakingsgegevens.  Deze voor controle en gebruik de algemene basisfunctionaliteit in Azure bouwen en krachtige analytics voorzien van verzamelde gegevens unieke inzichten geven tot uw toepassingen en infrastructuur.  Deze presenteren gegevens in de context van bepaalde scenario's die zijn gericht op verschillende doelgroepen.

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) Hiermee kunt u controleren van de beschikbaarheid, prestaties en gebruik van uw toepassing of deze opgenomen in de cloud of on-premises.  Door het instrumenteren van uw toepassing met Application Insights werkt, kunt u veel inzicht zodat u snel identificeren en te diagnosticeren fouten zonder te wachten op voor een gebruiker voor het rapporteren van deze bereiken. Met de informatie die u hebt verzameld, kunt u geïnformeerd over opties voor het onderhoud en verbeteringen van uw toepassing.  Naast de uitgebreide hulpprogramma's voor interactie met de gegevens die worden verzameld, opslaat Application Insights gegevens in een algemene opslagplaats gedeelde functionaliteit, zoals waarschuwingen, dashboards en grondige analyse met de Log Analytics query language gebruik te maken.

### <a name="log-analytics"></a>Log Analytics
[Meld u Analytics](http://azure.microsoft.com/documentation/services/log-analytics) speelt een centrale rol in de Azure-bewaking door het verzamelen van gegevens uit diverse bronnen op één opslagplaats waar deze kan worden geanalyseerd met een krachtige querytaal.  Application Insights en Azure Security Center kunt u hun gegevens opslaan in de logboekanalyse gegevens opslaan en gebruikmaken van de analyse-engine.  Deze gecombineerd met gegevens die worden verzameld van de Monitor in Azure, oplossingen voor beheer, en agents zijn geïnstalleerd op virtuele machines in de cloud of on-premises kunnen u een volledig overzicht van uw hele omgeving vormen. 


### <a name="service-map"></a>Serviceoverzicht
[Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) verschaft inzicht in uw omgeving IaaS door virtuele machines met hun verschillende processen en afhankelijkheden van andere computers en externe processen analyseren.  Het is geïntegreerd gebeurtenissen, prestatiegegevens en oplossingen in logboekanalyse zodat u deze gegevens in de context van elke computer en de relatie ervan met de rest van uw omgeving bekijken kunt.  Serviceoverzicht is vergelijkbaar met de [toepassingstoewijzing in Application Insights](../application-insights/app-insights-app-map.md) maar is gericht op de onderdelen van de infrastructuur ondersteuning van uw toepassingen.

### <a name="network-watcher"></a>Network Watcher
[Netwerk-Watcher](../network-watcher/network-watcher-monitoring-overview.md) biedt scenario's gebaseerde controle en diagnostische gegevens voor scenario's met verschillende netwerken in Azure.  Deze gegevens worden opgeslagen in Azure metrische gegevens en diagnostische gegevens voor verdere analyse en werkt met [beheeroplossingen in logboekanalyse](../log-analytics/log-analytics-azure-networking-analytics.md) voor volledige controle van uw netwerkbronnen.


### <a name="management-solutions"></a>Beheeroplossingen
[Oplossingen voor](../log-analytics/log-analytics-add-solutions.md) verpakte sets van logica die inzicht voor een bepaalde toepassing of service bieden.  Ze afhankelijk van logboekanalyse opslaan en analyseren van de bewakingsgegevens die ze verzamelen.  Oplossingen voor het beheer zijn van Microsoft en partners die voorzien in bewaking voor verschillende Azure en derden services beschikbaar. Voorbeeld van de controle van oplossingen omvatten [Container bewaking](../log-analytics/log-analytics-containers.md) waarmee u weergeven en beheren van de container-hosts en [Azure SQL Analytics](../log-analytics/log-analytics-azure-sql.md) die worden verzameld en visualiseren maatstaven voor prestaties voor SQL Azure-databases.


## <a name="shared-functionality"></a>Gedeelde functionaliteit
De volgende Azure-hulpprogramma's bieden essentiële functionaliteit naar de premium-services controleren.  Ze worden gedeeld door meerdere services zodat u kunt gebruikmaken van de algemene functies en configuraties op meerdere services.

### <a name="alerts"></a>Waarschuwingen
[Waarschuwingen van Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proactief waarschuwen u essentiële voorwaarden en mogelijk corrigerende maatregelen te nemen.  Waarschuwingsregels kunnen gebruikmaken van gegevens uit meerdere bronnen, met inbegrip van metrische gegevens en logboekbestanden. Ze gebruiken [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md) die unieke reeksen ontvangers en acties in reactie op een waarschuwing bevat.  U kunt waarschuwingen externe acties met behulp van webhooks starten en integreren met uw ITSM hulpprogramma's op basis van uw vereisten, hebben.

### <a name="dashboards"></a>Dashboards
[Azure Dashboards](../azure-portal/azure-portal-dashboards.md) kunt u verschillende soorten gegevens combineren tot één venster in de Azure-portal en delen met andere Azure-gebruikers.  U kunt bijvoorbeeld een dashboard dat worden gecombineerd met een grafiek van metrische gegevens, een tabel met activiteitenlogboeken, een gebruiksgrafiek met informatie over het van Application Insights en de uitvoer van een zoekopdracht logboek in logboekanalyse tegels maken.

U kunt ook logboekanalyse gegevens exporteren naar [Power BI](https://docs.microsoft.com/power-bi/) om te profiteren van extra visualisaties en zodat de gegevens beschikbaar voor anderen binnen en buiten uw organisatie.

### <a name="metrics-explorer"></a>Metrics Explorer
[Metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) worden numerieke waarden die worden gegenereerd door de Azure-resources waarmee u inzicht in de werking en prestaties van de resource. U kunt metrische gegevens verzenden naar Log Analytics voor analyse met gegevens uit andere bronnen.



### <a name="activity-logs"></a>Activiteitenlogboeken
[Activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) bevatten gegevens over de werking van Azure-resources.  Dit omvat deze informatie als configuratiewijzigingen voor de resource, service health incidenten, aanbevelingen over het beter gebruik van de bron en informatie met betrekking tot bewerkingen voor automatisch schalen.  U kunt de logboeken voor een bepaalde bron weergeven op de pagina in de Azure portal of de weergave van de logboeken van meerdere resources in activiteit logboek Explorer.  U kunt ook activiteitenlogboeken verzenden met Log Analytics, zodat ze kunnen worden geanalyseerd met gegevens die worden verzameld door oplossingen voor het beheer, agents op virtuele machines en andere bronnen.


## <a name="example-scenarios"></a>Voorbeeldscenario 's
Hieronder vindt u hoog niveau voorbeelden van hoe u wilt gebruikmaken van andere controleprogramma's in Azure voor verschillende scenario's.

### <a name="monitoring-a-web-application"></a>Bewaking van een webtoepassing
U kunt een webtoepassing die is geïmplementeerd in Azure met behulp van de App-Services, Azure Storage en een SQL-database.  U zou kunnen beginnen met het openen van [metrische gegevens](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) voor elk van deze afzonderlijke bronnen op de pagina's in Azure-portal.  Dit omvat essentiële informatie zoals het aantal aanvragen voor de toepassing en de gemiddelde reactietijd naast configuratiewijzigingen te identificeren.

Vervolgens kan gaat u naar Monitor in de portal om metrische gegevens en logboeken voor de verschillende bronnen samen weergeven.  Standaard parameters voor de metrische gegevens en vast te stellen u [waarschuwingsregels maken](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proactief om u te waarschuwen wanneer bijvoorbeeld gemiddelde reactietijd is hoger dan de drempelwaarde.  Om een overzicht van de dagelijkse prestaties van uw toepassing, moet u een Azure-dashboard om weer te geven van grafieken met metrische gegevens die kritieke KPI's maken.

Om uit te voeren diepe bewaking van uw toepassing u [configureren voor Application Insights](../application-insights/quick-monitor-portal.md).  U kunt nu aanvullende gegevens verder biedt meer informatie over de werking en prestaties van uw toepassing verzamelen.  Application Insights detecteert de onderliggende relaties tussen onderdelen van uw app waardoor een visuele representatie via de [toepassingstoewijzing](../application-insights/app-insights-app-map.md) samen met [end-to-end tracering](../application-insights/app-insights-transaction-diagnostics.md) op te sporen de exacte onderdeel, afhankelijkheid of uitzondering waar een probleem opgetreden.  U maakt [beschikbaarheidstests](../application-insights/app-insights-monitor-web-app-availability.md) proactief de toepassing te testen uit verschillende regio's.  Om uw ontwikkelaars te helpen u [inschakelen van de Profiler](../application-insights/enable-profiler-compute.md) zodat u kunt aanvragen en eventuele uitzonderingen naar beneden op een specifieke coderegel bijhouden.  

U toevoegen om nog meer inzicht verkrijgen in services die worden gebruikt in uw toepassing, de [SQL Analytics-oplossing](../log-analytics/log-analytics-azure-sql.md) voor het verzamelen van aanvullende gegevens in logboekanalyse. U besluit na enige tijd voor het onderzoeken van de hoofdoorzaak voor perioden wanneer op de prestaties van de site onder de drempelwaarde ligt.  U schrijft een query met logboekanalyse correleren van gegevens van het gebruik en prestaties door Application Insights met configuratie en prestaties gegevens verzameld over de Azure-resources die uw toepassing te ondersteunen.


### <a name="monitoring-virtual-machines"></a>Bewaking van virtuele machines
U hebt een combinatie van Windows en Linux virtuele machines in Azure.  U Azure Monitor weergeven [activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) en [niveau metrische gegevens hosten](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en voeg vervolgens de [extensie voor diagnostische gegevens van Azure](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) aan de virtuele machines om te kunnen verzamelen van meetgegevens over vanuit het gastbesturingssysteem.  Vervolgens maakt u [waarschuwing regels](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proactief om u te waarschuwen als basis metrische gegevens tussen verschillende drempelwaarden die het gebruik van processors en geheugen.

Meer informatie over virtuele machines met een bedrijfstoepassing verzamelen u [maken van een werkruimte voor logboekanalyse en de VM-extensie inschakelen](../log-analytics/log-analytics-quick-collect-azurevm.md) op elke machine.  U configureert [verzameling van verschillende gegevensbronnen](../log-analytics/log-analytics-data-sources.md) voor uw toepassing en [weergaven maken](../log-analytics/log-analytics-view-designer.md) voor het rapporteren van de dagelijkse werking en prestaties.  U vervolgens [waarschuwingsregels maken](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) om u te waarschuwen wanneer bepaalde foutgebeurtenissen worden ontvangen.  U toevoegen om de status van de geïnstalleerde agent continu bewaakt, de [Health Agent-beheeroplossing](../operations-management-suite/oms-solution-agenthealth.md).

Verder om inzicht te krijgen in de toepassing u [toevoegen van de agent voor afhankelijkheden](../operations-management-suite/operations-management-suite-service-map-configure.md) aan de virtuele machines om te kunnen toevoegen aan [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md).  Het kritieke processen detecteert en identificeert de verbindingen tussen machines met andere services.  Na een storing gemelde u Serviceoverzicht gebruiken om uit te voeren forensische om te identificeren van de specifieke machines die het probleem optrad.  Vervolgens maakt u een [query op de gegevens logboekanalyse](../log-analytics/log-analytics-log-search-new.md) vaststellen van het probleem in de toekomst en maken van een waarschuwingsregel proactief om u te waarschuwen wanneer de voorwaarde is gedetecteerd.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over

* [Azure Monitor in een video Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Aan de slag met Azure Monitor](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) als u probeert te analyseren van problemen in uw Cloud-Service, de virtuele Machine virtuele machine instellen of de Service Fabric-toepassing schalen.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) als u te diagnostische problemen in uw App Service-Web-app probeert.
* [Meld u Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) voor het analyseren van verzamelde bewakingsgegevens.
